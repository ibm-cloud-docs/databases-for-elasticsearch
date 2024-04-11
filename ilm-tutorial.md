---

copyright:
  years: 2024
lastupdated: "2024-04-11"

keywords: elasticsearch, index-lifecycle-management

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 2hrs

---

{{site.data.keyword.attribute-definition-list}}

# Configuring the Index Lifecycle Management capabilities of {{site.data.keyword.databases-for-elasticsearch}}
{: #ilm-elasticsearch}
{: toc-content-type="tutorial"}
{: toc-completion-time="2hrs"}

Index Lifecycle Management (ILM) is a great feature of Elasticsearch. It allows you to proactively manage your indices to make efficient use of resources, both in terms of storage and search capabilities. For example, if an application needs to store 30 days of events, ILM can be used to create an index called “events”, which can be written to and queried easily, but in reality consists of thirty separate indices in the background. Older indices can be made “read only” and optionally optimized, and when they reach the age of 30 days, deleted.

Lifecycle rules can be defined, including:

- When creating a new index: by age, data volume, or document count.
- Whether to make older indices “read only”.
- Whether to change the shard count of older indices.
- Setting the priority of each index, which defines the order in which indices are restored on node reboots.
- If and when older data is deleted.

ILM is very flexible and feature-rich. For a full description of its capabilities, see the [ILM overview](https://www.elastic.co/guide/en/elasticsearch/reference/current/overview-index-lifecycle-management.html).

In this tutorial, you will get familiar with ILM by creating a set of simple rules and then watching how they get implemented. Although the set up is relatively simple, you will need to let the rules take their course over a couple of days to see the full effect.

{{site.data.keyword.databases-for-elasticsearch}} is a paid-for service, so following this tutorial will incur charges.
{: note}

## Before you start
{: #ilm-elasticsearch-before-start}
{: step}

Before you begin, ensure that you have the following:

- An [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - to deploy infrastructure.

## Obtain an API key to deploy infrastructure to your account
{: #ilm-elasticsearch-obtain-key}
{: step}

Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an {{site.data.keyword.cloud_notm}} API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

## Clone the project
{: #ilm-elasticsearch-clone-project}
{: step}

To clone the project, run the following command:

```sh
git clone https://github.com/IBM/elasticsearch-index-lifecycle-management.git
```
{: pre}

## Install the Elasticsearch cluster
{: #ilm-elasticsearch-install-infra}
{: step}

1. Navigate into the Terraform folder of the cloned project.

```sh
cd elasticsearch-index-lifecycle-management/terraform
```
{: pre}
2. Create a document that is named `terraform.tfvars`, with the following fields:

```sh
ibmcloud_api_key = "<your_api_key_from_step_1>"
region = "<your_region>"
elastic_password = "<make-up-a-password>"
```
{: pre}
The `terraform.tfvars` document contains variables that you may want to keep secret, so it is excluded from the public Github repository.
{: important}
3. Install the infrastructure with the following command:

```sh
terraform init
terraform apply --auto-approve
```
{: pre}
4. Export the database access URL to your terminal environment (it will be required by subsequent steps).

```sh
terraform output --json
export ES="<the url value obtained from the output>"
```
{: pre}


## Create an ILM process
{: #ilm-elasticsearch-ilm-setup}
{: step}

Let’s assume that you have logs coming from your applications into an Elasticsearch instance. The logs are very important on day one because you are checking for anomalies. After day two, the logs become less useful but you still need them around for things like trying to spot trends. After three days, these logs are stale and you have no further use for them. So, create a three day lifecycle for your index:

- Day 1: Your data is in the Hot Tier, meaning it is readily available for search. It is your most recent, most searched data.
- Day 2: Your data is in the Warm Tier. After day 1, your data is rolled over to a “warm” state. In this tier, it is optimized for search rather than indexing. In this tier, you will force a merge to reduce the number of segments in the index’s shards, for more efficient searching.
- Day 3: Delete. After day 3 your data is no longer needed, so it gets deleted.


### Create an Index Lifecycle policy
{: #ilm-elasticsearch-ilm-lifecycle}
{: step}

First, create an ILM policy that defines the appropriate phases and actions as described above.

```sh
curl -kX PUT -H 'Content-Type: application/json' -d'{"policy":{"phases":{"hot":{"actions":{"rollover":{"max_age":"1d"},"set_priority":{"priority":100},"forcemerge":{"max_num_segments":1},"shrink":{"number_of_shards":1},"readonly":{}},"min_age":"0ms"},"warm":{"min_age":"1d","actions":{"set_priority":{"priority":50}}},"delete":{"min_age":"3d","actions":{"delete":{}}}}}}' $ES/_ilm/policy/ilm-test-1
```
{: pre}

### Create an index template
{: #ilm-elasticsearch-ilm-template}
{: step}

An index template defines how indices are going to be created. Because our lifecycle policy above moves data from hot to warm every day, a new index will be created each day. This template tells Elasticsearch what patterns to use to create these new indices: in this case all related indices will be called “logs-”, followed by an incrementing number. The template also has other settings, such as what lifecycle policy to use for the index. Let's use the one we created in the previous step.

Index templates are created in two steps. First, you create one or more “component” templates. These are reusable blocks that can be combined later to create multiple templates. In this very simple example you create only one component template.

```sh
curl -kX PUT -H 'Content-Type: application/json' -d'{"template":{"mappings":{"properties":{"@timestamp":{"type":"date"}}}}}' $ES/_component_template/component_template1
```
{: pre}

(This component template is basically empty except for a mapping to a timestamp field (all logs have a timestamp) but a real-world use case can contain complex mappings and other instructions).

Second, you create the index template itself, making use of your component template(s). This one tells Elasticsearch about the index pattern and other data, such as  for example that every new index must have two shards and two replicas.

```sh
curl -kX PUT -H 'Content-Type: application/json' -d'{"index_patterns":["logs-*"],"template":{"settings":{"number_of_shards":2,"number_of_replicas":2,"index.lifecycle.name":"ilm-test-1","index.lifecycle.rollover_alias":"logs"}},"priority":500,"composed_of":["component_template1"],"version":3,"_meta":{"description":"my custom template"}}' $ES/_index_template/my_index_template
```
{: pre}

### Create an index
{: #ilm-elasticsearch-ilm-index}
{: step}

The last step is to create an Elasticsearch index that will use your template (and therefore your lifecycle policy). By calling it “logs-000001” and aliasing it to the alias defined in the template, you ensure that the right template is used and that it is incremented numerically.

```sh
curl -kX PUT -H 'Content-Type: application/json' -d'{"aliases": {"logs": { "is_write_index": true } } }' $ES/logs-000001
```
{: pre}


## Add documents to the index
{: #ilm-elasticsearch-add-documents}
{: step}

Documents can be added without knowing the name of the current `logs` index, we simply write to `logs`.

```sh
curl -kX PUT -d’{document goes here}’ $ES/logs/_doc/mydocid
```
{: pre}

## Query the index
{: #ilm-elasticsearch-query-index}
{: step}

Although Elasticsearch is storing data in multiple indices, it can still be queried as if it were one.

```sh
curl -X POST -d’{query goes here}’ $ES/logs/_search
```
{: pre}


### Watch your index manage itself
{: #ilm-elasticsearch-ilm-observe}
{: step}

Now you have to wait a bit. On day one you will be able to see an index called `logs-000001`.

```sh
curl -kX GET $ES/_cat/indices | grep logs-
```
{: pre}

But on day two you will see another index called `logs-000002` appear, if you run the above command again. And on day three, `logs-000001` should disappear (because it was deleted) but you should see `logs-000003` appear. You can always search the entire contents of your logs at any point by using the alias `logs`. Your indices are managing themselves!


## Tear down your infrastructure
{: #ilm-elasticsearch-tear-down}
{: step}

Your {{site.data.keyword.databases-for-elasticsearch}} incurs charges. After you finish this tutorial, you can remove all the infrastructure by going to the `terraform` directory of the project and using the command:

```sh
terraform destroy
```
{: pre}

## Next Steps
{: #ilm-elasticsearch-next-steps}

ILM is very feature-rich and in this tutorial you have only explored the basics of it. ILM can help you manage your data in an efficient way. For more information, see [ILM: Manage the index lifecycle](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html).
