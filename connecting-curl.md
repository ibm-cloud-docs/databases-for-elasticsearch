---
copyright:
  years: 2018, 2025
lastupdated: "2025-05-14"

keywords: connecting elasticsearch, databases, curl

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Connecting with `cURL`
{: #connecting-curl}

You can access your Elasticsearch database directly from a command-line terminal through cURL. Elasticsearch has a wide variety of REST APIs that allow for [cluster monitoring](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster.html){: external}, [index management](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices.html){: .external} and [searching](https://www.elastic.co/guide/en/elasticsearch/reference/current/search.html){: .external} within the database. 

Connection strings are displayed in the _Endpoints_ panel of your deployment's _Overview_ page, and can also be retrieved from the [cloud databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api/cloud-databases-api-v5#getconnection).

![CLI Endpoints panel](images/cli-endpoints-pane.png){: caption="Endpoints section, CLI tab" caption-side="bottom"}

The information that you need to make a connection with cURL to your deployment is also in the "CLI" section of a credential created on the *Service credentials* page. The table contains a breakdown for reference.

| Field Name | Index | Description |
| ---------- | ----- | ----------- |
| `Bin` | | The recommended binary to create a connection; in this case it is `curl`. |
| `Composed` | | A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses `Arguments` as command-line parameters.
| `Environment` | | A list of key/values you set as environment variables. |
| `Arguments` | 0... | The information that is passed as arguments to the command shown in the Bin field. |
| `Certificate` | Base64 | A service proprietary certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded. |
| `Certificate` | Name | The allocated name for the service proprietary certificate. |
| `Type` | | The type of package that uses this connection information; in this case `cli`.  |
{: caption="curl connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## Elasticsearch API `cURL` example
{: #elasticsearch-api-curl-example}

```sh
CURL_CA_BUNDLE="/path-to/your_cert_file" curl -u admin:<password> 'https://d5eeee66-5bc4-499a-b73b-1307848f1eac.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:31821/_cluster/health?pretty'
```

* `CURL_CA_BUNDLE` - curl performs SSL certificate verification by default. Since your deployment uses a service proprietary certificate, you must specify what certificate to use.
* `curl` - The command itself.  
* `-u` - The parameter for the username and password, separated by a colon, to be used as credentials to log in to the Elasticsearch deployment. 
* `https://...` - The parameter that specifies the endpoints where the `curl` command connects. It's composed of HTTPS protocol URLs and includes a port number.
* `/_cluster/health?pretty` - An Elasticsearch Cluster API endpoint that returns the status of your cluster. 

## Using the service proprietary certificate
{: #using-cert}

1. Copy the certificate information from the _Endpoints_ panel or the Base64 field of the service credential connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the `CURL_CA_BUNDLE` variable.

## CLI plug-in support for the service proprietary certificate
{: #cli-plugin-cert-support}

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the `CURL_CA_BUNDLE` variable.
