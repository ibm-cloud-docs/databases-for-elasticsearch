---
copyright:
  years: 2019, 2022
lastupdated: "2022-07-25"

keywords: elasticsearch plug-in, elasticsearch plugins

subcollection: databases-for-elasticsearch

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Elasticsearch plug-ins
{: #plugins}

Elasticsearch supports many plug-ins to extend its core functions. {{site.data.keyword.databases-for-elasticsearch_full}} comes with a selection of plug-ins that already enabled on your deployment. Plug-in management is handled by the service, and users cannot install, uninstall, enable, or disable plug-ins.

You can check the plug-ins that are installed in your Elasticsearch cluster by using the `/_nodes/plugins` cluster API endpoint. Example, 
```sh
CURL_CA_BUNDLE=certificate.crt curl -u ibm_cloud_es_user:password https://f1b5e4a9-7179-4af2-a795-b7a0d71ec80a.974550db55eb4ec0983f023940bf637f.databases.appdomain.cloud:30909/_nodes/plugins
```

## Available plug-ins
{: #avail-plugins}

| Name | Description |
| ------- | ------- |
| `analysis-icu` | The ICU Analysis plug-in integrates the Lucene ICU module into Elasticsearch, adding ICU-related analysis components. |
| `analysis-kuromoji` | The Japanese (kuromoji) Analysis plug-in integrates the Lucene kuromoji analysis module into Elasticsearch. |
| `analysis-nori` | The Korean (nori) Analysis plug-in integrates the Lucene nori analysis module into Elasticsearch. |
| `analysis-phonetic` | The Phonetic Analysis plug-in integrates a phonetic token filter analysis with Elasticsearch. |
| `analysis-smartcn` | Smart Chinese Analysis plug-in integrates the Lucene Smart Chinese analysis module into Elasticsearch. |
| `analysis-stempel` | The Stempel (Polish) Analysis plug-in integrates the Lucene stempel (polish) analysis module into Elasticsearch. |
| `analysis-ukrainian` | The Ukrainian Analysis plug-in integrates the Lucene UkrainianMorfologikAnalyzer into Elasticsearch. |
| `ingest-attachment` | An ingest processor that uses Apache Tika to extract contents. |
| `mapper-size` | The Mapper Size plug-in allows document to record their uncompressed size at index time. |
| `repository-s3` | The S3 repository plug-in adds the repositories that are used for automated backups of your data. |
{: caption="Available Elasticsearch plugins" caption-side="top"}

More details on the plug-ins are available in the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/plugins/current/index.html).
