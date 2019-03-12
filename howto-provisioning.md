---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-02"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Provisioning {{site.data.keyword.databases-for-elasticsearch}}
{: #provisioning}

To create an {{site.data.keyword.databases-for-elasticsearch_full}} deployment, you need to create an {{site.data.keyword.cloud}} service instance. A service instance can represent different types of service. The service type is determined by the service ID and you need to specify the appropriate service ID when you create a new service instance. The service ID for {{site.data.keyword.databases-for-elasticsearch}} is `databases_for_elasticsearch`.

## Using the catalog

You can create a {{site.data.keyword.databases-for-elasticsearch}} service from the [{{site.data.keyword.databases-for-elasticsearch}} page](https://{DomainName}/catalog/services/databases-for-elasticsearch/) in the {{site.data.keyword.cloud_notm}} catalog.

![Catalog Deployment Page](images/catalog-deployment.png)

When you create the deployment from the catalog, you need to specify the following parameters.

1. **The service name** - The name can be any string and is the name that is used on the web and in the command line to identify the new database deployment.
2. **The region** - The region in which the database deployment resides.
3. **The database version** - The major version of the database to be created within the deployment. The latest minor version is always be used automatically. 

Users can optionally set:

1. **The resource group** - If you are organizing your services into [resource groups](/docs/resources?topic=resources-bp_resourcegroups), you can specify the resource group in this field. Otherwise, you can leave it at default.
2. **Disk encryption** - Optionally, a Key Protect instance can be selected if the user has Key Protect configured. If it is configured, once the service is selected, a disk encryption key can be selected from the Key Protect service. By default, Key Protect is not used and the deployment automatically creates and manages its own disk encryption key. 
3. **Initial resource allocation** - Specify initial memory and disk sizes for your databases. The minimum sizes of memory and disk are selected by default.

Once you select the appropriate settings, click **Create** to start the provisioning process off.

The database takes some time to deploy, and you are navigated back to your {{site.data.keyword.cloud_notm}} Resource List.

## Using the Command Line

The {{site.data.keyword.cloud_notm}} CLI tool is what you use to communicate with {{site.data.keyword.cloud_notm}} from your terminal or command line. For more information, see [Download and install {{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

To create a {{site.data.keyword.databases-for-elasticsearch}} deployment, you use the CLI to request a service instance with a `databases-for-elasticsearch` service ID.

The command template is:

```
ibmcloud resource service-instance-create <service-name> <service-id> <service-plan-id> <region>
```

More information about this command, in general, is available in the [CLI reference for resource groups](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_resource_service_instance_create).

In the specific case of creating a {{site.data.keyword.databases-for-elasticsearch}} deployment, set the service name (quote any name with spaces in it). Then, set `databases-for-elasticsearch` as the service ID. Enter `standard` for the service plan ID and `us-south` (or your region) for the region.

```
ibmcloud resource service-instance-create example-es databases-for-elasticsearch standard us-south
```

When the command is run, provisioning the database deployment begins. The database takes some time to deploy. You can check on its progress on your {{site.data.keyword.cloud_notm}} Dashboard. You can also run:

```
ibmcloud resource service-instance <service-name>
```

This command reports the current state of the service instance.

### Additional parameters

The `service-instance-create` command supports a `-p` flag, which allows [additional parameters](#list-of-additional-parameters) to be passed to the provisioning process. The parameters are in JSON format. Some parameters values are CRNs (Cloud Resource Name), which uniquely identifies a resource in the cloud. All parameter names and values are passed as strings.

For example, if a database is being provisioned from a particular backup and the new database deployment needs each database member to have with 2 GB, then the command looks like:

```
ibmcloud resource service-instance-create example-es databases-for-elasticsearch standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "members_memory_allocation_mb": "6144"
}'
```

## Provisioning through the Resource Controller API

You can provision new deployments by using the Resource Controller API. However, in order to use the Resource Controller API, you need some additional preparation.

1. [Obtain an IAM token from your API token](https://{DomainName}/apidocs/resource-controller#authentication).
2. You need to know the ID of the resource group that you would like to deploy to. This information is available through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_resource_groups). You can find a list of resource groups with `ibmcloud resource groups` and the ID of a resource group with `ibmcloud resource group`. 
3. You need to know the region that you would like to deploy to.

Once you have all the information, the create request is a `POST` to the `https://resource-controller.bluemix.net/v2/resource_instances` endpoint.

```
curl -X POST \
  https://resource-controller.bluemix.net/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "bluemix-us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-elasticsearch-standard"
  }'
```
The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. If needed, you can send [additional parameters](#list-of-additional-parameters) in the request body.

More information on the Resource Controller API is found in its [API Reference](https://{DomainName}/apidocs/resource-controller#create-provision-a-new-resource-instance).

## Provisioning with Terraform

If you use [Terraform](https://www.terraform.io/) to manage your infrastructure, the [{{site.data.keyword.cloud_notm}} provider for Terraform](https://ibm-cloud.github.io/tf-ibm-docs/) supports provisioning {{site.data.keyword.databases-for-elasticsearch}}. 

Example configuration:

```
data "ibm_resource_group" "group" {
  name = "default"
}
resource "ibm_resource_instance" "resource_instance" {
  name              = "example-terraform-elasticsearch"
  location          = "us-south"
  service           = "databases-for-elasticsearch"
  plan              = "standard"
  resource_group_id = "${data.ibm_resource_group.group.id}"
  parameters = {}
}
```

The `name`, `location`, `service`, and `plan` fields are all required. The `resource_group_id` is not required, and it uses the default resource group if not supplied.

You can send any needed [additional parameters](#list-of-additional-parameters) in the `parameters` field as a JSON object.

More information about this specific {{site.data.keyword.cloud_notm}} provider configuration is available under the [Cloud Foundry Resources, `resource_instance`](https://ibm-cloud.github.io/tf-ibm-docs/v0.14.0/r/resource_instance.html) documentation.

## List of Additional Parameters

* `backup_id`- A CRN of a backup resource to restore from. The backup must have been created by a database deployment with the same service ID. The backup is loaded after provisioning and the new deployment starts up that uses that data. A backup CRN is in the format `crn:v1:<...>:backup:<uuid>`. If omitted, the database is provisioned empty. This parameter cannot be set with a **version** parameter
* `version` - The version of the database to be provisioned. If omitted, the database is created with the most recent major and minor version. This parameter cannot be set with a **backup_id** parameter.
* `key_protect_key` - A CRN that references a Key Protect key, which is then used for disk encryption.
* `members_memory_allocation_mb` -  Total amount of memory to be shared between the database members within the database. For example, if the value is "6144" then the three database members get 6 GB of RAM between them, giving 2 GB of RAM per member. If omitted, the default value is used; "3072".
* `members_disk_allocation_mb` - Total amount of disk to be shared between the database members within the database. For example, if the value is "30720" then the  database members get 30 GB of disk between them, giving 10 GB of disk per member. If omitted, the default value is used; "15360".