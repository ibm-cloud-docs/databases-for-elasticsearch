---
copyright:
  years: 2019, 2023
lastupdated: "2023-03-29"

keyowrds: elasticsearch, databases, upgrading, 7.x, reindex, indices, update user passwords, retrieve user passwords, elasticsearch 7.17

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading to a new Major Version
{: #upgrading}

When a major version of a database is at its end of life (EOL), upgrade to the current major version. You can upgrade {{site.data.keyword.databases-for-elasticsearch_full}} deployments to use the newest version of Elasticsearch.

Upgrade to the latest version of Elasticsearch available to {{site.data.keyword.databases-for-elasticsearch}}. You can find the latest version from the catalog page, from the cloud databases cli plug-in command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show), or from the cloud databases API [`/deployables`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-all-deployable-databases) endpoint.

Upgrading is handled through [restoring a backup](/docs/databases-for-elasticsearch?topic=cloud-databases-dashboard-backups#restoring-a-backup) of your data into a new deployment. Restoring from a backup has a number of advantages:

- The original database stays running and production work can be uninterrupted.
- You can test the new database out of production and act on any application incompatibilities.
- The entire process can be rerun at any point.
- A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.

## Before upgrading
{: #before-upgrading}

Before you start to upgrade your cluster to version 7.x, you must take the following actions.

- Check the [deprecation logs](https://www.elastic.co/guide/en/elasticsearch/reference/current/logging.html#deprecation-logging){: .external} that are automatically enabled on Databases for Elasticsearch and sent to [{{site.data.keyword.la_full}}](/docs/databases-for-elasticsearch?topic=cloud-databases-logging) to see whether you are using any deprecated features and update your code.
- Review the [breaking changes](https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking-changes.html){: .external} and make any necessary changes to your code and configuration for version 7.x.
- If you use any plug-ins, make sure that there is a version of each plug-in that is compatible with Elasticsearch version 7.x.
- Reindex your data before running the 6.x to 7.x version upgrade by using [the guidance here](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-upgrading#upgrade-reindexing).

### Reindexing guidelines
{: #upgrade-reindexing}

{{site.data.keyword.databases-for-elasticsearch}} indexes are only compatible with the same version or plusOne version from the release on which they are created. Only an explicit reindex operation updates an index to the current database version.

## Upgrading in the UI
{: #upgrading-ui}
{: ui}

You can upgrade to a new version when [restoring a backup](/docs/databases-for-elasticsearch?topic=cloud-databases-dashboard-backups#restoring-a-backup) from the _Backups_ tab of your _Deployment Overview_. Clicking **Restore** on a backup brings up a dialog box where you can change some options for the new deployment. One of them is the database version, which is auto-populated with the versions available for you to upgrade to. Select a version and click **Restore** to start the provision and restore process.

## Upgrading through the CLI
{: #upgrading-cli}
{: cli}

When you upgrade and restore from backup through the {{site.data.keyword.cloud_notm}} CLI, use the provisioning command from the resource controller.
```sh
ibmcloud resource service-instance-create <service-name> <service-id> <service-plan-id> <region>
```
The parameters `service-name`, `service-id`, `service-plan-id`, and `region` are all required. You also supply the `-p` with the version and backup ID parameters in a JSON object. The new deployment is automatically sized with the same disk and memory as the source deployment at the time of the backup.

```sh
ibmcloud resource service-instance-create example-es-upgrade databases-for-elasticsearch standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "version":7.9
}'
```

## Upgrading through the API
{: #upgrading-api}
{: api}

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](/docs/databases-for-elasticsearch?topic=cloud-databases-provisioning#provisioning-through-the-resource-controller-api) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version and backup ID. The new deployment has the same memory and disk allocation as the source deployment at the time of the backup.

```sh
curl -X POST \
  https://resource-controller.cloud.ibm/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "bluemix-us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-elasticsearch-standard",
    "backup_id": "crn:v1:bluemix:public:databases-for-elasticsearch:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
    "version":7.9
  }'
```

## Migration Notes for New Elasticsearch 7.x Users
 {: #migration-notes}

As in previous version upgrades, there are many changes. For more information, see [Elastic documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/breaking-changes.html){: .external}. 
 

### Index mappings 
{: #index-mappings}

Mapping types are removed in Elasticsearch 7.x and above. Indexes that are created in Elasticsearch 7.x or later no longer accept a _default_ mapping. Types are also deprecated in APIs in 7.x. Further details on these changes are in the Elastic documentation on the [removal of mapping types](https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html){: .external}.

## Retrieve and update user passwords
{: #esupgrade-retrieve-update-user-passwords}

### Retrieve Elasticsearch user passwords
{: #esupgrade-retrieve-user-passwords}

A restore to Elasticsearch 7.17 invalidates existing user passwords. Existing user passwords must be reset after the restore. Follow this procedure to list all users and change user passwords.

First, run the [ibmcloud plug-in update](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_commands_settings#ibmcloud_plugin_update) command: 

```sh
ibmcloud plugin update cloud-databases
```
{: pre}

Then, log in to your {{site.data.keyword.cloud}} account:

```sh
ibmcloud login -a cloud.ibm.com -r <region> --sso
```
{: pre}

Next, run the `user-list` command with the `--help` flag. This command outputs various options for your account's user list.

```sh
ibmcloud cloud-databases es user-list --help
```
{: pre}

The `user-list` command output looks like: 

```screen
ibmcloud cloud-databases es user-list --help
NAME:
  user-list, ul - List all users from the database internal credential store.

USAGE:
  ibmcloud cdb elasticsearch user-list (NAME|ID) (ADMIN_PASSWORD) [--json] [-c DIRECTORY] [--api-version]
  
OPTIONS:
  --json, -j         --json, -j                     Results as JSON.
  --certroot, -c     --certroot value, -c value     Certificate Root. (default: "< >") [$CERTROOT]
  --api-version, -v  --api-version value, -v value  API Version used for request.
```

Before running the command to update user passwords, ensure that you have updated the admin password. For more information, see [Setting the Admin Password](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-admin-password&interface=cli). Then, run the following command: 

```sh
ibmcloud cloud-databases es user-list <formation_name> <admin_password>
```
{: pre}

If this command is run without the `--json` option, the plug-in `user-list` also prints the reset password commands for all users.

The `user-list` command output looks like: 

```screen
ibmcloud cloud-databases es user-list es-akshay-res-717-new 12345678987654321   
Getting user list for es-akshay-res-717-new...
8 user accounts are present in the Elasticsearch credential store:
Username                                         Fullname         Email
fred                                             Fred Name        fred@example.com
admin                                            admin Name       admin@example.com
ibmkibana                                        ibmkibana Name   ibmkibana@example.com
ibm_cloud_d68c8129_fea9_4e83_abc8_4a6c514248ac
ibm_cloud_2a526d0b_e101_49f3_bd13_7c065870a7b1
ibm_cloud_7052ee45_d702_4073_aea2_a5435989568e
ibm_cloud_29075661_76d0_405a_9619_480eb7a173d4
ibm_cloud_eb148643_099a_4334_a88a_f2e5e1f8ccdd

You can use these commands to set new passwords for them: 
ibmcloud cdb deployment-user-password "es-akshay-res-717-new" fred

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" admin

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibmkibana

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_d68c8129_fea9_4e83_abc8_4a6c514248ac

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_2a526d0b_e101_49f3_bd13_7c065870a7b1

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_7052ee45_d702_4073_aea2_a5435989568e

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_29075661_76d0_405a_9619_480eb7a173d4

ibmcloud cdb deployment-user-password "es-akshay-res-717-new" ibm_cloud_eb148643_099a_4334_a88a_f2e5e1f8ccdd

OK
```

### Update Elasticsearch user passwords
{: #esupgrade-update-user-passwords}

To update user passwords, run the following command for _each_ user. You are then prompted to enter the new password for that user.

```sh
ibmcloud cdb deployment-user-password "example-deployment" <your-user-1>
```
{: pre}

The `deployment-user-password` command needs to be run for each user.
{: important}
