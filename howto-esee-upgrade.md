---
copyright:
  years: 2019, 2023
lastupdated: "2023-03-21"

keywords: elasticsearch migration, databases, elasticsearch migrating, elasticsearch enterprise, snapshot, elasticsearch update

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Upgrade to Elasticsearch 7.17 
{: #snapshot-elasticsearch-upgrade}

To upgrade your {{site.data.keyword.databases-for-elasticsearch_full}} deployment, migrate all of your data by using your own [object storage](https://www.ibm.com/topics/object-storage) and snapshots of your current Elasticsearch database. Store those snapshots securely in your preferred {{site.data.keyword.cos_full_notm}}/S3-compatible object storage bucket. Lastly, restore those snapshots in your {{site.data.keyword.databases-for-elasticsearch}} deployment. 

Before you begin migration, install [Terraform](https://www.terraform.io/){: external} to codify and deploy infrastructure.

## Step 1: Clone the Elasticsearch Snapshot/Restore GitHub Repository
{: #esupgrade-clone-project}

Clone the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} to your local machine. 

```sh
git clone https://github.com/IBM/elasticsearch-cos-snapshot-restore.git
```
{: pre}

After you've cloned this folder, navigate to the newly created project folder on your local machine. 

## Step 2: Install the infrastructure with Terraform
{: #esupgrade-install-infra}

The [terraform folder](https://github.com/IBM/elasticsearch-cos-snapshot-restore/tree/main/terraform){: external} contains files that create the necessary infrastructure to create and restore your snapshots: 
- [`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf) (creates a Cloud Object Storage instance and a bucket)
- [`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf) (creates a source and target, and necessary configuration)
- [`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf) (contains the main set of configuration for your module)
- [`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf) (contains the variable definitions)

### cos.tf
{: #esupgrade-costf}

[`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf) creates a Cloud Object Storage instance and a bucket. Before you run the script, you need to update your resources for the `restoreCOSInstance`, `restoreBucket`, `resourceKey`. This script then outputs the `bucket_credentials` and `bucket_name`.

### elastic.tf
{: #esupgrade-elastictf}

[`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf) creates a source and target, and necessary configuration. Input the variables for the `resource "ibm_database" "esSource"` and `resource "ibm_database" "esTarget"` the script will output the necessary configuration variables. 

### main.tf
{: #esupgrade-maintf}

[`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf) contains the main set of configuration for your module. For the `ibmcloud_api_key` variable, create or retrieve an [{{site.data.keyword.cloud}} API key](/docs/account?topic=account-userapikey&interface=ui#create_user_key). Then specify the Resource Group and fill in the `ibm_resource_group` variable, which will output the `resource_group_name`.

### variables.tf
{: #esupgrade-varstf}

[`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf) contains the variable definitions `ibmcloud_api_key`, `region`, and `elastic_password`.  Once you've input these variables, you're ready to run your Terraform script. 


## Step 3: Run the Terraform Script
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

## Step 4: Run the shell snapshot script
{: #esupgrade-snapshot-script}

In the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} main folder, find the *migrate.sh* file. This shell script uses the information that is provided by the `config.json` file to perform the necessary migration steps.

- Create different snapshot names by adding a timestamp.
- Get database and S3 parameters.
- Mount S3/COS bucket on source deployment.
- Mount S3/COS bucket on {{site.data.keyword.databases-for-elasticsearch_full}}
- Close all indices on the target so the restore can be performed on top of it without touching the `icd-auth index`, which is protected by {{site.data.keyword.databases-for}}.

Run *migrate.sh* as many times as necessary to fully back up your data. 

Snapshots are incremental, so the first snapshot will take longer than the rest. 
{: note}

Once your COS bucket has all the necessary snapshots, stop any writes to the source. Then, take a final snapshot and restore it to the target. All of your data is now in the target, just point your applications to the target database. Your upgrade is now complete.

## Update user passwords
{: #update-user-passwords}

A restore to Elasticsearch 7.17 invalidates existing user passwords. Existing user passwords must be reset after the restore. Follow the procedure below to list all users and change user passwords. 


[final process from Takshil](https://github.ibm.com/ibm-cloud-databases/cloud-databases/pull/128)

First, run the [ibmcloud plugin update](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_settings#ibmcloud_plugin_update) command: 

```sh
`ibmcloud plugin update cloud-databases` 
```
{: pre}

Then, log in to your {{site.data.keyword.cloud}} account:

```sh
`ibmcloud login -a cloud.ibm.com -r <region> --sso`
```
{: pre}

Then, run the following command:

```sh
`ibmcloud cloud-databases es user-list --help`
```
{: pre}

This outputs the command with all the required and optional arguments.

Before running the final command, ensure that you have used the UI to update the admin password. Then, run the following command: 

```sh
`ibmcloud cloud-databases es user-list <$formation_name> <$admin_password>`
```
{: pre}

If this command is run without the `--json` option, the plugin`user-list` also prints the reset password commands for all users.
