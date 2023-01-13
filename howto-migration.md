---
copyright:
  years: 2019, 2023
lastupdated: "2023-01-13"

keywords: elasticsearch migration, databases, elasticsearch migrating

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Migrating Elasticsearch
{: #compose-migrating}

If you are a current user of Elasticsearch, use the [Snapshot/restore Elasticsearch API](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html){: external}
and the [S3 repository plug-in](https://www.elastic.co/guide/en/elasticsearch/plugins/current/repository-s3.html){: external} to migrate your data into {{site.data.keyword.databases-for-elasticsearch_full}}.

Take a snapshot of your existing Elasticsearch, store the snapshot in an AWS S3 or {{site.data.keyword.cloud}} Object Storage bucket and then restore the snapshot to your {{site.data.keyword.databases-for-elasticsearch}} deployment.

To migrate while data is still being written to your existing Elasticsearch, take multiple snapshots and perform multiple incremental restores. After the {{site.data.keyword.databases-for-elasticsearch}} deployment catches up to the state of your Elasticsearch, move your application writes to {{site.data.keyword.databases-for-elasticsearch}}.

## Requirements
{: #compose-migrating-req}

Ensure you meet the following requirements before migrating: 

- Your existing Elasticsearch must have the [S3 repository plug-in](https://www.elastic.co/guide/en/elasticsearch/plugins/current/repository-s3.html){: external}. If you are migrating from a Compose for Elasticsearch deployment, the plug-in is already enabled.
- An Elasticsearch that is version 5.x or 6.x. Migration is possible between major versions, but the versions must have compatible indexes. Indexes that are made in Elasticsearch 2.x are not compatible with 6.x and need reindexing in 5.x before migration.
- A matching {{site.data.keyword.databases-for-elasticsearch}} deployment that has _at least_ as many resources allocated to it your existing Elasticsearch. Also, ensure that the same [plug-ins](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-plugins) are available on {{site.data.keyword.databases-for-elasticsearch}} that you have on the existing Elasticsearch.
- Your our own S3 or IBM Cloud Object Storage repository.

Incremental restores work only if the number of shards of each index on both deployments match. Don't try to reindex and change the number of shards of any indexes once you've taken snapshots.
{: note}

## Example Migration
{: #compose-migrating-example-migrate}

A migration is explored in detail in [Migrate your data from Compose to Databases for Elasticsearch](https://www.ibm.com/cloud/blog/a-how-to-for-migrating-elasticsearch-to-ibm-cloud-databases-for-elasticsearch). 

The example migrates a Compose Elasticsearch deployment that has 20 indexes and a total size on disk of 35 GB to a {{site.data.keyword.databases-for-elasticsearch}} deployment. Data is still being written while the migration occurs, so the example uses multiple snapshots and restores.

The example's shell script is available in the [{{site.data.keyword.cloud_notm}} GitHub repository](https://github.com/IBM-Cloud/clouddatabases-migration-examples/tree/master/elasticsearch). It is provided as a starting point for you to adapt for your use-case.
