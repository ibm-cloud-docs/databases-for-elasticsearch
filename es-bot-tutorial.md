---

copyright:
  years: 2024, 2025
lastupdated: "2025-05-13"

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

This tutorial shows how an IBM watsonx.ai model can be enhanced with knowledge gleaned by spidering content from your website to produce a chatbot that is capable of answering questions related to your knowledge base. This technique is known as Retrieval-Augmented Generation (RAG). Pre-trained large language models have good _general knowledge_, being trained with a large corpus of public content, but they lack _domain-specific knowledge_ about your business, such as:

- "Can I get a refund if the box is opened?"
- "What is the waiting list for treatment?"
- "Do you deliver on Saturdays?" 

We can build a chatbot using the following {{site.data.keyword.cloud_notm}} services:

- {{site.data.keyword.databases-for-elasticsearch}} that runs the [ELSER](https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-elser.html){: external} Natural Languge Processing (NLP) model to enhance the incoming data before being stored in an Elasticsearch index. An _ingest pipeline_ is used to allow data to feed into ELSER before the enhanced data is stored.
- Elastic Enterprise Search is deployed on {{site.data.keyword.codeenginefull}} and is used to spider your website to collect domain-specific data and feed it into Elasticsearch's ingest pipeline.
- Kibana is deployed on {{site.data.keyword.codeenginefull_notm}} and becomes the web UI for Elasticsearch and Elastic Enterprise Search. It is used to specify and to set off the web crawler.
- IBM wastonx.ai runs a pre-trained machine learning model to answer chatbot requests. The model's API is used to produce chatbot responses given the user prompt and the contextual data collected by running the prompt against the spidered and enhanced data in Elasticsearch.
- A simple Python app is deployed on {{site.data.keyword.codeenginefull_notm}} to provide a chatbot web interface. It collects prompts from users, queries the Elasticsearch data and then uses IBM watsonx.ai to produce the response.

{{site.data.keyword.databases-for-elasticsearch}}, {{site.data.keyword.codeenginefull_notm}}, and IBM watsonx are paid products so this tutorial incurs charges.
{: important}

## Prerequisites
{: #build-es-chatbot-prereqs}

- An [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - to deploy infrastructure.
- A website containing public-facing text content that we will "spider" to bolster the chatbot's expertise.
- [Docker](https://www.docker.com/products/docker-desktop/) running on your local machine.

Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an {{site.data.keyword.cloud_notm}} API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

## Set up an IBM watsonx.ai project
{: #build-es-chatbot-watsonxai-project}

Most of the infrastructure is deployed through Terraform, but IBM watsonx.ai must be set up manually.

[IBM watsonx.ai](https://www.ibm.com/products/watsonx-ai){: external} is a studio of integrated tools for working with generative AI capabilities that is powered by foundation models for building machine learning applications. The IBM watsonx.ai component provides a secure and collaborative environment where you access your organization's trusted data, automate AI processes, and deliver AI in your applications.

Follow these steps to set up IBM watsonx.ai:

1. Sign Up for [IBM watsonx.ai as a Service](https://cloud.ibm.com/watsonx/overview){: external} and click on the **Get Started** link for watsonx.ai. Select a region and log in.
2. [Create a project](https://dataplatform.cloud.ibm.com/projects/new-project?context=wx){: external} within watsonx.ai. In the **Projects** box, click **+** and **Create Project**. Name you project and supply the {{site.data.keyword.cos_full}} instance that will be used to store the project's state. (If you don't have a  {{site.data.keyword.cos_short}} instance, [create one](https://cloud.ibm.com/objectstorage/create).
3. In the project's **Manage** tab, in the General page, make a note of the "Project ID".
4. In the project's **Manage** tab, in the **Services and Integrations** page, click **Associate Service**. Then, click **New Service** and choose the **Watson Machine Learning** option. You will use the Lite plan, so just click **Create**. 

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

Pick a secure Elasticsearch password that together with the Elasticsearch username will become the credentials required to access Elasticsearch and the Kibana web user interface.

Now deploy the infrastructure with:

```sh
terraform init
terraform apply --auto-approve
```

Terraform will output:

- the URL of your Kibana instance.
- the URL of the Python app.

Make a note of these values for the next steps.

## Feed the data
{: #build-es-chatbot-feed-data}

This step feeds your website's data to your {{site.data.keyword.databases-for-elasticsearch}} instance. You will use the [Elastic web crawler](https://www.elastic.co/guide/en/enterprise-search/current/crawler-private-network-cloud.html){: external}. This feature, accessed through Kibana, is used to extract data from any website.

Follow the steps to add your data:

1. Navigate to your Kibana URL - Terraform output this URL from the previous section. Log in with the Elasticsearch username and password you chose.
2. In the Kibana UI, in the Search section, choose the **Overview** option. Click on **Crawl URL**.
3. Name your index *search-bot* (the `search-` prefix is already present). Click **Create index**.
4. Add your website's URL in the **Manage domain** section of the index. Click **Validate domain** and then **Add domain**.
5. Click Add Inference Pipeline in the Machine Learning Inference Pipeline section and follow the steps. Select .elser_model_1 for the trained ML Model and make sure the model is in a *Started* state. Select the title field in Select field mappings step and click **Add**, then **Continue**, and **Create pipeline**.
5. Click on **Crawl all domains** and then **Crawl all domains on this index**. Then, wait until the data is collected.

## Query the data
{: #build-es-chatbot-query-data}

Navigate to the Python app's URL in your web browser - this can be found in the output from Terraform as `python_endpoint` from previous steps.

Start interacting with the model by asking questions, and you will receive answers generated by IBM watsonx.ai. The Python app takes your prompt and searches the Elasticsearch index populated with spidered and enhanced data from the web crawl. It then uses the IBM watsonx.ai API to produce a response from the context provided by the Elasticsearch result.

## Conclusion

In this tutorial you created an {{site.data.keyword.databases-for-elasticsearch}} instance that is paired with Kibana and Elastic Enterprise Search hosted on {{site.data.keyword.codeenginefull_notm}}. You then configured Elasticsearch to use its Elser model in a pipeline so that when you spidered a website's contents, its JSON documents were augmented with _sparse vector_ data. You also deployed a Python app that takes user prompts, queries the spidered data in Elasticsearch to gather domain-specific context before sending the prompt and the context to a watsonx.ai large language model to formulate a response to the prompt.

Now you can create your own chat applications using watsonx.ai, enhanced by modelling your own domain-specfic data held in {{site.data.keyword.databases-for-elasticsearch}}.

Your {{site.data.keyword.databases-for-elasticsearch}} incurs charges. After you finish this tutorial, you can remove all the infrastructure by going to the `terraform` directory of the project and using the command:

```sh
terraform destroy
```
