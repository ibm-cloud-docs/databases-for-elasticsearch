---

copyright:
  years: 2024
lastupdated: "2024-01-30"

keywords: machine learning, elasticsearch, artificial intelligence, ai, model, vector search, bot

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 30mins

---

{{site.data.keyword.attribute-definition-list}}

# Build an Elasticsearch chatbot
{: #build-es-chatbot}
{: toc-content-type="tutorial"}
{: toc-completion-time="30mins"}

## Objectives
{: #build-es-chatbot-objectives}

This tutorial is designed to assist you in harnessing the full potential of your data by transforming it into an intelligent bot. This bot will be capable of answering queries related to your data. It effortlessly stores the knowledge base in an Elasticsearch index and taps into AI to intelligently handle your questions related to the knowledge base. Use this AI tool to create a chatbot that can learn from your documentation.

Elastic Learned Sparse EncodeR (ELSER) is a Natural Language Processing (NLP) model trained by Elastic that enables you to perform semantic search by using sparse vector representation. Instead of literal matching on search terms, semantic search retrieves results based on the intent and the contextual meaning of a search query.

This tutorial helps you to leverage the full potential of your data by transforming it into an assistant bot. The bot will be capable of answering queries related to your data.

In this tutorial, you set up a {{site.data.keyword.databases-for-elasticsearch}} instance and then use its capabilities to crawl your website and store that knowledge base in an Elasticsearch index. You use Elastic's built-in Machine learning (ML) model ELSER to analyze and extract meaning from your data. [Elastic Learned Sparse EncodeR (ELSER)](https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-elser.html){: external} is a Natural Language Processing (NLP) model trained by Elastic that enables you to perform semantic search by using sparse vector representation. Instead of literal matching on search terms, semantic search retrieves results based on the intent and the contextual meaning of a search query.

You then use IBM watsonx to create a chatbot interface that uses this data to intelligently handle questions related to your knowledge base.

## Getting productive
{: #build-es-chatbot-prereqs}

To begin, install some must-have productivity tools:

- You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - To codify and provision infrastructure.
- [Python](https://www.python.org/downloads/){: external}
- [Docker](https://www.docker.com/get-started/){: external}
- [Provision a {{site.data.keyword.databases-for-elasticsearch}} instance](https://cloud.ibm.com/databases/databases-for-elasticsearch/create){: external} with version 8.1x.x.

Once your instance is provisioned, download the TLS certificate. Navigate to your instance's **Overview** and scroll down to the *Endpoints* section. Select **Download Certificate*.
- If required, change the Admin password. Navigate to your instance's **Overview** and scroll down to the *Change Database Admin Password* section. Select **Change Password*.

Make sure you have a running server of your data presented in HTML format. We can also use bulk API to upload data to index that may be covered in future tutorials.
{: important}

## Set up a IBM Watson X AI Project
{: #build-es-chatbot-watsonxai-project}

IBM watsonx.ai is a studio of integrated tools for working with generative AI capabilities that are powered by foundation models and for building machine learning models. The IBM watsonx.ai component provides a secure and collaborative environment where you can access your organization's trusted data, automate AI processes, and deliver AI in your applications.

Follow these steps to integrate powerful AI capabilities into your projects:

1. [Sign Up for IBM watsonx.ai as a Service](https://www.ibm.com/docs/en/watsonx-as-a-service?topic=started-signing-up-watsonx){: external}.
1. To access a range of foundation and Machine learning (ML) models, [set up a new project](https://www.ibm.com/docs/en/watsonx-as-a-service?topic=projects-creating-project#create-a-project){: external}.
1. Once your project is established, navigate to the **Projects** section, select your project, and go to *Manage* > *General* to find and copy your Project ID.
1. Insert the copied Project ID into your `.env` file, under the variable `PROJECT-ID`.

## Set up your server and connect to your {{site.data.keyword.databases-for-elasticsearch}} instance:
{: #build-es-chatbot-watsonxai-server-connect}

1. First, [clone the repo](https://github.ibm.com/Dhananjay-Meena/icd-elastic-bot){: external}.
1. See the [README file](https://github.ibm.com/Dhananjay-Meena/icd-elastic-bot/blob/main/README.md){: external} of the repo and add the `.env` file in the root of your project.

For the servers to run, run a command like:

```sh
docker-compose up
```
{: pre}

## Feed The data
{: #build-es-chatbot-feed-data}

This step feeds your data to your {{site.data.keyword.databases-for-elasticsearch}} instance. We will be using the [Elastic web crawler](https://www.elastic.co/guide/en/enterprise-search/current/crawler-private-network-cloud.html){: external}. This feature can be used to extract data from any web server.

Follow the steps to add your data:

1. Navigate to kibana, located at `localhost:5601` in your web browser.
1. Go to the **Search** section from left sidebar.
1. Press *Start* under `web-crawler` section and create your index on next page, prefixed `search-`.
1. Once the index is created, the next step is to validate your data's domain. Ensure that your domain includes HTML data so that crawlers can comprehend its content. You can include relevant entry points of your domain to fetch data from.
1. After adding all the entry points, click on **Crawl All Domains** or choose custom settings from the dropdown menu next to the crawl button at the top right. Now, all that's left to do is wait until all the data is collected.
1. Also, update the `.env` file with the index name.

## Add ML pipeline to your index
{: #build-es-chatbot-ml-pipeline}

1. Navigate to the pipeline settings for your index by going to Kibana, then selecting **Search**, followed by your specific index, and finally, clicking on the **Pipelines** tab.
1. If not done earlier, deploy the [ELSER](https://www.elastic.co/guide/en/machine-learning/8.10/ml-nlp-elser.html#download-deploy-elser){: external} Model.
1. Click *Add Inference Pipeline* in the Machine Learning Inference Pipeline section and follow the steps.
1. Select `.elser_model_1` for the trained ML Model.
1. Access the Streamlit user interface at `localhost:8501` in your browser. Here, you can start interacting with the model by asking questions, and you receive answers generated by IBM watsonx.ai.

This streamlined process enables you to leverage the AI capabilities of IBM watsonx.ai and use your documentation effectively. Happy querying!

## Troubleshooting
{: #build-es-chatbot-ts}

If you are unable to create a crawl index, delete the `ibm_default` template, and the `.elastic-connectors` and `.elastic-connectors-v1` indices. Then, recreate the index.

```sh
curl --cacert ca.crt -u ${ELASTIC_USER}:${ELASTIC_PASS} -XDELETE ${ELASTIC_HOST}/.elastic_connectors
curl --cacert ca.crt -u ${ELASTIC_USER}:${ELASTIC_PASS} -XDELETE ${ELASTIC_HOST}/.elastic-connectors-v1
curl --cacert ca.crt -u ${ELASTIC_USER}:${ELASTIC_PASS} -XDELETE ${ELASTIC_HOST}/_template/ibm_defaults
```
{: pre}
