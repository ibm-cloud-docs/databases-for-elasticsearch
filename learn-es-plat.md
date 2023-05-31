---
copyright:
  years: 2023
lastupdated: "2023-05-31"

keywords: elasticsearch enterprise

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.databases-for-elasticsearch_full}} Plan Offerings
{: #elastic-offerings}

All {{site.data.keyword.databases-for-elasticsearch}} plans deploy as one highly available Elasticsearch cluster with three data members. Your data is replicated across members. Plans are priced based on the total amount of disk storage, RAM, dedicated cores, and backup storage that is allocated to deployments, prorated hourly. {{site.data.keyword.databases-for-elasticsearch}} deployments have a minimum of 5 GB of disk and 1 GB of RAM per data member.

## {{site.data.keyword.databases-for-elasticsearch_full}} Plans
{: #elastic-features}

{{site.data.keyword.databases-for}} offers two Elasticsearch services: {{site.data.keyword.databases-for-elasticsearch}} Enterprise and {{site.data.keyword.databases-for-elasticsearch}} Platinum. Both plans provide you with a fully managed and scalable Elasticsearch service, allowing you to focus on your applications and data rather than the underlying infrastructure. {{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan deploys the Basic version of Elasticsearch. {{site.data.keyword.databases-for-elasticsearch}} Platinum Plan deploys the Platinum version of Elasticsearch.

{{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan does not deploy Elasticsearch Enterprise version. 
{: note}

### {{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan
{: #es-enter-plan}

Here are some key features and benefits of {{site.data.keyword.databases-for-elasticsearch}}:

- Easy Deployment: IBM Cloud provides a straightforward process to create and deploy Elasticsearch clusters within minutes. Users can quickly provision Elasticsearch instances without the need for manual installation, configuration, or maintenance.

- Fully Managed Service: The service handles the operational aspects of managing and maintaining the Elasticsearch infrastructure, including scaling, patching, backups, and security updates. This allows users to offload the administrative burden and focus on utilizing Elasticsearch for their data analysis needs.

- High Scalability and Performance: {{site.data.keyword.databases-for-elasticsearch}} is designed to handle large volumes of data and high query loads. Users can easily scale their Elasticsearch clusters up or down based on their workload requirements to ensure optimal performance and responsiveness.

- Integration with IBM Cloud Ecosystem: The service seamlessly integrates with other IBM Cloud offerings, enabling users to leverage additional services such as IBM Cloud Monitoring and IBM Cloud Identity and Access Management for enhanced monitoring, security, and access control.

- Security and Compliance: {{site.data.keyword.databases-for-elasticsearch}} provides robust security features to protect data. It includes encryption in transit and at rest, built-in user authentication, and access controls to ensure data privacy and compliance with industry regulations.

- Monitoring and Logging: The service offers built-in monitoring and logging capabilities, allowing users to gain insights into the performance and health of their Elasticsearch clusters. This facilitates proactive troubleshooting and optimization of resource utilization.

- High Availability and Disaster Recovery: {{site.data.keyword.databases-for-elasticsearch}} provides redundancy and fault tolerance to ensure high availability of data. It offers automated failover and backup capabilities, ensuring data durability and facilitating disaster recovery scenarios.

Overall, {{site.data.keyword.databases-for-elasticsearch}} simplifies the deployment, management, and scaling of Elasticsearch clusters, allowing users to harness the power of Elasticsearch for their search and analytics needs while leveraging the benefits of a fully managed service.

### {{site.data.keyword.databases-for-elasticsearch}} Platinum Plan
{: #es-platinum-plan}

Our strategic partnership with [Elastic](https://www.elastic.co/about/){: external} since January 2023 means that we are able to offer more and richer functionality, as well as world-class levels of support.

{{site.data.keyword.databases-for-elasticsearch}} Platinum Plan offers rich functionality primarily determined by the accessibility of a feature through the Elasticsearch API and the need for modifications to the configuration files. If a particular feature can be accessed and utilized through the API, it is highly likely that we support it. These features are designed to seamlessly integrate with our system, providing a smooth and efficient user experience.

The {{site.data.keyword.databases-for-elasticsearch}} Platinum Plan offers all the deep functionality of the {{site.data.keyword.databases-for-elasticsearch}} Enterprise Plan, with added benefits such as: 
- [Role Based Access Control (RBAC)](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/es-security-principles.html#security-create-appropriate-users){: external}
- [Index Lifecycle Management (ILM)](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/index-lifecycle-management.html){: external}

Once deployed, {{site.data.keyword.databases-for-elasticsearch}} Platinum Plan clusters make full use of other Elastic Stack components such as [Kibana](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started#kibana), [Logstash](https://www.elastic.co/logstash/){: external}, and [Beats](https://www.elastic.co/beats/){: external}.

### {{site.data.keyword.databases-for-elasticsearch}} Enterprise and Platinum Feature Comparison
{: #es-plan-feature-comparison}

|                                                          **Feature**                                                          | **Basic** | **Platinum** |
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
| Data Management - Searchable Snapshots                                                                                        |           | x            |
| Data Management - Snapshot/restore APIs                                                                                       | x         | x            |
| Data Management - Snapshot as simple archives                                                                                 |           | x            |
| Data Management - Snapshot lifecycle management                                                                               | x         | x            |
| Data Management - Snapshot-based peer recoveries                                                                              |           | x            |
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
| Analytics - Geoline aggregation                                                                                               |           | x            |
| Analytics - Geoshape aggregations                                                                                             |           | x            |
| Analytics - Geohexgrid aggregations                                                                                           |           | x            |
| Analytics - Graph exploration                                                                                                 |           | x            |
| Analytics - Text categorization aggregation                                                                                   |           | x            |
| Data Exploration - Drilldown to URL                                                                                           |           | x            |
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
{: caption="Table 1. ICD Elasticsearch Platinum Features Comparison" caption-side="bottom"}

