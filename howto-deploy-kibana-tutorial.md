---

copyright:
   years: 2023
lastupdated: "2023-10-25"

keywords: elasticsearch, kibana

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 1h

---

{{site.data.keyword.attribute-definition-list}}	

# Deploy Kibana using IBM Code Engine and connect to your {{site.data.keyword.databases-for-elasticsearch}} instance
{: #kibana-code-engine-icd-elasticsearch}
{: toc-content-type="tutorial"}
{: toc-completion-time="1h"}

Deploy Kibana using IBM Code Engine and connect to your IBM Cloud Databases for Elasticsearch instance.

Using this repo you can deploy Kibana using IBM Code Engine and connect to your IBM Cloud Databases for Elasticsearch instance.

Kibana is a web interface that allows you to visualise the data in Elasticsearch instances. IBM Code Engine is a a fully managed, serverless platform that allows you to run workloads without worrying about deploying infrastructure. Elasticsearch is a NoSQL database with powerful search capabilities.

The IBM Elasticsearch service does not include a managed Kibana service, but following this tutorial you can have a Kibana instance up and running and connected to your Elasticsearch database within a few minutes and still using the managed service model of the IBM Cloud.

NOTE: Code Engine is a paid-for service, so following this tutorial will incur charges.

## Before you start
{: #kibana-code-engine-icd-elasticsearch-before-start}
{: step}

- An [IBM Cloud Account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - to deploy infrastructure
- A pre-existing [IBM Cloud Databases for Elasticsearch instance](https://cloud.ibm.com/databases/databases-for-elasticsearch/create){: external}

## Obtain an API key to deploy infrastructure to your account
{: #kibana-code-engine-icd-elasticsearch-obtain-key}
{: step}

Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an IBM Cloud API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.

## Clone the project
{: #kibana-code-engine-icd-elasticsearch-clone-project}
{: step}

```sh
git clone https://github.com/IBM/elasticsearch-kibana-codeengine.git
```
{: pre}

## Install the infrastructure
{: #kibana-code-engine-icd-elasticsearch-install-infra}
{: step}

1. Navigate into the terraform folder of the cloned project.

   ```sh
   cd elasticsearch-kibana-codeengine/terraform
   ```
   {: pre}

1. On your machine, create a document that is named terraform.tfvars, with the following fields:

   ```sh
    ibmcloud_api_key = "<your_api_key_from_step_1>"
    region = "<your_region>"
    es_url = "<the url of your elasticsearch deployment>"
    es_username = "<the username of your elasticsearch deployment>"
    es_password = "<the password of your elasticsearch user>"
    es_version="<the version of your elasticsearch deployment>"
   ```
   {: pre}

   The terraform.tfvars document contains variables that you might want to keep   secret so it is ignored by the GitHub repository.
   {: important}

   Kibana runs with `ELASTICSEARCH_SSL_VERIFICATIONMODE` set to `none`, so although the traffic between Kibana and Elasticsearch is encrypted, the Elasticsearch service is not verified against the CA certificate provided by IBM Databases for Elasticsearch credentials.
   {: note}

1. Install the infrastructure with the following command:

   ```sh
   terraform init 
   terraform apply --auto-approve
   ```
   {: pre}

## Visit your Kibana deployment
{: #kibana-code-engine-icd-elasticsearch-visit-deployment}
{: step}

The previous step produces a URL, which is the public URL of your Kibana deployment. It looks something like: `https://kibana-app.1834dcfgrtygbg.eu-gb.codeengine.appdomain.cloud`.

Visit that URL in your web browser you should see the Kiban login screen where you can log in with your credentials and have access to your Elasticsearch deployment!

If at any time you wish to tear down your Kibana application and project from IBM Code Engine, simply run:

```sh
terraform destroy
```
{: pre}
