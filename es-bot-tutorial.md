---

copyright:
  years: 2024
lastupdated: "2024-05-20"

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

This tutorial shows how an IBM watsonx.ai model can be enhanced with knowledge gleaned by spidering content from your website to produce a chatbot capable of answering questions related to your knowledge base. This technique is known as Retrieval-Augmented Generation (RAG). Pre-trained large language models have good _general knowledge_, having being trained with a large corpus of public content, but they lack _domain-specifc knowledge_ about your business e.g.

- "Can I get a refund if the box is opened?"
- "What is the waiting list for treatment?"
- "Do you deliver on Saturdays?" 

We can build a chatbot using the following IBM Cloud services:

- {{site.data.keyword.databases-for-elasticsearch}} which runs the [ELSER](https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-elser.html){: external} Natural Languge Processing (NLP) model to enhance the incoming data before being stored in an Elasticsearch index. An _ingest pipeline_ is used to allow data to fed into ELSER before the enhanced data is stored.
- Elastic Enterprise Search is deployed on IBM Code Engine and is used to spider your website to collect domain-specific data and feed it into Elasticsearch's ingest pipeline.
- Kibana is deployed on IBM Code Engine and becomes the web UI for Elasticsearch and Elastic Enterprise Search. We will use it specify and set off the web crawler.
- IBM wastonx.ai runs a pre-trained machine learning model to answer chatbot requests. The model's API is used to produce chatbot responses given the user prompt and the contextual data collected by running the prompt against the spidered and enhanced data in Elasticsearch.
- A simple Python app is deployed on IBM Code Engine to provide a chatbot web interface. It collects prompts from users, queries the Elasticsearch data and then uses IBM watsonx.ai to produce the response.

{{site.data.keyword.databases-for-elasticsearch}}, IBM Code Engine and IBM watsonx are paid products so this tutorial incurs charges.
{: important}

## Pre-requisites
{: #build-es-chatbot-prereqs}

You will need:

- An [{{site.data.keyword.cloud_notm}} Account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - to deploy infrastructure
- A website containing public-facing text content which we will "spider" to bolster the chatbot's expertise.
- Docker running on your local machine. See [Docker Desktop](https://www.docker.com/products/docker-desktop/).

Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an {{site.data.keyword.cloud_notm}} API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

## Set up an IBM watsonx.ai Project
{: #build-es-chatbot-watsonxai-project}

Most of our infrastructure wil be deployed through Terraform, but we need to setup IBM watsonx.ai manually.

[IBM watsonx.ai](https://www.ibm.com/products/watsonx-ai){: external} is a studio of integrated tools for working with generative AI capabilities that is powered by foundation models for building machine learning applications. The IBM watsonx.ai component provides a secure and collaborative environment where you access your organization's trusted data, automate AI processes, and deliver AI in your applications.

Follow these steps to setup IBM watsonx.ai:

1. Sign Up for [IBM watsonx.ai as a Service](https://cloud.ibm.com/watsonx/overview){: external} and click on the "Get Started" link for watsonx.ai. Select a region and log in.
2. [Create a project](https://dataplatform.cloud.ibm.com/projects/new-project?context=wx){: external} within watsonx.ai. In the "Projects" box, click the "+" icon and the "Create Project" button. Name you project and supply the Cloud Object Storage instance which will be used to store the project's state. (If you don't have a Cloud Object Storage instance, [create one here](https://cloud.ibm.com/objectstorage/create)) 
3. In the project's "Manage" tab, in the General page, make a note of the "Project ID".
4. In the project's "Manage" tab, in the Services and Integrations page, click the "Associate Service" button. Click "New Service" button and choose the "Watson Machine Learning" option. You'll be using the Lite plan, so just click "Create". 

## Provision the infrastructure with Terraform
{: #provision-infrastructure-terraform}

Clone the repo:

```sh
git clone https://github.com/IBM/icd-elastic-bot.git
cd icd-elastic-bot
cd terraform
```

In this directory, create a file called `terraform.tfvars` containing the following data, but replacing the placeholder (`MY_*` values) with your own:

```
ibmcloud_api_key="MY_IBM_CLOUD_API_KEY"
region="eu-gb"
es_username="admin"
es_password="MY_ELASTICSEARCH_PASSWORD"
es_version="8.12"
wx_project_id="MY_WATSONX_PROJECT_ID"
```

> Note: pick a secure Elasticsearch password which, together with the Elasticsearch username will become the credentials required to access Elasticsearch and the Kibana web user interface.

Now deploy the infrastructure with:

```sh
terraform init
terraform apply --auto-approve
```

Terraform will output 

- the URL of your Kibana instance.
- the URL of the Python app.

Make a note of these values for the next steps

## Feed the data
{: #build-es-chatbot-feed-data}

This step feeds your website's data to your {{site.data.keyword.databases-for-elasticsearch}} instance. We will be using the [Elastic web crawler](https://www.elastic.co/guide/en/enterprise-search/current/crawler-private-network-cloud.html){: external}. This feature, accessed through Kibana, is used to extract data from any website.

Follow the steps to add your data:

1. Navigate to your Kibana URL - Terraform will have outputed this URL from the previous section. Log in with the Elasticsearch username and password you chose.
2. In the Kibana UI, in the Search section, choose the Overview option. Click on the "Crawl URL" button.
3. Name your index "search-bot" (the `search-` prefix is already present). Click "Create Index".
4. Add your website's URL in the *Manage domain* section of the index. Click "Validate Domain" and then "Add Domain".
5. Click Add Inference Pipeline in the Machine Learning Inference Pipeline section and follow the steps. Select .elser_model_1 for the trained ML Model and make sure the model is in a "Started" state. Select title field in Select field mappings step and click "Add", then "Continue" and "Create Pipeline".
5. Click on **Crawl All Domains** and then "Crawl all domains on this index". All that's left to do now is wait until the data is collected.

## Query the data
{: #build-es-chatbot-query-data}

Navigate to the Python app's URL in your web browser - this can be found in the output from Terraform as `python_endpoint` from previous steps.

Start interacting with the model by asking questions, and you will receive answers generated by IBM watsonx.ai. The Python app takes your prompt and searches the Elasticsearch index populated with spidered and enhanced data from the web crawl. It then uses the IBM watsonx.ai API to produce a response from the context provided by the Elasticsearch result.

## Conclusion

In this tutorial we created an {{site.data.keyword.databases-for-elasticsearch}} instance that is paired with Kibana and Elastic Enterprise Search hosted on IBM Code Engine. We then configured Elasticsearch to use its Elser model in an pipeline so that when we spidered a website's contents, its JSON documents were augmented with _sparse vector_ data. We also deployed a Python app which takes user prompts, queries the spidered data in Elasticsearch to gather domain-specific context before sending the prompt and the context to a watsonx.ai large language model to formulate a response to the prompt.

Now it's over to you to create your own chat applications using watsonx.ai, enhanced by modelling your own domain-specfic data held in {{site.data.keyword.databases-for-elasticsearch}}.
------

Your {{site.data.keyword.databases-for-elasticsearch}} incurs charges. After you finish this tutorial, you can remove all the infrastructure by going to the `terraform` directory of the project and using the command:

```sh
terraform destroy
```
