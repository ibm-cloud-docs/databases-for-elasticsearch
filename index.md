---

Copyright:
  years: 2018
lastupdated: "2018-11-21"

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# About {{site.data.keyword.databases-for-elasticsearch_full_notm}}
{: #about}

{{site.data.keyword.databases-for-elasticsearch_full}} is a managed Elasticsearch service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. 
{:shortdesc}

## Provisioning {{site.data.keyword.databases-for-elasticsearch}}

{{site.data.keyword.databases-for-elasticsearch}} is an {{site.data.keyword.cloud_notm}} service. Provisioning and account management is handled through your {{site.data.keyword.cloud_notm}} account. If you already have an account, you can provision {{site.data.keyword.databases-for-elasticsearch}} from the [{{site.data.keyword.cloud_notm}} catalog](https://{DomainName}/catalog/services/databases-for-elasticsearch).

For detailed provisioning information, including {{site.data.keyword.cloud_notm}} CLI instructions, see the [Provisioning](/docs/services/databases-for-elasticsearch?topic=cloud-databases-provisioning) page.

If you don't yet have an {{site.data.keyword.cloud_notm}} account, sign up on the [registration](https://{DomainName}/registration/) page.

### Managing Access to {{site.data.keyword.databases-for-elasticsearch}}

{{site.data.keyword.databases-for-elasticsearch}} is an Identity and Access Management (IAM) integrated service. Access to the service is governed by the roles and attributes that are consistent across IAM-integrated services in {{site.data.keyword.cloud_notm}}. Get started with managing your users on the [IAM Getting Started tutorial](/docs/iam?topic=iam-getstarted). For more information about IAM, see the [What is IAM?](/docs/iam?topic=iam-iamoverview) documentation.

More information on IAM roles and actions for the {{site.data.keyword.databases-for-elasticsearch}} service is available on the [Access Management](/docs/services/databases-for-elasticsearch?topic=cloud-databases-iam) page.

## Using {{site.data.keyword.databases-for-elasticsearch}}

{{site.data.keyword.databases-for-elasticsearch}} provides a UI, accessible by selecting _Manage_ from the left sidebar of your service and opening the management panel. You get a quick [Overview](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-dashboard-overview) of your service as well as configuration settings on the [Settings](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-dashboard-overview#settings) tab and access to your backups on the [Backups](/docs/services/databases-for-elasticsearch?topic=cloud-databases-dashboard-backups) tab.

### Using the {{site.data.keyword.cloud_notm}} command-line interface

The [{{site.data.keyword.cloud_notm}} command-line interface](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli) provides in interactive terminal for your {{site.data.keyword.cloud_notm}} account and your {{site.data.keyword.cloud_notm}} services. The cloud databases plug-in extends this functionality to your {{site.data.keyword.databases-for-elasticsearch}} deployments. More information and installation instructions are on the [{{site.data.keyword.cloud_notm}} CLI Plug-in](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference) page.

### Using the cloud databases API

You can use the {{site.data.keyword.cloud_notm}} databases API to manage your deployments. Authentication is IAM-based and you use your {{site.data.keyword.cloud_notm}} account's platform API keys to access the API. More information on API keys is in the [IAM documentation](/docs/iam/apikeys?topic=iam-manapikey). The API foundation endpoint for your service is on the deployment's [_Overview_](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-dashboard-overview) page. For more information, see the [API reference](https://{DomainName}/apidocs/cloud-databases-api).

## Connecting to {{site.data.keyword.databases-for-elasticsearch}}

General information on getting connection strings can be found on the [Getting Connection Strings](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-connection-strings) page.

{{site.data.keyword.databases-for-elasticsearch}} deployments are secured with authentication and SSL/TLS encrypted connections. Deployments have an admin user provided by default. [Set the admin password](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-admin-password) to use it to access your deployment. Then, if you want to use the Elasticsearch API directly, [connect by using `curl`](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-connecting-curl).

Specific guidance on connecting with Elasticsearch drivers is on the [Connecting External Applications](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-external-app) page. If you want to connect a Cloud Foundry application that is running in {{site.data.keyword.cloud_notm}}, see the [Connecting an {{site.data.keyword.cloud_notm}} Application](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-ibmcloud-app) page. The [Getting Started tutorial](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started) provides a sample application that can run locally or on {{site.data.keyword.cloud_notm}} to test-drive your {{site.data.keyword.databases-for-elasticsearch}} deployment.

## Other {{site.data.keyword.cloud_notm}} Integrations

{{site.data.keyword.databases-for-elasticsearch}} deployments offer other cloud services integrations. 
- View events with [Activity Tracker](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-activity-tracker)
- Monitor deployment resource use with the [Monitoring](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-monitoring) service.
- BYOK encryption is available if you use [Key Protect](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-key-protect)
- [Service Endpoints Integration](/docs/services/databases-for-elasticsearch?topic=cloud-databases-service-endpoints) allows you to select public or private networking for your deployment at provision










