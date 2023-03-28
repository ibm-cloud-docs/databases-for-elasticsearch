---
copyright:
  years: 2019, 2023
lastupdated: "2023-03-28"

keywords: elasticsearch migration, databases, elasticsearch migrating, elasticsearch enterprise, snapshot, elasticsearch update

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Elasticsearch data migration using snapshot and restore
{: #esmigration-elasticsearch-snapshot-restore}

Migrate data between two instances of Elasticsearch with the help of Object Storage. Using the snapshot and restore method, take snapshots from your source instance, store them in Object Storage, and then restore those snapshots from Object Storage into your target instance.

This tutorial uses snapshot and restore, {{site.data.keyword.cos_full_notm}} and two instances of {{site.data.keyword.databases-for-elasticsearch}}. However, this process is applicable to any S3-compatible object storage solution and any deployment of Elasticsearch.

We've simplified the process by using [Terraform](https://www.terraform.io/){: external} and shell scripts. Simply follow the procedure outlined on this page, plugging in the necessary variables as you go.

## Getting Productive
{: #esmigration-get-productive}

Before you migrate your data, install [Terraform](https://www.terraform.io/){: external} to codify and deploy necessary infrastructure. You also need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration).

## Step 1: Obtain an API key to deploy infrastructure to your account
{: #esmigration-obtain-api-key}

Follow [these steps](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key) to create an {{site.data.keyword.cloud_notm}} API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.{: .important}

## Step 2: Clone the Elasticsearch Snapshot/Restore GitHub Repository
{: #esmigration-clone-project}

Clone the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} to your local machine.

```sh
git clone https://github.com/IBM/elasticsearch-cos-snapshot-restore.git
```
{: pre}

After cloning this folder, navigate to the newly created project folder on your local machine. 

## Step 3: Install and run the Terraform script
{: #esmigration-install-infra}

The [terraform folder](https://github.com/IBM/elasticsearch-cos-snapshot-restore/tree/main/terraform){: external} contains files that create the necessary infrastructure to create and restore your snapshots: 
- [`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf)
- [`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf)
- [`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf)
- [`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf)

### cos.tf
{: #esmigration-costf}

[`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf) creates a Cloud Object Storage instance and a bucket. Update your resources for the `restoreCOSInstance`, `restoreBucket`, `resourceKey`. This script then outputs the `bucket_credentials` and `bucket_name`.

### elastic.tf
{: #esmigration-elastictf}

[`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf) creates a source, target, and necessary configurations. Input the variables for the `resource "ibm_database" "esSource"` and `resource "ibm_database" "esTarget"`. This script then outputs the necessary configuration variables. 

### main.tf
{: #esmigration-maintf}

[`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf) contains the main set of configuration for your module. For the `ibmcloud_api_key` variable, create or retrieve an [{{site.data.keyword.cloud}} API key](/docs/account?topic=account-userapikey&interface=ui#create_user_key). Then, specify the Resource Group and `ibm_resource_group` variable, which outputs the `resource_group_name`.

### variables.tf
{: #esmigration-varstf}

[`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf) contains the variable definitions `ibmcloud_api_key`, `region`, and `elastic_password`. Update these variables with your API key, preferred region, and your Elasticsearch password. 

After setting up your resources, configurations, and variables, go ahead and run your Terraform script. Navigate to your terraform folder and install the infrastructure with the following command:

```sh
terraform init 
terraform apply --auto-approve
```
{: pre}

The Terraform script outputs configuration data that is needed to run the application, so copy it into the root folder:

```sh
terraform output -json >../config.json
```
{: pre}

## Step 5: Run the shell snapshot script
{: #esmigration-snapshot-script}

In the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} main folder, find the *migrate.sh* file. This shell script uses the information that is provided by the `config.json` file to perform the necessary migration steps.

- Create different snapshot names by adding a timestamp.
- Get database and S3/COS parameters.
- Mount S3/COS bucket on source deployment.
- Mount S3/COS bucket on target deployment.
- Close all indexes on the target so the restore can be run without touching the `icd-auth index`, which is protected by {{site.data.keyword.databases-for}}.

Run *migrate.sh* as many times as necessary to fully back up your data. 

Snapshots are incremental, so the first snapshot takes longer than the rest. 
{: note}

Once your COS bucket has all the necessary snapshots, stop any writes to the source. Then, run *migrate.sh* one more time to take a final snapshot and restore it to the target. All of your data is now in the target. Point your applications to the target database and your upgrade is complete.

## Retrieve and update user passwords
{: #esmigration-retrieve-update-user-passwords}

If you're restoring to Elasticsearch 7.17 as an update from an earlier version, existing user passwords will be invalidated and must be reset after the restore. For more information, see [Retrieve and update user passwords](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-upgrading&interface=ui#esupgrade-retrieve-update-user-passwords).
