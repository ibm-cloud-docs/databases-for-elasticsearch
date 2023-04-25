---
copyright:
  years: 2018, 2023
lastupdated: "2023-04-25"

keywords: kibana, elasticsearch container, elasticsearch getting started

subcollection: databases-for-elasticsearch

content-type: tutorial
services: 
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting Started
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-elasticsearch_full}} deployment. You will connect to your deployment using [Kibana](https://www.elastic.co/guide/en/kibana/current/index.html){: external}, an open source tool that adds visualization capabilities to your Elasticsearch database. This tutorial runs Kibana in a Docker container, by using the Kibana image from the Docker image repository. You can also [download and install Kibana](https://www.elastic.co/guide/en/kibana/current/install.html) to run a system you control. 

## Before you begin
{: #before-begin}

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- And a {{site.data.keyword.databases-for-elasticsearch}} deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/databases-for-elasticsearch). Give your deployment a memorable name that appears in your account's Resource List.
- [Set the Admin Password](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-admin-password) for your deployment.
- Install [Docker](https://www.docker.com/) so that you can pull the Kibana container image to connect to Databases for Elasticsearch.

Review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-elasticsearch_full}} deployment.

## Connection to your deployment
{: #connection-info-deployment}
{: step}

Your deployment's _Overview_ page shows all the relevant connection information.

![Endpoints panel](images/getting-started-endpoints-panel.png){: caption="Figure 1. Endpoints panel" caption-side="bottom"}

To connect, Kibana needs the username, password, url, and port.

It also needs the CA certificate to access the database. 
   1. Copy the certificate information from _Endpoints_.
   2. Save the certificate to a file. You can use the name that is provided in the download, or your own file name.

Remember where you save the certificate on your file system. If you are running Kibana locally (not in Docker), then the certificate should go into `$KIBANA_HOME/config/<filename>`.
{: important}

## Set up Kibana
{: #kibana}
{: step}

Before running the Docker container that includes Kibana, create a configuration file that contains some basic Kibana settings.

Create a YAML file called `kibana.yml`. Inside the file, you need the following Kibana configuration settings:
```yaml
elasticsearch.ssl.certificateAuthorities: "/usr/share/kibana/config/cacert"
elasticsearch.username: "admin"
elasticsearch.password: "<password>"
elasticsearch.hosts: ["https://<hostname:port>"]
server.name: "kibana"
server.host: "0.0.0.0"
```

The first setting, `elasticsearch.ssl.certificateAuthorities`, is the location where the deployment's certificate lives in the Docker container. It gets placed in this location when you first run Docker. You can change this to a location of your choice, but the example path is the Kibana’s config directory.

Next, is `elasticsearch.username` and `elasticsearch.password`. Use the deployment's admin username and password. Be sure that you set the admin password before trying to connect. For `elasticsearch.url` enter the deployment's hostname and port, which is separated by a `:`. 

Finally, there is `server.name`, which is a machine-readable name for the Kibana instance, and `server.host`, which is the host of the backend server and where you can connect to Kibana in your web browser.

The settings are just a simplified example to get started. See the [Kibana documentation](https://www.elastic.co/guide/en/kibana/current/settings.html){: external} for more configuration settings that you can set for your use case.

If you are running Kibana locally (not in Docker), then the yaml file goes in `$KIBANA_HOME/config/kibana.yml`, where Kibana reads its configuration from.

## Run the Kibana Container
{: #running-kibana-container}
{: step}

Now that the `kibana.yml` file is set up, use Docker to attach the yaml file and your certificate file to the Docker container, while pulling the `<kibana_version>` image from the Docker image repository. 

Match the Kibana Version with the appropriate Elasticsearch version, for v7.9.2 it must be 7.9.2. Retrieve the Elasticsearch version from the `https_endpoint` API endpoint, by using your preferred http client. Here is an example with curl (if you don't have the certificate that is installed, use the `--insecure` flag to disable peer verification). The referenced `<http_endpoint>` can be found in the _Endpoints_ panel from your instance:

```sh
curl --cacert <path-to-cert> <https_endpoint>
```

Use an image with a version of Kibana that is compatible with the version of Elasticsearch that your deployment is running. Refer to the Elasticsearch [compatibility matrix](https://www.elastic.co/support/matrix#matrix_compatibility){: external}.
{: .tip}

Run the Docker command in your terminal to start the Kibana container.
```sh
docker container run -it --name kibana \
-v <path_to_config_folder>:/usr/share/kibana/config \
-p 5601:5601 docker.elastic.co/kibana/kibana-oss:<kibana_version>
```

The Docker command has one volume that is attached with the `-v` flag. These are mounted to the Kibana container at the path `/usr/share/kibana/config/`, which is a configuration directory that Kibana looks at for configuration files. 
- The `-p` specifies which port is exposed from the container, and the port you use to access Kibana.
- The Kibana version should correspond to the version of Elasticsearch you are using.

When you run the command from your terminal, it downloads the Kibana Docker image and runs Kibana. 
Once Kibana has connected to your {{site.data.keyword.databases-for-elasticsearch}} deployment and is running successfully, you see the output in your terminal.
```sh
log   [01:19:31.839] [info][status][plugin:<kibana_version>] Status changed from uninitialized to green - Ready
log   [01:19:31.925] [info][status][plugin:elasticsearch@<kibana_version>] Status changed from uninitialized to yellow - Waiting for Elasticsearch
log   [01:19:32.120] [info][status][plugin:timelion@<kibana_version>] Status changed from uninitialized to green - Ready
log   [01:19:32.134] [info][status][plugin:console@<kibana_version>] Status changed from uninitialized to green - Ready
log   [01:19:32.147] [info][status][plugin:metrics@<kibana_version>] Status changed from uninitialized to green - Ready
log   [01:19:33.132] [info][status][plugin:elasticsearch@<kibana_version>] Status changed from yellow to green - Ready
log   [01:19:33.378] [info][listening] Server running at http://0.0.0.0:5601
```

If you don't want to see the output of Kibana in your terminal, use the `-d` flag to detach the container.
{: .tip}

Visit `http://0.0.0.0:5601` in your browser to see Kibana. (`0.0.0.0` is the `server.host` in `kibana.yml` and `5601` is the port that is exposed from the container.) Once you go to the URL, a pop-up window prompts you for your username and password. Use the admin credentials or any other credentials you made that have access to your deployment. They don’t have to be the same username and password you provided in the `kibana.yml` file.

![Kibana Start Page](images/getting-started-kibana-start.png){: caption="Figure 1. Kibana start page" caption-side="bottom"}

## Next Steps
{: #elasticsearch-next-steps}

For more information, see the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html){: external}. 

Looking for more tools on managing your databases and data? You can connect to your deployment with the [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli), the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you plan to use {{site.data.keyword.databases-for-elasticsearch}} for your applications, check out [Connecting an external application](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-external-app) and [Connecting an IBM Cloud application](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-ibmcloud-app).

To ensure the stability of your applications and your database, check out [High-Availability](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-high-availability) and [Performance](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-performance).
