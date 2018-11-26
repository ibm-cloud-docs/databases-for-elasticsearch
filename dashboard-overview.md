---

Copyright:
  years: 2018
lastupdated: "2018-10-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Service Overview

The _Overview_ page shows you information about your {{site.data.keyword.databases-for-elasticsearch_full}} database. The overview includes essential identifying information.

## Deployment Details

The _Deployment Details_ panel shows a quick description of your deployment.

### Type

The type of database that is offered by the service, and the database version that your service uses.

### ID

The ID is a [CRN (Cloud Resource Name)](https://{DomainName}/docs/overview/crn.html) which uniquely identifies the database deployment. The CRN is used to refer to the database in the API and can be used with the CLI.

## Recent Tasks

Every time that you make administrative changes to your service (such as scaling, or taking a manual backup), a task starts up. The _Recent Tasks_ panel shows the task name and progress bar for any running tasks, and a list of the most recent completed tasks.

## Security

Encryption at rest is enabled for all {{site.data.keyword.databases-for-elasticsearch}} deployments. If you brought your own encryption key from [Key Protect](./reference-key-protect.html), the panel provides a link to your Key Protect instance and the _Encryption Key_ field has the name of the key.

## Instance Administration API

You can manage your {{site.data.keyword.databases-for-elasticsearch}} service through the {{site.data.keyword.cloud_notm}} databases API. This panel provides the essential information for using the API.

### Foundation Endpoint

The foundation endpoint is the opening stanza of the URL to be used to send API requests to. It combines the region that your service resides in and the base {{site.data.keyword.cloud_notm}} databases API endpoint. 

### Deployment ID

Many API calls require the ID of the database deployment. The database deployment's ID/CRN shown here is the same as the ID/CRN shown in the Deployment Details, in a click-to-copy field to make it simpler to use. 

The ID needs to be URL encoded to be used in an API call because the CRN includes a "/".
{: .tip}

### {{site.data.keyword.cloud_notm}} databases API Reference

For more information about the {{site.data.keyword.cloud_notm}} databases API, see the [API reference](https://{DomainName}/apidocs/cloud-databases-api) page.