---
copyright:
  years: 2019, 2023
lastupdated: "2023-03-14"

keywords: elasticsearch migration, databases, elasticsearch migrating, elasticsearch enterprise, snapshot, elasticsearch update

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Use snapshot/restore to upgrade to Elasticsearch 7.17 
{: #snapshot-elasticsearch-upgrade}

To upgrade your {{site.data.keyword.databases-for-elasticsearch_full}}, take steps to successfully migrate all of your data using your own [object storage](https://www.ibm.com/topics/object-storage).

The migration process entails taking snapshots of your current Elasticsearch database and storing those securely in your preferred {{site.data.keyword.cos_full_notm}}/S3-compatible object storage bucket. Then, restore those snapshots in your {{site.data.keyword.databases-for-elasticsearch}} deployment. 

## Getting productive 
{: #esupgrade-install-terraform}

To begin the process, install [Terraform](https://www.terraform.io/){: external} - to codify and deploy infrastructure

## Clone the project
{: #esupgrade-clone-project}

Clone the project from the {{site.data.keyword.databases-for}} [Elasticsearch Snapshot/Restore GitHub Repository](https://github.com/IBM/elasticsearch-cos-snapshot-restore).{: external}.

```sh
git clone https://github.com/IBM/elasticsearch-cos-snapshot-restore.git
```
{: pre}

## Install the infrastructure
{: #esupgrade-install-infra}

In this step, you <>. The GitHub repository contains <>.

## Update user passwords
{: #update-user-passwords}

A restore from Elasticsearch Standard edition to Elasticsearch 7.17 invalidates existing user passwords. Existing user passwords must be reset after the restore. Follow the procedure below to list all users and change user passwords. 

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
