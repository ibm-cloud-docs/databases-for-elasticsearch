---

copyright:
  years: 2019, 2025
lastupdated: "2025-06-05"

keywords: elasticsearch, databases, shards, JVM heap, monitoring, elasticsearch disk I/O

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Performance
{: #performance}

{{site.data.keyword.databases-for-elasticsearch_full}} deployments can be [scaled to your usage](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-resources-scaling), configured to [autoscale](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-autoscaling) under certain resource conditions, or [horizontally scaled](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-horizontal-scaling) with more Elasticsearch nodes. If you are tuning the performance of your deployment, consider a few factors.

## Monitoring your deployment
{: #monitor-deployment}

{{site.data.keyword.databases-for-elasticsearch}} deployments offer an integration with the [{{site.data.keyword.monitoringfull}} service](/docs/cloud-databases?topic=cloud-databases-monitoring) for basic monitoring of resource usage on your deployment. Many of the available metrics, like disk usage and IOPS, are presented to help you configure [autoscaling](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-autoscaling) on your deployment. Observing trends in your usage and configuring the autoscaling to respond to them can help alleviate performance problems before your databases become unstable due to resource exhaustion.

## Elasticsearch sharding
{: #es-sharding}

When you add an index to Elasticsearch, it splits the data into shards and spreads those shards across the nodes in the cluster. The sharded configuration allows for Elasticsearch to run concurrent operations on your data across all the nodes. To gain extra concurrency and performance, [add nodes to Elasticsearch cluster](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-horizontal-scaling). When you add nodes, your shards are automatically rebalanced across the cluster to spread resource usage across all the nodes and increasing performance.

## Memory management
{: #mem-manage}

Elasticsearch memory is divided into to categories, JVM heap size and system memory. It uses heap for internal caching, and the rest of the system memory for the operating system, file system caches, and garbage collection. The more memory that is allocated to the heap, the less is allocated to the rest of the system.

{{site.data.keyword.databases-for-elasticsearch}} deployments have their memory allocation policy set at 50% heap and 50% system memory, with a max heap size of 32 GB. In some cases, it is useful to scale your deployment above 64 GB of RAM even with the heap limit as Elasticsearch does make use of the file system cache and alleviate pressure on disk I/O utilization. You can configure autoscaling to increase memory when disk I/O utilization reaches a certain threshold.

## Disk IOPS
{: #disk-iops}

The number of Input/Output Operations per second (IOPS) is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-elasticsearch}} deployments are provisioned on [Block storage endurance volumes in the 10 IOPS per GB tier](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=ui). Hitting IOPS limits can cause your databases to respond slowly or appear unresponsive.

Indexing uses disk, so if your use-case is write-heavy, your indexing speed can be limited by the IOPS available to your deployment. Some bottlenecks can be ameliorated by [tuning your indexes for disk usage](https://www.elastic.co/guide/en/elasticsearch/reference/current/tune-for-disk-usage.html). In addition, searching can use disk if your working data set does not fit in the file system cache, increasing IOPS load. If your use-case involves searching a large data set, increasing the memory on your deployment can help Elasticsearch rely less on disk.

Another thing to note is the default Lucene file system management policy is `niofs`, which allows concurrent reads on a file, which also can be constrained by disk I/O limits.  Information on file system storage types in the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-store.html).

If you need more IOPS, you can increase the number IOPS available to your deployment by increasing disk space. If you are aware of trends in your indexing or usage that increases disk I/O, you can configure autoscaling to increase disk based on IOPS.
