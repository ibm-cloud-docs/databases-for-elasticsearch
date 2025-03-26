---

copyright:
  years: 2025
lastupdated: "2025-03-26"

keywords: HA for _servicename_, DR for _servicename_, _servicename_ recovery time objective, _servicename_ recovery point objective

subcollection: content-kit

---

{{site.data.keyword.attribute-definition-list}}



# Understanding high availability and disaster recovery for _service-name_
{: #service-name-ha-dr}



[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state.
{: shortdesc}



_service-name_ is a highly available <global, regional, zonal> service designed for availability during a <regional, zonal> outage. _Service-name_ is designed to meet the [Service Level Objectives (SLO)](/docs/resiliency?topic=resiliency-slo) with the Standard plan.



For more information about the available region and data center locations, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).


## High availability architecture
{: #ha-architecture}



### High availability features
{: #ha-features}

_Service-name_ supports the following high availability features:



| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| HA Feature | Description of HA feature | Consideration information for service name |
| HA Feature | Description of HA feature | Consideration information for service name |
{: caption="HA features for _service-name_" caption-side="bottom"}


#### _HA feature 1_ for _service-name_
{: #ha-feature-1}



## Disaster recovery architecture
{: #disaster-recovery-intro}



### Disaster recovery features
{: #dr-features}

_service-name_ supports the following disaster recovery features:



| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| DR Feature | Description of DR feature | Consideration information for service name |
| DR Feature | Description of DR feature | Consideration information for service name |
{: caption="DR features for _service-name_" caption-side="bottom"}



As a customer, you can create and support the following other disaster recovery options:

| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| External source of truth | All content... created by a script. | Customer must create the script and persist the configuration where it can be used during disaster |
| Backup and restore | Backup a service instance by using customer written script. | Customer must create the script and persist the backup copy where it can be used during recovery |
| Live synchronization | Secret changes in production are automatically observed and propagated to recover service instance. | Customer must create and maintain tools. Data corruption is synchronized to the recovery instance. |
{: caption="Customer DR features for _service-name_" caption-side="bottom"}

#### _DR feature1_ for _service-name_
{: #dr-feature-1}



### Planning for DR
{: #features-for-disaster-recovery}

The DR steps must be practiced regularly. As you build your plan, consider the following failure scenarios and resolutions.



| Failure | Resolution |
| -------------- | -------------- |
| Hardware failure (single point) | (Example) {{site.data.keyword.IBM_notm}} provides a database that is resilient from single point of hardware failure within a zone. No customer configuration required. |
| Zone failure | Resolution 1 for zone failure `/n  /n` Resolution 2 for zone failure 1 |
| Data corruption | Resolution for data failure |
| Regional failure | Resolution for regional failure |
{: caption="DR scenarios for _service-name_" caption-side="bottom"}

## Your responsibilities for HA and DR
{: #feature-responsibilities}



It is your responsibility to continuously test your plan for HA and DR.

Interruptions in network connectivity and short periods of unavailability of a service might occur. It is your responsibility to make sure that application source code includes [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha) to maintain high availability of the application.
{: note}

Use the following checklists associated with each feature to help you create and practice your plan.

- _Feature name 1_
   - [ ] Verify configuration A
   - [ ] Verify availability of B

- _Feature name 2_
   - [ ] Verify configuration A
   - [ ] Verify availability of B



For more information about responsibility ownership between you and {{site.data.keyword.cloud_notm}} for _service-name_, see [link to your products Shared responsibilities topic_]().

## Recovery time objective (RTO) and recovery point objective (RPO)
{: #rto-rpo-features}



| Feature | RTO and RPO |
| -------------- | -------------- |
| Feature | RTO = x, RPO = x |
| Feature | RTO = x, RPO = x |
| Feature | RTO = x, RPO = x  |
{: caption="RTO/RPO features for _service-name_" caption-side="bottom"}

## Change management
{: #change-management}



Change management includes tasks such as upgrades, configuration changes, and deletion.

Grant users and processes the IAM roles and actions with the least privilege that is required for their work. For more information, see [How can I prevent accidental deletion of services?](/docs/resiliency?topic=resiliency-dr-faq#prevent-accidental-deletion).
{: tip}



Consider creating a manual backup before you upgrade to a new version of _service-name_.

## How {{site.data.keyword.IBM}} helps ensure disaster recovery
{: #ibm-disaster-recovery}

{{site.data.keyword.IBM}} takes specific recovery actions for _service-name_ if a disaster occurs.

### How {{site.data.keyword.IBM_notm}} recovers from zone failures
{: #ibm-zone-failure}



### How {{site.data.keyword.IBM_notm}} recovers from regional failures
{: #ibm-regional-failure}





If {{site.data.keyword.IBM_notm}} can’t restore the service instance, you must restore the service as described in the [Disaster recovery architecture](#disaster-recovery-intro).

## How {{site.data.keyword.IBM_notm}} maintains services
{: #ibm-service-maintenance}



All upgrades follow {{site.data.keyword.IBM_notm}} service best practices, including recovery plans and rollback processes. Regular maintenance might cause short interruptions, mitigated by [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha). Changes are rolled out sequentially, region by region, and zone by zone within a region. {{site.data.keyword.IBM_notm}} reverts updates at the first sign of a defect.



Complex changes are enabled and disabled with feature flags to control exposure.



Changes that impact customer workloads are detailed in {{site.data.keyword.cloud_notm}} notifications. For more information about planned maintenance, announcements, and release notes that impact this service, see [Monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status).
