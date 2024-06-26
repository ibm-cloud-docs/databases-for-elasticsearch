---

copyright:
  years: 2019, 2024
lastupdated: "2024-05-23"

keywords: elasticsearch upload files, filter, index, indices, indexes

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Uploading files to Elasticsearch 
{: #uploading-files}

A few Elasticsearch features allow indexes to read from files on the file system, so {{site.data.keyword.databases-for-elasticsearch_full}} allows you to upload files to your deployment. The files are stored at a known location, and Elasticsearch is configured so that it is allowed to read files from the location.

Files that are uploaded to your deployment use disk resources, both in the index and on the file system. Make sure that you [scale your deployment](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-resources-scaling) before uploading files.
{: .tip} 

## Basic Process
{: #process}

1. You base64 encode the file on client side.
1. The base64 strings are stored as documents in an index named `ibm_file_sync` in your Elasticsearch deployment.
1. You trigger a file sync from the {{site.data.keyword.databases-for}} API.
1. All nodes in your Elasticsearch cluster download the file contents from the index, decode the base64, and restore the files on the deployment's disk in the `/data/ibm_file_sync/current` directory.
1. At regular intervals, and on restarts, the files get resynced to assure they are present on all nodes.
1. Files that are on disk, but not in the index, get deleted. You can delete files from disk by removing them from the index.  

The index in Elasticsearch is `ibm_file_sync`.  
The location of the files on disk is `/data/ibm_file_sync/current`.

In Elasticsearch 7 the API removes doc types. Refer to the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html){: .external} for more details. This is especially good to keep in mind when updating from prior versions.
{: .note}

## Uploading the files to the Index
{: #uploading-files-index}

The structure of the documents in the index is as follows: `name` is the file name of the file, `blob` is the base64-encoded file contents, and `md5` is an optional hash value over the file contents. The recommended mapping for the index is split out based on version.

For Elasticsearch 6:
```sh
curl -X PUT "https://user:password@host:port/ibm_file_sync" -H 'Content-Type: application/json' -d'
{
    "mappings": {
        "files": {
            "properties": {
                "name": {
                    "type": "text"
                },
                "blob": {
                    "type": "binary"
                },
                "md5": {
                    "type": "text"
                }
            }
        }
    }
}'
```
{: .pre}

For Elasticsearch 7 (note the removal of the `files` section):
```sh
curl -X PUT "https://user:password@host:port/ibm_file_sync" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "blob": {
        "type": "binary"
      },
      "md5": {
        "type": "text"
      }
    }
  }
}'
```
{: .pre}

The URL is the `https` [connection string](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-connection-strings) from your deployment.
{: .tip}

To use the index, encode the file contents as base64. To encode an example file `README.md` in bash, `ENC=$(base64 -w 0  README.md)`. Then, build a checksum over the content, `HASH=$(md5sum README.md)`.

The download function compares the hash values on each sync run and if the values are unchanged since the last sync, no new download is attempted.  If any document in the index has no md5 value, all downloads are attempted again.
{: .tip}

Next, upload the document to the index. Note that the file name is supplied in the URL as well.

For Elasticsearch 6:
``` sh
curl -X PUT "https://user:password@host:port/ibm_file_sync/files/README1.md" -H 'Content-Type: application/json' -d'
{
    "name": "README1.md",
    "blob": '"\"$ENC\""',
    "md5": '"\"$HASH\""'
}'
```
{: .pre}

For Elasticsearch 7 (note only the URL is changed):
``` sh
curl -X PUT "https://user:password@host:port/ibm_file_sync/_doc/README1.md" -H 'Content-Type: application/json' -d'
{
    "name": "README1.md",
    "blob": '"\"$ENC\""',
    "md5": '"\"$HASH\""'
}'
```
{: .pre}

You can verify the uploaded data. 

For Elasticsearch 6:
```sh
curl https://user:password@host:port/ibm_file_sync/files/README.md?pretty
```
{: .pre}

For Elasticsearch 7:
```sh
curl https://user:password@host:port/ibm_file_sync/_doc/README.md?pretty
```
{: .pre}

If everything went smoothly, the returned data looks like this (shortened) example. The "md5" field can contain a file name alongside the hash.

For Elasticsearch 6:
```sh
{
  "_index" : "ibm_file_sync",
  "_type" : "files",
  "_id" : "README1.md",
  "_version" : 1,
  "found" : true,
  "_source" : {
    "name" : "README1.md",
    "blob" : "IyBF ... KWBgCg==",
    "md5" : "270f60e62d3d37add3702ced7f6969a1  README.md"
  }
}
```
{: .pre}

For Elasticsearch 7: 
```sh
{
  "_index" : "ibm_file_sync",
  "_id" : "README1.md",
  "_version" : 1,
  "found" : true,
  "_source" : {
    "name" : "README1.md",
    "blob" : "IyBF ... KWBgCg==",
    "md5" : "270f60e62d3d37add3702ced7f6969a1  README.md"
  }
}
```
{: .pre}

## Syncing files to disk
{: #syncing-files}

After the files are uploaded to the index, they can be synced to the disk. Call the `/elasticsearch/file_syncs` endpoint from the {{site.data.keyword.databases-for}} API.
```curl
curl -X POST \
https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/elasticsearch/file_syncs \
-H 'authorization: Bearer <token>'
```
{: .pre}

The `region` is the region that your deployment is in, and the `id` (CRN) part of the URL needs to be url-encoded. More information is in the [API Reference](https://cloud.ibm.com/apidocs/cloud-databases-api).

The call starts and returns a [task](https://cloud.ibm.com/apidocs/cloud-databases-api#get-currently-running-tasks-on-a-deployment) so you can monitor its progress. After the returned task finishes, the contents in the index are present on all the nodes in your cluster.

Any number of files can be uploaded and synced. The contents of the files are not validated. Ensure that they can be processed by Elasticsearch.
{: .tip}

## Using the files
{: #using-files}

Elasticsearch features that use files on the file system do so by accepting the path to the file when defining the index. An uploaded file `example.txt` is at `/data/ibm_file_sync/current/example.txt`. This list contains examples and is not exhaustive.
- [Keep Words Token Filter](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-keep-words-tokenfilter.html){: .external}
- [Mapping Char Filter](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-mapping-charfilter.html){: .external}
- [Compound Word Token Filters](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-compound-word-tokenfilter.html){: .external}
