---
copyright:
  years: 2018
lastupdated: "2018-11-27"

subcollection: databases-for-elasticsearch

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Getting Started
{: #getting-started}

This tutorial uses a [sample app](https://github.com/IBM-Cloud/clouddatabases-elasticsearch-helloworld-nodejs) to demonstrate how to connect a Cloud Foundry application in {{site.data.keyword.cloud_notm}} to an {{site.data.keyword.databases-for-elasticsearch_full}} service. The application creates, reads from, and writes to a database that uses data that is supplied through the app's web interface.
{: shortdesc}

If you have already created your deployment and just want to connect to your Elasticsearch databases, you can skip to [getting your connection strings](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-connection-strings) and [connecting with cURL](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-connecting-curl).
{: .tip}

## Before you begin

Make sure that you have an [{{site.data.keyword.cloud_notm}} account][ibm_cloud_signup_url]{:new_window}.

You also need to install [Node.js](https://nodejs.org/) and [Git](https://git-scm.com/downloads).

## Step 1. Create a {{site.data.keyword.databases-for-elasticsearch}} service instance
{: #create-service}

You can create a {{site.data.keyword.databases-for-elasticsearch}} service from the [{{site.data.keyword.databases-for-elasticsearch}} page](https://cloud.ibm.com/catalog/services/databases-for-elasticsearch/) in the {{site.data.keyword.cloud_notm}} catalog.

Choose a service name, region, organization and space to provision the service in, and for the **Select a database version** field, choose _Latest Preferred Version_. In this example, the service name is "example-elasticsearch".

Click **Create** to provision your service. Provisioning can take a while to complete. You are taken back to your {{site.data.keyword.cloud_notm}} _Dashboard_ while the service is provisioning. 

You can not connect an application to the service until provisioning has completed.
{: .tip}

## Step 2. Clone the Hello World sample app from GitHub

Clone the Hello World app to your local environment from your terminal by using the following command:

```
git clone https://github.com/IBM-Cloud/clouddatabases-elasticsearch-helloworld-nodejs.git
```

## Step 3. Install the app dependencies

Use npm to install dependencies.

1. From your terminal, change the directory to where the sample app is located.
  
  ```
  cd clouddatabases-elasticsearch-helloworld-nodejs
  ```

2. Install the dependencies listed in the `package.json` file.
  
  ```
  npm install
  ```

## Step 4. Download and install the {{site.data.keyword.cloud_notm}} CLI tool

The {{site.data.keyword.cloud_notm}} CLI tool is what you use to communicate with {{site.data.keyword.cloud_notm}} from your terminal or command line. For more information, see [Download and install {{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli).

## Step 5. Connect to {{site.data.keyword.cloud_notm}}

1. Connect to {{site.data.keyword.cloud_notm}} in the command line tool and follow the prompts to log in.

  ```
  ibmcloud login
  ```

  If you have a federated user ID, use the `ibmcloud login --sso` command to log in with your single sign-on ID. See [Logging in with a federated ID](/docs/iam?topic=iam-federated_id) to learn more.
  {: .tip}

2. Make sure that you are targeting the correct {{site.data.keyword.cloud_notm}} org and space.

  ```
  ibmcloud target --cf
  ```

  Choose from the options provided, by using the same values that you used when you created the service.

## Step 6. Create a Cloud Foundry alias for the database service.
{: #create-alias}

Make the database service discoverable by Cloud Foundry applications by giving it a Cloud Foundry alias. 

`ibmcloud resource service-alias-create alias-name --instance-name instance-name`

The alias name can be the same as the database service instance name. For example, use this command for database that was created in step 1.

`ibmcloud resource service-alias-create example-elasticsearch --instance-name example-elasticsearch`

## Step 7. Update the app's manifest file
{: #update-manifest}

{{site.data.keyword.cloud_notm}} uses a manifest file - `manifest.yml` to associate an application with a service. Follow these steps to create your manifest file.

1. In an editor, open a new file and add the following text:

  ```
  ---
  applications:
  - name:    example-helloworld-nodejs
    routes:
    - route: example-helloworld-nodejs.us-south.cf.appdomain.cloud
    memory:  128M
    services:
      - example-elasticsearch
  ```

2. Change the `route` value to something unique. The route that you choose determines the subdomain of your application's URL:  `<route>.{region}.cf.appdomain.cloud`. Be sure the `{region}` matches where your application is deployed.
3. Change the `name` value. The name that you choose is displayed in your {{site.data.keyword.cloud_notm}} dashboard.
4. Update the `services` value to match the alias of the service you created in [Create a Cloud Foundry alias for the database service](#create-alias).

## Step 8. Push the app to {{site.data.keyword.cloud_notm}}.

This step fails if the service is not finished provisioning from Step 1. You can check its progress on your {{site.data.keyword.cloud_notm}} _Dashboard_.
{: .tip}

When you push the app, it is automatically bound to the service specified in the manifest file.

```
ibmcloud cf push
```

## Step 9. Check that the app is connected to your {{site.data.keyword.databases-for-elasticsearch}} service

1. Go to your {{site.data.keyword.databases-for-elasticsearch}} service dashboard
2. Select _Connections_ from the dashboard menu. Your application is listed under _Connected Applications_.

If your application is not listed, repeat Steps 7 and 8, making sure that you entered the correct details in [manifest.yml](#update-manifest).

## Step 10. Use the app

Now, when you visit `<route>.{region}.cf.appdomain.cloud/` you can see the contents of your {{site.data.keyword.databases-for-elasticsearch}} collection. As you add words and their definitions, they are added to the database and displayed. If you stop and restart the app, you see any words and definitions that were already added are now listed.

## Running the app locally

Instead of pushing the app into {{site.data.keyword.cloud_notm}} you can run it locally to test the connection to your {{site.data.keyword.databases-for-elasticsearch}} service instance. To connect to the service, you need to create a set of service credentials.

2. Select _Service Credentials_ from the main menu to open the Service Credentials view.
3. Click **New Credential**.
4. Choose a name for your credentials and click **Add**.
5. Your new credentials are now listed. Click **View credentials** in the corresponding row of the table to view the credentials, and click the **Copy** icon to copy your credentials.
6. In your editor of choice, create a new file with the following, inserting your credentials as shown:

  ```
  {
    "services": {
      "databases-for-elasticsearch": [
        {
          "credentials": INSERT YOUR CREDENTIALS HERE
        }
      ]
    }
  }
  ```
7. Save the file as `vcap-local.json` in the directory where the sample app is located.

To avoid accidentally exposing your credentials when you push an application to GitHub or {{site.data.keyword.cloud_notm}}, make sure that the file that contains your credentials is listed in the relevant ignore file. If you open `.cfignore` and `.gitignore` in your application directory, you can see that `vcap-local.json` is listed in both. It is not included in the files that are uploaded when you push the app to either GitHub or {{site.data.keyword.cloud_notm}}.
{: .tip}

Now start the local server.
```
npm start
```

The app is now running at http://localhost:8080. You can add words and definitions to your {{site.data.keyword.databases-for-elasticsearch}} database. When you stop and restart the app, any words you added are displayed when you refresh the page.

## Next steps

To understand more about how the [sample app](https://github.com/IBM-Cloud/clouddatabases-elasticsearch-helloworld-nodejs) works, you can read the application's readme file, or the code comments in `server.js`, which give some information about the app's functions.

To start exploring your {{site.data.keyword.databases-for-elasticsearch}} service, see the following topics about the service dashboard:

- [Dashboard Overview](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-dashboard-overview)
- [Backups](/docs/services/databases-for-elasticsearch?topic=cloud-databases-dashboard-backups)
- [Creating Users and Getting Connection Strings](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-connection-strings)


[ibm_cloud_signup_url]: https://ibm.biz/databases-for-elasticsearch-signup
