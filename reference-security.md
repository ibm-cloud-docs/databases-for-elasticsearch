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

# Security and Compliance


## Protection Against Unauthorized Access

{{site.data.keyword.databases-for-elasticsearch_full}} use the following methods to protect data in transit or in storage.
- All {{site.data.keyword.databases-for-elasticsearch}} connections use TLS/SSL encryption for data in transit. The current supported version of this encryption is TLS 1.2.
- Access to the Account, Management Console UI, and API is secured via IAM (Identity and Access Management).
- Access to the database is secured through the standard access controls provided by the database. These access controls are configured to require valid database-level credentials that are obtainable only through prior access to the database or through our Management Console UI or API.
- All {{site.data.keyword.databases-for-elasticsearch}} storage is provided on encrypted volumes that use the Linux Unified Keys Setup (LUKS). If you require bring-your-own-key (BYOK) for encryption, it is available through [{{site.data.keyword.cloud_notm}} Key Protect](https://{DomainName}/docs/services/key-protect/about.html#about). 
- IP Whitelisting - All deployments support whitelisting IP addresses to restrict access to the service.

## Data Resilience

- Backups are included in the service. Backups reside in {{site.data.keyword.cloud_notm}} Object Storage and are also encrypted.
- {{site.data.keyword.databases-for-elasticsearch}} deployments are configured with replication, so the data exists with multiple copies and each copy resides on a different cluster and host. Where available, those clusters are also spread in different availability zones within the region where the service is deployed.
 
## SOC 2 Type 1 Certification

{{site.data.keyword.IBM_notm}} provides a Service Organization Controls (SOC) 2 Type 1 report for {{site.data.keyword.databases-for-elasticsearch}}. The reports evaluate IBM's operational controls according to the criteria set by the American Institute of Certified Public Accountants (AICPA) Trust Services Principles. The Trust Services Principles define adequate control systems and establish industry standards for service providers such as IBM Cloud to safeguard their customers' data and information.

You can request an SOC 2 Type 1 report from the customer portal or contact your sales representative. Alternatively, you can open a support ticket with [IBM Cloud support](https://watson.service-now.com/wcp){:new_window}

## General Data Protection Regulation (GDPR) 

If you have an account with IBM Cloud, your personal data is held by {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.IBM_notm}} Data Processing Addendum (Addendum) applies to the processing of client's personal data by {{site.data.keyword.IBM_notm}} on behalf of client in order to provide {{site.data.keyword.IBM_notm}} standard services.  
[IBM DPA](https://www.ibm.com/support/customer/zz/en/dpa.html){:new_window}

{{site.data.keyword.databases-for-elasticsearch}} processes limited client Personal Information (PI) in the course of running the service and optimizing the user experience. 

{{site.data.keyword.databases-for-elasticsearch}} provides a [Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=B9C96C207A5E11E89D57EFEED3CB8BE9){:new_window} with its policies as a Data Processor regarding content and data protection. 

## HIPAA

{{site.data.keyword.databases-for-elasticsearch}} meets the required {{site.data.keyword.IBM_notm}} controls that are commensurate with the Health Insurance Portability and Accountability Act of 1996 (HIPAA) Security and Privacy Rule requirements. These requirements include the appropriate administrative, physical, and technical safeguards required of Business Associates in 45 CFR Part 160 and Subparts A and C of Part 164. HIPAA must be requested at the time of provisioning and requires a representative to sign a Business Associate Addendum (BAA) agreement with {{site.data.keyword.IBM_notm}}.

## Terms

- [The IBM Privacy Policy](https://www.ibm.com/privacy/us/en/)
- [The IBM Cloud Notices and Terms of Use](https://{DomainName}/docs/overview/terms-of-use/notices.html#notices)


