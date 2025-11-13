---

copyright:
  years: 2025
lastupdated: "2025-11-13"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, elasticsearch


subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery for {{site.data.keyword.databases-for-elasticsearch}}
{: #elasticsearch-ha-dr}

[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state.
{: shortdesc}

{{site.data.keyword.databases-for-elasticsearch}} is a regional service that fulfills the defined [Service Level Objectives (SLO)](/docs/resiliency?topic=resiliency-slo) with the Standard plan.

For more information, see [Service Level Agreement (SLA)](https://www.ibm.com/support/customer/csol/terms/?id=i126-9268&lc=en). For more information about the available {{site.data.keyword.cloud_notm}} regions and data centers for {{site.data.keyword.databases-for-elasticsearch}}, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

## High availability architecture
{: #ha-architecture}

![Architecture](/images/Elasticsearch_high_availability.svg){: caption="Elasticsearch architecture" caption-side="bottom"}

{{site.data.keyword.databases-for-elasticsearch}} provides replication, failover, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and some failures. Deployments contain a cluster with three data members. Elasticsearch uses indexes to store data and each index has a primary shard and a replica shard. Elasticsearch utilizes a data replication model based on the primary-backup model. The primary shard serves as the main entry point for indexing operations, while the other copies are known as replica shards. Asynchronous replication is employed to keep the replica shards up to date. Elasticsearch ensures cluster state stability and facilitates failover. In the event that the master node becomes unavailable, a new master is elected and the replica shards on the new master get promoted to master. The leader and replica shards are geographically distributed across different zones within the cluster to mitigate the risk of simultaneous failures.

You can extend high availability further by adding more replicas to the indexes however, this comes with additional storage costs that should be taken into consideration.

Review the Elasticsearch documentation on [replication techniques](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-replication.html){: .external} to understand the constraints and tradeoffs that are associated with the asynchronous replication strategy that is deployed by default.

### High availability features
{: #ha-features}

{{site.data.keyword.databases-for-elasticsearch}} supports the following high availability features:

| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Automatic failover| Standard on all clusters and resilient against a zone or single member failure. | |
| Member count | Minimum 3 members. Default is a Standard three deployment. | |
| Horizontal scaling | It is possible to scale your {{site.data.keyword.cloud_notm}} {{site.data.keyword.databases-for-elasticsearch}} deployment horizontally by adding more Elasticsearch nodes (also referred to as members). Adding more nodes increases capacity and reliability. |  |
{: caption="High availability features" caption-side="top"}

## Disaster recovery architecture
{: #disaster-recovery-intro}



### Disaster recovery features
{: #dr-features}

![Architecture](/images/Elasticsearch_disaster-recovery.svg){: caption="Elasticsearch architecture" caption-side="bottom"}

{{site.data.keyword.databases-for-elasticsearch}} supports the following disaster recovery features:


| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Backup restore | Create a database from previously created backup. For more information, see [Managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups). | New connection strings for the restored database must be referenced throughout the workload. |
| Horizontal scaling | It is possible to scale your {{site.data.keyword.cloud_notm}} {{site.data.keyword.databases-for-elasticsearch}} deployment horizontally by adding more Elasticsearch nodes (also referred to as members). Adding more nodes will make your database more resilient to multiple zone failures. | |
|Snapshots | You can store snapshots in a snapshot repository that is accessible from multiple regions. However, snapshots are not real-time and are typically used for backup and restore purposes rather than immediate failover. In case of a failure, you must restore from a snapshot manually. | |
{: caption="Disaster recovery features" caption-side="top"}

### Planning for DR
{: #features-for-disaster-recovery}

The disaster recovery steps must be practiced regularly. As you build your plan, consider the following failure scenarios and resolutions.

| Failure | Resolution |
| -------------- | -------------- |
| Hardware failure (single point) | IBM provides a database that is resilient from a single point of hardware failure within a zone - no configuration is required. |
| Zone failure | Automatic failover. The database members are distributed between zones. Configuring three members provides additional resiliency to multiple zone failures.|
| Data corruption | Backup restore. Use the restored database in production or for source data to correct the corruption in the restored database.|
| Regional failure | Backup restore. Use the restored database in production. |
{: caption="Failure scenarios and resolutions" caption-side="top"}

## Application-level high availability
{: #application-level-ha}

Applications that communicate over networks and cloud services are subject to transient connection failures. Design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

As {{site.data.keyword.databases-for-elasticsearch}} is a managed service, regular updates and database maintenance occur as part of normal operations. This can occasionally cause short intervals where your database is unavailable. It can also cause the database to trigger a graceful failover, retry, and reconnect. It takes a short time for the database to determine which member is a replica and which is the leader, so you might also see a short connection interruption. Failovers generally take less than 30 seconds.

Your applications must be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption are not expected. Open a [support case](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have periods longer than a minute with no connectivity so we can investigate.

## Connection limits
{: #connection-limits-ha}

{{site.data.keyword.databases-for-elasticsearch}} does not have any restrictions on the number of connections you open to the database as it uses REST API to interact with the database.  Elasticsearch’s default settings provide a good out-of-box experience for basic operations, such as full text search, highlighting, aggregations, and indexing.

If you want more performance out of our database, see the [Optimize Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/how-to.html){: .external} page for more information.

## Your responsibilities for HA and DR
{: #feature-responsibilities}

The following information can help you create and continuously practice your plan for HA and DR.

When restoring a database from backups or using point-in-time restore, a new database is created with new connection strings. Existing workloads and processes must be adjusted to consume the new connection strings.

A recovered database may also need the same customer-created dependencies of the disaster database. Ensure that these and other services exist in the recovered region:

- {{site.data.keyword.keymanagementservicefull}}
- {{site.data.keyword.hscrypto}}

Remember that deleting a database also deletes its associated backups. However, deleted databases may be recoverable within a limited timeframe. For more information, see the [Backup FAQ documentation](/docs/cloud-databases?topic=cloud-databases-faq-backups) for specific details on database recovery procedures.

It is not possible to copy backups off the {{site.data.keyword.cloud_notm}}, so consider using the database-specific tools for additional backups. It may be required to recover from malicious database deletion followed by a reclamation-delete of a database. Careful management of IAM access to databases can help reduce exposure to this problem.

The following checklist associated with each feature can help you create and practice your plan.

- Backup restore
   - Verify backups are available at the desired frequency to meet RPO requirements. [Managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups) documents backup frequency.
   - There are some restrictions on database restore regions - verify your restore goals can be achieved by reading [managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups).
   - Verify the retention period of the backups meet your requirements.
   - Schedule test restores regularly to verify that the actual restored times meet the defined RTO. Remember that database size significantly impacts restore time. Please consider strategies to minimize restore times, such as breaking down large databases into smaller, more manageable units and purging unused data.
   - Verify the Key Protect service.

To find out more about responsibility ownership between the customer and {{site.data.keyword.cloud_notm}} for using {{site.data.keyword.databases-for-elasticsearch}}, see [Shared responsibilities for {{site.data.keyword.databases-for}}](/docs/cloud-databases?topic=cloud-databases-responsibilities-cloud-databases).

 ## Stay informed: {{site.data.keyword.IBM_notm}} notifications
{: #ibm-service-notifications}

Updates affecting customer workloads are communicated through {{site.data.keyword.cloud_notm}} notifications. To stay informed about planned maintenance, announcements, and release notes related to this service, refer to the [Monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) page. In addition, regularly review the [Version policy](/docs/cloud-databases?topic=cloud-databases-versioning-policy) page for the latest updates on End-of-Life versions and dates.

## Additional guidance
{: #ha_dr-guidance}

- [Understanding high availability for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-ha-dr)
- [Understanding business continuity and disaster recovery for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-bc-dr)
- [Horizontal scaling](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-horizontal-scaling&interface=ui)
