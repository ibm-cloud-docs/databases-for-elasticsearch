---

copyright:
  years: 2017,2018
lastupdated: "2018-10-08"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Backups
{: #backups}

{{site.data.keyword.databases-for-elasticsearch_full}} backups are accessible from the _Backups_ tab of your service dashboard. Daily and on-demand backups are available for 30 days. Each backup is labeled with its type, and when the backup was taken. Click the backup to reveal the full ID of the backup and a command that you can use to restore a backup with the {{site.data.keyword.cloud_notm}} CLI.

## Restoring a Backup

To restore a backup to a new service instance,

1. Click in the corresponding row to expand the options for the backup you want to restore.
2. Click the **Restore** button.  A message is displayed that a restore from backup has started. Clicking on **Your new instance is available now.** will take you to your _Resources List_.

You can also use the {{site.data.keyword.cloud_notm}} CLI to restore a backup.

```
ibmcloud resource service-instance-create SERVICE_INSTANCE_NAME databases-for-elasticsearch standard REGION -p '{"backup_id":"{backup_id"}
```

**Note:** Change the value of `SERVICE_INSTANCE_NAME` to the name you want for your new service. Change the `REGION` to the region where you want the service to run.

A pre-formatted command for a specific backup is available in detailed view of the backup on the _Backups_ tab of the service dashboard.
{: .tip}

## Managing Backups via the {{site.data.keyword.cloud_notm}} CLI cloud databases plugin

Use the `cdb deployment-backups-list` command to view the list of all available backups for your deployment. To get the details about a specific backup, use `cdb backup-show` command.

For example, to view the backups for a deployment named "example-deployment", use the following command.

```
ibmcloud cdb deployment-backups-list example-deployment
```
{: codeblock}

The response is a table with backup `ID`, `Type`, `Status`, and `Created At` fields.

To see the details of one of the backups from the list, take the ID from the `ID` field of the table and use it with the `backup-show` command.

```
ibmcloud cdb backup-show crn:v1:staging:public:databases-for-elasticsearch:us-south:a/6284014dd5b487c87a716f48aeeaf99f:3b4537bf-a585-4594-8262-2b1e24e2701e:backup:a3364821-d061-413f-a0df-6ba0e2951566
```
{: codeblock}

The response has the backup `ID`, the `Deployment ID`, `Type`, `Created At`, `Status`, and `Is Restorable` information.

## Managing Backups via the {{site.data.keyword.cloud_notm}} databases API

The _Foundation Endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/backups` endpoint if you need to manage or automate backups programmatically.

For more information and examples, see the [API Reference](https://{DomainName}/apidocs/cloud-databases-api#get-information-about-a-backup).

## Backups and Restoration

* {{site.data.keyword.cloud_notm}} Databases is not responsible for restoration, timeliness, or validity of said backups.
* Actions that you take as a user can compromise the integrity of backups, such as under-allocating memory and disk. Users can monitor that backups were performed successfully via the API, and periodically restore a backup to ensure validity and integrity. Users can retrieve the most recent scheduled backup details from the IBM Cloud Databases plug-in: `ibmcloud cdb backups deploymentname -s -f`.
* As a managed service, {{site.data.keyword.cloud_notm}} Databases monitors the state of your backups and can attempt to remediate when possible. If you encounter issues you cannot recover from, you can contact support for additional help.
