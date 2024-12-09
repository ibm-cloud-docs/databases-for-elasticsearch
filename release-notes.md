---

copyright:
  years: 2018, 2024
lastupdated: "2024-12-09"

keywords: databases-for-elasticsearch release notes

subcollection: databases-for-elasticsearch

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes
{: #elasticsearch-relnotes}

Use these release notes to learn about the latest updates to {{site.data.keyword.databases-for-elasticsearch_full}} that are grouped by _date_ or _build number_.
{: shortdesc}

## 15 November 2024
{: #databases-for-elasticsearch-15nov2024}
{: release-note}

{{site.data.keyword.databases-for}} logs and events are now available on {{site.data.keyword.logs_full}}
: {{site.data.keyword.databases-for}} has onboarded {{site.data.keyword.logs_full_notm}}, a scalable logging service that persists logs and provides users with capabilities for querying, tailing, and visualizing logs. Customers are expected to use {{site.data.keyword.logs_full_notm}} to review their database logs and events starting **November 15, 2024**. For more information, see [Set up logging and monitoring](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started-cdb-logging-monitoring) and [About IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-about-cl).

## 16 September 2024
{: #databases-for-elasticsearch-16sept2024}
{: release-note}

Private endpoints as new default
:  To ensure best possible security for your databases, private endpoints are now the default in the {{site.data.keyword.cloud}} console. CLI and Terraform now require the endpoint type to be provided as part of creating an instance.

## 1 May 2024
{: #databases-for-elasticsearch-01may2024}
{: release-note}

New hosting models
:  You can choose between two hosting models: Isolated Compute and Shared Compute. Isolated Compute is a secure single-tenant offering for complex, highly performant enterprise workloads. Shared Compute is a flexible multi-tenant offering for dynamic, fine-tuned, and decoupled capacity selections. For more information, see [Hosting models](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-hosting-models&interface=ui){: external}.

## 12 April 2024
{: #databases-for-elasticsearch-12apr2024}
{: release-note}

Configuring the Index Lifecycle Management capabilities of Databases for Elasticsearch tutorial published
:  This tutorial is designed to assist you to proactively manage your indices to make efficient use of resources, both in terms of storage and search capabilities. For more information, see [Configuring the Index Lifecycle Management capabilities of Databases for Elasticsearch](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-configuring-the-index-lifecycle-management-capabilities-of-databases-for-elasticsearch){: external}.

## 06 February 2024
{: #databases-for-elasticsearch-06feb2024}
{: release-note}

Build an Elasticsearch chatbot tutorial published
:  This tutorial is designed to assist you in harnessing the full potential of your data by transforming it into an intelligent bot capable of answering queries related to your data. The bot effortlessly stores the knowledge base in an Elasticsearch index and taps into AI to intelligently handle your questions related to the knowledge base. For more information, see [Build an Elasticsearch chatbot](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-build-es-chatbot){: external}.

## 17 January 2024
{: #databases-for-elasticsearch-17jan2024}
{: release-note}

Elasticsearch machine learning tutorials documentation published
:  New tutorials created to highlight the ML features of Elasticsearch. For more information, see [Use Elasticsearch vector search capabilities](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-vector-search-elasticsearch){: external}, [Use ELSER, Elastic's Natural Language Processing model](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-elser-embeddings-elasticsearch){: external}, and [Use machine learning models with Elasticsearch to tag content](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-nlp-ml-tutorial).

## 03 January 2024
{: #databases-for-elasticsearch-03jan2024}
{: release-note}

Elasticsearch, machine learning, and AI
:  Elasticsearch, a robust search-capable database, is well-suited to seamlessly incorporate essential ML features sought by enterprises embracing new technologies. For more information, see [Elasticsearch, machine learning, and AI](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-es-ml-ai){: external}.

## 22 November 2023
{: #databases-for-elasticsearch-22nov2023}
{: release-note}

Monitoring Integration documentation updated
:  Monitoring Integration documentation now lists metrics for all {{site.data.keyword.databases-for}} services. For more information, see [Monitoring Integration](/docs/cloud-databases?topic=cloud-databases-monitoring){: external}.

## 15 November 2023
{: #databases-for-elasticsearch-15nov2023}
{: release-note}

{{site.data.keyword.databases-for-elasticsearch}} Platinum Plan released
:  {{site.data.keyword.databases-for-elasticsearch}} Platinum Plan offers all the features of the Enterprise Plan, plus a richer array of functionality around integrations (connectors and web crawlers), security features (logging and access control), and document-level security, among others. For more information, see [{{site.data.keyword.databases-for-elasticsearch}} Plans](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-elastic-offerings){: external}.

## 26 October 2023
{: #databases-for-elasticsearch-26oct2023}
{: release-note}

Deploy Kibana using Code Engine and connect to your {{site.data.keyword.databases-for-elasticsearch}} instance
:  With this tutorial, deploy Kibana using Code Engine and connect to your Databases for Elasticsearch instance. Kibana is a web interface that allows you to visualize the data in Elasticsearch instances. Code Engine is a fully managed, serverless platform that allows you to run workloads without worrying about deploying infrastructure. Elasticsearch is a NoSQL database with powerful search capabilities. For more information, see [Deploy Kibana using Code Engine and connect to your {{site.data.keyword.databases-for-elasticsearch}} instance](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-kibana-code-engine-icd-elasticsearch){: external}.

## 12 October 2023
{: #databases-for-elasticsearch-12oct2023}
{: release-note}

{{site.data.keyword.databases-for-elasticsearch}} v7.17 end of life
:  End of life announcement: Version 7.17 reaches end of life 26 April 2024. All {{site.data.keyword.databases-for-elasticsearch}} instances on deprecated versions that are still active will be upgraded in-place to the next major version. We recommend that you upgrade following our [backup and restore process](/docs/cloud-databases?topic=cloud-databases-dashboard-backups){: external} before the EOL date of your version.

For more information, see [Upgrading to a new Major Version](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-upgrading){: external}.

## 02 October 2023
{: #databases-for-elasticsearch-02oct2023}
{: release-note}

Elasticsearch end of life for versions 7.9 and 7.10
:  Action is required before 30 November 2023 for your {{site.data.keyword.databases-for-elasticsearch_full}} version 7.9 and 7.10 deployments. On 30 November 2023, these versions will be end of life. Our recommended procedure for this is restoring from a backup. For more information, see [Managing {{site.data.keyword.databases-for}} backups](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-dashboard-backups).

## 18 September 2023
{: #databases-for-elasticsearch-18sept2023}
{: release-note}

Configuring an Enterprise Search 7.17 server with an {{site.data.keyword.databases-for-elasticsearch}} instance
:  Enterprise Search extends the capabilities of {{site.data.keyword.databases-for-elasticsearch}} to provide a unified search experience across various data sources, including documents, emails, databases, and more. It offers a seamless interface for users to find relevant information across disparate data silos, enhancing productivity and collaboration. By integrating Enterprise Search with your {{site.data.keyword.databases-for-elasticsearch}} instance, you gain a comprehensive search solution that uses the strengths of both platforms to efficiently discover insights from your data. For more information, see [Configuring an Enterprise Search 7.17 server with an {{site.data.keyword.databases-for-elasticsearch}} instance](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-tutorial-elasticsearch-enterprise-search-tutorial).

## 22 June 2023
{: #databases-for-elasticsearch-22june2023}
{: release-note}

Elasticsearch 8.7.0 (preview)
:  For more information, see [{{site.data.keyword.databases-for-elasticsearch_full}} Enterprise Plan](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-pricing#elastic-enterprise-pricing).

## 18 May 2023
{: #databases-for-elasticsearch-18may2023}
{: release-note}

Setting up disk alerts for disk utilization tutorial
:  In this tutorial, you use the {{site.data.keyword.cloud_notm}} API and the [{{site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started){: external} to set up an alert that emails you whenever the disk utilization of your database exceeds 90%. This specific example creates an alert on a {{site.data.keyword.databases-for-elasticsearch}} deployment, but it is applicable to all the databases in the IBM {{site.data.keyword.databases-for}} catalog. For more information, see [Setting up disk alerts for disk utilization](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-disk-util-alert-tutorial).

## 03 May 2023
{: #databases-for-elasticsearch-03may2023}
{: release-note}

{{site.data.keyword.databases-for-elasticsearch_full}} Enterprise Plan
:  Our strategic partnership with [Elastic](https://www.elastic.co/about/){: external} since January 2023 means that we are able to offer more and richer functionality, as well as world-class levels of support. {{site.data.keyword.databases-for-elasticsearch_full}} Enterprise Plan Version 7.17 is the current supported version of {{site.data.keyword.databases-for-elasticsearch}} and is offered under our Enterprise Plan. This version includes functionality that is not available in our Standard plan. {{site.data.keyword.databases-for-elasticsearch_full}} Standard Plan Versions 7.9 and 7.10 are available on {{site.data.keyword.databases-for}} under the Standard Plan, but will be End of Life (EOL) by 30 November 2023. For more information, see [{{site.data.keyword.databases-for-elasticsearch}} Pricing](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-pricing).

## 19 October 2022
{: #databases-for-elasticsearch-19oct2022}
{: release-note}

Deploying and Connecting a {{site.data.keyword.databases-for}} Instance Tutorial
:  This tutorial guides you through the process of deploying a {{site.data.keyword.databases-for}} instance and connecting it to a web front end by creating a webpage that allows visitors to input a word and its definition. These values are then stored in a database running on {{site.data.keyword.databases-for}}. You install the database infrastructure by using Terraform and your web application uses the popular Express framework. The application can then be run locally, or by using Docker. For more information, see [Deploying and Connecting a {{site.data.keyword.databases-for}} Instance](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-create-instance-tutorial&interface=ui).

## 11 October 2022
{: #databases-for-elasticsearch-11oct2022}
{: release-note}

Protecting {{site.data.keyword.databases-for-elasticsearch_full}} resources with context-based restrictions
:  Context-based restrictions (CBR) give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud}} resources based on the context of access requests. Access to {{site.data.keyword.databases-for}} resources can be controlled with CBR and identity and access management (IAM) policies. For more information, see [Protecting {{site.data.keyword.databases-for}} resources with context-based restrictions](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-cbr&interface=ui).

## 25 March 2022
{: #databases-for-elasticsearch-25mar2022}
{: release-note}

{{site.data.keyword.databases-for-elasticsearch_full}} Security Goals Updated
:  Available security and compliance goals updated. See updated goals [here](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-manage-security-compliance&interface=ui).

## 08 December 2020
{: #databases-for-elasticsearch-08dec2020}
{: release-note}

{{site.data.keyword.databases-for-elasticsearch_full}} 6 End of Life
:  On April 29, 2021, all {{site.data.keyword.databases-for-elasticsearch_full}} instances on version 6.x that are still active will be disabled. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/databases-for-elasticsearch-6-end-of-life).

## 09 November 2020
{: #databases-for-elasticsearch-09nov2020}
{: release-note}

{{site.data.keyword.databases-for-elasticsearch_full}} v7.9 is now available
:  We are pleased to announce the release of version 7.9 on {{site.data.keyword.databases-for-elasticsearch_full}}. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/databases-for-elasticsearch-v7-9-is-now-available).

## 13 April 2020
{: #databases-for-elasticsearch-13apr2020}
{: release-note}

{{site.data.keyword.databases-for-elasticsearch_full}} autoscaling
:  We are excited to announce that autoscaling of your deployments based on disk capacity and disk I/O utilization is now available for {{site.data.keyword.databases-for-elasticsearch_full}} via the UI, API, and CLI. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-databases-portfolio-introduces-autoscaling).

## 20 February 2020
{: #databases-for-elasticsearch-20feb2020}
{: release-note}

{{site.data.keyword.databases-for-elasticsearch_full}} customers are now able to scale their {{site.data.keyword.databases-for-elasticsearch_full}} instances up to 20 members.
:  This new feature allows Elasticsearch instances to now grow to 2.2 TB RAM, 80 TB of Disk, and 560 vCPUs. For more information, see [Databases for Elasticsearch Introduces Horizontal Scaling](https://www.ibm.com/cloud/blog/announcements/databases-for-elasticsearch-introduces-horizontal-scaling).

## 6 August 2019
{: #databases-for-elasticsearch-06aug2019}
{: release-note}

New Regions Available for IBM Cloud Database Services
:  {{site.data.keyword.databases-for-elasticsearch_full}} is now available to be deployed in Seoul; South Korea; and Chennai, India. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/new-regions-available-for-ibm-cloud-database-services).

## 19 December 2018
{: #databases-for-elasticsearch-19dec2018}
{: release-note}

General Availability of {{site.data.keyword.databases-for-elasticsearch_full}}
:  {{site.data.keyword.databases-for-elasticsearch_full}} added to the [{{site.data.keyword.databases-for}}](https://www.ibm.com/cloud/databases) family. See blog post announcement [here](https://www.ibm.com/cloud/blog/ibm-cloud-databases-for-etcd-elasticsearch-and-messages-for-rabbitmq-are-now-generally-available).
