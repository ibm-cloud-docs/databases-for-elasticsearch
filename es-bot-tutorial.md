---

copyright:
  years: 2024
lastupdated: "2024-03-18"

keywords: machine learning, elasticsearch, artificial intelligence, ai, model, vector search, bot

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 45mins

---

{{site.data.keyword.attribute-definition-list}}

# Build an Elasticsearch chatbot
{: #build-es-chatbot}
{: toc-content-type="tutorial"}
{: toc-completion-time="45mins"}

## Objectives
{: #build-es-chatbot-objectives}

This tutorial is designed to assist you in harnessing the full potential of your data by transforming it into an intelligent bot capable of answering queries related to your data. The bot effortlessly stores the knowledge base in an Elasticsearch index and taps into AI to intelligently handle your questions related to the knowledge base.

In this tutorial, you set up a {{site.data.keyword.databases-for-elasticsearch}} instance and then use its capabilities to crawl your website and store that knowledge base in an Elasticsearch index. You use Elastic's built-in Machine learning (ML) model ELSER to analyze and extract meaning from your data. [Elastic Learned Sparse EncodeR (ELSER)](https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-elser.html){: external} is a Natural Language Processing (NLP) model trained by Elastic that enables you to perform semantic search by using sparse vector representation. Instead of literal matching on search terms, semantic search retrieves results based on the intent and the contextual meaning of a search query.

You then use IBM watsonx to create a chatbot interface that uses this data to intelligently handle questions related to your knowledge base.

{{site.data.keyword.databases-for-elasticsearch}} and IBM watsonx are paid products so this tutorial incurs charges.
{: important}

## Getting productive
{: #build-es-chatbot-prereqs}

To get productive:

- You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- Install [Docker](https://www.docker.com/get-started/){: external}
- Provision a [{{site.data.keyword.databases-for-elasticsearch}} Platinum instance](https://cloud.ibm.com/databases/databases-for-elasticsearch/create){: external} with version 8.1x.x.
- Once your instance is provisioned, download the TLS certificate. Navigate to your instance's **Overview**, scroll down to the *Endpoints* section, and then select **Download Certificate**.
- If required, change the Admin password. Navigate to your instance's **Overview** and scroll down to the *Change Database Admin Password* section. Select **Change Password**.

Make sure you have a running server of your data presented in HTML format. We can also use bulk API to upload data to index that may be covered in future tutorials.
{: important}

## Set up an IBM watsonx.ai Project
{: #build-es-chatbot-watsonxai-project}

[IBM watsonx.ai](https://www.ibm.com/products/watsonx-ai){: external} is a studio of integrated tools for working with generative AI capabilities that is powered by foundation models for building machine learning models. The IBM watsonx.ai component provides a secure and collaborative environment where you access your organization's trusted data, automate AI processes, and deliver AI in your applications.

Follow these steps to integrate powerful AI capabilities into your projects:

1. Sign Up for [IBM watsonx.ai as a Service](https://www.ibm.com/docs/en/watsonx-as-a-service?topic=started-signing-up-watsonx){: external}.
1. To access a range of foundation and Machine learning (ML) models, [set up a new project](https://www.ibm.com/docs/en/watsonx-as-a-service?topic=projects-creating-project#create-a-project){: external}.
1. Once your project is established, navigate to the **Projects** section, select your project, and go to *Manage* > *General* to find and copy your Project ID.
1. Insert the copied Project ID into your `.env` file, under the variable `PROJECT_ID`.

## Set up your server and connect to your {{site.data.keyword.databases-for-elasticsearch}} instance
{: #build-es-chatbot-watsonxai-server-connect}

1. First, [clone the repo](https://github.com/IBM/icd-elastic-bot){: external}.
1. See the [README file](https://github.ibm.com/Dhananjay-Meena/icd-elastic-bot/blob/main/README.md){: external} of the repo and add the `.env` file in the root of your project.
1. To start your server, run a command like:
   ```sh
   docker-compose up --build
   ```
   {: pre}

## Set up your crawler index
{: #build-es-chatbot-crawler-index}

1. Navigate to [Kibana](https://www.elastic.co/kibana){: external}, located at `localhost:5601` in your web browser.
1. Go to the **Search** section in the left sidebar.
1. Press **Start** under the [web crawler](https://www.elastic.co/web-crawler){: external} section and create your index on the next page, prefixed `search-`. Make sure the index name is same as written in your `.env` file.
1. Navigate to the pipeline settings for your index by clicking on the *Pipelines* tab.
1. Click *Copy and Customize* and select the `ml` label from the settings in the *Ingest Pipelines* section.
1. Click *Add Inference Pipeline* in the *Machine Learning Inference Pipeline* section and follow the steps. Select `.elser_model_1` for the trained ML Model and make sure the model is in a running state. Select *title field* in **Select field mappings** step.

This streamlined process enables you to leverage the AI capabilities of IBM watsonx.ai and use your documentation effectively. Happy querying!

## Feed the data
{: #build-es-chatbot-feed-data}

This step feeds your data to your {{site.data.keyword.databases-for-elasticsearch}} instance. We will be using the [Elastic web crawler](https://www.elastic.co/guide/en/enterprise-search/current/crawler-private-network-cloud.html){: external}. This feature can be used to extract data from any web server.

Follow the steps to add your data:

1. Add your domain in the *Manage domain* section of the index. Ensure that your domain includes HTML data so that crawlers can comprehend its content. You can include relevant entry points of your domain to fetch data from.
1. After adding all of the entry points, click on **Crawl All Domains** or choose **Custom Settings** from the dropdown menu next to **Crawl** at the top right. All that's left to do now is wait until all the data is collected.

## Query the data
{: #build-es-chatbot-query-data}

Access the Streamlit user interface at `localhost:8501` in your web browser. Start interacting with the model by asking questions, and you receive answers generated by IBM watsonx.ai.

Ensure compatibility by testing your bot setup on a development instance first.
{: note}


## Troubleshooting
{: #build-es-chatbot-ts}

If you are unable to create a crawl index, delete the `ibm_default` template, and the `.elastic-connectors` and `.elastic-connectors-v1` indices. Then, recreate the index.

```sh
curl --cacert ca.crt -u ${ELASTIC_USER}:${ELASTIC_PASS} -XDELETE ${ELASTIC_HOST}/.elastic_connectors
curl --cacert ca.crt -u ${ELASTIC_USER}:${ELASTIC_PASS} -XDELETE ${ELASTIC_HOST}/.elastic-connectors-v1
curl --cacert ca.crt -u ${ELASTIC_USER}:${ELASTIC_PASS} -XDELETE ${ELASTIC_HOST}/_template/ibm_defaults
```
{: pre}
