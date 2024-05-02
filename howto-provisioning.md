---

copyright:
  years: 2019, 2024
lastupdated: "2024-05-02"

keywords: provision cloud databases, terraform, provisioning parameters, cli, resource controller api, provision elasticsearch

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning
{: #provisioning}

Provision a {{site.data.keyword.databases-for-elasticsearch_full}} deployment through the [catalog](https://cloud.ibm.com/databases/databases-for-elasticsearch/create){: external}, the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference){: external}, the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api){: external}, or through [Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.

## Provisioning through the {{site.data.keyword.cloud_notm}} console
{: #catalog}
{: ui}

Deploy from the console by specifying the following parameters:

### Service details
{: #service_details}
{: ui}

- **Service name** - The name can be any string and is the name that is used on the web and in the CLI to identify the new deployment.
- **The resource group** - If you are organizing your services into [resource groups](/docs/account?topic=account-account_setup), specify the resource group in this field. Otherwise, you can leave it at default. For more information, see [Managing resource groups](/docs/account?topic=account-rgs).
- **Location** - The deployment's public cloud region or Satellite location.

### Hosting model
{: #hosting_model}
{: ui}

- **Isolated:** Secure single-tenant offering for complex, highly-performant enterprise workloads.
- **Shared:** Flexible multi-tenant offering for dynamic, fine-tuned, and decoupled capacity selections.<br>
For more information, see [Hosting models](/docs/cloud-databases?topic=cloud-databases-hosting-types).

### Resource allocation
{: #resource_allocation}
{: ui}

Fine tune your resource allocation. The available options differ based on your selected hosting model.

- **Isolated:** Use the table to choose the machine size for each member of your deployment, and specify the disk size.
- **Shared:** By default, the smallest possible resource allocation is selected. This is ideal for small applications or testing. For larger allocations, select the *Custom* tile, which allows flexible resource configuration with 2+ cores. 

Specify the disk size depending on your requirements. It can be increased after provisioning but cannot be decreased to prevent data loss.
{: note}

### Service configuration
{: #service_configuration}
{: ui}

- **Database Version** [Set only at deployment]{: tag-red} - The deployment version of your database. To ensure optimal performance, run the preferred version. The latest minor version is used automatically. For more information, see [Versioning Policy](/docs/cloud-databases?topic=cloud-databases-versioning-policy){: external}.
- **Database Edition** - Select the edition you would like to deploy. Choose from Enterprise or Standard.
- **Encryption** - If you use [Key Protect](/docs/cloud-databases?topic=cloud-databases-key-protect&interface=ui), an instance and key can be selected to encrypt the deployment's disk. If you do not use your own key, the deployment automatically creates and manages its own disk encryption key.
- **Endpoints** [Set only at deployment]{: tag-red} - Configure the [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) on your deployment.

After you select the appropriate settings, click **Create** to start the provisioning process.

## Provisioning through the CLI
{: #use-cli}
{: cli}

### Create a service instance through the CLI
{: #create-service-instance-cli}
{: cli}

Before provisioning, follow the instructions provided in the documentation to install the [{{site.data.keyword.cloud_notm}} CLI tool](https://www.ibm.com/cloud/cli){: external}.

1. Log in to {{site.data.keyword.cloud_notm}}. If you use a federated user ID, it's important that you switch to a one-time passcode (`ibmcloud login --sso`), or use an API key(`ibmcloud --apikey key or @key_file`) to authenticate. For more information about how to log in by using the CLI, see [General CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login){: external} under `ibmcloud login`.

      ```sh
      ibmcloud login
      ```
      {: pre}

1. Provision a {{site.data.keyword.databases-for-elasticsearch}} Isolated instance with the same `"members_host_flavor"` -p flag, and then specify the `host_flavor value` parameter. For example: `{"members_host_flavor": "b3c.4x16.encrypted"}`

   ```sh
   ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE_NAME> <SERVICE_PLAN_NAME> <LOCATION> <SERVICE_ENDPOINTS_TYPE> <RESOURCE_GROUP> -p `{"members_host_flavor": "<host_flavor value>"}`
   ```
   {: pre}

   The fields in the command are described in the table that follows.
   
   | Field | Description | Flag |
   |-------|------------|------------|
   | `NAME` [Required]{: tag-red} | The instance name can be any string and is the name that is used on the web and in the CLI to identify the new deployment. |  |
   | `SERVICE_NAME` [Required]{: tag-red} | Name or ID of the service. For {{site.data.keyword.databases-for-elasticsearch}}, use `databases-for-elasticsearch`. |  |
   | `SERVICE_PLAN_NAME` [Required]{: tag-red} | `enterprise` or `platinum` |  |
   | `LOCATION` [Required]{: tag-red} | The location where you want to deploy. To retrieve a list of regions, use the `ibmcloud regions` command. |  |
   | `SERVICE_ENDPOINTS_TYPE` | Configure the [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) of your deployment, either `public` or `private`. The default value is `public`. |  |
   | `RESOURCE_GROUP` | The Resource group name. The default value is `default`. | -g |
   | `--parameters` | JSON file or JSON string of parameters to create service instance | -p |
   | `host_flavor` | To provision an Isolated Compute instance, use `{"members_host_flavor": "<host_flavor value>"}`. The `host_flavor value` parameter defines your Isolated Compute sizing. For Shared Compute, specify `multitenant`. For more information, see [Hosting models](/docs/cloud-databases?topic=cloud-databases-hosting-types).| |
   {: caption="Table 1. Basic command format fields" caption-side="top"}

   You will see a response like:

   ```text
   Creating service instance INSTANCE_NAME in resource group default of account    USER...
   OK
   Service instance INSTANCE_NAME was created.

   Name:                INSTANCE_NAME
   ID:                  crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/   40ddc34a846383BGB5b60e:dd13152c-fe15-4bb6-af94-fde0af5303f4::
   GUID:                dd13152c-fe15-4bb6-af94-fde0af56897
   Location:            LOCATION
   State:               provisioning
   Type:                service_instance
   Sub Type:            Public
   Service Endpoints:   public
   Allow Cleanup:       false
   Locked:              false
   Created at:          2023-06-26T19:42:07Z
   Updated at:          2023-06-26T19:42:07Z
   Last Operation:
                        Status    create in progress
                        Message   Started create instance operation
   ```
   {: codeblock}

1. To check provisioning status, use the following command:

   ```sh
   ibmcloud resource service-instance <INSTANCE_NAME>
   ```
   {: pre}

   When complete, you will see a response like:

   ```text
   Retrieving service instance INSTANCE_NAME in resource group default under account USER's Account as USER...
   OK

   Name:                  INSTANCE_NAME
   ID:                    crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/40ddc34a953a8c02f109835656860e:dd13152c-fe15-4bb6-af94-fde0af5303f4::
   GUID:                  dd13152c-fe15-4bb6-af94-fde5654765
   Location:              <LOCATION>
   Service Name:          databases-for-elasticsearch
   Service Plan Name:     standard
   Resource Group Name:   default
   State:                 active
   Type:                  service_instance
   Sub Type:              Public
   Locked:                false
   Service Endpoints:     public
   Created at:            2023-06-26T19:42:07Z
   Created by:            USER
   Updated at:            2023-06-26T19:53:25Z
   Last Operation:
                          Status    create succeeded
                          Message   Provisioning elasticsearch with version 7.17 (100%)
   ```
   {: codeblck}

1. (Optional) Deleting a service instance
   Delete an instance by running a command like this one:

   ```sh
   ibmcloud resource service-instance-delete <INSTANCE_NAME>
   ```
   {: pre}
   
### The `host flavor` parameter
{: #host-flavor-parameter-cli}
{: cli}   
   
 The `host_flavor` parameter defines your Compute sizing. Input the appropriate value for your desired size. To provision a Shared Compute instance, specify `multitenant`.

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

### The `--parameters` parameter
{: #flags-params-service-endpoints}
{: cli}

The `service-instance-create` command supports a `-p` flag, which allows JSON-formatted parameters to be passed to the provisioning process. Some parameter values are Cloud Resource Names (CRNs), which uniquely identify a resource in the cloud. All parameter names and values are passed as strings.

For example, if a database is being provisioned from a particular backup and the new database deployment needs a total of 9 GB of memory across three members, then the command to provision 3 GBs per member looks like:

```sh
ibmcloud resource service-instance-create databases-for-elasticsearch <SERVICE_NAME> standard us-south \
-p \ '{
  "backup_id": "crn:v1:blue:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "members_memory_allocation_mb": "3072"
}'
```
{: .pre}

## Provisioning through the Resource Controller API
{: #provision-controller-api}
{: api}

Follow these steps to provision using the [Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller){: external}.

1. Obtain an [IAM token from your API token](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#authentication){: external}.
1. You need to know the ID of the resource group that you would like to deploy to. This information is available through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_groups).

   Use a command like:
   ```sh
   ibmcloud resource groups
   ```
   {: pre}

1. You need to know the region you would like to deploy to.

   To list all of the regions that deployments can be provisioned into from the current region, use the [{{site.data.keyword.databases-for}} CLI plug-in](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference){: external}.

   The command looks like:

   ```sh
   ibmcloud cdb regions --json
   ```
   {: pre}


   Once you have all the information, [provision a new resource instance](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance){: external} with the {{site.data.keyword.cloud_notm}} Resource Controller.

   ```sh
   curl -X POST \
     https://resource-controller.cloud.ibm.com/v2/resource_instances \
     -H 'Authorization: Bearer <>' \
     -H 'Content-Type: application/json' \
       -d '{
       "name": "my-instance",
       "target": "blue-us-south",
       "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
       "resource_plan_id": "databases-for-elasticsearch-standard"
     }'
   ```
   {: .pre}

   The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required.
   {: required}
   
   The fields in the command are described in the table that follows.
   
   | Field | Description | Flag |
   |-------|------------|------------|
   | `NAME` [Required]{: tag-red} | The instance name can be any string and is the name that is used on the web and in the CLI to identify the new deployment. |  |
   | `SERVICE_NAME` [Required]{: tag-red} | Name or ID of the service. For {{site.data.keyword.databases-for-elasticsearch}}, use `databases-for-elasticsearch`. |  |
   | `SERVICE_PLAN_NAME` [Required]{: tag-red} | `enterprise` or `platinum` |  |
   | `LOCATION` [Required]{: tag-red} | The location where you want to deploy. To retrieve a list of regions, use the `ibmcloud regions` command. |  |
   | `SERVICE_ENDPOINTS_TYPE` | Configure the [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) of your deployment, either `public` or `private`. The default value is `public`. |  |
   | `RESOURCE_GROUP` | The Resource group name. The default value is `default`. | -g |
   | `--parameters` | JSON file or JSON string of parameters to create service instance | -p |
   | `host_flavor` | To provision an Isolated Compute instance, use `{"members_host_flavor": "<host_flavor value>"}`. The `host_flavor value` parameter defines your Isolated Compute sizing. For Shared Compute, specify `multitenant`. For more information, see [Hosting models](/docs/cloud-databases?topic=cloud-databases-hosting-types).| |
   {: caption="Table 1. Basic command format fields" caption-side="top"}
   
### The `host flavor` parameter
{: #host-flavor-parameter-api}
{: api}   
   
 The `host_flavor` parameter defines your Compute sizing. Input the appropriate value for your desired size. To provision a Shared Compute instance, specify `multitenant`.

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

## List of additional parameters
{: #provisioning-parameters-api}
{: api}

* `backup_id`- A CRN of a backup resource to restore from. The backup must be created by a database deployment with the same service ID. The backup is loaded after provisioning and the new deployment starts up that uses that data. A backup CRN is in the format `crn:v1:<...>:backup:<uuid>`. If omitted, the database is provisioned empty.
* `version` - The version of the database to be provisioned. If omitted, the database is created with the most recent major and minor version.
* `disk_encryption_key_crn` - The CRN of a KMS key (for example, [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) or [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about)), which is then used for disk encryption. A KMS key CRN is in the format `crn:v1:<...>:key:<id>`.
* `backup_encryption_key_crn` - The CRN of a KMS key (for example, [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started) or [{{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-about)), which is then used for backup encryption. A KMS key CRN is in the format `crn:v1:<...>:key:<id>`.

   To use a key for your backups, you must first [enable the service-to-service delegation](/docs/cloud-databases?topic=cloud-databases-key-protect#byok-for-backups).
   {: note}

* `members_memory_allocation_mb` -  Total amount of memory to be shared between the database members within the database. For example, if the value is "6144", and there are three database members, then the deployment gets 6 GB of RAM total, giving 2 GB of RAM per member. If omitted, the default value is used for the database type is used. This parameter only applies to `multitenant'.
* `members_disk_allocation_mb` - Total amount of disk to be shared between the database members within the database. For example, if the value is "30720", and there are three members, then the deployment gets 30 GB of disk total, giving 10 GB of disk per member. If omitted, the default value for the database type is used. This parameter only applies to `multitenant'.
* `members_cpu_allocation_count` - Enables and allocates the number of specified cores to your deployment. For example, to use two dedicated cores per member, use `"members_cpu_allocation_count":"2"`. If omitted, the default Shared Compute CPU:RAM ratios will be applied. This parameter only applies to `multitenant'.
* `service-endpoints` - The [Service Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints) supported on your deployment, `public` or `private`.

   In the CLI, `service-endpoints` is a flag, not a parameter.
   {: note}

## Provisioning with Terraform
{: #provisioning-terraform}
{: terraform}

Use Terraform to manage your infrastructure through the [`ibm_database` Resource for Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database) supports provisioning {{site.data.keyword.databases-for}} deployments.

Before executing a Terraform script on an existing instance, use the `terraform plan` command to compare the current infrastructure state with the desired state defined in your Terraform files. Any alteration to the `resource_group_id`, `service plan`, `version`, `key_protect_instance`, `key_protect_key`, `backup_encryption_key_crn` attributes recreates your instance. For a list of current argument references with the `Forces new resource` specification, see the [ibm_database Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.
{: important}

To provision an instance through Isolated Compute:

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
output "ICD Etcd database connection string" {
  value = "http://${ibm_database.test_acc.ibm_database_connection.icd_conn}"
}
```
{: codeblock}

To provision an instance through Shared Compute:

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
      allocation_count = 2
    }
    memory {
      allocation_mb = 4096
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
output "ICD Etcd database connection string" {
  value = "http://${ibm_database.test_acc.ibm_database_connection.icd_conn}"
}
```
{: codeblock}

### The `host flavor` parameter
{: #host-flavor-parameter-terraform}
{: terraform}   
   
 The `host_flavor` parameter defines your Compute sizing. Input the appropriate value for your desired size. To provision a Shared Compute instance, specify `multitenant`.

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
