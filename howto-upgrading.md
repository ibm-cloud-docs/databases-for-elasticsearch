---
copyright:
  years: 2019, 2024
lastupdated: "2024-11-15"

keyowrds: elasticsearch, databases, upgrading, 7.x, reindex, indices, update user passwords, retrieve user passwords, elasticsearch 7.17, indexes, reindexing, reindex

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading to a new major version
{: #upgrading}

When a major version of a database is at its end of life (EOL), upgrade to the current major version. You can upgrade {{site.data.keyword.databases-for-elasticsearch_full}} deployments to use the newest version of Elasticsearch.

Upgrade to the latest version of Elasticsearch available to {{site.data.keyword.databases-for-elasticsearch}}. Find the latest version through the [catalog](https://cloud.ibm.com/catalog/services/databases-for-elasticsearch){: external}, the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show){: external}, or the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeployables){: external}.

Upgrading is handled by [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui#restore-backup) of your data into a new deployment. Restoring from a backup has a number of advantages:

- The original database stays running and production work can be uninterrupted.
- You can test the new database out of production and act on any application incompatibilities.
- The entire process can be rerun at any point.
- A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.

## Before upgrading
{: #before-upgrading}

Before you upgrade your cluster to version 8.x, take the following actions:

- Check the [deprecation logs](https://www.elastic.co/guide/en/elasticsearch/reference/current/logging.html#deprecation-logging){: .external} that are automatically enabled on {{site.data.keyword.databases-for-elasticsearch}} and sent to [{{site.data.keyword.logs_full}}](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-logging) to see whether you are using any deprecated features and update your code.
- Review the [breaking changes](https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking-changes.html){: .external} and make any necessary changes to your code and configuration for version 8.x.
- If you use plug-ins, make sure that each plug-in version is compatible with Elasticsearch version 8.x.

### Index mappings
{: #index-mappings}

Mapping types are removed in Elasticsearch 7.x and above. Indexes that are created in Elasticsearch 7.x or later no longer accept a *default* mapping. Types are also deprecated in APIs in 7.x. For more information, see [Elasticsearch removal of mapping types](https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html){: .external}.

### Reindexing guidelines
{: #upgrade-reindexing}

{{site.data.keyword.databases-for-elasticsearch}} indexes are only compatible with the same version or plusOne version from the release on which they are created. Only an explicit reindex operation updates an index to the current database version.

### Connection to Kibana
{: #kibana-connection}

If you are deploying [Kibana](https://www.elastic.co/kibana/){: external} to connect to your {{site.data.keyword.databases-for-elasticsearch}} instance, remember that the version of Kibana that you deploy must match the version of your Elasticsearch instance. Provision a new version of Kibana to maintain connectivity with your upgraded instance.

## Upgrading in the UI
{: #upgrading-ui}
{: ui}

Upgrade to a new version when [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui#restore-backup) from the **Backups and restore** tab on the *Overview* page of your deployment. Click **Restore backup** on either the overflow menu or the expanded table row of your chosen backup. This opens the restore provisioning page where you can select options for the new deployment. One of the options is the Database Version, which is auto-populated with the versions available to you. Select a version and click **Restore backup** to start the provision and restore process.

## Upgrading through the CLI
{: #upgrading-cli}
{: cli}

When you upgrade and restore from backup through the {{site.data.keyword.cloud_notm}} CLI, use the following provisioning command from the resource controller.

```sh
ibmcloud resource service-instance-create <service-name> <service-id> <service-plan-id> <region>
```
{: pre}

The parameters `service-name`, `service-id`, `service-plan-id`, and `region` are all required. You also supply the `-p` with the version and backup ID parameters in a JSON object. The new deployment is automatically sized with the same disk and memory as the source deployment at the time of the backup.

The list of backups and backup IDs for a deployment can be retrieved using the following command.

```sh
ibmcloud cdb deployment-backups-list <deployment name or CRN> --json
```
{: pre}

Use the ID of your chosen backup as a parameter in the resource controller command as shown below.

```sh
ibmcloud resource service-instance-create example-es-upgrade databases-for-elasticsearch standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "version":8.12
}'
```
{: pre}

## Upgrading through the API
{: #upgrading-api}
{: api}

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-provisioning&interface=api) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version and backup ID. The new deployment has the same memory and disk allocation as the source deployment at the time of the backup.

The list of backups and backup IDs for a deployment can be retrieved using the following API request.

```sh
curl -X GET  https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/backups  
-H 'Authorization: Bearer <>' \
```
{: pre}

Use the ID of your chosen backup in the resource controller API request as in the following example.

```sh
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
    "version":8.12
  }'
```
{: pre}
