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

The basic procedure is to take a snapshot of the Compose deployment, and perform a restore to the {{site.data.keyword.databases-for-elasticsearch}} deployment. If you want to preform the migration while data is still being written, you can take multiple snapshots and perform multiple incremental restores.

## Requirements

- An Elasticsearch deployment on Compose that is version 5.x or 6.x. Migration is possible between major versions, but the versions have to have compatible indices. Indices that are made in Elasticsearch 2.x are not compatible with 6.x and will need re-indexing in 5.x before migration.
- A matching {{site.data.keyword.databases-for-elasticsearch}} deployment that has _at least_ as much resources allocated to it as the Compose deployment. Also, ensure that the same [plugins](/docs/services/databases-for-elasticsearch?topic=databases-for-elastcisearch-plugins) are available on {{site.data.keyword.databases-for-elasticsearch}} that you have enabled in Compose.
- You need to have your own S3 or IBM Cloud Object Storage repository.
- `<compose_endpoint>:<compose_port>` - the hostname/port combination used to talk to the Compose deployment
- `<icd_endpoint>:<icd_port>` - the hostname/port combination used to talk to the {{site.data.keyword.databases-for-elasticsearch}} deployment
- `<compose_username>:<compose_password>` - the username/password combination used to authenticate against the Compose deployment
- `<icd_username>:<icd_password>` - the username/password combination used to authenticate against the {{site.data.keyword.databases-for-elasticsearch}} deployment
- `<bucket_name>` - the name of the bucket we're going to use
- `<path_to_snapshot_folder>` - the base path inside the bucket where we want to write all snapshot data. Do not include a leading forward slash as this causes an error.
- `<access_key>` - the S3/COS repo access key
- `<secret_key>` - the S3/COS repo secret key
- `<storage_service_endpoint>` - the S3/COS endpoint hostname. If using COS, the endpoint must be a regional endpoint such as `us-south` and not a cross-regional endpoint.
- `$CURL_CA_BUNDLE` - path to a file that contains the SSL client certificate used to connect to your Databases for Elasticsearch deployment.


## Things to watch out for

Incremental restores can only work if the number of shards of each index on both deployments match. Don't try to reindex and change the number of shards of any indices once you've started taking snapshots.

If you have an index that is called `searchguard` in your Compose deployment, you have to reindex it to a different name. `searchguard` is a reserved index name in {{site.data.keyword.databases-for-elasticsearch}}

