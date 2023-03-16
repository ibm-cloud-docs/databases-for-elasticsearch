---
copyright:
  years: 2019, 2023
lastupdated: "2023-03-15"

keywords: elasticsearch migration, databases, elasticsearch migrating, elasticsearch enterprise, snapshot, elasticsearch update

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Upgrade to Elasticsearch 7.17 
{: #snapshot-elasticsearch-upgrade}

To upgrade your {{site.data.keyword.databases-for-elasticsearch_full}} deployment, migrate all of your data by using your own [object storage](https://www.ibm.com/topics/object-storage) and snapshots of your current Elasticsearch database. Store those snapshots securely in your preferred {{site.data.keyword.cos_full_notm}}/S3-compatible object storage bucket. Then, restore those snapshots in your {{site.data.keyword.databases-for-elasticsearch}} deployment. 

Before you begin migration, install [Terraform](https://www.terraform.io/){: external} - to codify and deploy infrastructure.

## Clone the Elasticsearch Snapshot/Restore GitHub Repository
{: #esupgrade-clone-project}

Clone the [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore).{: external}.

```sh
git clone https://github.com/IBM/elasticsearch-cos-snapshot-restore.git
```
{: pre}

## Install the infrastructure
{: #esupgrade-install-infra}

Within the [repository's terraform folder](https://github.com/IBM/elasticsearch-cos-snapshot-restore/tree/main/terraform).{: external} there are the following files that will create the infrastructure necessary to create and restore your snapshots: 
- cos.tf (creates a COS insstance and a bucket)
- elastic.tf (creates a source and a target, as well as necessary configuration)
- main.tf (contains the main set of configuration for your module)
- variables.tf (contains the variable definitions)

After you have cloned the repo, navigate to the folder and install the infrastructure with the following command:

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

In the main [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore){: external} main folder, you find the *migrate.sh* file. This shell script uses the information provided by the `config.json` file to 




## Update user passwords
{: #update-user-passwords}

A restore to Elasticsearch 7.17 invalidates existing user passwords. Existing user passwords must be reset after the restore. Follow the procedure below to list all users and change user passwords. 

To list all users: 

```sh
curl -k https://admin:$PASSWORD@<HOST>:<PORT>/_security/user
```
{: pre}

The command will produce a list like:

```sh

{"testing":{"username":"testing","roles":["superuser"],"full_name":null,"email":null,"metadata":{},"enabled":true},"admin":{"username":"admin","roles":["superuser"],"full_name":null,"email":null,"metadata":{},"enabled":true},"elastic":{"username":"elastic","roles":["superuser"],"full_name":null,"email":null,"metadata":{"_reserved":true},"enabled":true},"kibana":{"username":"kibana","roles":["kibana_system"],"full_name":null,"email":null,"metadata":{"_deprecated":true,"_reserved":true,"_deprecated_reason":"Please use the [kibana_system] user instead."},"enabled":true},"kibana_system":{"username":"kibana_system","roles":["kibana_system"],"full_name":null,"email":null,"metadata":{"_reserved":true},"enabled":true},"logstash_system":{"username":"logstash_system","roles":["logstash_system"],"full_name":null,"email":null,"metadata":{"_reserved":true},"enabled":true},"beats_system":{"username":"beats_system","roles":["beats_system"],"full_name":null,"email":null,"metadata":{"_reserved":true},"enabled":true},"apm_system":{"username":"apm_system","roles":["apm_system"],"full_name":null,"email":null,"metadata":{"_reserved":true},"enabled":true},"remote_monitoring_user":{"username":"remote_monitoring_user","roles":["remote_monitoring_collector","remote_monitoring_agent"],"full_name":null,"email":null,"metadata":{"_reserved":true},"enabled":true}}
```
{: screen}

Exclude any users where `metadata._reserved=true`.

Also exclude the admin user, because that user can be set through the UI and is used to change all other user passwords.

For ever user (other than admin), run the following command:

```sh
curl -kX POST https://admin:$PASSWORD@<HOST>:<PORT>/_security/user/<USERNAME>/_password -H 'Content-Type: application/json' -d'{"password":"<A_PASSWORD>" }'
```
{: pre}
