---
copyright:
  years: 2019, 2025
lastupdated: "2025-09-18"

keyowrds: elasticsearch, databases, upgrading, 7.x, reindex, indices, update user passwords, retrieve user passwords, elasticsearch 7.17, indexes, reindexing, reindex

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading to a new major version
{: #upgrading}

{{site.data.keyword.databases-for-elasticsearch_full}}  provides two different upgrade paths:

- In-place upgrade to a new major version
- Restoring from backup (supported for Elasticsearch Enterprise Plan, Elasticsearch Platinum Plan)

## In-place major version upgrades
{: #upgrading-in-place}

In-place major version upgrade allows you to upgrade your deployment to the next new major version, eliminating the need to restore a backup into a new deployment. This approach maintains the same connection strings, without the need to reconfigure the deployment. However, if the new major version requires application adjustments, these must be addressed.

### Options for in-place upgrade
{: #upgrading-in-place-options}

- **With backup**: Creates a backup before performing the upgrade, providing an added layer of safety.
- **Without backup**: Proceeds without creating a backup. If the upgrade fails, restoration must be done from the latest backup into a new deployment.

In-place upgrade without backup is not recommended. It may result in data loss if the upgrade fails at any stage, , as there will be no immediate backup to restore from.
{: important}

## Before you begin
{: #upgrading-considerations}

- Ensure that your deployment is in a healthy state.
- Ensure at least 4 GB of free disk space.
- Only upgrade to the next major version, instead of specifying the version of your choice.
- Each major version contains some features that may not be backward-compatible with previous versions. Review the [release notes](https://www.mongodb.com/docs/manual/release-notes/) from the database vendor to see any changes that may affect your applications.
- Downgrading a deployment to a previous version is not supported.
- In-place upgrade cannot be cancelled once started.

## Upgrading in the UI
{: #upgrading-in-place-ui}
{: ui}

1. Create a {{site.data.keyword.databases-for-elasticsearch}} to test the upgrade process. 
  Create the deployment [by restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui#restore-backup) from your existing deployment with the same version.
2. Point your staging application to the test deployment.
  Update your staging application to point to the test deployment. Confirm that your test application can connect successfully to the staging deployment and that the application operates as expected. Perform any required performance and operational testing of the staging environment.
3. Upgrade the major version of your test deployment by clicking on the Upgrade major version button on the *Overview* page.
  Your database will not be accessible during the upgrade window. Note how long the upgrade takes to complete so that you can use the upgrade expiry setting to contain upgrades within your maintenance window.
4. Confirm that your staging application works with the new database version.
  If your application works, this step confirms that it should be safe to upgrade your production database.
5. Upgrade your production deployment by clicking **Upgrade major version**. 
  Once you confirmed that your application works correctly by using the new version of the database, return to the management console and start the process of upgrading your production deployment. In the *Deployment details* section of the *Overview* page, click **Upgrade major version** and follow the steps.

Once the in-place upgrade starts, it cannot be stopped or rolled back. So, in the unlikely event of an error, your database deployment could become unrecoverable. Therefore, create a backup that you can then use to restore to a new deployment. If you select *In-place major version upgrade with backup*, the backup that is created can be used to restore in a new deployment.

The *expiration for starting upgrade* allows you to configure a 'timeout' period that the upgrade job must start within before it is automatically cancelled. In addition, test the upgrade in staging upfront to ensure that the upgrade completes within your desired time window. If, for example, you want to complete the upgrade within 1 hour, and you tested the upgrade and know that it takes 30 minutes, then your upgrade job must start within 30 minutes of you confirming that you want to upgrade. Therefore, set the expiration to 30 minutes, so that if it doesn't start within that time, it won't overrun your window.

## Troubleshooting
{: #upgrading-in-place-troubleshooting}

### Healthchecks
{: #upgrading-in-place-healthchecks}

- Before starting an Elasticsearch upgrade, it is critical to verify that the cluster has sufficient resources and is in a healthy state.
- Ensure that the cluster health status is GREEN.
- Confirm that disk usage is below 85% to avoid upgrade failures due to insufficient space.
- Perform a secondary precheck to detect deprecations in the cluster.
- If deprecations are found, the upgrade process will stop and must be retried only after all issues are resolved.

If issues persist, open a support ticket with [IBM Cloud support](https://cloud.ibm.com/login?redirect=%2Funifiedsupport%2Fsupportcenter).

### Elastic resources
{: #upgrading-in-place-links}

- [Elasticsearch health report API](https://www.elastic.co/docs/api/doc/elasticsearch/operation/operation-health-report)
- [Elasticsearch migration deprecations API](https://www.elastic.co/docs/api/doc/elasticsearch/operation/operation-migration-deprecations)

## Restoring from backup
{: #upgrading-restoring-from-backup}

Before a major version of a database reaches its end of life (EOL), upgrade to the next available major version by restoring from a backup into a new database instance.

Prepare to run on, and then migrate to, the latest version before the EOL date. For more information, see [Versioning Policy](/docs/cloud-databases?topic=cloud-databases-versioning-policy){: external}.

Rolling back versions is not supported.
{: .note}

Upgrade to the latest version of Elasticsearch available to {{site.data.keyword.databases-for-elasticsearch}}. Find the latest version from the catalog page, from the {{site.data.keyword.databases-for}} CLI plug-in command [`ibmcloud cdb deployables-show`](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show), or from the [{{site.data.keyword.databases-for}} API /deployables endpoint(https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeployables)].

Upgrading is handled by [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups) of your data into a new deployment. Restoring from a backup has various advantages:

  - The original database stays running and production work can be uninterrupted.
  - You can test the new database out of production and act on any application incompatibilities.
  - The entire process can be rerun at any point.
  - A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.

## Upgrade paths
{: #upgrading-paths}

| Current version     | Major version upgrade path |
|---------------------|----------------------------|
| Elasticsearch 8.7   | Elasticsearch 8.15         |
| Elasticsearch 8.10  | Elasticsearch 8.15         |
| Elasticsearch 8.12  | Elasticsearch 8.15         |
| Elasticsearch 8.15  | Elasticsearch 8.19         |
| Elasticsearch 8.19  | Elasticsearch 9.1          |
{: caption="Major version upgrade paths" caption-side="top"}

## Upgrading in the UI
{: #upgrading-ui}
{: ui}

For new hosting models (isolated compute and shared compute), upgrading to a new major version is available via [CLI](docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=cli) and [API](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=api)

You can upgrade to a new version by [restoring a backup](/docs/cloud-databases?topic=cloud-databases-dashboard-backups&interface=ui#restore-backup) from the *Backups and restore* page of your deployment on the IBM Cloud console. Click **Restore backup** on a backup to open a page in a new tab where you can change some options for the new deployment. One of them is the database version, which is auto-populated with the versions available for you to upgrade to. Select a version and click **Restore backup** to start the provision and restore process.
