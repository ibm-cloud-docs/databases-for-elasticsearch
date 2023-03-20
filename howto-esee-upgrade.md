---
copyright:
  years: 2019, 2023
lastupdated: "2023-03-16"

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

Clone the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external}.

```sh
git clone https://github.com/IBM/elasticsearch-cos-snapshot-restore.git
```
{: pre}

## Step 2: Install the infrastructure with Terraform
{: #esupgrade-install-infra}

Within the [repository's terraform folder](https://github.com/IBM/elasticsearch-cos-snapshot-restore/tree/main/terraform){: external} the following files create the infrastructure necessary to create and restore your snapshots: 
- [`cos.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/cos.tf) (creates a Cloud Object Storage instance and a bucket)
- [`elastic.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/elastic.tf) (creates a source and target, and necessary configuration)
- [`main.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/main.tf) (contains the main set of configuration for your module)
- [`variables.tf`](https://github.com/IBM/elasticsearch-cos-snapshot-restore/blob/main/terraform/variables.tf) (contains the variable definitions)

After cloning the repo, navigate to your terraform folder and install the infrastructure with the following command:

```sh
terraform init 
```
{: pre}

The Terraform script outputs configuration data that is needed to run the application, so copy it into the root folder:

```sh
terraform output -json >../config.json
```
{: pre}

## Run the shell snapshot script
{: #esupgrade-snapshot-script}

In the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} main folder, find the *migrate.sh* file. This shell script uses the information that is provided by the `config.json` file to perform the necessary migration steps.

- Create different snapshot names by adding a timestamp.
- Get database and S3 parameters.
- Mount S3/COS bucket on source deployment.
- Mount S3/COS bucket on {{site.data.keyword.databases-for-elasticsearch_full}}
- Close all indices on the target so the restore can be performed on top of it without touching the `icd-auth index`, which is protected by {{site.data.keyword.databases-for}}.

Run *migrate.sh* as many times as necessary to fully bake up your data. Snapshots are incremental, so the first snapshot will take longer than the rest. 

Once your COS bucket has all the necessary snapshots, stop writes to the source. Then, take a final snapshot and restore it to the target. All of your data is now in the target, just point your applications to the target database. Your upgrade is now complete.

## Update user passwords
{: #update-user-passwords}

A restore to Elasticsearch 7.17 invalidates existing user passwords. Existing user passwords must be reset after the restore. Follow the procedure below to list all users and change user passwords. 

Use the following command to list all users:

[final process from Takshil](https://github.ibm.com/ibm-cloud-databases/cloud-databases/pull/128)
