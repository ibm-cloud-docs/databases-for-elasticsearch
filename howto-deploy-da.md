---

copyright:
  years: 2024
lastupdated: "2024-12-09"

keywords: elasticsearch deployable architecture, elasticsearch automation

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Working with the {{site.data.keyword.databases-for-elasticsearch}} deployable architecture
{: #deployable-architecture}

{{site.data.keyword.databases-for-elasticsearch_full}} is available as a deployable architecture called Cloud automation for {{site.data.keyword.databases-for-elasticsearch}} in the catalog, so that you can use Elasticsearch as an add-on or building block for the solutions that you build and deploy by using the automation available by managing your resource deployments with {{site.data.keyword.cloud_notm}} [projects](#x2035151){: term}. Projects are used to manage related resources and deployments across accounts, embracing an Infrastructure as Code (IaC) approach to deployments.
{: shortdesc}

If you are not managing your resource deployments with projects, you can deploy {{site.data.keyword.databases-for-elasticsearch}} directly from the catalog. For more information, see [Creating a {{site.data.keyword.databases-for-elasticsearch}} instance](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-create-instance-tutorial).
{: note}

## What is Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}?
{: #es-deployable-arch}

Using the {{site.data.keyword.databases-for-elasticsearch}} deployable architecture enables your team to efficiently deploy and manage a fully operational Elasticsearch instance in {{site.data.keyword.cloud}}, providing scalable, secure, and optimized search capabilities for your applications.

This deployable architecture is designed to showcase a fully configured Elasticsearch instance, leveraging IBM Cloud’s integrated security and scalability features. With support for encrypted data and optional autoscaling, it ensures that your Elasticsearch deployment can meet dynamic workloads while maintaining data security.

This architecture simplifies your organization’s search and analytics processes and enhances the reliability of Elasticsearch deployments.

With this architecture, you can:

Centralize search and analytics
:   The Elasticsearch instance enables your team to store, search, and analyze large datasets in a centralized location, ensuring quick and reliable access to insights.

Secure data with key management system (KMS) encryption
:   Using a pre-configured KMS key, your Elasticsearch data is encrypted, safeguarding it against unauthorized access and ensuring compliance with security standards.

### Features and capability
{: #features-capability}

- Deploys and configures an {{site.data.keyword.databases-for-elasticsearch}} instance.
- Integrates with IBM Key Management Service (KMS) for encrypted data storage.
- Supports optional autoscaling rules to optimize resource usage.
- Provides an optional Kibana dashboard for enhanced data visualization and interaction with Elasticsearch data.

## Deploying Cloud automation for {{site.data.keyword.databases-for-elasticsearch}} with projects
{: #deploy-databases-for-elasticsearch}

1. From the {{site.data.keyword.cloud_notm}} catalog, search for Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}.
1. Add it to an existing project or create a project.
1. Complete the next steps depending on how you plan to use the deployable architecture:
   - [Configure](/docs/secure-enterprise?topic=secure-enterprise-config-project) it in your project and [deploy](/docs/secure-enterprise?topic=secure-enterprise-deploy-project).
   - You can [stack deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-config-stack) together in a project to create a robust end-to-end solution architecture. You don't need to code Terraform to connect the member deployable architectures within the stack. As you configure input values in a member deployable architecture, you can reference inputs or outputs from another member to link the deployable architectures together. After you deploy the deployable architectures in your stack, you can add the stack to a private catalog to easily share it with others in your organization.

### Resources for working with Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}
{: #es-deployable-arch-resources}

When using {{site.data.keyword.databases-for-elasticsearch}} after it's deployed, there are some differences regarding responsibilities and managing your deployment through projects. Use the following resources to learn more.

- After you deploy Cloud automation for {{site.data.keyword.databases-for-elasticsearch}} from a project, you can use the [{{site.data.keyword.databases-for-elasticsearch}}](/docs/databases-for-elasticsearch) documentation for information about working with your new instance.
- If you encounter any issues working with projects to validate or deploy your resources, refer to the [Troubleshooting for projects](/docs/secure-enterprise?topic=secure-enterprise-troubleshooting-for-projects) documentation.
- Review the [Understanding your responsibilities when you use {{site.data.keyword.IBM_notm}} deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-responsibilities-deployable-architectures) to learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.IBM_notm}} deployable architectures.
