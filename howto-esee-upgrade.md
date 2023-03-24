---
copyright:
  years: 2019, 2023
lastupdated: "2023-03-24"

keywords: elasticsearch migration, databases, elasticsearch migrating, elasticsearch enterprise, snapshot, elasticsearch update

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Upgrade to Elasticsearch 7.17 
{: #snapshot-elasticsearch-upgrade}

To upgrade your {{site.data.keyword.databases-for-elasticsearch_full}} deployment, migrate all of your data by using your own [object storage](https://www.ibm.com/topics/object-storage) and snapshots of your current Elasticsearch database. Store those snapshots securely in your preferred {{site.data.keyword.cos_full_notm}}/S3-compatible object storage bucket. Lastly, restore those snapshots in your {{site.data.keyword.databases-for-elasticsearch}} deployment. 

Before you begin migration, install [Terraform](https://www.terraform.io/){: external} to codify and deploy infrastructure.

## Taking and restoring snapshots
{: #esupgrade-take-restore-snapshots}

### Step 1: Clone the Elasticsearch Snapshot/Restore GitHub Repository
{: #esupgrade-clone-project}

Clone the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} to your local machine. 

```sh
git clone https://github.com/IBM/elasticsearch-cos-snapshot-restore.git
```
{: pre}

After cloning this folder, navigate to the newly created project folder on your local machine. 

### Step 2: Install the infrastructure with Terraform
{: #esupgrade-install-infra}

The [terraform folder](https://github.com/IBM/elasticsearch-cos-snapshot-restore/tree/main/terraform){: external} contains files that create the necessary infrastructure to create and restore your snapshots: 
- [`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf) (creates a Cloud Object Storage instance and a bucket)
- [`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf) (creates a source and target, and necessary configuration)
- [`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf) (contains the main set of configuration for your module)
- [`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf) (contains the variable definitions)

#### cos.tf
{: #esupgrade-costf}

[`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf) creates a Cloud Object Storage instance and a bucket. Before you run the script, you need to update your resources for the `restoreCOSInstance`, `restoreBucket`, `resourceKey`. This script then outputs the `bucket_credentials` and `bucket_name`.

#### elastic.tf
{: #esupgrade-elastictf}

[`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf) creates a source and target, and necessary configuration. Input the variables for the `resource "ibm_database" "esSource"` and `resource "ibm_database" "esTarget"` the script will output the necessary configuration variables. 

#### main.tf
{: #esupgrade-maintf}

[`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf) contains the main set of configuration for your module. For the `ibmcloud_api_key` variable, create or retrieve an [{{site.data.keyword.cloud}} API key](/docs/account?topic=account-userapikey&interface=ui#create_user_key). Then, specify the Resource Group and `ibm_resource_group` variable, which outputs the `resource_group_name`.

#### variables.tf
{: #esupgrade-varstf}

[`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf) contains the variable definitions `ibmcloud_api_key`, `region`, and `elastic_password`. After you input these variables, you're ready to run your Terraform script.


### Step 3: Run the Terraform Script
{: #esupgrade-run-tf-script}

Navigate to your terraform folder and install the infrastructure with the following command:

```sh
terraform init 
```
{: pre}

The Terraform script outputs configuration data that is needed to run the application, so copy it into the root folder:

```sh
terraform output -json >../config.json
```
{: pre}

### Step 4: Run the shell snapshot script
{: #esupgrade-snapshot-script}

In the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} main folder, find the *migrate.sh* file. This shell script uses the information that is provided by the `config.json` file to perform the necessary migration steps.

- Create different snapshot names by adding a timestamp.
- Get database and S3 parameters.
- Mount S3/COS bucket on source deployment.
- Mount S3/COS bucket on {{site.data.keyword.databases-for-elasticsearch_full}}
- Close all indexes on the target so the restore can be run without touching the `icd-auth index`, which is protected by {{site.data.keyword.databases-for}}.

Run *migrate.sh* as many times as necessary to fully back up your data. 

Snapshots are incremental, so the first snapshot takes longer than the rest. 
{: note}

Once your COS bucket has all the necessary snapshots, stop any writes to the source. Then, take a final snapshot and restore it to the target. All of your data is now in the target. Point your applications to the target database and your upgrade is complete.

## Retrieve and update user passwords
{: #esupgrade-retrieve-update-user-passwords}

### Retrieve Elasticsearch user passwords
{: #esupgrade-retrieve-user-passwords}

A restore to Elasticsearch 7.17 invalidates existing user passwords. Existing user passwords must be reset after the restore. Follow this procedure to list all users and change user passwords.

First, run the [ibmcloud plug-in update](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_settings#ibmcloud_plugin_update) command: 

```sh
ibmcloud plugin update cloud-databases
```
{: pre}

Then, log in to your {{site.data.keyword.cloud}} account:

```sh
ibmcloud login -a cloud.ibm.com -r <region> --sso
```
{: pre}

Next, run the `user-list` command with the `--help` flag. This command outputs various options for your account's user list.

```sh
ibmcloud cloud-databases es user-list --help
```
{: pre}

The `user-list` command output looks like: 

```screen
ibmcloud cloud-databases es user-list --help
NAME:
  user-list, ul - List all users from the database internal credential store.

USAGE:
  ibmcloud cdb elasticsearch user-list (NAME|ID) (ADMIN_PASSWORD) [--json] [-c DIRECTORY] [--api-version]
  
OPTIONS:
  --json, -j         --json, -j                     Results as JSON.
  --certroot, -c     --certroot value, -c value     Certificate Root. (default: "< >") [$CERTROOT]
  --api-version, -v  --api-version value, -v value  API Version used for request.
```

Before running the command to update user passwords, ensure that you have updated the admin password. For more information, see [Setting the Admin Password](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-admin-password&interface=cli). Then, run the following command: 

```sh
ibmcloud cloud-databases es user-list <formation_name> <admin_password>
```
{: pre}

If this command is run without the `--json` option, the plug-in `user-list` also prints the reset password commands for all users.

The `user-list` command output looks like: 

```screen
ibmcloud cloud-databases es user-list es-akshay-res-717-new 12345678987654321   
Getting user list for es-akshay-res-717-new...
8 user accounts are present in the Elasticsearch credential store:
Username                                         Fullname         Email
fred                                             Fred Name        fred@example.com
admin                                            admin Name       admin@example.com
ibmkibana                                        ibmkibana Name   ibmkibana@example.com
ibm_cloud_d68c8129_fea9_4e83_abc8_4a6c514248ac
ibm_cloud_2a526d0b_e101_49f3_bd13_7c065870a7b1
ibm_cloud_7052ee45_d702_4073_aea2_a5435989568e
ibm_cloud_29075661_76d0_405a_9619_480eb7a173d4
ibm_cloud_eb148643_099a_4334_a88a_f2e5e1f8ccdd

You can use these commands to set new passwords for them: 
ibmcloud cdb deployment-user-password "es-akshay-res-717-new" fred

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" admin

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibmkibana

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_d68c8129_fea9_4e83_abc8_4a6c514248ac

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_2a526d0b_e101_49f3_bd13_7c065870a7b1

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_7052ee45_d702_4073_aea2_a5435989568e

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_29075661_76d0_405a_9619_480eb7a173d4

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_eb148643_099a_4334_a88a_f2e5e1f8ccdd

OK
```

## Step 6: Update Elasticsearch user passwords
{: #esupgrade-update-user-passwords}

To update user passwords, run the following command for *each* user. You are then prompted to enter the new password for that user.

```sh
ibmcloud cdb deployment-user-password "example-deployment" <your-user-1>
```
{: pre}

The `deployment-user-password` command needs to be run for each user.
{: important}
