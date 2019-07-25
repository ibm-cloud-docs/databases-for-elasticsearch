---
Copyright:
  years: 2018, 2019
lastupdated: "2019-02-13"

keywords: elasticsearch, databases

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Security and Compliance
{: #security-compliance}

## Protection Against Unauthorized Access

{{site.data.keyword.databases-for-elasticsearch_full}} use the following methods to protect data in transit or in storage.
- All {{site.data.keyword.databases-for-elasticsearch}} connections use TLS/SSL encryption for data in transit. The current supported version of this encryption is TLS 1.2.
- Access to the Account, Management Console UI, and API is secured via [Identity and Access Management (IAM)](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-iam).
- Access to the database is secured through the standard access controls provided by the database. These access controls are configured to require valid database-level credentials that are obtainable only through prior access to the database or through our Management Console UI or API.
- All {{site.data.keyword.databases-for-etcd}} storage is provided on storage encrypted with [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-about). Bring-your-own-key (BYOK) for encryption is also available through [Key Protect Integration](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-key-protect).
- IP Whitelisting - All deployments support [whitelisting IP addresses](/docs/services/databases-for-elasticsearch?topic=cloud-databases-whitelisting) to restrict access to the service.
- Public and Private Networking - {{site.data.keyword.databases-for-etcd}} is integrated with [Service Endpoints](/docs/services/databases-for-elasticsearch?topic=cloud-databases-service-endpoints). You can select whether to use connections over the public network, the {{site.data.keyword.cloud_notm}} internal network, or both.

## Data Resilience

- [Backups](/docs/services/databases-for-elasticsearch?topic=cloud-databases-dashboard-backups) are included in the service. {{site.data.keyword.databases-for-elasticsearch}} backups reside in [{{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-about-ibm-cloud-object-storage) and are also [encrypted](/docs/services/cloud-object-storage?topic=cloud-object-storage-security).
- {{site.data.keyword.databases-for-elasticsearch}} deployments are configured with replication. Deployments contain a cluster with three nodes where all three are data nodes and any node can be the master node. 
- If you deploy to an {{site.data.keyword.cloud_notm}} Single-Zone Region (SZR), each database node resides on a different host in the datacenter. 
- If you deploy to an {{site.data.keyword.cloud_notm}} Multi-Zone Region (MZR), the nodes are spread over the region's availability zone locations.

## General Data Protection Regulation (GDPR) 

If you have an account with IBM Cloud, your personal data is held by {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.IBM_notm}} Data Processing Addendum (Addendum) applies to the processing of client's personal data by {{site.data.keyword.IBM_notm}} on behalf of client in order to provide {{site.data.keyword.IBM_notm}} standard services.  
[IBM DPA](https://www.ibm.com/support/customer/zz/en/dpa.html){:new_window}

{{site.data.keyword.databases-for-elasticsearch}} processes limited client Personal Information (PI) in the course of running the service and optimizing the user experience. 

{{site.data.keyword.databases-for-elasticsearch}} provides a [Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=B9C96C207A5E11E89D57EFEED3CB8BE9){:new_window} with its policies as a Data Processor regarding content and data protection. 

## HIPAA

{{site.data.keyword.databases-for-elasticsearch}} meets the required {{site.data.keyword.IBM_notm}} controls that are commensurate with the Health Insurance Portability and Accountability Act of 1996 (HIPAA) Security and Privacy Rule requirements. These requirements include the appropriate administrative, physical, and technical safeguards required of Business Associates in 45 CFR Part 160 and Subparts A and C of Part 164. HIPAA must be requested at the time of provisioning and requires a representative to sign a Business Associate Addendum (BAA) agreement with {{site.data.keyword.IBM_notm}}.

## Terms

- [The IBM Privacy Policy](https://www.ibm.com/privacy/us/en/)
- [The IBM Cloud Notices and Terms of Use](/docs/overview/terms-of-use?topic=overview-terms)


