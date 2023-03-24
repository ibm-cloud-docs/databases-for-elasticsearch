---
copyright:
  years: 2019, 2023
lastupdated: "2023-03-24"

keywords: elasticsearch migration, databases, elasticsearch migrating, elasticsearch enterprise, snapshot, elasticsearch update

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Elasticsearch data migration using snapshot and restore
{: #esmigration-elasticsearch-snapshot-restore}

To migrate your existing Elasticsearch data to a new {{site.data.keyword.databases-for-elasticsearch_full}} deployment, use your own [object storage](https://www.ibm.com/topics/object-storage) and snapshots of your current Elasticsearch database. Store those snapshots securely in your preferred {{site.data.keyword.cos_full_notm}}/S3-compatible object storage bucket. Then, complete the migration process by restoring those snapshots into a {{site.data.keyword.databases-for-elasticsearch}} deployment.

We've simplified the migration process by using Terraform and shell scripts. Simply follow the prodecure outlined on this page, plugging in the necessary variables as you go.

## Getting Productive
{: #esmigration-get-productive}

Before you migrate your data, install [Terraform](https://www.terraform.io/){: external} to codify and deploy necessary infrastructure.

## Taking and restoring snapshots
{: #esmigration-take-restore-snapshots}

### Step 1: Clone the Elasticsearch Snapshot/Restore GitHub Repository
{: #esmigration-clone-project}

Clone the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} to your local machine.

```sh
git clone https://github.com/IBM/elasticsearch-cos-snapshot-restore.git
```
{: pre}

After cloning this folder, navigate to the newly created project folder on your local machine. 

### Step 2: Install the infrastructure with Terraform
{: #esmigration-install-infra}

The [terraform folder](https://github.com/IBM/elasticsearch-cos-snapshot-restore/tree/main/terraform){: external} contains files that create the necessary infrastructure to create and restore your snapshots: 
- [`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf) (creates a Cloud Object Storage instance and a bucket)
- [`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf) (creates a source and target, and necessary configuration)
- [`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf) (contains the main set of configuration for your module)
- [`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf) (contains the variable definitions)

#### cos.tf
{: #esmigration-costf}

[`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf) creates a Cloud Object Storage instance and a bucket. Before you run the script, you need to update your resources for the `restoreCOSInstance`, `restoreBucket`, `resourceKey`. This script then outputs the `bucket_credentials` and `bucket_name`.

#### elastic.tf
{: #esmigration-elastictf}

[`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf) creates a source and target, and necessary configuration. Input the variables for the `resource "ibm_database" "esSource"` and `resource "ibm_database" "esTarget"` the script will output the necessary configuration variables. 

#### main.tf
{: #esmigration-maintf}

[`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf) contains the main set of configuration for your module. For the `ibmcloud_api_key` variable, create or retrieve an [{{site.data.keyword.cloud}} API key](/docs/account?topic=account-userapikey&interface=ui#create_user_key). Then, specify the Resource Group and `ibm_resource_group` variable, which outputs the `resource_group_name`.

#### variables.tf
{: #esmigration-varstf}

[`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf) contains the variable definitions `ibmcloud_api_key`, `region`, and `elastic_password`. After you input these variables, you're ready to run your Terraform script.


### Step 3: Run the Terraform Script
{: #esmigration-run-tf-script}

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
{: #esmigration-snapshot-script}

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
{: #esmigration-retrieve-update-user-passwords}

If you're restoring to Elasticsearch 7.17 as an update from an earlier version, existing user passwords will be invalidated and must be reset after the restore. Follow <INSERT URL HERE> this procedure to list all users and change user passwords.

