---
Copyright:
  years: 2018
lastupdated: "2018-10-04"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Database Versioning Policy
{: #versioning}

{{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} allow users to select major versions for its database. We are committed to providing the more secure, up-to-date versions of databases. With this in mind, we take control of upgrading database minor versions when appropriate.

## Major Versions

From their introduction on {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}}, we set out to support any major version of a database for at least 3 years. If a database version is deprecated or marked end of life by the open source project owners, we will move to immediately deprecate that version on {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}}.

## Deprecation of Major Versions 

When a major version is deprecated, a six-month transition window is opened for current users of that deprecated version.

At the beginning of the period, we will seek to contact effected users of the deprecation. 30 days after the communication, deprecated major versions cannot be deployed as new deployments on {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}}.

During the six-month transition window, users will be able to initiate an upgrade to a supported major version. Restoration of databases into new deployments of the deprecated major version will be possible through the deprecation, although we recommend upgrading to a non-deprecated major version as soon as possible. Existing customers instances will continue to run as normal.

At the end of the six-month window, we will remove access to the database and take a backup. This backup will be available to be restored into a new supported database version.

## Major versions defined

Elasticsearch major versions are the first number in a release.version.maintenance version number. Major versions of Elasticsearch have therefore been 2, 5, 6 and in the future 7.

