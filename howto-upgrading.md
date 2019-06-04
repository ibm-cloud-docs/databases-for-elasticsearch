---
copyright:
  years: 2019
lastupdated: "2019-06-04"

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Upgrading to a new Major Version
{: #upgrading}

Once a major version of a database is at its End Of Life (EOL), it is a good idea to upgrade to the current major version. You can upgrade {{site.data.keyword.databases-for-elasticsearch_full}} deployments to use the newest version of Elasticsearch. It is currently possible to migrate from Elasticsearch 5.x to 6.x.

You upgrade to the latest version of Elasticsearch available to {{site.data.keyword.databases-for-elasticsearch}}. You can find the latest version from the catalog page, from the cloud databases cli plugin command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployables-show) or from the cloud databases API [`/deployables`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-all-deployable-databases) endpoint.

Upgrading is handled through restoring your data into a new deployment. Restoring from a backup has a number of advantages:

- The original database stays running and production work can be uninterrupted.
- You can test the new database out of production and act on any application incompatibilities.
- The entire process can be re-run at any point.
- A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.

In order to restore a deployment from a backup into a specific version, you have to use the {{site.data.keyword.cloud_notm}} CLI or the API.

## Upgrading through the CLI

When you upgrade and restore from backup through the  {{site.data.keyword.cloud_notm}} CLI, use the provisioning command from the resource controller.
```
ibmcloud resource service-instance-create <service-name> <service-id> <service-plan-id> <region>
```
The parameters `service-name`, `service-id`, `service-plan-id`, and `region` are all required. You also supply the `-p` with the version, backup ID, and the size parameters in a JSON object.

If your deployment is larger than the default deployment (5120 MB disk and 1028 MB memory per member), you need to provide the memory and disk allocation to the command. Otherwise, the provisioned deployment gets the default resource allocation. If it is too small, the upgrade will fail.
{: .tip}

```
ibmcloud resource service-instance-create example-es-upgrade databases-for-elasticsearch standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "version":6.5,
  "members_memory_allocation_mb": "6144"
}'
```

## Upgrading through the API

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](docs/services/databases-for-elasticsearch?topic=cloud-databases-provisioning#provisioning-through-the-resource-controller-api) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version, backup ID, and the size parameters.
```
curl -X POST \
  https://resource-controller.bluemix.net/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "bluemix-us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-elasticsearch-standard",
    "backup_id": "crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
    "version":6.5,
    "members_memory_allocation_mb": "6144"
  }'
```

## Migration Notes for New Elasticsearch 6.x Users

As in previous version upgrades, there are many changes. There are two in particular that might affect how you use Elasticsearch.

### Pre-5.x Indices
Elasticsearch will usually support reading indices that are created in the previous major version. This means that any indices created in Elasticsearch 5.x will be readable by Elasticsearch 6.x.

If you have indices that were created in Elasticsearch 2.x, they are **not** be readable in Elasticsearch 6.x. Those indices will need to be re-indexed in Elasticsearch 5.x before migrating to 6.x. If your data contains indices made in Elasticsearch 2.x and you attempt to migrate to Elasticsearch 6.x, the nodes fail to start and the provisioning fails.

[Re-indexing can be done in-place](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html) in Elasticsearch 5.x before migrating to 6.x through the [Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html).

### Index mappings
The ability to create multiple mapping types per index has been removed in Elasticsearch 6.x. Any multiple index mappings created in Elasticsearch 5.x continues to work, but Elasticsearch 6.x only supports creation of a single mapping type per index.

Mapping types are being removed in Elasticsearch 7.x and above.

### Other changes
Full documentation of breaking changes can be found in the [Elasticsearch docs](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html).