---
copyright:
  years: 2019
lastupdated: "2019-02-21"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Migrating Elasticsearch from Compose
{: #compose-migrating}

If you are a current user of Elasticsearch on Compose, you can use the Snapshot/restore Elasticsearch API: https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html
and the S3 repository plugin: https://www.elastic.co/guide/en/elasticsearch/plugins/current/repository-s3.html to migrate your data into {{site.data.keyword.databases-for-elasticsearch_full}}

The basic procedure is to take a snapshot of the Compose deployment, store the snapshot in an AWS S3 or {{site.data.keyword.cloud}} Object Storage bucket, and perform a restore of the snapshot to the {{site.data.keyword.databases-for-elasticsearch}} deployment. 

If you want to preform the migration while data is still being written to Compose, you can take multiple snapshots and perform multiple incremental restores. Once the {{site.data.keyword.databases-for-elasticsearch}} deployment is caught up to the state of the Compose deployment, you can move your application writes to {{site.data.keyword.databases-for-elasticsearch}}.

## Requirements

- An Elasticsearch deployment on Compose that is version 5.x or 6.x. Migration is possible between major versions, but the versions have to have compatible indices. Indices that are made in Elasticsearch 2.x are not compatible with 6.x and will need re-indexing in 5.x before migration.
- A matching {{site.data.keyword.databases-for-elasticsearch}} deployment that has _at least_ as much resources allocated to it as the Compose deployment. Also, ensure that the same [plugins](/docs/services/databases-for-elasticsearch?topic=databases-for-elastcisearch-plugins) are available on {{site.data.keyword.databases-for-elasticsearch}} that you have enabled in Compose.
- You need to have your own S3 or IBM Cloud Object Storage repository.

## Things to watch out for

Incremental restores can only work if the number of shards of each index on both deployments match. Don't try to reindex and change the number of shards of any indices once you've started taking snapshots.

If you have an index that is called `searchguard` in your Compose deployment, you have to reindex it to a different name. `searchguard` is a reserved index name in {{site.data.keyword.databases-for-elasticsearch}}

