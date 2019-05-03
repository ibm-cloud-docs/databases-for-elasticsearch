---
copyright:
  years: 2018,2019
lastupdated: "2019-04-10"

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an external application
{: #external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-elasticsearch_full}}. Each deployment has connection strings specifically for drivers and applications. Connection strings are displayed in the _Connections_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The connection strings can be used by any of the credentials you have created on your deployment. While you can use the admin user for all of your connections and applications, it might be better to create users specifically for your applications to connect with. Documentation on creating users is on the [Creating Users and Getting Connection Strings](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-connection-strings) page.

## Connecting with a language's driver

All the information a driver needs to make a connection to your deployment is in the "https" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Type`||Type of connection - for Elasticsearch, it is "uri"
`Scheme`||Scheme for a URI - for Elasticsearch, it is "https"
`Path`||Path for a uri
`Authentication`|`Username`|The username that you use to connect.
`Authentication`|`Password`|A password for the user - might be shown as `$PASSWORD`
`Authentication`|`Method`|How authentication takes place; "direct" authentication is handled by the driver.
`Hosts`|`0...`|A hostname and port to connect to
`Composed`|`0...`|A URI combining Scheme, Authentication, Host, and Path
`Certificate`|`Name`|The allocated name for the self-signed certificate for database deployment
`Certificate`|Base64|A base64 encoded version of the certificate.
{: caption="Table 1. `https`/`URI` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

Many Elasticsearch drivers are able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example,`https://admin:$PASSWORD@d5eeee66-5bc4-498a-b73b-1307848f1eac.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:31821`

Elasticsearch has a vast array of language drivers. The table covers a few of the most common.

Language|Driver|Documentation
----------|-----------
Node|`elasticsearch-js`|[Link](https://github.com/elastic/elasticsearch-js)
Ruby|`elasticsearch-ruby`|[Link](https://github.com/elastic/elasticsearch-ruby)
Ruby on Rails|elasticsearch-rails|[Link](https://github.com/elastic/elasticsearch-rails)
Python|`elasticsearch-py`|[Link](https://www.elastic.co/guide/en/elasticsearch/client/python-api/current/index.html)
Java|`Jest`|[Link](https://github.com/searchbox-io/Jest/tree/master/jest)
Go|`elastic`|[Link](https://olivere.github.io/elastic/)
{: caption="Table 2. Common Elasticsearch drivers" caption-side="top"}

## Driver TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-elasticsearch}} are TLS 1.2 enabled, so the driver you use to connect needs to be able to support encryption. Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection. 

### Using the self-signed certificate

1. Copy the certificate information from the _Connections_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.

### CLI plug-in support for the self-signed certificate

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver.






 
