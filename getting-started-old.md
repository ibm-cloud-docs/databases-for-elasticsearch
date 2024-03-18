---
copyright:
  years: 2018, 2024
lastupdated: "2024-03-18"

keywords: kibana, elasticsearch container, elasticsearch getting started

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting Started
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-completion-time="30m"}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-elasticsearch_full}} deployment. Connect to your deployment using [Kibana](https://www.elastic.co/guide/en/kibana/current/index.html){: external}, an open source tool that adds visualization capabilities to your Elasticsearch database. This tutorial runs Kibana in a Docker container by using the Kibana image from the Docker image repository.

## Before you begin
{: #before-begin}

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- You also need a {{site.data.keyword.databases-for-elasticsearch}} deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/databases-for-elasticsearch). Give your deployment a memorable name that appears in your account's Resource List.
- [Set the Admin Password](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-user-management&interface=ui#user-management-set-admin-password-ui) for your deployment.
- Install [Docker](https://www.docker.com/) so that you can pull the Kibana container image to connect to {{site.data.keyword.databases-for-elasticsearch}}.
- If you prefer to avoid running Kibana locally and install Docker, you can also deploy Kibana using {{site.data.keyword.codeenginefull}}. For more information, see [Deploy Kibana using {{site.data.keyword.codeengineshort}} and connect to your {{site.data.keyword.databases-for-elasticsearch}} instance](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-kibana-code-engine-icd-elasticsearch){: external}.

## Connect to your deployment
{: #connection-info-deployment}
{: step}

Your deployment's _Overview_ shows all the relevant connection information.

To connect, Kibana needs the username, password, url, and port for your Elasticsearch deployment. It also needs the Elasticsearch CA certificate to access the database. To get this, copy the certificate information from the _IBM Cloud Platform Endpoints_ section within the created Elasticsearch instance. Then, download the certificate to a local folder. You can use the name that is provided in the download, or your own file name.

Remember where you save the certificate on your file system. If you are running Kibana locally, not in Docker, then the certificate goes in `$KIBANA_HOME/config/<filename>`.
{: important}

## Set up Kibana
{: #kibana}
{: step}

Before running the Docker container that includes Kibana, create a configuration file in the same folder with the downloaded Elasticsearch certificate from Step 1. The configuration file will contain some basic Kibana settings as follows.

Create a YAML file called `kibana.yml`. Inside the file, you need the following Kibana configuration settings:
```yaml
elasticsearch.ssl.certificateAuthorities: "/usr/share/kibana/config/cacert"
elasticsearch.username: "admin"
elasticsearch.password: "<password>"
elasticsearch.hosts: ["https://<hostname:port>"]
server.name: "kibana"
server.host: "0.0.0.0"
```

The first setting, `elasticsearch.ssl.certificateAuthorities`, is the location where Docker will store the Elasticsearch certificate. It gets placed in this location when you first run Docker. You can change it to a location of your choice, but the example path is the Kibana’s configuration directory. Ensure that the certificate name (in our example "cert") within the `kibana.yml` and the certificate name file stored in Step 1 have the same name.

Next, is `elasticsearch.username` and `elasticsearch.password`. Use the deployment's admin username and password. Be sure that you set the admin password before trying to connect. For `elasticsearch.hosts`, enter the deployment's hostname and port, which is separated by a `:`.

Lastly, `server.name` is a machine-readable name for the Kibana instance and `server.hosts` is the host of the backend server where you can connect to Kibana in your web browser.

These settings are just a simplified example to get started. For more infortmation, see [Configure Kibana](https://www.elastic.co/guide/en/kibana/current/settings.html){: external}.

If you are running Kibana locally, not in Docker, then the YAML file goes in `$KIBANA_HOME/config/kibana.yml`, where Kibana reads its configuration.

## Run the Kibana Container
{: #running-kibana-container}
{: step}

Now that the `kibana.yml` file is set up, use Docker to attach the YAML file and your certificate file to the Docker container, while pulling the `<kibana_version>` image from the Docker image repository.

Use an image with a version of Kibana that is compatible with the version of Elasticsearch that your deployment is running. Retrieve the Elasticsearch version from the `https_endpoint` API endpoint by using your preferred http client. For more information, see the [Elasticsearch compatibility matrix](https://www.elastic.co/support/matrix#matrix_compatibility){: external}.
{: .important}

Here is an example with curl. If you don't have the certificate that is installed, use the `--insecure` flag to disable peer verification. The `<http_endpoint>` can be found in your instance's _Endpoints_ UI:

```sh
curl --cacert <path-to-cert> <https_endpoint>
```
{: pre}

Next, run the Docker command in your terminal to start the Kibana container.
```sh
docker container run -it --name kibana \
-v <path_to_config_folder_created_in_step_1>:/usr/share/kibana/config \
-p 5601:5601 docker.elastic.co/kibana/kibana:<kibana_version>
```
{: pre}

The Docker command has one volume that is attached with the `-v` flag. These are mounted to the Kibana container at the path `/usr/share/kibana/config/`, which is a configuration directory where Kibana looks for configuration files.
- The `-p` specifies which port is exposed from the container, and the port you use to access Kibana.
- The Kibana version should correspond to the version of Elasticsearch you are using.

When you run the command from your terminal, it downloads the Kibana Docker image and runs Kibana.
Once Kibana has connected to your {{site.data.keyword.databases-for-elasticsearch}} deployment and is running successfully, you see the output in your terminal.
```text
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

Visit `http://0.0.0.0:5601` in your browser to see Kibana. `0.0.0.0` is the `server.host` in `kibana.yml` and `5601` is the port that is exposed from the container. Once you go to the URL, a pop-up window prompts you for your username and password. Use the admin credentials, or any other credentials that you made, to access to your deployment. The credentials don't have to be the same username and password you provided in the `kibana.yml` file.

## Next Steps
{: #elasticsearch-next-steps}

For more information, see the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html){: external}.

Looking for more tools on managing your databases and data? You can connect to your deployment with the [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli), the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you plan to use {{site.data.keyword.databases-for-elasticsearch}} for your applications, check out [Connecting an external application](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-external-app) and [Connecting an IBM Cloud application](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-ibmcloud-app).

To ensure the stability of your applications and your database, check out [High-Availability](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-high-availability) and [Performance](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-performance).
