---

copyright:
  years: 2020, 2025
lastupdated: "2025-06-05"

keywords: troubleshooting for Elasticsearch

subcollection: databases-for-elasticsearch

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can’t I connect to my Elasticsearch deployment?
{: #troubleshoot-connect}
{: troubleshoot}
{: support}

If you encounter errors when connecting to your {{site.data.keyword.databases-for-elasticsearch_full}} deployment, review these common causes and resolutions.
{: #shortdesc}

You receive an error message or fail to connect to a {{site.data.keyword.databases-for-elasticsearch}} deployment.  If reviewing application logs, you might see errors that mention intermittent connection timeouts or unable to connect.
{: #tsSymptoms}

Review the following information to troubleshoot and resolve common connectivity problems:
{: #tsResolve}

* An unsecured connection is a common cause of connectivity errors.  All {{site.data.keyword.databases-for-elasticsearch}} connections use TLS/SSL encryption; {{site.data.keyword.databases-for-elasticsearch}} rejects unsecured connections.  To avoid errors, make sure you configured a secure connection.  Refer to [Getting started](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started) for an example of a secure connection.
* If you are using a private endpoint, make sure that you specify [connection strings that contain the private endpoint](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-service-endpoints&interface=ui#private-endpoints-credentials) and that you followed the steps in [Connecting through Private Endpoints](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-service-endpoints&interface=ui#private-endpoint-connections).
* If your application log captures a short connection interruption, that behavior is expected as a normal part of operations for this managed service. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to IBM Cloud. However, if you experience several minutes of connection interruption check the Cloud Status for the service. For more information, see [Application-level high availability](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-elasticsearch-ha-dr#application-level-ha).
