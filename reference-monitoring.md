---
copyright:
  years: 2020, 2023
lastupdated: "2023-04-17"

keywords: elasticsearch disk i/o, monitoring elasticsearch, metrics, cluster status, JVM heap

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}


# Monitoring Integration
{: #monitoring}

Monitoring for {{site.data.keyword.databases-for-elasticsearch_full}} deployments is provided through integration with the {{site.data.keyword.monitoringfull}} service. Your deployments forward select information so you can monitor deployment health and resource usage. To see your {{site.data.keyword.databases-for-elasticsearch}} dashboards in {{site.data.keyword.monitoringfull_notm}}, you must [Enable Platform Metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling) in the same region as your deployment. If you have deployments in more than one region, you must provision {{site.data.keyword.monitoringfull_notm}} and enable platform metrics in each region.

To access {{site.data.keyword.monitoringfull_notm}} from your deployment, use the _Monitoring_ link from the right menu. (If you do not already have a monitoring service in the same region as your deployment it says _Add monitoring_.)

![The Monitoring link in a deployment](images/monitoring-ui-link.png){: caption="Figure 1. Monitoring link" caption-side="bottom"}

To access your deployment's monitoring dashboard from {{site.data.keyword.monitoringfull_notm}}, it's in the sidebar, under _IBM_.

![Cloud databases dashboard in monitoring](images/monitoring-ibm-list.png){: caption="Figure 2. Cloud databases dashboard" caption-side="bottom"}

## Monitoring Availability
{: #monitoring-avail}

{{site.data.keyword.monitoringfull_notm}} is available for deployments in every region. Deployments in Multi-zone Regions (MZRs) - `eu-gb`, `eu-de`, `us-east`, `us-south`, `jp-tok`, `au-syd` - have their metrics in the corresponding region.

If you have deployments that are in Single-zone Region (SZR) `che01` then your logs are forwarded to an {{site.data.keyword.monitoringfull_notm}} instance in another region. You need to provision monitoring instances in the region where your metrics are forwarded to. Metrics for deployments in `che01` go to `jp-tok`. 

## Available Metrics
{: #metrics-by-plan}

| Metric Name |
|-----------|
| [Cluster status](#ibm_databases_for_elasticsearch_cluster_status) |
| [Disk read latency mean](#ibm_databases_for_elasticsearch_disk_read_latency_mean) | 
| [Disk write latency mean](#ibm_databases_for_elasticsearch_disk_write_latency_mean) | 
| [GC Percentage](#ibm_databases_for_elasticsearch_garbage_collection_percent_average_15m)|
| [IO usage as a percent -  5-minute average](#ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_5m) |
| [IO usage as a percent - 15-minute average](#ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_15m) | 
| [IO usage as a percent - 30-minute average](#ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_30m) | 
| [IO usage as a percent - 60-minute average](#ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_60m) | 
| [IOPS read and write total count for an instance.](#ibm_databases_for_elasticsearch_disk_iops_read_write_total) | 
| [Max allowed memory for an instance.](#ibm_databases_for_elasticsearch_memory_limit_bytes) | 
| [The number of unassigned shards.](#ibm_databases_for_elasticsearch_unassigned_shards_total) | 
| [Total disk space for an instance.](#ibm_databases_for_elasticsearch_disk_total_bytes) | 
| [Used CPU for an instance.](#ibm_databases_for_elasticsearch_cpu_used_percent) | 
| [Used JVM heap for a database member of the instance in percent.](#ibm_databases_for_elasticsearch_jvm_heap_percent) | 
| [Used disk space for an instance.](#ibm_databases_for_elasticsearch_disk_used_bytes) | 
| [Used memory for an instance.](#ibm_databases_for_elasticsearch_memory_used_bytes) | 
{: caption="Table 1. Available Metrics Reference Table" caption-side="top"}

### Cluster status
{: #ibm_databases_for_elasticsearch_cluster_status}

A number derived from the status value of the `/_cluster/health` endpoint. Possible Values: 'green' = 1.0, 'yellow' = 0.5, 'red' = 0, ERROR = -1.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_cluster_status`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 2. Cluster status metric metadata" caption-side="top"}

### Disk read latency mean
{: #ibm_databases_for_elasticsearch_disk_read_latency_mean}

Disk read latency mean

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_read_latency_mean`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 3: Disk read latency mean metric metadata" caption-side="top"}

### Disk write latency mean
{: #ibm_databases_for_elasticsearch_disk_write_latency_mean}

Disk write latency mean

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_write_latency_mean`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 4: Disk write latency mean metric metadata" caption-side="top"}

### GC Percentage
{: #ibm_databases_for_elasticsearch_garbage_collection_percent_average_15m}

Percentage of time the Elasticsearch JVM spends on garbage collection over 15 minutes.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_garbage_collection_percent_average_15m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 5. Cluster status metric metadata" caption-side="top"}

### IO usage in percent 15-minute average
{: #ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_15m}

Disk I/O usage over 15 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_15m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 6. IO utilization in percent 15 minute average metric metadata" caption-side="top"}

### IO usage in percent 30-minute average
{: #ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_30m}

Disk I/O usage over 30 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_30m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 7. IO utilization in percent 30 minute average metric metadata" caption-side="top"}

### IO usage in percent 5-minute average
{: #ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_5m}

Disk I/O usage over 5 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_5m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 8. IO utilization in percent 5 minute average metric metadata" caption-side="top"}

### IO usage in percent 60 minute average
{: #ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_60m}

Disk I/O usage over 60 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_io_utilization_percent_average_60m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 9. IO utilization in percent 60 minute average metric metadata" caption-side="top"}

### IOPS read and write total count for an instance
{: #ibm_databases_for_elasticsearch_disk_iops_read_write_total}

How many input/output operations per second your deployment is performing.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_iops_read_write_total`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 10. IOPS read and write total count for an instance metric metadata" caption-side="top"}

### Max allowed memory for an instance
{: #ibm_databases_for_elasticsearch_memory_limit_bytes}

The maximum amount of memory available to your deployment.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_memory_limit_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 11. Max allowed memory for an instance metric metadata" caption-side="top"}

### Number of unassigned shards
{: #ibm_databases_for_elasticsearch_unassigned_shards_total}

The number of unassigned shards.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_unassigned_shards_total`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 12. Number of unassigned shards metric metadata" caption-side="top"}

### Total disk space for an instance
{: #ibm_databases_for_elasticsearch_disk_total_bytes}

The total amount of disk available to your deployment.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_total_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 13. Total disk space for an instance metric metadata" caption-side="top"}

### Used CPU for an instance
{: #ibm_databases_for_elasticsearch_cpu_used_percent}

How much CPU is used as a percentage of total CPU available. Only for deployments that have dedicated CPU.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_cpu_used_percent`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 14. Used CPU for an instance metric metadata" caption-side="top"}

### Used JVM heap for a database member of the instance in percent
{: #ibm_databases_for_elasticsearch_jvm_heap_percent}

How much JVM heap is used as a percentage of total JVM heap is available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_jvm_heap_percent`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 15. Used JVM heap for a database member of the instance in percent metric metadata" caption-side="top"}

### Used disk space for an instance
{: #ibm_databases_for_elasticsearch_disk_used_bytes}

How much disk your deployment is using.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_disk_used_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 16. Used disk space for an instance metric metadata" caption-side="top"}

### Used memory for an instance
{: #ibm_databases_for_elasticsearch_memory_used_bytes}

How much memory your deployment is using.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_elasticsearch_memory_used_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 16. Used memory for an instance metric metadata" caption-side="top"}

## Attributes for segmentation
{: #attributes}

### Global Attributes
{: #global-attributes}

The following attributes are available for segmenting all of the metrics.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud Type` | `ibm_ctype` | The cloud type is a value of public, dedicated, or local. |
| `Location` | `ibm_location` | The location of the monitored resource - this can be a region, data center, or global. |
| `Resource` | `ibm_resource` | The resource being measured by the service - typically an identifying name or GUID. |
| `Resource Type` | `ibm_resource_type` | The type of the resource being measured by the service. |
| `Scope` | `ibm_scope` | The scope is the account, organization, or space GUID associated with this metric. |
{: caption="Table 17. Global Attributes" caption-side="top"}

### More Attributes
{: #additional-attributes}

The following attributes are available for segmenting one or more attributes as described in the reference. See the individual metrics for segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Service instance` | `ibm_service_instance` | The service instance segment identifies the instance that the metric is associated with. |
| `Service instance name` | `ibm_service_instance_name` | The service instance name provides the user-provided name of the service instance, which isn't necessarily a unique value depending on the name that is provided by the user. |
| `Resource group` | `ibm_resource_group_name` | The resource group where the service instance was created. |
{: caption="Table 18. Additional Attributes" caption-side="top"}

