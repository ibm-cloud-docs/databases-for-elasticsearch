---
copyright:
  years: 2017,2018
lastupdated: "2018-10-10"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an external application
{: #connecting-external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-elasticsearch_full}}. The service provides connection strings specifically for drivers and applications. 

## Getting Connection Strings

The easiest way to get connection strings for an application is to create a set of _Service Credentials_ specifically for your application to connect with. Doing so also returns all the connection information as JSON in a click-to-copy field. Full documentation on generating and retrieving connection strings is on the [Getting Connection Strings](./howto-getting-connection-strings.html) page.

Other options for getting connection strings are through the [CLI cloud databases plug-in](./howto-getting-connection-strings.html#generating-connection-strings-from-the-command-line), and the [cloud databases API](https://console.{DomainName}/apidocs/cloud-databases-api). A [table](./howto-getting-connection-strings.html#the-elasticsearch-section) with a breakdown of all the connection information is included for reference.

## Connecting with a language's driver

Elasticsearch has a vast array of language drivers. The table covers a few of the most common.

Language|Driver|Examples
----------|-----------
Node|`elasticsearch-js`|[Link](https://github.com/elastic/elasticsearch-js)
Ruby|`elasticsearch-ruby`|[Link](https://github.com/elastic/elasticsearch-ruby)
Ruby on Rails|elasticsearch-rails|[Link](https://github.com/elastic/elasticsearch-rails)
Python|`elasticsearch-py`|[Link](https://www.elastic.co/guide/en/elasticsearch/client/python-api/current/index.html)
Java|`Jest`|[Link](https://github.com/searchbox-io/Jest/tree/master/jest)
Go|`elastic`|[Link](https://olivere.github.io/elastic/)

## Driver TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-elasticsearch}} are TLS 1.2 enabled, so the driver you use to connect need to be able to support encryption. Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection. In most cases, you want to decode and save a copy of the certificate, and then provide the path to the driver. For more information, see [Using the self-signed certificate](./howto-getting-connection-strings.html#using-the-self-signed-certificate).







 
