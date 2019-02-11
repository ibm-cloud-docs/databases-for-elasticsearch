---
copyright:
  years: 2017,2018
lastupdated: "2018-10-04"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Activity Tracker Integration
{: #activity-tracker}

{{site.data.keyword.databases-for-elasticsearch_full}} is integrated with  [Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov), so you can view service-level events.

In order to see the events, you need to [provision the Activity Tracker service](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-provision) from the [{{site.data.keyword.cloud_notm}}  catalog](https://{DomainName}/catalog/services/activity-tracker). Activity Tracker has a _Lite_ plan available at no additional cost.

Some IBM Cloud regions do not have the Activity Tracker service available. If you have a {{site.data.keyword.databases-for-elasticsearch}} deployment in a region that is not supported, provision Activity Tracker in the region on the table.

Deployment Region|Monitoring Region|UI Link
----------|-----------|-----------
US South | US South | https://logging.ng.bluemix.net
Frankfurt | Frankfurt | https://logging.eu-fra.bluemix.net
Oslo | Frankfurt | https://logging.eu-fra.bluemix.net
Tokyo | not supported | not supported
Sydney | Sydney | https://logging.au-syd.bluemix.net
US East | US South | https://logging.ng.bluemix.net
London | Frankfurt | https://logging.eu-fra.bluemix.net

{: caption="Table 1. Activity Tracker service regions" caption-side="top"}

Once you have the Activity Tracker service, the {{site.data.keyword.databases-for-elasticsearch}} events appear under _Account Logs_ from the _View Logs_ dropdown menu. 

## Event Fields
A description of the common fields for an Activity Tracker event is on the [Activity Tracker event fields](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-at_event) page.

## List of Events

The table lists the events that get sent to Activity Tracker from {{site.data.keyword.databases-for-elasticsearch}}.

Action|Description
-------|-------
`databases-for-elasticsearch.backup.create`|A backup of your deployment was created. If the backup failed, a "-failure" flag is included in the message.
`databases-for-elasticsearch.user-password.update`|A user's password was updated. A "-failure" flag is included in the message if the attempt to update a user's password failed.
`databases-for-elasticsearch.user.create`|A user was created. A "-failure" flag is included in the message if the attempt to create a user failed.
`databases-for-elasticsearch.user.delete`|A user was deleted. A "-failure" flag is included in the message if the attempt to delete a user failed.
`databases-for-elasticsearch.backup.restore`|A restore from backup was created. If the attempted restore failed, a "-failure" flag is included in the message.
`databases-for-elasticsearch.resources.scale`|A scaling operation was performed. If the scaling operation failed, a "-failure" flag is included in the message.
`databases-for-elasticsearch.whitelisted-ips-list.update`|The whitelist was modified. A "-failure" flag is included in the message if the attempt to modify the whitelist failed.
{: caption="Table 1. List of Events and Event Descriptions" caption-side="top"}

