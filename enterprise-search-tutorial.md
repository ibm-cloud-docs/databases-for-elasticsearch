---

copyright:
   years: 2023, 2024
lastupdated: "2024-01-16"

keywords: IBM Cloud Databases, ICD, enterprise search, ca certificate

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 1h

---

{{site.data.keyword.attribute-definition-list}}

# Configuring Kibana and Enterprise Search server with an {{site.data.keyword.databases-for-elasticsearch}} instance
{: #tutorial-elasticsearch-enterprise-search-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="1h"}

This tutorial will guide you through the steps to configure a functional Enterprise Search server integrated with an {{site.data.keyword.databases-for-elasticsearch_full}} instance. Elasticsearch is a powerful and versatile search and analytics engine that helps you store, search, and analyze large volumes of data quickly and in near-real-time. 

{{site.data.keyword.databases-for-elasticsearch_full}} is an Elasticsearch service that is offered by {{site.data.keyword.cloud_notm}} that provides a managed and scalable solution for deploying and running Elasticsearch clusters. 

Kibana complements Elasticsearch by offering a flexible visualization platform. It allows you to explore, visualize, and share insights from your data, enabling you to create custom dashboards and visualizations to better understand your information.

Enterprise Search extends the capabilities of {{site.data.keyword.databases-for-elasticsearch}} to provide a unified search experience across various data sources, including documents, emails, databases, and more.

By integrating Enterprise Search with your {{site.data.keyword.databases-for-elasticsearch}} instance, you gain a comprehensive search solution that uses the strengths of both platforms to efficiently discover insights from your data.
{: shortdesc}

Kibana and Enterprise Search will be deployed on [IBM Code Engine](https://www.ibm.com/products/code-engine), a fully-managed serverless platform that can be used to host cloud native applications such as web apps.

## Before you start
{: #kibana-ent-search-before-start}

Before you begin, ensure you have the following:

- An [{{site.data.keyword.cloud_notm}} Account](https://cloud.ibm.com/registration){: external}
- The [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started)
- [Terraform](https://www.terraform.io/){: external} - to deploy infrastructure

## Obtain an API key to deploy infrastructure to your account
{: #kibana-ent-search-obtain-key}
{: step}

Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an {{site.data.keyword.cloud_notm}} API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

## Clone the project
{: #kibana-ent-search-clone-project}
{: step}

```sh
git clone https://github.com/IBM/elasticsearch-kibana-enterprise-search.git
```
{: pre}

## Install your infrastructure
{: #kibana-ent-search-install-infra}
{: step}

1. Navigate into the terraform folder of the cloned project.

   ```sh
   cd elasticsearch-kibana-codeengine/terraform
   ```
   {: pre}

1. On your machine, create a document that is named `terraform.tfvars`, with the following fields:

   ```sh
   ibmcloud_api_key = "<your api key>"
   region = "<an ibm cloud region>" #e.g. eu-gb
   es_username = "admin"
   es_password = "<make up a password>" #Passwords have a 15 character minimum and must contain a number. Other acceptable characters are A-Z, a-z, 0-9, -, _
   es_version="<a supported major version>" # eg 8.12
   ```
   {: pre}

   The `terraform.tfvars` document contains variables that you might want to keep secret.
   {: important}

1. Install the infrastructure with the following command:

   ```sh
   terraform init 
   terraform apply --auto-approve
   ```
   {: pre}

## Visit your Kibana deployment
{: #kkibana-ent-search-visit-deployment}
{: step}

The previous step will output the URL of the Kibana deployment, e.g. something like:

```sh
kibana_endpoint = "https://kibana-app.1dqmr45rt678g05.eu-gb.codeengine.appdomain.cloud"
```
{: pre}

Log in at this URL with the username and password you supplied above.

Once logged in, you can configure Enterprise Search by visiting 

```sh
https://kibana-app.1dqmr45rt678g05.eu-gb.codeengine.appdomain.cloud/app/enterprise_search/app_search/engines
```
You can find more information on the many features of Enterprise search on the [Elastic website](https://www.elastic.co/guide/en/enterprise-search/current/start.html).

The output of the previous step also contains the URL of the Elasticsearch deployment itself, which can be used to connect it to [WatsonX Assistant](https://www.ibm.com/products/watsonx-assistant) or other applications.


## Wrapping up
{: #kkibana-ent-search-wrapping-up}
{: step}

Your {{site.data.keyword.databases-for-elasticsearch}} incurs charges, as does the Code Engine resources that host Kibana and Enterprise Search. After you finish this tutorial, you can remove all the infrastructure by going to the `terraform` directory of the project and using the command:

```sh
terraform destroy
```
{: pre}