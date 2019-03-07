---
copyright:
  years: 2019
lastupdated: "2019-03-04"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Elasticsearch Plugins
{: #plugins}

Elasticsearch supports a variety of plugins to extend its core functionality. {{site.data.keyword.databases-for-elasticsearch_full}} comes with a selection of plugins already enabled on your deployment. Plugin management is handled by the service, and users can not enable or disable plugins.

You can check the plug-ins installed in your Elasticsearch cluster using the `/_nodes/plugins` cluster API endpoint. Example, 
```
CURL_CA_BUNDLE=certificate.crt curl -u ibm_cloud_es_user:password https://f1b5e4a9-7179-4af2-a795-b7a0d71ec80a.974550db55eb4ec0983f023940bf637f.databases.appdomain.cloud:30909/_nodes/plugins
```

## Available Plugins

Name | Description
-------|-------
analysis-icu | The ICU Analysis plugin integrates Lucene ICU module into elasticsearch, adding ICU relates analysis components.
analysis-kuromoji | The Japanese (kuromoji) Analysis plugin integrates Lucene kuromoji analysis module into elasticsearch.
analysis-nori | The Korean (nori) Analysis plugin integrates Lucene nori analysis module into elasticsearch.
analysis-phonetic | The Phonetic Analysis plugin integrates phonetic token filter analysis with elasticsearch.
analysis-smartcn | Smart Chinese Analysis plugin integrates Lucene Smart Chinese analysis module into elasticsearch.
analysis-stempel | The Stempel (Polish) Analysis plugin integrates Lucene stempel (polish) analysis module into elasticsearch.
analysis-ukrainian | The Ukrainian Analysis plugin integrates the Lucene UkrainianMorfologikAnalyzer into elasticsearch.
ingest-attachment | Ingest processor that uses Apache Tika to extract contents.
mapper-size | The Mapper Size plugin allows document to record their uncompressed size at index time.
repository-s3 | The S3 repository plugin adds the repositories used for automated backups of your data.
search-guard-6 | Provide access control related features for Elasticsearch 6.
{: caption="Table 1. Available Elasticsearch plugins" caption-side="top"}

More details on the plugins is available in the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/plugins/current/index.html).

## The Search Guard Plugin

{{site.data.keyword.databases-for-elasticsearch}} uses the [Search Guard plugin](https://docs.search-guard.com/latest/index.html) to provide strong TLS encryption for both internal and external network traffic. It also provides user management features, allows you to create users through _Service Credentials_ and, use IAM integration.