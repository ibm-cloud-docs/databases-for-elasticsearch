---

copyright:
  years: 2019, 2022
lastupdated: "2022-06-27"

keywords: elasticsearch nodes, elasticsearch data members, databases, scaling, elasticsearch horizontal scaling

subcollection: databases-for-elasticsearch

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Adding Elasticsearch Nodes
{: #horizontal-scaling}

It is possible to scale your {{site.data.keyword.databases-for-elasticsearch_full}} deployment horizontally by adding more Elasticsearch nodes (also referred to as members). If your deployment starts to strain or slow down, adding nodes increases capacity and reliability. When a node is added, Elasticsearch automatically balances the workload across all the nodes in your deployment.

Nodes that you add to your deployment are added with the amount of disk, memory, and CPU as the other nodes currently in your deployment. A visual representation of your data members and their resource allocation is available on the _Resources_ tab of your deployment's _Manage_ page. However, horizontal scaling is only available by using the API.

![The Scale Resources Pane in _Resources_](images/settings-scaling.png){: caption="Figure 1. The Scale Resources Pane" caption-side="bottom"}

A default {{site.data.keyword.databases-for-elasticsearch_full}} deployment runs with three data members in a cluster, and resources are allocated to all three members equally. For example, the minimum storage of an Elasticsearch deployment is 15360 MB, which equates to an initial size of 5120 MB per member. The minimum RAM for an Elasticsearch deployment is 3072 MB, which equates to an initial allocation of 1028 MB per member. Adding a node adds another member with a size of 5120 MB of disk and 1028 MB of RAM, bringing your total resource usage for your deployment to 20480 MB of disk and 4096 MB of RAM.

Billing is based on the _total_ amount of resources that are allocated to the service. 
{: .tip}

## Adding Nodes through the API
{: #adding-nodes-api}

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API.

To view the current and scalable resources on a deployment, use the [/deployments/{id}/groups](https://cloud.ibm.com/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl)
```sh
curl -X GET -H "Authorization: Bearer $APIKEY" `https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups'
```

To add nodes, use the [/deployments/{id}/groups/{group_id}](https://cloud.ibm.com/apidocs/cloud-databases-api#set-scaling-values-on-a-specified-group) API endpoint, sending a PATCH request with the number of nodes you want in your deployment. The example request increases the number of nodes from the default of 3 to 5.
```sh
curl -X PATCH 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups/member' \
-H 'Authorization: Bearer <>' \
-H 'Content-Type: application/json' \
-d '{"members": {"allocation_count": 5}}' \
```

When you use the CRN, remember to URL encode the CRN value as it might include the forward-slash (/) `%2F` character. 
{: .tip}

When properly encoded, a CRN that uses a forward-slash (/) character substitutes with a `%2F` character string. For example, the following CRN 
```sh
crn:v1:bluemix:public:databases-for-redis:us-south:a/274074dce64e9c423ffc238516c755e1:29caf0e7-120f-4da8-9551-3abf57ebcfc7::
```
becomes
```sh
crn:v1:bluemix:public:databases-for-redis:us-south:a%2F274074dce64e9c423ffc238516c755e1:29caf0e7-120f-4da8-9551-3abf57ebcfc7::
``` 
