---

Copyright:
  years: 2019, 2021
lastupdated: "2021-02-04"

keywords: elasticsearch, databases, manual scaling, disk I/O, memory, CPU

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Adding Disk, Memory, and CPU 
{: #resources-scaling}

You can manually adjust the amount of resources available to your {{site.data.keyword.databases-for-elasticsearch_full}} deployment to suit your workload and the size of your data.

## Resource Breakdown

A default {{site.data.keyword.databases-for-elasticsearch}} deployment runs with three data members in a cluster, and resources are allocated to all three members equally. For example, the minimum storage of an Elasticsearch deployment is 15360 MB, which equates to an initial size of 5120 MB per member. The minimum RAM for an Elasticsearch deployment is 3072 MB, which equates to an initial allocation of 1028 MB per member.

Billing is based on the _total_ amount of resources that are allocated to the deployment.
{: .tip}

### Disk Usage

Storage shows the amount of disk space that is allocated to your service. Each member gets an equal share of the allocated space. Your data is replicated across all the data members in the Elasticsearch cluster.

Disk allocation also affects the performance of the disk, with larger disks having higher performance. Baseline Input-Output Operations per second (IOPS) performance for disk is 10 IOPS for each GB. Scale disk to increase the IOPS your deployment can handle.

You cannot scale down storage. If your data set size has decreased, you can recover space by backing up and restoring to a new deployment.
{: .tip} 

### RAM

If you find that your queries and database activity suffer from performance issues due to a lack of memory, you can scale the amount of RAM allocated to your service. Adding memory to the total allocation adds memory to the members equally. {{site.data.keyword.databases-for-elasticsearch}} deployments have their memory allocation policy set at 50% heap and 50% system memory, so increasing the amount of RAM increases both heap and system memory.

### Dedicated Cores

You can enable or increase the CPU allocation to the deployment. With dedicated cores, your resource group is given a single-tenant host with a reserve of CPU shares. Your deployment is then guaranteed the minimum number of CPUs you specify. The default of 0 dedicated cores uses compute resources on shared hosts. Going from a 0 to a >0 CPU count provisions and moves your deployment to new hosts, and your databases are restarted as part of that move. Going from >0 to a 0 CPU count, moves your deployment to a shared host and also restarts your databases as part of the move.

## Scaling Considerations

- Scaling your deployment up might cause your RabbitMQ to restart. If you scale RAM or CPU and your deployment needs to be moved to a host with more capacity, then the RabbitMQ is restarted as part of the move.

- Scaling down RAM or CPU does not trigger restarts.

- Disk can not be scaled down.

- A few scaling operations can be more long running than others. Enabling dedicated cores moves your deployment to its own host and can take longer than just adding more cores. Similarly, drastically increasing CPU, RAM, or Disk can take longer than smaller increases to account for provisioning more underlying hardware resources.

- Scaling operations are logged in [{{site.data.keyword.at_full}}](/docs/databases-for-elasticsearch?topic=cloud-databases-activity-tracker).

- If you find consistent trends in resource usage or would like to set up scaling when certain resource thresholds are reached, you can enable [autoscaling](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-autoscaling) on your deployment.

- Elasticsearch is designed to balance work load across a cluster and can benefit from being horizontally scaled. If you are concerned about performance, check out [Adding Elasticsearch Nodes](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-horizontal-scaling).

## Scaling in the UI

A visual representation of your data members and their resource allocation is available on the _Resources_ tab of your deployment's _Manage_ page. 

![The Scale Resources Panel in _Resources_](images/scaling-update.png)

Adjust the slider to increase or decrease the resources that are allocated to your service. The slider controls how much memory or disk is allocated per member. The UI currently uses a coarser-grained resolution of 8 GB increments for disk and 1 GB increments for memory. The UI shows the total allocated memory or disk for the position of the slider. Click **Scale** to trigger the scaling operations and return to the dashboard overview. 

## Resources and Scaling in the CLI 

[{{site.data.keyword.cloud_notm}} CLI cloud databases plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) supports viewing and scaling the resources on your deployment. Use the command `cdb deployment-groups` to see current resource information for your service, including which resource groups are adjustable. To scale any of the available resource groups, use `cdb deployment-groups-set` command. 

For example, the command to view the resource groups for a deployment named "example-deployment":  
`ibmcloud cdb deployment-groups example-deployment`

This produces the output:
```
Group   member
Count   3
|
+   Memory
|   Allocation              3072mb
|   Allocation per member   1024mb
|   Minimum                 3072mb
|   Step Size               384mb
|   Adjustable              true
|
+   CPU
|   Allocation              0
|   Allocation per member   0
|   Minimum                 9
|   Step Size               3
|   Adjustable              true
|
+   Disk
|   Allocation              15360mb
|   Allocation per member   5120mb
|   Minimum                 15360mb
|   Step Size               3072mb
|   Adjustable              true
```

The deployment has three members, with 3072 MB of RAM and 15360 MB of disk allocated in total. The "per member" allocation is 1024 MB of RAM and 5120 MB of disk. The minimum value is the lowest the total allocation can be set. The step size is the smallest amount by which the total allocation can be adjusted.

The `cdb deployment-groups-set` command allows either the total RAM or total disk allocation to be set, in MB. For example, to scale the memory of the "example-deployment" to 2048 MB of RAM for each memory member (for a total memory of 6144 MB), you use the command:  
`ibmcloud cdb deployment-groups-set example-deployment member --memory 6144`

## Scaling via the API

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically.

To view the current and scalable resources on a deployment, use the [/deployments/{id}/groups](https://cloud.ibm.com/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl) endpoint.
```
curl -X GET -H "Authorization: Bearer $APIKEY" `https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups'
```

To scale the memory of a deployment to 2048 MB of RAM for each member (there are 3 so a total memory of 6144 MB), use the use the [/deployments/{id}/groups/{group_id}](https://cloud.ibm.com/apidocs/cloud-databases-api#set-scaling-values-on-a-specified-group) API endpoint.
```
curl -X PATCH 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups/member' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"memory": {
        "allocation_mb": 6144
      }
    }'
```

