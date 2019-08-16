---

Copyright:
  years: 2019
lastupdated: "2019-08-07"

keywords: elasticsearch, databases

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Performance
{: #performance}

{{site.data.keyword.databases-for-elastcisearch_full}} deployments can be [scaled to your usage](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-resources-scaling), but they do not auto-scale. There are a few factors to consider if you are concerned about the performance of your deployment.

## Memory Management

Elasticsearch memory is is divided into to categories, JVM heap size and system memory. It uses heap for internal caching, and the rest of the system memory for the operating system, filesystem caches, and garbage collection. The more memory allocated to the heap, the less is allocated to the rest of the system.

{{site.data.keyword.databases-for-elasticsearch}} deployments have their memory allocation policy set at 50% heap and 50% system memory, with a max heap size of 32 GB. In some cases, it is useful to scale your deployment above 64 GB of RAM even with the heap limit as Elasticsearch does make use of the filesystem cache to speed up search results. 

## Disk IOPS

The number of Input-Output Operations per second (IOPS) is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-elasticsearch}} deployments are provisioned on [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/infrastructure/BlockStorage?topic=BlockStorage-About#provendurance). Hitting IOPS limits can cause your databases to respond slowly or appear unresponsive. 

Indexing uses disk, so if your use-case is write-heavy, your indexing speed can be limited by the IOPS available to your deployment. Some of this can be ameliorated by [tuning your indexes for disk usage](https://www.elastic.co/guide/en/elasticsearch/reference/current/tune-for-disk-usage.html). In addition, searching can use disk if your working data set does not fit in the filesystem cache, increasing IOPS load. If your use-case involves searching a large data set, increasing the memory on your deployment can help Elasticsearch rely less on disk. 

Another good thing to note is the default Lucene file system management policy is `niofs`.  Information on file system storage types in in the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-store.html).

If you need more IOPS, you can increase the number IOPS available to your deployment by increasing disk space.

## Monitoring your deployment

You can use the [monitoring integration](/docs/services/databases-for-elasticsearch?topic=cloud-databases-monitoring) to estimate typical resource usage, and scale your deployment accordingly.

If you are planning on running operations that might put a spike in the usual RAM usage, increase the size of your data, or an increase in IOPS, you can scale your deployment's resources to avoid hitting limits that affect deployment operations.
