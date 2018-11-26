---

Copyright:
  years: 2018
lastupdated: "2018-11-21"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# About {{site.data.keyword.databases-for-elasticsearch_full_notm}}
{: #about-databases-for-elasticsearch}

{{site.data.keyword.databases-for-elasticsearch_full}} is a managed Elasticsearch service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. 
{:shortdesc}

## Provisioning {{site.data.keyword.databases-for-elasticsearch}}

{{site.data.keyword.databases-for-elasticsearch}} is an {{site.data.keyword.cloud_notm}} service. Provisioning and account management is handled through your {{site.data.keyword.cloud_notm}} account. If you already have an account, you can provision {{site.data.keyword.databases-for-elasticsearch}} from the [{{site.data.keyword.cloud_notm}} catalog](https://{DomainName}/catalog/services/databases-for-elasticsearch).

For detailed provisioning information, including {{site.data.keyword.cloud_notm}} CLI instructions, see the [Provisioning](./howto-provisioning.html) page.

If you don't yet have an {{site.data.keyword.cloud_notm}} account, sign up on the [registration](https://{DomainName}/registration/) page.

### Managing Access to {{site.data.keyword.databases-for-elasticsearch}}

{{site.data.keyword.databases-for-elasticsearch}} is an Identity and Access Management (IAM) integrated service. Access to the service is governed by the roles and attributes that are consistent across IAM-integrated services in {{site.data.keyword.cloud_notm}}. Get started with managing your users on the [IAM Getting Started tutorial](https://{DomainName}/docs/iam/quickstart.html#getstarted). For more information about IAM, see the [What is IAM?](https://{DomainName}/docs/iam/index.html#iamoverview) documentation.

More information on IAM roles and actions for the {{site.data.keyword.databases-for-elasticsearch}} service is available on the [Access Management](./reference-access-management.html) page.

## Using {{site.data.keyword.databases-for-elasticsearch}}

{{site.data.keyword.databases-for-elasticsearch}} provides a UI, accessible by selecting _Manage_ from the left sidebar of your service and opening the management panel. You get a quick [Overview](./dashboard-overview.html) of your service as well as configuration settings on the [Settings](./dashboard-settings.html) tab and access to your backups on the [Backups](./dashboard-backups.html) tab.

### Using the {{site.data.keyword.cloud_notm}} command-line interface

The {{site.data.keyword.cloud_notm}} command-line interface provides in interactive terminal for your {{site.data.keyword.cloud_notm}} account and your {{site.data.keyword.cloud_notm}} services. The cloud databases plug-in extends this functionality to your {{site.data.keyword.databases-for-elasticsearch}} deployments. More information and installation instructions are on the [Using {{site.data.keyword.cloud_notm}} CLI for {{site.data.keyword.databases-for-elasticsearch}}](./howto-using-ibmcloud-cli.html) page.

### Using the cloud databases API

You can use the {{site.data.keyword.cloud_notm}} databases API to manage your service. Authentication is IAM-based and you use your {{site.data.keyword.cloud_notm}} account's platform API keys to access the API. More information on API keys is in the [IAM documentation](https://{DomainName}/docs/iam/apikeys.html#platform-api-keys). The API foundation endpoint for your service is on the deployment's [_Overview_](./dashboard-overview.html) page. For more information, see the [API reference](https://{DomainName}/apidocs/cloud-databases-api).

## Connecting to {{site.data.keyword.databases-for-elasticsearch}}

General information on getting connection strings can be found on the [Getting Connection Strings](./howto-getting-connection-strings) page.

{{site.data.keyword.databases-for-elasticsearch}} deployments are secured with authentication and SSL/TLS encrypted connections. Deployments have an admin user provided by default. [Set the admin password](./howto-admin-password.html) to use it to access your deployment. Then, if you want to use the Elasticsearch API directly, [connect by using `curl`](./connecting-curl.html).

Specific guidance on connecting with Elasticsearch drivers is on the [Connecting External Applications](./connecting-external.html) page. If you want to connect a Cloud Foundry application that is running in {{site.data.keyword.cloud_notm}}, see the [Connecting an {{site.data.keyword.cloud_notm}} Application](./connecting-ibmcloud-app.html) page. The [Getting Started tutorial](./getting-started.html) provides a sample application that can run locally or on {{site.data.keyword.cloud_notm}} to test-drive your {{site.data.keyword.databases-for-elasticsearch}} deployment.

## Other {{site.data.keyword.cloud_notm}} Integrations

{{site.data.keyword.databases-for-elasticsearch}} deployments offer other cloud services integrations. 
- View events with [Activity Tracker](./reference-activity-tracker.html)
- BYOK encryption is available if you use [Key Protect](./reference-key-protect.html)










