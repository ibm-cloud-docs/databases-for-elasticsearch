---
copyright:
  years: 2018,2020
lastupdated: "2020-03-31"

keywords: elasticsearch-py, java, elasticsearch driver,

subcollection: databases-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:generic: .ph data-hd-programlang='generic'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:pre: .pre}

# Connecting an external application
{: #external-app}

Your applications and drivers use connection strings to make a connection to {{site.data.keyword.databases-for-elasticsearch_full}}. Each deployment has connection strings specifically for drivers and applications. Connection strings are displayed in the _Connections_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The connection strings can be used by any of the credentials you have created on your deployment. While you can use the admin user for all of your connections and applications, it might be better to create users specifically for your applications to connect with. Documentation on creating users is on the [Creating Users and Getting Connection Strings](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-connection-strings) page.

## Connecting with a language's driver

All the information a driver needs to make a connection to your deployment is in the "https" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Type`||Type of connection - for Elasticsearch, it is "uri"
`Scheme`||Scheme for a URI - for Elasticsearch, it is "https"
`Path`||Path for a uri
`Authentication`|`Username`|The username that you use to connect.
`Authentication`|`Password`|A password for the user - might be shown as `$PASSWORD`
`Authentication`|`Method`|How authentication takes place; "direct" authentication is handled by the driver.
`Hosts`|`0...`|A hostname and port to connect to
`Composed`|`0...`|A URI combining Scheme, Authentication, Host, and Path
`Certificate`|`Name`|The allocated name for the self-signed certificate for database deployment
`Certificate`|Base64|A base64 encoded version of the certificate.
{: caption="Table 1. `https`/`URI` connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.



Many Elasticsearch drivers are able to make a connection to your deployment when given the URI-formatted connection string found in the "composed" field of the connection information. For example,
{: generic}
```
https://admin:$PASSWORD@d5eeee66-5bc4-498a-b73b-1307848f1eac.8f7bfd8f3faa4218aec56e069eb46187.databases.appdomain.cloud:31821
```
{: generic}


This example uses Java to connect.
{: java}
```java
import io.searchbox.client.JestClientFactory;
import io.searchbox.client.JestResult;
import io.searchbox.client.config.HttpClientConfig;
import io.searchbox.client.JestClient;
import io.searchbox.cluster.Health;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.ssl.SSLContextBuilder;

import javax.net.ssl.SSLContext;
import java.io.File;
import java.security.*;
import java.security.cert.CertificateException;


import java.io.IOException;


public class ESConnect {

    public static void main(String[] args) {

        // Add CA cert to truststore using something like:
        //  keytool -import -alias mycert -file /path/to/cert -keystore ./mycert -storetype pkcs12 -storepass mysecret
        // and use the path of the keystore below
        File truststore = new File("/Users/code/java-example/icdcerts");

        try {
            // use your secret and add the variable containing the truststore that you created above with the secret as a CharArray
            SSLContext sslContext = new SSLContextBuilder().loadTrustMaterial(truststore, "mysecret".toCharArray()).build();

            SSLConnectionSocketFactory sslSocketFactory = new SSLConnectionSocketFactory(sslContext);

            // set up a Jest factory
            JestClientFactory factory = new JestClientFactory();

            //configure and build Jest HTTP client with IBM Cloud Databases for Elasticsearch connection strings
            factory.setHttpClientConfig(
                    // add the Elasticsearch host and port
                    new HttpClientConfig.Builder("https://60d1b41b-2478-4767-9fc0-d99b1d00b6d1.bkvfu0nd0m8k95k94ujg.databases.appdomain.cloud:31347")
                            .multiThreaded(true)
                            // Add the credentials username and password
                            .defaultCredentials("admin", "mypassword")
                            .sslSocketFactory(sslSocketFactory)
                            .build());

            // create a JestClient
            JestClient client = factory.getObject();
            // create the call for the cluster health
            Health health = new Health.Builder().build();
            // get the cluster health as a JestResult
            JestResult result = client.execute(health);
            // print out the cluster's health
            System.out.printf("\n\n<------ CLUSTER HEALTH ------>\n%s\n\n", result.getJsonObject());
            // shutdown the connection
            client.close();

        } catch (IOException | KeyStoreException | NoSuchAlgorithmException | KeyManagementException | CertificateException e) {
            e.printStackTrace();
        }

    }

}
```
{: java}



This example uses the Python library [`elasticsearch-py`](https://www.elastic.co/guide/en/elasticsearch/client/python-api/current/index.html) to connect.
{: python}
```python
from elasticsearch import Elasticsearch
from ssl import create_default_context


context = create_default_context(cafile="path/to/cert.pem")

es = Elasticsearch(
    ['60d1b41b-2478-4767-9fc0-d99b1d00b6d1.bkvfu0nd0m8k95k94ujg.databases.appdomain.cloud'],
    http_auth=('admin', 'password'),
    port='31347',
    ssl_context=context
)

health = es.cluster.health()
print(health)
```
{: python}


## Driver TLS and self-signed certificate support

All connections to {{site.data.keyword.databases-for-elasticsearch}} are TLS 1.2 enabled, so the driver you use to connect needs to be able to support encryption. Your deployment also comes with a self-signed certificate so the driver can verify the server upon connection. 

### Using the self-signed certificate

1. Copy the certificate information from the _Connections_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the driver or client.

### CLI plug-in support for the self-signed certificate

You can display the decoded certificate for your deployment with the CLI plug-in with the command `ibmcloud cdb deployment-cacert "your-service-name"`. It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the driver.

## Other Drivers

Elasticsearch has a vast array of language drivers. The table covers a few of the most common.

Language|Driver|Documentation
----------|-----------
Node|`elasticsearch-js`|[Link](https://github.com/elastic/elasticsearch-js)
Ruby|`elasticsearch-ruby`|[Link](https://github.com/elastic/elasticsearch-ruby)
Ruby on Rails|elasticsearch-rails|[Link](https://github.com/elastic/elasticsearch-rails)
Python|`elasticsearch-py`|[Link](https://www.elastic.co/guide/en/elasticsearch/client/python-api/current/index.html)
Java|`Jest`|[Link](https://github.com/searchbox-io/Jest/tree/master/jest)
Go|`elastic`|[Link](https://olivere.github.io/elastic/)
{: caption="Table 2. Common Elasticsearch drivers" caption-side="top"}






 
