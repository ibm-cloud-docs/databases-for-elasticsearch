---

copyright:
  years: 2019, 2024
lastupdated: "2024-06-10"

keywords: elasticsearch dedicated cores, databases, manual scaling, disk I/O, memory, CPU, elasticsearch resources, elasticsearch scaling

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Adding disk, memory, and CPU 
{: #resources-scaling}

For new [hosting models](/docs/cloud-databases?topic=cloud-databases-hosting-models), scaling is available through the CLI, API, and Terraform.
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

If you find that your queries and database activity suffer from performance issues due to a lack of memory, you can scale the amount of RAM allocated to your service. If your database instance is on an Isolated Compute hosting model, select the CPU x RAM configuration that matches your resource needs. If your database instance is on a Shared Compute or Dedicated Core hosting model, select the RAM allocation that you want for your database. Note that Dedicated Core is deprecated, and will be removed in May 2025. 

Adding memory to the total allocation adds memory to the members equally. {{site.data.keyword.databases-for-elasticsearch}} deployments have their memory allocation policy set at 50% heap and 50% system memory, so increasing the amount of RAM increases both heap and system memory. RAM can be scaled up or down. 

### vCPU
{: #resources-scaling-cpu}

If you find that your database workloads need more CPU resources, you can scale the amount of CPU allocated to your service. If your database instance is on an Isolated Compute hosting model, select the CPU x RAM configuration that matches your resource needs. If your database instance is on a Shared Compute or Dedicated Core hosting model, select the CPU allocation that you want for your database. Note that Dedicated Core is deprecated, and will be removed in May 2025. 

The default of 0 cores uses compute resources on multi-tenanted hosts. This style of multi-tenant is deprecated, and will be removed in September 2025 in favor of Shared Compute. CPU can be scaled up or down. 

## Scaling considerations
{: #resources-scaling-consider}

- Scaling up might cause your deployment to restart. If your deployment needs to be moved to a host with more capacity, the deployment is restarted as part of the move.

- Scaling down RAM or CPU does not trigger restarts.
- Disk cannot be scaled down.
- Scaling between hosting models (Shared Compute, Isolated Compute, and Dedicated Cores) moves your deployment to new hosts. Your databases are restarted as part of that move. As your deployment is moved to a new host, this can also take longer than just adding more resources. For more information, see [Shared Compute and Isolated Compute](/docs/cloud-databases?topic=cloud-databases-hosting-models).
- Similarly, drastically scaling up CPU, RAM, or disk can take longer to run than small resource increases to account for provisioning more underlying hardware resources.
- Scaling operations are logged in [{{site.data.keyword.at_full}}](/docs/databases-for-elasticsearch?topic=cloud-databases-activity-tracker).
- If you find consistent trends in resource usage or want to scale when certain resource thresholds are reached, enable [autoscaling](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-autoscaling) on your deployment.
- {{site.data.keyword.databases-for-elasticsearch}} is designed to balance work load across a cluster and can benefit from being horizontally scaled. If you are concerned about performance, check out [Adding Elasticsearch nodes](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-horizontal-scaling).

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
`ibmcloud cdb deployment-groups-set example-deployment member --memory 6144`.


If your database is a [Shared Compute](https://cloud.ibm.com/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-shared-compute-ui) instance, you can adjust the memory, CPU, and disk options with the following command. This can also be used to move a database from a different hosting model to the Shared Compute hosting model.

```sh
ibmcloud cdb deployment-groups-set <deploymentid> <groupid> [--memory <val>] [--cpu <val>] [--disk <val>] [--hostflavor multitenant]
```
{: pre}

For example, use: 

```sh
ibmcloud cdb deployment-groups-set crn:abc ... xyz:: member  --memory 24576 --cpu 6  --hostflavor multitenant
```
{: pre}

If your database is an [Isolated Compute](https://cloud.ibm.com/docs/cloud-databases?topic=cloud-databases-hosting-models&interface=ui#hosting-models-iso-compute-ui) instance, memory and CPU are adjusted together by selecting the Isolated Compute size (see all sizes in Table 1). Disk is scaled separately. To scale a {{site.data.keyword.databases-for}} Isolated Compute instance, use a command, such as the following that is used to scale to a 4 CPU by 16 RAM instance. This command  can also be used to move a database from a different hosting model to the Isolated Compute hosting model. 

```sh
ibmcloud cdb deployment-groups-set <deploymentid> <groupid> [--disk <val>] [--hostflavor b3c.4x16.encrypted]
```
{: pre}

For example, use: 

```sh
ibmcloud cdb deployment-groups-set crn:abc ... xyz:: member  --hostflavor b3c.4x16.encrypted
```
{: pre}

CPU and RAM autoscaling is not supported on {{site.data.keyword.databases-for}} Isolated Compute. Disk autoscaling is available. If you provisioned an isolated instance or switched over from a deployment with autoscaling, monitor your resources using [{{site.data.keyword.monitoringfull}} integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring), which provides metrics for memory, disk space, and disk I/O utilization. To add resources to your instance, manually scale your deployment.
{: note}

| **Host flavor** | **host_flavor value** |
|:-------------------------:|:---------------------:|
| Shared Compute            | `multitenant`    |
| 4 CPU x 16 RAM            | `b3c.4x16.encrypted`    |
| 8 CPU x 32 RAM            | `b3c.8x32.encrypted`    |
| 8 CPU x 64 RAM            | `m3c.8x64.encrypted`    |
| 16 CPU x 64 RAM           | `b3c.16x64.encrypted`   |
| 32 CPU x 128 RAM          | `b3c.32x128.encrypted`  |
| 30 CPU x 240 RAM          | `m3c.30x240.encrypted`  |
{: caption="Table 1. Host flavor sizing parameter" caption-side="bottom"}

## Determine the hosting model of your database
{: #resources-hosting-determine}

Use the following command to review the value of the `host_flavor` attribute. This will be null if the database is on a deprecated hosting model (not Shared or Isolated Compute). 

```sh
ibmcloud cdb groups <deployment_id> --json
```
{: pre}

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
{: pre}

To scale any {{site.data.keyword.databases-for}} instance to a Shared Compute instance, use the `host_flavors` parameter, such as in the following command:

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/{group_id}
-H 'Authorization: Bearer <>'
-H 'Content-Type: application/json'
-d '{"host_flavor":
        {"id": "multitenant"},
      "cpu":
        {"allocation_count": 3},
      "memory":
        {"allocation_mb": 2048}
    }' \
```
{: pre}

To scale any instance into a {{site.data.keyword.databases-for}} Isolated Compute instance or to scale to a different Isolated Compute size, use the `host_flavor` parameter, this time set to the desired Isolated Compute size. Available hosting sizes and their `host_flavor` value parameters are listed in [Table 1](#host-flavor-parameter-api). For example, `{"members_host_flavor": "b3c.4x16.encrypted"}`. Note that since the host flavor selection includes CPU and RAM sizes (`b3c.4x16.encrypted` is 4 CPU and 16 RAM), this request does not accept both, an Isolated size selection and separate CPU and RAM allocation selections. Scale with the {{site.data.keyword.databases-for}} [API Scaling endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#setdeploymentscalinggroup){: external}, with a command like: 

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/{group_id}
-H 'Authorization: Bearer <>'
-H 'Content-Type: application/json'
-d '{"host_flavor": {"id": "b3c.4x16.encrypted"}}' \
```
{: pre}

CPU and RAM allocation is not allowed when provisioning or scaling through Isolated Compute. You must specify `mulitenant` for the `host_flavor` parameter.
{: note}

CPU and RAM autoscaling is not supported on {{site.data.keyword.databases-for}} Isolated Compute. Disk autoscaling is available. If you have provisioned an Isolated instance or switched over from a deployment with autoscaling, keep an eye on your resources using [{{site.data.keyword.monitoringfull}} integration](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring), which provides metrics for memory, disk space, and disk I/O utilization. To add resources to your instance, manually scale your deployment.
{: note}

### The `host flavor` parameter
{: #host-flavor-parameter-api}
{: api
   
The `host_flavor` parameter defines your compute sizing. To provision a Shared Compute instance, specify `multitenant`. To provision an Isolated Compute instance, input the appropriate value for your desired CPU and RAM configuration. 

| **Host flavor** | **host_flavor value** |
|:-------------------------:|:---------------------:|
| Shared Compute            | `multitenant`    |
| 4 CPU x 16 RAM            | `b3c.4x16.encrypted`    |
| 8 CPU x 32 RAM            | `b3c.8x32.encrypted`    |
| 8 CPU x 64 RAM            | `m3c.8x64.encrypted`    |
| 16 CPU x 64 RAM           | `b3c.16x64.encrypted`   |
| 32 CPU x 128 RAM          | `b3c.32x128.encrypted`  |
| 30 CPU x 240 RAM          | `m3c.30x240.encrypted`  |
{: caption="Table 1 Host flavor sizing parameter" caption-side="bottom"}

## Scaling with Terraform
{: #resources-scaling-terraform}
{: terraform}

To scale a {{site.data.keyword.databases-for}} Isolated Compute instance, use Terraform.
Update `host_flavor` with your desired allocation; choose from either an Isolated Compute CPU x RAM configuration, or `multitenant` to land on Shared Compute. To implement your change, run `terraform apply`.

Before executing a Terraform script on an existing instance, use the `terraform plan` command to compare the current infrastructure state with the desired state defined in your Terraform files. Any alteration to the `resource_group_id`, `service plan`, `version`, `key_protect_instance`, `key_protect_key`, `backup_encryption_key_crn` attributes recreates your instance. For a list of current argument references with the `Forces new resource` specification, see the [ibm_database Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.
{: important}

Select the [hosting model]([/docs/cloud-databases?topic=cloud-databases-hosting-models) you want your database to be provisioned on. You can change this later. 

Provision a {{site.data.keyword.databases-for-elasticsearch}} Shared hosting model instance with the `"members_host_flavor"` -p parameter set to `multitenant`. See the following example:  

```terraform
data "ibm_resource_group" "group" {
  name = "<your_group>"
}
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "standard"
  location          = "eu-gb"
  service           = "databases-for-elasticsearch"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]
  adminpassword                = "password12"
  group {
    group_id = "member"
    host_flavor {
      id = "multitenant"
    },
    cpu {
      allocation_count = 6
    }
    memory {
      allocation_mb = 24576
    }
    disk {
      allocation_mb = 256000
    }
  }
  users {
    name     = "user123"
    password = "password12"
  }
  allowlist {
    address     = "172.168.1.1/32"
    description = "desc"
  }
}
output "ICD Elasticsearch database connection string" {
  value = "http://${ibm_database.test_acc.ibm_database_connection.icd_conn}"
}
```
{: codeblock}


Provision a {{site.data.keyword.databases-for-elasticsearch}} Isolated instance with the same `"members_host_flavor"` -p parameter, set to the desired Isolated size. Available hosting sizes and their `host_flavor value` parameters are listed in [Table 1](#host-flavor-parameter-cli). For example, `{"members_host_flavor": "b3c.4x16.encrypted"}`. Note that since the host flavor selection includes CPU and RAM sizes (`b3c.4x16.encrypted` is 4 CPU and 16 RAM), this request does not accept both, an Isolated size selection and separate CPU and RAM allocation selections. 

```terraform
data "ibm_resource_group" "group" {
  name = "<your_group>"
}
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "standard"
  location          = "eu-gb"
  service           = "databases-for-elasticsearch"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]
  adminpassword                = "password12"
  group {
    group_id = "member"
    host_flavor {
      id = "b3c.8x32.encrypted"
    }
    disk {
      allocation_mb = 256000
    }
  }
  users {
    name     = "user123"
    password = "password12"
  }
  allowlist {
    address     = "172.168.1.1/32"
    description = "desc"
  }
}
output "ICD Elasticsearch database connection string" {
  value = "http://${ibm_database.test_acc.ibm_database_connection.icd_conn}"
}
```
{: codeblock}


### The `host flavor` parameter
{: #host-flavor-parameter-terraform}
{: terraform}   
   
The `host_flavor` parameter defines your compute sizing. To provision a Shared Compute instance, specify `multitenant`. To provision an Isolated Compute instance, input the appropriate value for your desired CPU and RAM configuration. 

| **Host flavor** | **host_flavor value** |
|:-------------------------:|:---------------------:|
| Shared Compute            | `multitenant`    |
| 4 CPU x 16 RAM            | `b3c.4x16.encrypted`    |
| 8 CPU x 32 RAM            | `b3c.8x32.encrypted`    |
| 8 CPU x 64 RAM            | `m3c.8x64.encrypted`    |
| 16 CPU x 64 RAM           | `b3c.16x64.encrypted`   |
| 32 CPU x 128 RAM          | `b3c.32x128.encrypted`  |
| 30 CPU x 240 RAM          | `m3c.30x240.encrypted`  |
{: caption="Table 1. Host flavor sizing parameter" caption-side="bottom"}

CPU and RAM autoscaling is not supported on {{site.data.keyword.databases-for}} Isolated Compute. Disk autoscaling is available. If you have provisioned an Isolated instance or switched over from a deployment with autoscaling, keep an eye on your resources using [{{site.data.keyword.monitoringfull}} integration](/docs/cloud-databases?topic=cloud-databases-monitoring), which provides metrics for memory, disk space, and disk I/O utilization. To add resources to your instance, manually scale your deployment.
{: note}
