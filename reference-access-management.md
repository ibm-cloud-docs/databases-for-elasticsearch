---

copyright:
  years: 2018
lastupdated: "2018-10-04"

subcollection: databases-for-elasticsearch

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Identity and Access Management Integration
{: #iam}

Access to {{site.data.keyword.databases-for-elasticsearch_full}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.databases-for-elasticsearch}} service in your account must be assigned an access policy with an IAM user role defined. That policy determines what actions the user can perform within the context of the service or instance you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. Some of the options include: 
* Access across all instances of the service in your account
* Access to an individual service instance in your account
* Access to a specific resource within an instance
* Access to all IAM-enabled services in your account

After you define the scope of the access policy, you assign a role. Review the following tables that outline what actions each role allows within the {{site.data.keyword.databases-for-elasticsearch}} service.

For information about assigning user roles in {{site.data.keyword.cloud_notm}}, see [Managing IAM access](/docs/iam?topic=iam-iammanidaccser).

The following table details actions that are mapped to service management roles. Service management roles enable users to perform tasks on service resources at the service level, for example assign user access for the service, create or delete service IDs, create instances, and bind instances to applications.

Service management role | Description of actions | Example actions
-----------------|-----------------|-----------------
Viewer | As a viewer, you can view service instances, but you can't modify them. | View Service Overview and View Alerts
Editor | As an editor, you can perform all platform actions except for managing the account and assigning access policies. | Scale a Deployment and Change a Deployment's Password
Operator | As an operator, you can perform all platform actions except for managing the account and assigning access policies. | Scale a Deployment and Change a Deployment's Password
Administrator | As an administrator, you can perform all platform actions based on the resource this role is being assigned, including assigning access policies to other users. | Scale a Deployment, Change a Deployment's Password, and Assign Access Policies
{: caption="Table 1. IAM user roles and actions" caption-side="top"}

## Actions for {{site.data.keyword.databases-for}} API

Platform Action  | Operation on service | Role |
----------|------------|----------|
`GET /v4/:platform/deployables` | Read deployable database types | Administrator, Editor, Operator, Viewer 
`GET /v4/:platform/tasks/:task_id` | Read a Task | Administrator, Editor, Operator, Viewer 
`GET /v4/:platform/backups/:backup_id` | Read a Backup | Administrator, Editor, Operator, Viewer 
`POST /v4/:platform/deployments` | Create a Deployment | Administrator, Editor, Operator
`GET /v4/:platform/deployments/:deployment_id` | Read a Deployment | Administrator, Editor, Operator, Viewer
`DELETE /v4/:platform/deployments/:deployment_id` | Remove a deployment | Administrator, Editor, Operator
`GET /v4/:platform/deployables/:deployable_id/groups` | Read deployable group | Administrator, Editor, Operator, Viewer 
`GET /v4/:platform/deployments/:deployment_id/tasks` | Read all deployment tasks | Administrator, Editor, Operator, Viewer 
`GET /v4/:platform/deployments/:deployment_id/backups` | Read all deployment backups | Administrator, Editor, Operator, Viewer 
`POST /v4/:platform/deployments/:deployment_id/backups` | Create an on-demand backup | Administrator, Editor, Operator
`GET /v4/:platform/deployments/:deployment_id/groups` | Read all deployment groups | Administrator, Editor, Operator, Viewer
`PATCH /v4/:platform/deployments/:deployment_id/groups/:group_id` | Read deployment group | Administrator, Editor, Operator
`POST /v4/:platform/deployments/:deployment_id/users` | Create a deployment user | Administrator, Editor, Operator
`PATCH /v4/:platform/deployments/:deployment_id/users/:user_id` | Update a deployment user | Administrator, Editor, Operator 
`DELETE /v4/:platform/deployments/:deployment_id/users/:user_id` | Remove a deployment user | Administrator, Editor, Operator 
`GET /v4/:platform/deployments/:deployment_id/users/:user_id/connections` | Read deployment user connections | Administrator, Editor, Operator, Viewer 
`POST /v4/:platform/deployments/:deployment_id/users/:user_id/connections` | Create deployment user connections | Administrator, Editor, Operator
`GET /v4/:platform/deployments/:deployment_id/whitelists/ip_addresses` | Read whitelisted IP addresses | Administrator, Editor, Operator, Viewer
`POST /v4/:platform/deployments/:deployment_id/whitelists/ip_addresses` | Create a whitelisted IP address | Administrator, Editor, Operator
`DELETE /v4/:platform/deployments/:deployment_id/whitelists/ip_addresses/:ip_address_id` | Remove a whitelisted IP address | Administrator, Editor, Operator
{: caption="Table 2. Platform actions and operations" caption-side="top"}

