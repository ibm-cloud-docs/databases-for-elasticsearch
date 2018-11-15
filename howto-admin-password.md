---
copyright:
  years: 2017,2018
lastupdated: "2018-10-08"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Setting the Admin Password

The {{site.data.keyword.databases-for-elasticsearch_full}} service is provisioned with an admin user.

You have to set the admin password before you can use it to connect. To set the password through the {{site.data.keyword.cloud_notm}} dashboard, select _Manage_ from the service dashboard to open the management panel for your service. Open the _Settings_ tab, and use the _Change Password_ panel to set a new admin password.

## Setting the admin password via the command line

Use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI cloud databases plug-in to set the admin password with the command line.

For example, to set the admin password for a deployment named "example-deployment", use the following command.
```
ibmcloud cdb user-password example-deployment admin <newpassword>
```

## Setting the admin password via the API

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/users/admin` endpoint if you need to manage or automate setting the password programmatically.

For more information, see the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#set-database-level-user-s-password).

