---

copyright:
  years: 2018, 2025
lastupdated: "2025-05-14"

keywords: deployment, crn, task, gui, api endpoint, elasticsearch dashboard, elasticsearch connection strings

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# The Dashboard overview
{: #dashboard-overview}

## Overview
{: #dashboard-overview-page}

The _Overview_ page shows you information about your {{site.data.keyword.databases-for-elasticsearch_full}} deployment. The overview includes essential identifying information.

### Deployment details
{: #dashboard-overview-deployment-details}

- **Type:** The type of database that is offered by the service, and the database version that your service uses.
- **CRN (deployment ID):** The ID is a [CRN (Cloud Resource Name)](/docs/account?topic=account-crn) that uniquely identifies the database deployment. The CRN is used to refer to the database in the API and can be used with the CLI. The _Overview_ pane shows details of your service.

### Resources
{: #dashboard-overview-resources}

The resources tile contains information and configuration options on the size and resource usage of your deployment. You can [ccale disk, memory, and CPU](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-resources-scaling) and [configure Autoscaling](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-autoscaling).

### Recent tasks
{: #dashboard-overview-tasks}

Every time that you make administrative changes to your service (such as scaling, or taking a manual backup), a task starts up. The _Recent Tasks_ panel shows the task name and progress bar for any running tasks, and a list of the most recently completed tasks. Depending on how busy your deployment is, successful tasks are shown for 24 - 48 hours. Unsuccessful tasks are shown for 7 - 8 days. Tasks can also be retrieved from the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeploymenttasks) and [CLI plug-in](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-tasks-list). A historical record of tasks from any time period is available through the [{{site.data.keyword.atracker_full}} integration](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-at_events).

### Observability
{: #dashboard-overview-observability}

Observability: The _Observability_ tile provides access to the {{site.data.keyword.monitoringlong}}, logging, and event tracking integrations available for your deployment.

- [{{site.data.keyword.atracker_full}}](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-at_events)
- [{{site.data.keyword.logs_full}}](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-logging)
- [{{site.data.keyword.monitoringfull}}](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-monitoring)

### Endpoints
{: #dashboard-overview-endpoints}

The _Endpoints_ pane within the _Overview_ pane contains connection strings for your deployment. Each tab contains connection information that is tailored to the type of connection or the protocol that uses it. Basic information includes things like _hostname_ and _port_, the TLS service proprietary certificate, TLS/SSL parameters, and the default database of your deployment.

Reference tables for the different connection types are available on the [Getting credentials and connection strings](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-connection-strings) page.

Connection strings reflect whether your deployment uses public or private endpoints. You can configure which endpoints are available on your deployment. For more information, see the [Service endpoints integration](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-service-endpoints) page.

You can manage your {{site.data.keyword.databases-for-elasticsearch}} service through the {{site.data.keyword.databases-for}} API. This panel provides the essential information for using the API. For more information, see the [API reference](https://cloud.ibm.com/apidocs/cloud-databases-api) page.

## Backups and restore
{: #dashboard-overview-backups-and-restore}

The _Backups_ tab is the UI for managing your deployments backups. All of the available backups are listed with their timestamps. Click a backup to grab its ID or to restore it into a new deployment. More information is on the [Managing backups](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-dashboard-backups) page.

## Settings
{: #dashboard-overview-settings}

The _Settings_ tab contains the UI for many of the tunable settings for your deployment. You can
- view encryption details. Encryption at rest is enabled for all {{site.data.keyword.databases-for-elasticsearch}} deployments. If you brought your own encryption key from [Key Protect](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-key-protect&interface=ui), the panel provides a link to your Key Protect instance and the _Encryption Key_ field has the name of the key.
- [Change the admin password](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-user-management&interface=ui#user-management-set-admin-password-ui)
- [Implement or modify an IP allowlist](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-allowlisting&interface=ui)
- [Context-based restrictions](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-cbr&interface=ui)


## Service credentials
{: #dashboard-overview-serv-cred}

You can generate a new set of credentials for cases where you want to manually [connect an app](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-ibmcloud-app) or [external consumer](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-external-app) to an IBM Cloud service. For more information, see [Adding and viewing credentials](/docs/account?topic=account-service_credentials).

## View docs
{: #dashboard-overview-view-docs}

The _View docs_ link from the `Actions` drop list opens the main documentation page for {{site.data.keyword.databases-for-elasticsearch}} in a new tab.
