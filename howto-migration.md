---
copyright:
  years: 2019
lastupdated: "2019-04-18"

keywords: elasticsearch, databases

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Migrating Elasticsearch
{: #compose-migrating}

If you are a current user of Elasticsearch, you can use the [Snapshot/restore Elasticsearch API](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html)
and the [S3 repository plugin](https://www.elastic.co/guide/en/elasticsearch/plugins/current/repository-s3.html) to migrate your data into {{site.data.keyword.databases-for-elasticsearch_full}}.

The basic procedure is to take a snapshot of your existing Elasticsearch, store the snapshot in an AWS S3 or {{site.data.keyword.cloud}} Object Storage bucket, and perform a restore of the snapshot to the {{site.data.keyword.databases-for-elasticsearch}} deployment. 

If you want to perform the migration while data is still being written to your existing Elasticsearch, you can take multiple snapshots and perform multiple incremental restores. Once the {{site.data.keyword.databases-for-elasticsearch}} deployment is caught up to the state of your Elasticsearch, you can move your application writes to {{site.data.keyword.databases-for-elasticsearch}}.

## Requirements

- Your existing Elasticsearch has to have the [S3 repository plugin](https://www.elastic.co/guide/en/elasticsearch/plugins/current/repository-s3.html). If you are migrating from a Compose for Elasticsearch deployment, the plugin is already enabled.
- An Elasticsearch that is version 5.x or 6.x. Migration is possible between major versions, but the versions have to have compatible indices. Indices that are made in Elasticsearch 2.x are not compatible with 6.x and will need reindexing in 5.x before migration.
- A matching {{site.data.keyword.databases-for-elasticsearch}} deployment that has _at least_ as many resources allocated to it your existing Elasticsearch. Also, ensure that the same [plugins](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-plugins) are available on {{site.data.keyword.databases-for-elasticsearch}} that you have on the existing Elasticsearch.
- You need to have your own S3 or IBM Cloud Object Storage repository.

## Things to watch out for

- Incremental restores can only work if the number of shards of each index on both deployments match. Don't try to reindex and change the number of shards of any indices once you've started taking snapshots.
- If you have an index that is called `searchguard` in your existing Elasticsearch, you have to reindex it to a different name. `searchguard` is a reserved index name in {{site.data.keyword.databases-for-elasticsearch}}

## Example Migration

An example migration is performed and explored in detail in [Migrate your data from Compose to Databases for Elasticsearch](https://www.ibm.com/cloud/blog/a-how-to-for-migrating-elasticsearch-to-ibm-cloud-databases-for-elasticsearch). 

The example migrates a Compose Elasticsearch deployment that has 20 indices and a total size on disk of 35 GB to a {{site.data.keyword.databases-for-elasticsearch}} deployment. Data is still being written while the migration occurs, so the example uses multiple snapshots and restores.

The example's shell script is available in the [{{site.data.keyword.cloud_notm}} GitHub repository](https://github.com/IBM-Cloud/clouddatabases-migration-examples/tree/master/elasticsearch). It is provided as a starting point for you to adapt for your use-case.

