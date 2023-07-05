---
copyright:
  years: 2019, 2023
lastupdated: "2023-07-05"

keywords: elasticsearch pricing, backup pricing, elasticsearch enterprise, elasticsearch standard

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Pricing
{: #pricing}

Our strategic partnership with [Elastic](https://www.elastic.co/about/){: external} since January 2023 means that we are able to offer more and richer functionality, as well as world-class levels of support.

All {{site.data.keyword.databases-for-elasticsearch}} plans deploy as one highly available Elasticsearch cluster with three data members. Your data is replicated across members. Plans are priced based on the total amount of disk storage, RAM, dedicated cores, and backup storage that is allocated to deployments, prorated hourly. {{site.data.keyword.databases-for-elasticsearch}} deployments have a minimum of 5 GB of disk and 1 GB of RAM per data member.

## {{site.data.keyword.databases-for-elasticsearch_full}} Enterprise Plan
{: #elastic-enterprise-pricing}

Versions 7.17 (preferred) and 8.7 (preview) are the current supported versions of {{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan. This plan includes functionality that is not available in our Standard Plan. These features were previously offered by Elastic as paid-for add-ons under the X-Pack label, such as [Role Based Access Control (RBAC)](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/es-security-principles.html#security-create-appropriate-users){: external} and [Index Lifecycle Management (ILM)](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/index-lifecycle-management.html){: external}.
Once deployed, {{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan clusters make full use of other Elastic Stack components such as [Kibana](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started#kibana), [Logstash](https://www.elastic.co/logstash/){: external}, and [Beats](https://www.elastic.co/beats/){: external}.

## {{site.data.keyword.databases-for-elasticsearch_full}} Standard Plan
{: #elastic-standard-pricing}

Versions 7.9 and 7.10 are available on {{site.data.keyword.databases-for}} under the Standard Plan, but will be End of Life (EOL) by 30 November 2023. To ensure access to the latest features and a longer timescale of version support, create new {{site.data.keyword.databases-for-elasticsearch}} deployments under the Enterprise Plan.

## Using the Pricing Calculator
{: #pricing-calc}

Prices for the different plans reflect different levels of available functionality. Be aware that prices for the Standard Plan will increase starting July 1 to bring them into line with the prices for the Enterprise Plan. Templates are provided for ease of use and provide balanced resource allocations appropriate for general-purpose workloads. The **Custom** tab can be used to configure Disk, RAM, and vCPU, as wanted.

For pricing estimation, use the **Add to Estimate** button on the [{{site.data.keyword.databases-for-elasticsearch}} catalog page](https://cloud.ibm.com/catalog/databases-for-elasticsearch). Input your total consumption across three data members into the calculator. For example, 5 GB of disk and 1 GB of RAM across three data members would be priced at 15 GB of disk and 3 GB of RAM.

## Dedicated Cores Pricing
{: #pricing-cores}

You have the option of selecting the CPU allocation for your deployment. With dedicated cores, your resource group is given a single-tenant host with a guaranteed minimum reserve of cpu shares. Your deployments are then allocated the number of CPUs you specify. The cost of dedicated cores is $30 per core per month, and each member gets the selected number of cores. For example, if you provision a deployment with 3 dedicated cores per member, that is a total of 9 cores and is billed at $270 per month. 

Dedicated cores are an optional feature. The default `Shared CPU` setting provisions your deployment on hosts with shared compute resources and incurs no additional charge.

## Backups Pricing
{: #pricing-backup}

You receive your total disk space purchased, per database, in free backup storage. For example, in a given month, if your {{site.data.keyword.databases-for-elasticsearch}} deployment has 20 GB of disk per member and three data members, you receive 60 GB of backup storage free for that month. If your backup storage utilization is greater than 60 GB for the month (in this scenario), you are charged an overage of $0.03/month per gigabyte. 

By default, {{site.data.keyword.databases-for}} provides a daily backup that is stored for 30 days. These backups, and any on-demand backups you make, all count toward the above allocation.

In the above example, if your database contains 2 GB of data and you have not taken any on-demand backups, then your total backup size is 2 GB x 30 = 60 GB. Your backup costs are nil.

If your database contains 15 GB of data and you have not taken any on-demand backups, then your total backup size is 15 GB x 30 = 450 GB. In this scenario, your backup costs are (450 GB - 60 GB) * 0.03 = $11.7 per month.

Most deployments will not ever go over the allotted credit.

## Scaling per Member
{: #pricing-member}

{{site.data.keyword.databases-for-elasticsearch}} deployments have minimum and maximum allocation for disk and RAM as shown. Scaling deployments through the API/CLI provides more granularity and also allows a user to scale a database instance up to 4 TB of disk per member.

| Resource | Minimum | Maximum | Scaling Granularity (API/CLI) |
| ---------- | ----- | ----- | ------- |
| Disk | 5 GB per member | 4 TB per member | 1024 MB per member |
| RAM | 1 GB per member | 112 GB per member | 128 MB per member |
| CPU (if enabled) | 3 CPUs per member | 28 CPUs per member| 1 CPU per member |
{: caption="Table 1. Per Member Scaling Limits" caption-side="top"}
