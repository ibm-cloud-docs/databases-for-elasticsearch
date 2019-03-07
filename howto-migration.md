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

## Things to watch out for

Incremental restores can only work if the number of shards of each index on both deployments match. Don't try to reindex and change the number of shards of any indices once you've started taking snapshots.

If you have an index that is called `searchguard` in your Compose deployment, you have to reindex it to a different name. `searchguard` is a reserved index name in {{site.data.keyword.databases-for-elasticsearch}}.

## Migration Example

An example migration is performed and explored in detail in [Migrate your data from Compose to Databases for Elasticsearch](https://www.ibm.com/blogs/bluemix/2019/02/a-how-to-for-migrating-elasticsearch-to-ibm-cloud-databases-for-elasticsearch/). The script is available in the [{{site.data.keyword.cloud_notm}} Github repository](https://github.com/IBM-Cloud/clouddatabases-migration-examples/tree/master/elasticsearch).

### Example script with step by step API calls

The migration example migrates a Compose deployment that has 20 indices and a total size on disk of 35 GB to a {{site.data.keyword.databases-for-elasticsearch}} deployment. Data is still being written while the migration occurs, so the example uses multiple snapshots and restores.

Assumptions -

- The mounted S3/COS repository to the Elasticsearch repository is called `migration`.
- Each snapshot will be named `snapshot-X`. `X` is incremented on each subsequent snapshot.

Variables - 

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

```sh
### Set up the environment, use your own values
compose_username=composetestuser
compose_password=composetestpassword
compose_endpoint=test.composedb.com
compose_port=33999
icd_username=icdtestuser
icd_password=icdtestpassword
icd_endpoint=my-es.test.databases.appdomain.cloud
icd_port=24000
storage_service_endpoint=s3-api.us-geo.objectstorage.service.networklayer.com
bucket_name=myawesomebucket
access_key=n9dh89h2189hd12hd
secret_key=nd0nd021nd012n0dn102nd01n20dn120d
path_to_snapshot_folder=elastic_search/deployment-1/migration
export CURL_CA_BUNDLE=/path/to/icd/ssl/certificate

### Mount S3/COS repo on Compose deployment
curl -H 'Content-Type: application/json' -sS -XPOST \
"https://${compose_username}:${compose_password}@${compose_endpoint}:${compose_port}/_snapshot/migration" \
-d '{
  "type": "s3",
  "settings": {
    "endpoint": "'"${storage_service_endpoint}"'",
    "bucket": "'"${bucket_name}"'",
    "base_path": "'"${path_to_snapshot_folder}"'",
    "access_key": "'"${access_key}"'",
    "secret_key": "'"${secret_key}"'"
  }
}'

### Mount S3/COS repo on Databases for Elasticsearch
curl -H 'Content-Type: application/json' -sS -XPOST \
"https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_snapshot/migration" \
-d '{
  "type": "s3",
  "settings": {
    "readonly": true,
    "endpoint": "'"${storage_service_endpoint}"'",
    "bucket": "'"${bucket_name}"'",
    "base_path": "'"${path_to_snapshot_folder}"'",
    "access_key": "'"${access_key}"'",
    "secret_key": "'"${secret_key}"'"
  }
}'

### Perform 1st snapshot on Compose deployment
curl -sS -XPUT \
"https://${compose_username}:${compose_password}@${compose_endpoint}:${compose_port}/_snapshot/migration/snapshot-1?wait_for_completion=true"

### Perform 1st restore on Databases for Elasticsearch
curl -H 'Content-Type: application/json' -sS -XPOST \
"https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_snapshot/migration/snapshot-1/_restore?wait_for_completion=true" \
-d '{"include_global_state": false}'

### The snapshot/restore steps took 2 hours. In the mean time, applications continued writing to the Compose deployment.
### Perform another snapshot/restore to get the new data to the Databases for Elasticsearch deployment.

### Close all indices on Databases for Elasticsearch so we can perform the next restore on top of it, without touching the searchguard index.

curl -sS "https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_cat/indices/?h=index" | \
grep -v -e '^searchguard$' | \
while read index; do
  curl -sS -XPOST "https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/$index/_close"
done

### Perform 2nd snapshot on Compose deployment
curl -sS -XPUT \
"https://${compose_username}:${compose_password}@${compose_endpoint}:${compose_port}/_snapshot/migration/snapshot-2?wait_for_completion=true"

### Perform 2nd restore on Databases for Elasticsearch
curl -H 'Content-Type: application/json' -sS -XPOST \
"https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_snapshot/migration/snapshot-2/_restore?wait_for_completion=true" \
-d '{"include_global_state": false}'

### The 2nd snapshot/restore steps took 20 minutes. Again, applications continued writing to the Compose deployment.
### Perform another snapshot/restore to get the new data to Databases for Elasticsearch

### Close all indices on Databases for Elasticsearch so we can perform the next restore on top of it, without touching the searchguard index.
curl -sS "https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_cat/indices/?h=index" | \
grep -v -e '^searchguard$' | \
while read index; do
  curl -sS -XPOST "https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/$index/_close"
done

### Perform 3rd snapshot on Compose deployment
curl -sS -XPUT \
"https://${compose_username}:${compose_password}@${compose_endpoint}:${compose_port}/_snapshot/migration/snapshot-3?wait_for_completion=true"

### Perform 3rd restore on Databases for Elasticsearch
curl -H 'Content-Type: application/json' -sS -XPOST \
"https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_snapshot/migration/snapshot-3/_restore?wait_for_completion=true" \
-d '{"include_global_state": false}'

### The 3rd snapshot/restore steps took 30 seconds. IF you can afford stopping writes for a minute, stop writing to the Compose deployment. 
### Then proceed with a final snapshot/restore cycle to get all the remaining changes to Databases for Elasticsearch.

### Close all indices on Databases for Elasticsearch so we can perform the next restore on top of it, without touching the searchguard index
curl -sS "https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_cat/indices/?h=index" | \
grep -v -e '^searchguard$' | \
while read index; do
  curl -sS -XPOST "https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/$index/_close"
done

### Perform 4th snapshot on the Compose deployment
curl -sS -XPUT \
"https://${compose_username}:${compose_password}@${compose_endpoint}:${compose_port}/_snapshot/migration/snapshot-4?wait_for_completion=true"

### Perform 4th restore on Databases for Elasticsearch
curl -H 'Content-Type: application/json' -sS -XPOST \
"https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_snapshot/migration/snapshot-4/_restore?wait_for_completion=true" \
-d '{"include_global_state": false}'

### Re-open all indices in Databases for Elasticsearch just in case some were not re-opened during the latest restore
curl -sS -XPOST "https://${icd_username}:${icd_password}@${icd_endpoint}:${icd_port}/_all/_open"

### At this point applications can start writing to Databases for Elasticsearch.
```
