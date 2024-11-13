---
copyright:
  years: 2018, 2024
lastupdated: "2024-11-08"

keywords: kibana, elasticsearch container, elasticsearch getting started

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.databases-for-elasticsearch}}
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-completion-time="30m"}

This tutorial guides you through the steps to quickly start using an {{site.data.keyword.databases-for-elasticsearch_full}} deployment by provisioning an instance, setting your Admin password and connecting to it.
{: shortdesc}

Follow these steps to complete the tutorial: {: ui}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision through the console](#provision_instance_ui)
* [Step 3: Set your Admin password through the console](#admin_pw)
* [Step 4: Connect to your instance](#elastic_connect)
* [Next steps](#next_steps)
{: ui}

Follow these steps to complete the tutorial: {: cli}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision through the CLI](#provision_instance_cli)
* [Step 3: Set your Admin password through the CLI](#admin_pw)
* [Step 4: Connect to your instance](#elastic_connect)
* [Next steps](#next_steps)
{: cli}

Follow these steps to complete the tutorial: {: api}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision through the API](#provision_instance_api)
* [Step 3: Set your Admin password](#admin_pw)
* [Step 4: Connect to your instance](#elastic_connect)
* [Next steps](#next_steps)
{: api}

Follow these steps to complete the tutorial: {: terraform}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision through Terraform](#provision_instance_tf)
* [Step 3: Set your Admin password](#admin_pw)
* [Step 4: Connect to your instance](#elastic_connect)
* [Next steps](#next_steps)
{: terraform}


## Before you begin
{: #prereqs}

* You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.

## Step 1: Choose your plan
{: #choose_plan}

{{site.data.keyword.databases-for-elasticsearch}} offers two different plans:

* {{site.data.keyword.databases-for-elasticsearch}} **Enterprise** deploys the Basic version of Elasticsearch.

* {{site.data.keyword.databases-for-elasticsearch}} **Platinum** deploys the Platinum version of Elasticsearch.

Both plans provide you with a fully managed and scalable Elasticsearch service, allowing you to focus on your applications and data rather than the underlying infrastructure.

### Using APIs
{: #using_apis}
{: api}

Use the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction){: external} to work with your {{site.data.keyword.databases-for-mongodb}} instance. The resource controller API is used to [provision an instance](#provision_instance_api).

You will need an API key to perform actions via the API. Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an IBM Cloud API key that enables you to use the API to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

## Step 2: Provision through the console
{: #provision_instance_ui}
{: ui}

1. Log in to the {{site.data.keyword.cloud_notm}} console.
1. Click the [{{site.data.keyword.databases-for-elasticsearch}} service](https://cloud.ibm.com/catalog/databases-for-elasticsearch){: external} in the **catalog**.

1. Follow [these steps](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-provisioning&interface=ui) to provision a {{site.data.keyword.databases-for-elasticsearch}} instance.

1. When your instance is provisioned, click the instance name to view more information.

## Step 2: Provision through the CLI
{: #provision_instance_cli}
{: cli}

You can provision a {{site.data.keyword.databases-for-elasticsearch}} instance by using the CLI. If you don't already have it, you need to install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

You can follow [these steps](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-provisioning&interface=cli) to provision an {{site.data.keyword.databases-for-elasticsearch}} instance.

## Step 2: Provision through the resource controller API
{: #provision_instance_api}
{: api}

Follow [these steps](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-provisioning&interface=api) to provision an {{site.data.keyword.databases-for-elasticsearch}} instance using the Resource Controller API.

## Step 2: Provision through Terraform
{: #provision_instance_tf}
{: terraform}

You need an API key to perform actions via Terraform. Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an IBM Cloud API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

Once you have an API Key, follow [these steps](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-provisioning&interface=terraform) to provision an {{site.data.keyword.databases-for-elasticsearch}} instance using Terraform.

## Step 3: Set the admin password
{: #admin_pw}

### The admin user
{: #admin_pw_admin_user}

When you provision a {{site.data.keyword.databases-for-mongodb}} deployment, an `admin` user is automatically created.

Set the admin password before using it to connect.
{: important}

### Set the admin password through the UI
{: #admin_pw_set_ui}
{: ui}

Set your admin password through the UI by selecting your instance from the [{{site.data.keyword.cloud_notm}} Resource list](https://cloud.ibm.com/resources){: external}. Then, select **Settings**. Next, select *Change Database Admin password*.

### Set the admin password through the CLI
{: #admin_pw_set_cli}
{: cli}

Use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI {{site.data.keyword.databases-for}} plug-in to set the admin password.

For example, to set the admin password for your deployment, use the following command:

```sh
ibmcloud cdb user-password <INSTANCE_NAME_OR_CRN> admin <NEWPASSWORD>
```
{: pre}

### Set the admin password through the API
{: #admin_pw_set_api}
{: api}

YOu can use the `id` parameter obtained in the response to Step 2 above with the [Set specified user's password](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#updateuser){: external} endpoint to set the admin password.

```sh
curl -X PATCH -H "Authorization: Bearer <TOKEN>" \
     -H 'Content-Type: application/json' \
     -d '{"password":"newrootpasswordsupersecure21"}' \
      "https://api.<REGION>.databases.cloud.ibm.com/v5/ibm/deployments/<DEPLOYMENT_ID>/users/database/admin"
```
{: pre}

The `id` parameter needs to be URL-encoded for the above API call to work.
{: important}

### Setting the admin password through Terraform
{: terraform}

The admin password is passed in as one of the database resource parameters in the Terraform script. There is no need for any further action.

## Step 4: Connect to your {{site.data.keyword.databases-for-elasticsearch}} instance
{: #elastic_connect}

Connect to your deployment using [Kibana](https://www.elastic.co/guide/en/kibana/current/index.html){: external}, an open source tool that adds visualization capabilities to your Elasticsearch database. This tutorial runs Kibana in a Docker container by using the Kibana image from the Docker image repository.

### Before you begin

- Install [Docker](https://www.docker.com/) so that you can pull the Kibana container image to connect to {{site.data.keyword.databases-for-elasticsearch}}.
- If you prefer to avoid running Kibana locally and install Docker, you can also deploy Kibana using {{site.data.keyword.codeenginefull}}. For more information, see [Deploy Kibana using {{site.data.keyword.codeengineshort}} and connect to your {{site.data.keyword.databases-for-elasticsearch}} instance](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-kibana-code-engine-icd-elasticsearch){: external}.

To connect, Kibana needs the username, password, URL, and port for your Elasticsearch deployment. It also needs the Elasticsearch TLS certificate to access the database. To get this, copy the certificate information from the _Endpoints_ section on the _Overview_ page of your created Elasticsearch instance. Then, download the certificate to a local folder. You can use the name that is provided in the download, or your own file name.

Remember where you save the certificate on your file system. If you are running Kibana locally, not in Docker, then the certificate goes in `$KIBANA_HOME/config/<filename>`.
{: important}

### Set up Kibana
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

### Run the Kibana Container
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

## Next steps
{: #next_steps}

For more information, see the [Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html){: external}.

Looking for more tools on managing your databases and data? You can connect to your deployment with the [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli), the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you plan to use {{site.data.keyword.databases-for-elasticsearch}} for your applications, check out [Connecting an external application](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-external-app) and [Connecting an IBM Cloud application](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-ibmcloud-app).

To ensure the stability of your applications and your database, check out [High-availability](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-high-availability) and [Performance](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-performance).
