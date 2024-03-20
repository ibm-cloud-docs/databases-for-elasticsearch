---
copyright:
  years: 2023, 2024
lastupdated: "2024-03-18"

keywords: elasticsearch enterprise, elasticsearch platinum, elasticsearch plans, features

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Plans
{: #elastic-offerings}

{{site.data.keyword.databases-for}} offers two Elasticsearch services: {{site.data.keyword.databases-for-elasticsearch}} Enterprise and {{site.data.keyword.databases-for-elasticsearch}} Platinum. Both plans provide you with a fully managed and scalable Elasticsearch service, allowing you to focus on your applications and data rather than the underlying infrastructure. {{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan deploys the Basic version of Elasticsearch. {{site.data.keyword.databases-for-elasticsearch}} Platinum Plan deploys the Platinum version of Elasticsearch.

Our strategic partnership with [Elastic](https://www.elastic.co/about/){: external} since January 2023 means that we are able to offer more and richer functionality, as well as world-class levels of support.

{{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan does not deploy Elasticsearch Enterprise version.
{: note}

All {{site.data.keyword.databases-for-elasticsearch}} clusters on any plan can make full use of other Elastic Stack components, such as [Kibana](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started#kibana), [Logstash](https://www.elastic.co/logstash/){: external}, and [Beats](https://www.elastic.co/beats/){: external}.

## {{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan
{: #es-enter-plan}

{{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan has all the key functionalities of the Elasticsearch Service, such as [Role Based Access Control (RBAC)](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/es-security-principles.html#security-create-appropriate-users){: external} and [Index Lifecycle Management (ILM)](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/index-lifecycle-management.html){: external}, security, alerting, monitoring, reporting, and graph capabilities.


## {{site.data.keyword.databases-for-elasticsearch}} Platinum Plan
{: #es-platinum-plan}

{{site.data.keyword.databases-for-elasticsearch}} Platinum Plan offers all the features of the Enterprise Plan, plus a richer array of functionalities around integrations (connectors and web crawlers), security features (logging and access control), document-level security, and machine learning capabilities (inference APIs, anomaly detection, data frame analysis, and inference and model management), among others.

Access to Platinum features on a fully managed IBM deployment is primarily determined by the accessibility of a feature through the Elasticsearch API and the need for modifications to configuration files. Configuration files update is not currently supported for fully managed IBM deployments.

For questions regarding specific feature support, submit a [support ticket](https://cloud.ibm.com/unifiedsupport/cases/add){: external}.
{: note}

### {{site.data.keyword.databases-for-elasticsearch}} Enterprise and Platinum Feature Comparison
{: #es-plan-feature-comparison}


|                                                          **Feature**                                                          | **Enterprise** | **Platinum** |
|:-----------------------------------------------------------------------------------------------------------------------------:|:---------:|:------------:|
| Storage Types - Inverted index (for search)                                                                                   | x         | x            |
| Storage Types - Evaluating calculated fields at index time                                                                    | x         | x            |
| Storage Types - Runtime fields                                                                                                | x         | x            |
| Storage Types - Lookup runtime field                                                                                          | x         | x            |
| Storage Types - Document store (for unstructured)                                                                             | x         | x            |
| Storage Types - Columnar store (for analytics)                                                                                | x         | x            |
| Storage Types - BKD trees (for numeric, dates, & geo)                                                                         | x         | x            |
| Storage Types - Flattened field type                                                                                          | x         | x            |
| Storage Types - Histogram field type                                                                                          | x         | x            |
| Storage Types - Match only text field type                                                                                    | x         | x            |
| Storage Types - Shape field type                                                                                              | x         | x            |
| Storage Types - Vector field type                                                                                             | x         | x            |
| Storage Types - Version field type                                                                                            | x         | x            |
| Storage Types - Wildcard field type                                                                                           | x         | x            |
| Data Management - Snapshot/restore APIs                                                                                       | x         | x            |
| Data Management - Snapshot as simple archives                                                                                 |           | x            |
| Data Management - Snapshot lifecycle management                                                                               | x         | x            |
| Data Management - Data rollups                                                                                                | x         | x            |
| Data Management - Data streams                                                                                                | x         | x            |
| Data Management - Index lifecycle management                                                                                  | x         | x            |
| Stack Management - Upgrade Assistant                                                                                          | x         | x            |
| Stack Management - License management                                                                                         | x         | x            |
| Stack Management - Centralized Logstash pipeline management                                                                   |           | x            |
| Scalability & resiliency - Clustering & high availability                                                                     | x         | x            |
| Scalability & resiliency - Cluster rebalancing                                                                                | x         | x            |
| Elastic Stack Security - Encrypted communications                                                                             | x         | x            |
| Elastic Stack Security - Role-based access control                                                                            | x         | x            |
| Elastic Stack Security - File and native authentication                                                                       | x         | x            |
| Elastic Stack Security - Kibana Spaces, Kibana feature controls                                                               | x         | x            |
| Elastic Stack Security - Kibana sub-feature privileges                                                                        |           | x            |
| Elastic Stack Security - API keys management                                                                                  | x         | x            |
| Elastic Stack Security - Kibana audit logging                                                                                 |           | x            |
| Elastic Stack Security - IP filtering                                                                                         |           | x            |
| Elastic Stack Security - Elasticsearch Token Service                                                                          |           | x            |
| Elastic Stack Security - Attribute-based access control                                                                       |           | x            |
| Elastic Stack Security - Field- and document-level security                                                                   |           | x            |
| Stack Monitoring - Multi-stack monitoring                                                                                     |           | x            |
| Stack Monitoring - Kibana alerting and actions                                                                                | x         | x            |
| Alerting - Noise reduction capabilities                                                                                       | x         | x            |
| Alerting - Geofencing / Tracking containment rule type                                                                        |           | x            |
| Alerting - Search threshold rule types for Discover                                                                           | x         | x            |
| Alerting - Case Management                                                                                                    | x         | x            |
| Alerting - Connectors (Actions) like: email, webhook, Jira, MS Teams, PagerDuty, Slack, IBM Resilient, ServiceNow®, OpsGenie, |           | x            |
| Full-text search - Vector Search                                                                                              | x         | x            |
| Full-text search - Semantic Search                                                                                            |           | x            |
| Analytics - Geoline aggregation                                                                                               |           | x            |
| Analytics - Geoshape aggregations                                                                                             |           | x            |
| Analytics - Geohexgrid aggregations                                                                                           |           | x            |
| Analytics - Graph exploration                                                                                                 |           | x            |
| Analytics - Text categorization aggregation                                                                                   |           | x            |
| Data Exploration - Drilldown to URL                                                                                           |           | x            |
| Machine LEarning - Data Drift                                                                                                 |           | x            |
| Anomaly Detection - Single Metric / Multi Metric                                                                              |           | x            |
| Anomaly Detection - Population / Entity Analysis                                                                              |           | x            |
| Anomaly Detection - Log message categorization                                                                                |           | x            |
| Anomaly Detection - Rare analysis                                                                                             |           | x            |
| Anomaly Detection - Root cause indication                                                                                     |           | x            |
| Anomaly Detection - Forecasting on time series                                                                                |           | x            |
| Anomaly Detection - Root cause indication                                                                                     |           | x            |
| Data Frame Analysis - Outlier Detection                                                                                       |           | x            |
| Data Frame Analysis - Regression                                                                                              |           | x            |
| Data Frame Analysis - Classification                                                                                          |           | x            |
| Data Frame Analysis - Feature Importance                                                                                      |           | x            |
| Model Management - Inference                                                                                                  |           | x            |
| Model Management - Third party models support                                                                                 |           | x            |
| Model Management - Kibana space model separation                                                                              |           | x            |
| Model Management - Elastic Learned Sparse Encoder for Semantic Search                                                         |           | x            |
| Data Exploration - Graph analytics                                                                                            |           | x            |
| Collaboration - PDF and PNG reports                                                                                           |           | x            |
| Content Management - Custom banners                                                                                           |           | x            |
| Elastic APM - Service maps                                                                                                    |           | x            |
| Elastic APM - Correlations                                                                                                    |           | x            |
| Elastic Security - Detection alert external actions                                                                           |           | x            |
| Elastic Security - Ransomware prevention                                                                                      |           | x            |
| Elastic Security - Malicious behavior protection                                                                              |           | x            |
| Elastic Security - Memory threat protection                                                                                   |           | x            |
| Elastic Security - Self healing                                                                                               |           | x            |
| Elastic Security - Host Isolation                                                                                             |           | x            |
| Elastic Security - Customizable on-endpoint protection notifications                                                          |           | x            |
| Elastic Security - CIS posture findings and dashboards                                                                        |           | x            |
| Integrations - Atlassian Jira, Swimlane SOAR, IBM Resilient, ServiceNow ITOM, ITSM, SecOps                                    |           | x            |
| Elastic Maps - Tracking alerts                                                                                                |           | x            |
| Elastic Maps - Containment alerts                                                                                             |           | x            |
| Elastic Maps - Geofencing                                                                                                     |           | x            |
| Elastic Enterprise Search - Ingestion pipeline management                                                                     |           | x            |
{: caption="Table 1. Cloud Databases for Elasticsearch Plan Features Comparison" caption-side="bottom"}
