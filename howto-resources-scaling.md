---

copyright:
  years: 2019, 2024
lastupdated: "2024-05-16"

keywords: elasticsearch dedicated cores, databases, manual scaling, disk I/O, memory, CPU, elasticsearch resources, elasticsearch scaling

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Adding disk, memory, and CPU 
{: #resources-scaling}

For new [hosting models](/docs/cloud-databases?topic=cloud-databases-hosting-models), scaling is currently available through the CLI, API, and Terraform.
{: note}

You can manually adjust the resources available to your {{site.data.keyword.databases-for-elasticsearch_full}} deployment to suit your workload and the size of your data.

## Resource breakdown
{: #resource-breakdown}

A default {{site.data.keyword.databases-for-elasticsearch}} deployment runs with three data members in a cluster and resources are allocated to all three members equally. For example, the minimum storage of an Elasticsearch deployment is 15360 MB, which equates to an initial size of 5120 MB per member. The minimum RAM for an Elasticsearch deployment is 3072 MB, which equates to an initial allocation of 1028 MB per member.

Billing is based on the _total_ resources that are allocated to the deployment.
{: .tip}

### Disk usage
{: #resources-scaling-disk-usage}

Storage shows the amount of disk space that is allocated to your service. Each member gets an equal share of the allocated space. Your data is replicated across all the data members in the Elasticsearch cluster.

Disk allocation also affects the performance of the disk, with larger disks having higher performance. Baseline input/output operations per second (IOPS) performance for disk is 10 IOPS for each GB. Scale disk to increase the IOPS that your deployment can handle.

You cannot scale down storage. If your data set size has decreased, you can recover space by backing up and restoring to a new deployment.
{: .tip} 


### RAM
{: #resources-scaling-ram}

If you find that your queries and database activity suffer from performance issues due to a lack of memory, you can scale the amount of RAM allocated to your service. If your database instance is on an Isolated Compute hosting model, select the CPU x RAM configuration that matches your resource needs. If your database instance is on a Shared Compute or Dedicated Core hosting model, simply select the RAM allocation you would like for your database. Please note that Dedicated Core is deprecated, and will be removed in May 2025. 

Adding memory to the total allocation adds memory to the members equally. {{site.data.keyword.databases-for-elasticsearch}} deployments have their memory allocation policy set at 50% heap and 50% system memory, so increasing the amount of RAM increases both heap and system memory. 

RAM can be scaled up or down. 

### vCPU
{: #resources-scaling-cores}

If you find that your database workloads need more CPU resources, you can scale the amount of CPU allocated to your service. If your database instance is on an Isolated Compute hosting model, select the CPU x RAM configuration that matches your resource needs. If your database instance is on a Shared Compute or Dedicated Core hosting model, simply select the CPU allocation you would like for your database. Please note that Dedicated Core is deprecated, and will be removed in May 2025. 

The default of 0 cores uses compute resources on multi-tenanted hosts. This style of multi-tenant is deprecated, and will be removed September 2025 in favor of Shared Compute. 

CPU can be scaled up or down. 



## Scaling considerations
{: #resources-scaling-consider}

- Scaling up might cause your deployment to restart. If your deployment needs to be moved to a host with more capacity, then the deployment is restarted as part of the move.

- Scaling down RAM or CPU does not trigger restarts.

- Disk cannot be scaled down.
- Scaling between hosting models (Shared Compute, Isolated Compute, and Dedicated Cores) moves your deployment to new hosts. Your databases are restarted as part of that move. As your deployment is moved to a new host, this can also take longer than just adding more resources. Learn more about Shared Compute and Isolated Compute [here](/docs/cloud-databases?topic=cloud-databases-hosting-models).

- Similarly, drastically scaling up CPU, RAM, or Disk can take longer to run than small resource increases to account for provisioning more underlying hardware resources.

- Scaling operations are logged in [{{site.data.keyword.at_full}}](/docs/databases-for-elasticsearch?topic=cloud-databases-activity-tracker).

- If you find consistent trends in resource usage or would like to scale when certain resource thresholds are reached, enable [autoscaling](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-autoscaling) on your deployment.

- Elasticsearch is designed to balance work load across a cluster and can benefit from being horizontally scaled. If you are concerned about performance, check out [Adding Elasticsearch Nodes](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-horizontal-scaling).

## Scaling in the UI
{: #resources-scaling-ui}
{: ui}

For new [hosting models](/docs/cloud-databases?topic=cloud-databases-hosting-models), scaling is currently available through the CLI, API, and Terraform.
{: note}

A visual representation of your data members and their resource allocation is available on the _Resources_ tab of your deployment's _Manage_ page. 

![The Scale Resources Panel in _Resources_](images/scaling-update.png){: caption="Figure 1. The Scale Resources Panel" caption-side="bottom"}

Adjust the slider to increase or decrease the resources that are allocated to your service. The slider controls how much memory or disk is allocated per member. The UI currently uses a coarser-grained resolution of 8 GB increments for disk and 1 GB increments for memory. The UI shows the total allocated memory or disk for the position of the slider. Click **Scale** to trigger the scaling operations and return to the dashboard overview. 

## Resources and scaling in the CLI 
{: #resources-scaling-cli}
{: cli}

[{{site.data.keyword.cloud_notm}} CLI cloud databases plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) supports viewing and scaling the resources on your deployment. Use the command `cdb deployment-groups` to see current resource information for your service, including which resource groups are adjustable. To scale any of the available resource groups, use `cdb deployment-groups-set` command. 

For example, the command to view the resource groups for a deployment named "example-deployment":  
`ibmcloud cdb deployment-groups example-deployment`

This produces the output:
```sh
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

<br>
______________________________________

<br>


To scale a {{site.data.keyword.databases-for}} Shared Compute instance, use a command like:

```sh
ibmcloud cdb deployment-groups-set <deploymentid> <groupid> [--memory <val>] [--cpu <val>] [--disk <val>] [--hostflavor multitenant]
```
{: pre}



To scale a {{site.data.keyword.databases-for}} Isolated Compute instance, use a command like the following used to scale to a 4 CPU by 16 RAM instance:

```sh
ibmcloud cdb deployment-groups-set <deploymentid> <groupid> [--memory <val>] [--cpu <val>] [--disk <val>] [--hostflavor b3c.4x16.encrypted]
```
{: pre}

CPU and RAM autoscaling is not supported on {{site.data.keyword.databases-for}} Isolated Compute. Disk autoscaling is available. If you provisioned an isolated instance or switched over from a deployment with autoscaling, monitor your resources using [{{site.data.keyword.monitoringfull}} integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring), which provides metrics for memory, disk space, and disk I/O utilization. To add resources to your instance, manually scale your deployment.
{: note}

## Scaling with the API
{: #resources-scaling-api}
{: api}

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically.

To view the current and scalable resources on a deployment, use the [/deployments/{id}/groups](https://cloud.ibm.com/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl) endpoint.
```sh
curl -X GET -H "Authorization: Bearer $APIKEY" `https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups'
```

To scale the memory of a deployment to 2048 MB of RAM for each member (there are 3 so a total memory of 6144 MB), use the [/deployments/{id}/groups/{group_id}](https://cloud.ibm.com/apidocs/cloud-databases-api#set-scaling-values-on-a-specified-group) API endpoint.
```sh
curl -X PATCH 'https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/groups/member' \
-H "Authorization: Bearer $APIKEY" \
-H "Content-Type: application/json" \
-d '{"memory": {
        "allocation_mb": 6144
      }
    }'
```

<br>
______________________________________

<br>

To scale a {{site.data.keyword.databases-for}} Isolated Compute instance, use the {{site.data.keyword.databases-for}} API [Scaling endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#setdeploymentscalinggroup){: external}.
Use a command like:

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/{group_id}
-H 'Authorization: Bearer <>'
-H 'Content-Type: application/json'
-d '{"group":
      {"host_flavor":
        {"id": "b3c.4x16.encrypted"}
      }
    }' \
```
{: pre}

To scale a {{site.data.keyword.databases-for}} Isolated Compute instance to a Shared Compute instance, use the following command:

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/{group_id}
-H 'Authorization: Bearer <>'
-H 'Content-Type: application/json'
-d '{"group":
      {"host_flavor":
        {"id": "multitenant"}
      },
      {"cpu":
        {"allocation_count": 3}
      },
      {"memory":
        {"allocation_mb": 2048}
      }
    }' \
```
{: pre}

CPU and RAM allocation is not allowed when provisioning or scaling through Isolated Compute. You must specify `mulitenant` for the `host_flavor` parameter.
{: note}

CPU and RAM autoscaling is not supported on {{site.data.keyword.databases-for}} Isolated Compute. Disk autoscaling is available. If you have provisioned an Isolated instance or switched over from a deployment with autoscaling, keep an eye on your resources using [{{site.data.keyword.monitoringfull}} integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring), which provides metrics for memory, disk space, and disk I/O utilization. To add resources to your instance, manually scale your deployment.
{: note}

## Scaling with Terraform
{: #resources-scaling-terraform}
{: terraform}

To scale a {{site.data.keyword.databases-for}} Isolated Compute instance, use Terraform.
Update `host_flavor` with your desired allocation; choose from either an Isolated Compute CPU x RAM configuration, or `multitenant` to land on Shared Compute. To implement your change, run `terraform apply`.
