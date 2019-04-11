---
Copyright:
  years: 2019
lastupdated: "2019-04-08"

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Service Endpoints Integration
{: #service-endpoints}

All {{site.data.keyword.databases-for}} deployments offer integration with [{{site.data.keyword.cloud_notm}} Service Endpoints](/docs/services/service-endpoint?topic=service-endpoint-about#about). It allows you to enable connections to your deployments from the public internet and over the {{site.data.keyword.cloud_notm}} private network.

Service Endpoints are currently not available in the `eu-de` or `oslo01` regions. If your deployments are in `Frankfort` or `Oslo 01` you aren't able to use private endpoints. Deployments in all other regions are able to use Service Endpoints.
{: .tip}

## Public Endpoints

Public endpoints provide a connection to your deployment on the public network. At provision time, this is the default option for all deployments. Your environment needs to have internet access to connect to a deployment.

## Private Endpoints

A deployment with a service endpoint on the private network gets an endpoint that is not accessible from the public internet. All traffic is routed to hardware dedicated to {{site.data.keyword.databases-for}} deployments and remains on the {{site.data.keyword.cloud_notm}} private network. Once your environment has access to the {{site.data.keyword.cloud_notm}} private network, an internet connection is not required to connect to your deployment.

## Enabling Service Endpoints

If you only wish to use connections over the public internet, you do not have to enable Service Endpoints on your {{site.data.keyword.cloud_notm}} account. If you want to enable private networking on your deployments you will need to follow the instructions in the Service Endpoint documentation under [Enabling your account for using Service Endpoints using IBM Cloud CLI](/docs/services/service-endpoint?topic=service-endpoint-getting-started#cs_cli_install_steps).

Currently, enabling Service Endpoints on your account is a manual step handled by support ticket. Once you have completed the [request](/docs/services/service-endpoint?topic=service-endpoint-getting-started#cs_cli_install_steps), you can check on the status of the ticket by going to your [Support](https://cloud.ibm.com/unifiedsupport/cases/manage) page on {{site.data.keyword.cloud_notm}}

## Provisioning with Service Endpoints

To configure your deployment's endpoints on provision, use the _Endpoints_ field on the catalog page. Select from the available options:
- Public Network
- Private Network
- Both public and private network (not available for {{site.data.keyword.databases-for-mongodb}})

### Provisioning from the CLI and API

Service Endpoints are enabled through an optional parameter when you provision through the CLI and API. Provisioning is handled by the Resource Controller, and you pass the `service-endpoints` parameter one of the options `public`, `private`, or `public-and-private`. 

For more information, see the [Provisioning](/docs/services/databases-for-elasticsearch?topic=databases-for-elasticsearch-provisioning) page.

{{site.data.keyword.databases-for}} deployments except {{site.data.keyword.databases-for-mongodb}} allow for both public and private networking to be enabled at the same time.
{: .tip}

## Changing Service Endpoints

Once you have provisioned a deployment, it is possible to change your public/private service endpoints configuration, with the exception of {{site.data.keyword.databases-for-mongodb}}. In the _Settings_ tab of your deployment's dashboard there is a card for _Service Endpoints_. You can toggle which types of connections are available to your deployment.

Changing the type of endpoints available on your deployment does not cause any downtime from a database perspective, however, if you disable an endpoint that is currently being used by you or your applications, those connections are dropped.

## Credentials for Private Endpoints

You can use either public or private connection strings with any set of credentials you make on your deployment. By default, the connection strings for a set of credentials are filled with strings for connecting over a public endpoint. If you are using private endpoints, you can specify that connection strings containing the private endpoint be generated instead. 

When creating credentials in _Service Credentials_, use the either `{ "service-endpoints": "public" }` / `{ "service-endpoints": "private" }` parameters to specify which endpoint gets filled into the connection strings. 

In the API you can use the [`/deployments/{id}/users/{userid}/connections/{endpoint_type}`](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026) to retrieve connection strings for both public or private endpoints.

If you only have private endpoints on your deployments, then all new credentials have private endpoints in the connection strings.