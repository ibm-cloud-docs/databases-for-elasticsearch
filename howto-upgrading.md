---
copyright:
  years: 2019, 2021
lastupdated: "2021-03-12"

keyowrds: elasticsearch, databases, upgrading, 5.x, 6.x, 7.x, reindex, indices

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

When a major version of a database is at its end of life (EOL), it is a good idea to upgrade to the current major version. You can upgrade {{site.data.keyword.databases-for-elasticsearch_full}} deployments to use the newest version of Elasticsearch. It is possible to upgrade from Elasticsearch 6.x to 7.x.

You upgrade to the latest version of Elasticsearch available to {{site.data.keyword.databases-for-elasticsearch}}. You can find the latest version from the catalog page, from the cloud databases cli plug-in command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show), or from the cloud databases API [`/deployables`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-all-deployable-databases) endpoint.

Upgrading is handled through [restoring a backup](/docs/databases-for-elasticsearch?topic=cloud-databases-dashboard-backups#restoring-a-backup) of your data into a new deployment. Restoring from a backup has a number of advantages:

- The original database stays running and production work can be uninterrupted.
- You can test the new database out of production and act on any application incompatibilities.
- The entire process can be rerun at any point.
- A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.


## Before upgrading
Before you start to upgrade your cluster to version 7.x, you must take the following actions.

- Check the [deprecation logs](https://www.elastic.co/guide/en/elasticsearch/reference/current/logging.html#deprecation-logging), that are automatically enabled on Databases for Elasticsearch and sent to [{{site.data.keyword.la_full}}](/docs/databases-for-elasticsearch?topic=cloud-databases-logging), to see whether you are using any deprecated features and update your code.
- Review the [breaking changes](https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking-changes.html) and make any necessary changes to your code and configuration for version 7.x.
- If you use any plug-ins, make sure that there is a version of each plug-in that is compatible with Elasticsearch version 7.x.
- Reindex your data before running the 6.x to 7.x version upgrade by using [the guidance here](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-upgrading#upgrade-reindexing).

### Reindexing guidelines
{: #upgrade-reindexing}

{{site.data.keyword.databases-for-elasticsearch}} indexes are only compatible with the same version or plus-one version from the release they are create on. By example, if you create an index on Elasticsearch (ES) 5 it can be read by ES5 and ES6. An index that is created in Elasticsearch 6 can be read by ES6 and ES7. Only an explicit reindex operation updates an index to the current database version.

Any index that originates from an ES5 backup is still in ES5 format, even if it resides in an ES6 deployment. In these cases, ES7 can't read those indexes. 

If you have a deployment with such an ES6 backup or restore, and you attempt to upgrade to ES7, those indexes are not restorable without reindexing before the upgrade to ES7.

### Check whether you have an ES5 index in your ES6 deployment 
  Some {{site.data.keyword.databases-for-elasticsearch}} deployments were upgraded from ES 5.x to ES 6.x. Any index that was not created on that ES6 deployment, but originated from the ES5 backup, is still in ES5 format.
  
  You must check the index version to determine the need to reindex:
    
  For each index, call the API `indexname/_settings?pretty`. For example,
    
  `curl -k https://user:password@host:port/test1/_settings?pretty`

  In the result, look for the version field. For example,

  ```
  "version" : {
       "created" : "6080699",
       "upgraded" : "7090299"
     }
  ```
  If the version field contains an upgraded entry, the index was imported from an older ES version and must be reindexed before upgrading.

  
### Reindex in place on your ES6 deployment before upgrading

  If you have indexes that were created in ES5, you must reindex or delete them before upgrading to ES7. 

  To reindex your ES5 indexes in place:

  1. Create a new index in your ES6 deployment with 7.x compatible mappings.
  2. Note down the existing `refresh_interval` and `number_of_replicas` values, then set the `refresh_interval` to `-1` and the `number_of_replicas` to `0` for efficient reindexing.
  3. Use the [reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html) to copy documents from the ES5 index into your new ES6 index. 
  4. Reset the `refresh_interval` and `number_of_replicas` to the values you noted in step 2.
  5. After the index status changes to green, use a single `update aliases` request to:
      - Delete the old index.
      - Add an alias with the old index name to the new index.
      - Add any other aliases that existed on the old index to the new index.

  For more details, refer to the information in the Elasticsearch documentation to [Reindex in place](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html) on your 6.x cluster before upgrading.

Use a script to perform any necessary modifications to the document data and metadata during reindexing.
{: .tip}



## Upgrading in the UI

You can upgrade to a new version when [restoring a backup](/docs/databases-for-elasticsearch?topic=cloud-databases-dashboard-backups#restoring-a-backup) from the _Backups_ tab of your _Deployment Overview_. Clicking **Restore** on a backup brings up a dialog box where you can change some options for the new deployment. One of them is the database version, which is auto-populated with the versions available for you to upgrade to. Select a version and click **Restore** to start the provision and restore process.

## Upgrading through the CLI

When you upgrade and restore from backup through the  {{site.data.keyword.cloud_notm}} CLI, use the provisioning command from the resource controller.
```
ibmcloud resource service-instance-create <service-name> <service-id> <service-plan-id> <region>
```
The parameters `service-name`, `service-id`, `service-plan-id`, and `region` are all required. You also supply the `-p` with the version and backup ID parameters in a JSON object. The new deployment is automatically sized with the same disk and memory as the source deployment at the time of the backup.

```
ibmcloud resource service-instance-create example-es-upgrade databases-for-elasticsearch standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "version":7.9
}'
```

## Upgrading through the API

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](/docs/databases-for-elasticsearch?topic=cloud-databases-provisioning#provisioning-through-the-resource-controller-api) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version and backup ID. The new deployment has the same memory and disk allocation as the source deployment at the time of the backup.
```
curl -X POST \
  https://resource-controller.cloud.ibm/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "bluemix-us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-elasticsearch-standard",
    "backup_id": "crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
    "version":7.9
  }'
```

## Migration Notes for New Elasticsearch 7.x Users 
 
As in previous version upgrades, there are many changes. Full documentation of breaking changes can be found in the [Elastic documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking-changes.html). 
 

### Index mappings 

Mapping types are removed in Elasticsearch 7.x and above. Indexes that are created in Elasticsearch 7.x or later no longer accept a _default_ mapping. Types are also deprecated in APIs in 7.x. Further details on these changes are in the Elastic documentation on the [removal of mapping types](https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html).

