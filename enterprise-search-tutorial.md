---

copyright:
   years: 2023
lastupdated: "2023-08-24"

keywords: IBM Cloud Databases, ICD, enterprise search, ca certificate

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 1h

---

{{site.data.keyword.attribute-definition-list}}	

# Configuring an Enterprise Search 7.17 server with an {{site.data.keyword.databases-for-elasticsearch}} instance
{: #tutorial-elasticsearch-enterprise-search-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="1h"}

Welcome to a guide on configuring a functional Enterprise Search 7.17 server integrated with an {{site.data.keyword.databases-for-elasticsearch_full}} instance. Elasticsearch is a powerful and versatile search and analytics engine that helps you store, search, and analyze large volumes of data quickly and in near-real-time. Elasticsearch is commonly used for log and event data analysis, full-text search, and various other use cases where data needs to be queried efficiently. 

{{site.data.keyword.databases-for-elasticsearch_full}} is an Elasticsearch service that is offered by {{site.data.keyword.cloud_notm}} that provides a managed and scalable solution for deploying and running Elasticsearch clusters. {{site.data.keyword.databases-for-elasticsearch}} simplifies the setup and maintenance of Elasticsearch, enabling you to focus on using the power of Elasticsearch without the complexities of managing the infrastructure. Kibana complements Elasticsearch by offering a flexible visualization platform. It allows you to explore, visualize, and share insights from your data, enabling you to create custom dashboards and visualizations to better understand your information.

Enterprise Search extends the capabilities of {{site.data.keyword.databases-for-elasticsearch}} to provide a unified search experience across various data sources, including documents, emails, databases, and more. It offers a seamless interface for users to find relevant information across disparate data silos, enhancing productivity and collaboration. By integrating Enterprise Search with your {{site.data.keyword.databases-for-elasticsearch}} instance, you gain a comprehensive search solution that uses the strengths of both platforms to efficiently discover insights from your data.
{: shortdesc}

## Preqequisites
{: #tutorial-elasticsearch-enterprise-search-tutorial-prereqs}
{: step}

Before you start the installation process, have the prerequisites in place:

- Have a working instance of [{{site.data.keyword.databases-for-elasticsearch}}](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-provisioning-new) and [Kibana](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started&interface=ui#kibana).

- Java 11: If your preferred installation method is through a package, verify that [Java 11](https://www.oracle.com/java/technologies/downloads/){: external} is installed on your system.


## Setting Up Elasticsearch and Kibana Instances
{: #tutorial-elasticsearch-enterprise-search-tutorial-instance-setup}
{: step}

1. [Configure](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started&interface=ui) your {{site.data.keyword.databases-for-elasticsearch}} and Kibana instances.

1. Add the following line in your Kibana.yml file:

   ```sh
   enterpriseSearch.host: ${ENTERPRISE_HOST_URL}
   ```
   {: pre}
   
   `ENTERPRISE_HOST_URL` depends upon your installation.

1. After successfully setting up the instances, obtain the following details:

- `ELASTICSEARCH_USERNAME`
- `ELASTICSEARCH_PASSWORD`
- `ELASTICSEARCH_HOST URL`
- `KIBANA_HOST URL`
- `CA Certificate`

## Configure Enterprise Search through Package
{: #tutorial-elasticsearch-enterprise-search-tutorial-config-package}
{: step}

Configure Enterprise Search to integrate with your {{site.data.keyword.databases-for-elasticsearch}} instance.

1. [Download](https://www.elastic.co/downloads/past-releases/enterprise-search-7-17-7){: external} Enterprise Search.
1. Extract the downloaded package to a suitable location.
1. Edit the Configuration File: Locate the `enterprise-search.yml` file within the config directory of the extracted package. Edit this file to incorporate the necessary settings.
1. Add configuration settings: Inside the `enterprise-search.yml` file, add the following lines:

   ```sh
   allow_es_settings_modification: true
   secret_management.encryption_keys:['AWESOME_SECRET'] 
   elasticsearch.username: ELASTICSEARCH_USERNAME
   elasticsearch.password: ELASTICSEARCH_PASSWORD 
   elasticsearch.host: ELASTICSEARCH_HOST
   elasticsearch.ssl.enabled: true 
   elasticsearch.ssl.certificate_authority: path/to/ca-cert
   kibana.external_url: KIBANA_HOST
   ```
   {: pre}
   
   Replace the placeholders with the details obtained in the previous step.

1. CA Certificate Placement: Ensure that the CA certificate you downloaded is placed in the specified path that is mentioned in the configuration.

1. Start Enterprise Search: Open your terminal and navigate to the location where you extracted the package. Start Enterprise Search by running the following command:

   ```sh
   ./bin/enterprise-search
   ```
   {: pre}
   
1. To ensure that Enterprise Search keeps running, consider running it as a background process. Do this with `&`, using a command like:
   
   ```sh
   ./bin/enterprise-search &
   ``` 
   {: pre}
   
   You can also use the `nohup` command:
   
   ```sh
   nohup <COMMAND> &
   ```
   {: pre}
   
   `<COMMAND>` is the command that you want to run in the background.

## Start Enterprise Search through Docker Run
{: #tutorial-elasticsearch-enterprise-search-tutorial-docker-run}
{: step}

In addition to the standard installation, start Enterprise Search by using Docker by following these steps:

1. Place CA Certificate: Copy the CA certificate into ```certs/ca/ca.crt``` within your project folder.
1. Add the following: 
1. Run Docker Command:

   Run the following command in your project folder to start Enterprise Search with Docker:

   ```sh
   docker run \                                                                                       
   --name "enterprise-search" \
   --network "elastic" \
   --publish "3002:3002" \
   --volume "./certs:/usr/share/enterprise-search/es-config/certs:ro" \
   --interactive \
   --tty \
   --rm \
   --env "allow_es_settings_modification=true" \
   --env "elasticsearch.ssl.enabled=true" \
   --env "elasticsearch.ssl.certificate_authority=/usr/share/enterprise-search/es-config/certs/ca/ca.crt" \
   --env "elasticsearch.host=${ELASTICSEARCH_HOST} \
   --env "elasticsearch.username=${ELASTICSEARCH_USERNAME}" \
   --env "elasticsearch.password=${ELASTICSEARCH_PASSWORD}" \
   --env "kibana.external_url=${KIBANA_HOST}" \
   --env "secret_management.encryption_keys=[${AWESOME_SECRET}]" \
   "docker.elastic.co/enterprise-search/enterprise-search:7.17.7"
   ```
   {: pre}

   Replace placeholders with your actual values.

## Starting Enterprise Search AND Kibana through Docker Compose
{: #tutorial-elasticsearch-enterprise-search-tutorial-docker-compose}
{: step}

You can start Kibana and Enterprise Search together with docker-compose. Follow these steps:

1. Place CA Certificate: Copy the CA certificate into `certs/ca/ca.crt` within your project folder.
1. Add the following to the `.env` file in your project directory:

   ```sh
   ELASTIC_VERSION=7.17.7
   ELASTICSEARCH_HOSTS=${ELASTICSEARCH_HOST}
   ELASTICSEARCH_PASSWORD=${ELASTICSEARCH_PASSWORD}
   ELASTICSEARCH_USERNAME=${ELASTICSEARCH_USERNAME}
   ENCRYPTION_KEYS=${AWESOME_SECRET}
   KIBANA_HOST=http://kibana:5601
   ENTERPRISE_SEARCH_HOST=http://enterprisesearch:3002
   ```
   {: pre}

1. Create a `docker-compose` file and add the following code to it:

   ```sh
   version: "2.2"
   services:
     kibana:
       image: docker.elastic.co/kibana/kibana:${ELASTIC_VERSION}
       volumes:
         - ./certs:/usr/share/kibana/config/certs
       ports:
         - 5601:5601
       environment:
         - SERVERNAME=kibana
         - ELASTICSEARCH_HOSTS=${ELASTICSEARCH_HOSTS}
         - ELASTICSEARCH_USERNAME=${ELASTICSEARCH_USERNAME}
         - ELASTICSEARCH_PASSWORD=${ELASTICSEARCH_PASSWORD}
         - ELASTICSEARCH_SSL_CERTIFICATEAUTHORITIES=config/certs/ca/ca.crt
         - ENTERPRISE_SEARCH_HOST=${ENTERPRISE_SEARCH_HOST}
   
     enterprisesearch:
       depends_on:
         kibana:
           condition: service_healthy
       image: docker.elastic.co/enterprise-search/enterprise-search:${ELASTIC_VERSION}
       volumes:
         - ./certs:/usr/share/enterprise-search/config/certs
       ports:
         - 3002:3002
       environment:
         - SERVERNAME=enterprisesearch
         - secret_management.encryption_keys=[${AWESOME_SECRET}]
         - allow_es_settings_modification=true
         - elasticsearch.host=${ELASTICSEARCH_HOSTS}
         - elasticsearch.username=${ELASTICSEARCH_USERNAME}
         - elasticsearch.password=${ELASTICSEARCH_PASSWORD}
         - elasticsearch.ssl.enabled=true
         - elasticsearch.ssl.certificate_authority=/usr/share/enterprise-search/config/certs/ca/ca.crt
         - kibana.external_url=${KIBANA_HOST}
   ```
   {: pre}

The placeholders in the `docker-compose.yml` file are automatically picked up from the `.env` file. Make sure the `.env` file is updated with actual details.

If you already have Kibana up and running, make sure to update the enterprise host url in the kibana.yml config.

## Verify Integration:
{: #tutorial-elasticsearch-enterprise-search-tutorial-verify-integration}
{: step}

After Enterprise Search is up and running, access it through your Kibana dashboard.

Remember, this guide provides a high-level overview of the setup process. For more information, see the official documentation for more detailed information and troubleshooting assistance. Happy searching!
