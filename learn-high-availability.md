---

copyright:
  years: 2019, 2022
lastupdated: "2022-06-28"

keywords: elasticsearch ha, elasticsearch high availability, databases, ha

subcollection: databases-for-elasticsearch

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability
{: #high-availability}

{{site.data.keyword.databases-for-elasticsearch_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} environment. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-elasticsearch}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with three nodes where all three are data nodes and any node can be the primary node. Elasticsearch clusters work on a quorum system, if one data member becomes unreachable, your cluster continues to operate normally. If more nodes go down and the cluster can't maintain a quorum, it becomes read-only to protect your data. The cluster resumes normal operations when the nodes are recovered or new nodes are added.

When you add an index to Elasticsearch, it splits the data into shards and spreads those shards across the nodes in the cluster. The sharded configuration allows for Elasticsearch to run concurrent operations on your data across all the nodes. Concerning high-availability, not all shards contain a complete copy of all the data in the index. To enforce that multiple complete copies of the data are spread across the cluster in a node failure, you should ensure that when you create an index you set the replica count to at least `1`. Setting the replica count to `0` can cause data loss.

If you [add nodes to your cluster](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-horizontal-scaling), Elasticsearch automatically rebalances the shards and replicas of your indexes across the newly added node or nodes. Adding nodes can provide more stability in a multi-node failure, since you can lose more nodes and maintain a quorum. More nodes also improves performance.

## Staying Production Ready 
{: #stay-prod-ready}

To keep your {{site.data.keyword.databases-for-elasticsearch_full}} database up and running, do not drop the `icd-auth index`. This index stores your database, as well as all user accounts and is critical to operational integrity.

## Application-level High-Availability
{: #app-level-high-availability}

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Because {{site.data.keyword.databases-for-elasticsearch}} is a managed service, regular updates and database maintenance occurs as part of normal operations. These operations can occasionally cause short intervals where your database is unavailable.

Your applications must be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption is not expected. Open a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## High availability, disaster recovery, and SLA resources
{: #high-availability-da-sla}

{{site.data.keyword.databases-for-elasticsearch}} deployments conform to the {{site.data.keyword.cloud_notm}} Databases [HA, DR, and SLA](/docs/cloud-databases?topic=cloud-databases-ha-dr) information and terms.
