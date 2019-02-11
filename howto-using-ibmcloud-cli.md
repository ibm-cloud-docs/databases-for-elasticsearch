---
copyright:
  years: 2017,2018
lastupdated: "2018-11-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Using the {{site.data.keyword.cloud_notm}} CLI for {{site.data.keyword.databases-for-elasticsearch}}

Use the {{site.data.keyword.cloud_notm}} CLI and the cloud databases plug-in to access and manage {{site.data.keyword.databases-for-elasticsearch_full}}.

## The {{site.data.keyword.cloud_notm}} CLI

The {{site.data.keyword.cloud_notm}} CLI is a general-purpose developer tool that provides access to your {{site.data.keyword.cloud_notm}} account and services through a command-line interface.

An introduction and installation instructions are available on the [{{site.data.keyword.cloud_notm}} CLI Overview](https://{DomainName}/docs/cli/index.html#overview) page. If you install the CLI from the cURL command that is provided, you get a selection of extra plug-ins and extensions for multiple IDEs.

You can install just the stand-alone package from the [Installing the stand-alone IBM Cloud CLI](https://{DomainName}/docs/cli/reference/ibmcloud/download_cli.html#install_use) page. 

## The cloud databases plug-in

Once you have the {{site.data.keyword.cloud_notm}} CLI, [login](https://{DomianName}/docs/cli/reference/ibmcloud/bx_cli.html#ibmcloud_login) and grab the cloud databases plug-in. 

`ibmcloud plugin install cloud-databases`

Use `ibmcloud cdb help` for a list of commands and usage, or see the [documentation](https://{DomainName}/docs/databases-cli-plugin/cloud-databases-cli.html#cloud-databases-cli-plug-in) for a command reference. 

## The {{site.data.keyword.cloud_notm}} CLI and IAM

Access to services via {{site.data.keyword.cloud_notm}} CLI is governed through Integrated Access Management. In order to use the CLI to view or manage a service (or to grant privileges to another user on your account), you have to set the correct permissions. For more information about IAM management, see the [IAM Getting Started tutorial](https://{DomainName}/docs/iam/quickstart.html#getstarted).

For more information on how IAM affects {{site.data.keyword.databases-for-elasticsearch}}, see the [IAM reference page](./reference-access-management.html)