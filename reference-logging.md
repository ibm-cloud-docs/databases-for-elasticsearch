---
copyright:
  years: 2019
lastupdated: "2019-04-02"

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Log Analysis Integration
{: #logging}

{{site.data.keyword.databases-for-elasticsearch_full}} is integrated with [{{site.data.keyword.la_full_notm}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about), so you can view database logs.

Currently, {{site.data.keyword.la_full_notm}} integration is only available for {{site.data.keyword.databases-for}} deployments in the `us-south` region.
{: .tip}

## Provisioning {{site.data.keyword.la_full_notm}}

Log information from your databases is automatically forwarded to {{site.data.keyword.la_full_notm}}, but in order to access it you have to [provision an {{site.data.keyword.la_full_notm}} service](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-provision) in your {{site.data.keyword.cloud_notm}} account and [configure the service to receive {{site.data.keyword.cloud_notm}} service logs](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-config_svc_logs).

This setting enables logs from **ALL** {{site.data.keyword.cloud_notm}} services on your account that have {{site.data.keyword.la_full_notm}} integration to send logs to your {{site.data.keyword.la_full_notm}} service. [A list of the integrated services is available](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-cloud_services#cloud_services).
{: .tip}

{{site.data.keyword.la_full_notm}} has a lite plan that is free to use, but it only offers streaming events. To take advantage of the tagging, export, retention, and other features, you need to use one of the [paid plans](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans).

### HIPAA 

{{site.data.keyword.la_full_notm}} does not currently offer a HIPAA-compliant plan for the service. Also, use caution when configuring the platform service logs, since this could impact other services which require HIPAA compliance.

## Using {{site.data.keyword.la_full_notm}}

Once logs are being live-streamed, each log can be expanded to a detailed view by clicking the arrow to the left of the timestamp.

The expanded view has some handy, color-coded fields to help you parse your logs. 

- _Line Identifiers_
    - `Source` - the region the logs are being sent from
    - `App` - the ID of the application that sent the log. It is the last part of your deployment ID and CRN.

- _Tags_
    - `#` - all of the logs that come from a {{site.data.keyword.databases-for}} deployment are tagged with `ibm-cloud-databases`

- _Labels_
    - `database` - the database type that this log is from
    - `member` - the member ID for which node in the cluster generated the log
    - `CRN` - the full deployment ID of your deployment.

{{site.data.keyword.la_full_notm}} offers [searching](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-view_logs#view_logs_step6), [filtering](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-view_logs#view_logs_step5) to help you navigate your logs.

[Export](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-export#export) and [archiving](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving#archiving) is available so you can customize retention (and cost) for your use-case.

To set up logging alerts, see [Working with Alerts](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-alerts#alerts)

For more information on features offered by {{site.data.keyword.la_full_notm}}, including integrating it with your other {{site.data.keyword.cloud_notm}} services, see [its full documentation](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#about).

## Internal Log Retention

Database logs for all {{site.data.keyword.databases-for}} deployments are kept internally for 30 days and then purged. If your {{site.data.keyword.la_full_notm}} plan is for a shorter period, logs will only be accessible by you for the length of your plan. Regardless of the plan that you choose, all database logs are deleted after 30 days.