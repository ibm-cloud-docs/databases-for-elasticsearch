---

copyright:
  years: 2023, 2024
lastupdated: "2024-01-16"

keywords: machine learning, elasticsearch, artificial intelligence, ai, model

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Use machine learning models with Elasticsearch to tag content
{: #nlp-ml-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="30m"}

## Objectives
{: #disk-util-alert-tutorial-objectives}

{{site.data.keyword.databases-for-elasticsearch}} supports machine learning workloads. In this tutorial, you learn how to provision a machine learning model to a {{site.data.keyword.databases-for-elasticsearch}} instance and then use it to extract meaningful additional information from a test data set. Only some basic knowledge of terminal commands is required to provision and understand this tutorial.

[Machine learning]((https://www.ibm.com/topics/machine-learning)){: external} is a branch of artificial intelligence (AI) and computer science that focuses on the use of data and algorithms to imitate the way that humans learn, gradually improving its accuracy. By using statistical methods, algorithms are trained to make classifications or predictions, and to uncover key insights in data mining projects.

These learning algorithms are known as “models”. In this tutorial, we use a pre-built Natural Language Processing (NLP) model, which extracts meaning out of sentences in written (Natural) language. Specifically, we use the *distilbert-base-uncased-finetuned-conll03-english* model that tries to identify the names of people, locations, and organizations within text.

[Many other models](https://huggingface.co/models?sort=trending){: external} specialize in analyzing other forms of data, like text extraction from images, speech conversion to text, or object identification in images. Models can be trained on domain-specific knowledge, but the training of new models is beyond the scope of this tutorial.

This tutorial guides you through the process of:

- Provisioning a {{site.data.keyword.databases-for-elasticsearch}} instance

- Uploading a machine learning model

- Uploading a data set of headlines and summaries from news articles

- Passing the data set through the NLP model

- Querying the augmented data to see the results of the model on the data.

## Getting productive
{: #nlp-ml-tutorial-getting-started}

To begin the provisioning process, install some must-have productivity tools:

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - To codify and provision infrastructure.
- [Python](https://www.python.org/downloads/){: external}
- [jq](https://jqlang.github.io/jq/download/){: external} - To process configuration files.

## Obtain an API key to provision infrastructure to your account
{: #nlp-ml-tutorial-api-key}
{: step}

Follow [these steps](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an {{site.data.keyword.cloud_notm}} API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: .important}

## Clone the project
{: #nlp-ml-tutorial-clone-project}
{: step}

Clone the project from the [GitHub repository](https://github.com/IBM/elasticsearch-nlp-ml-tutorial.git){: external}.

```sh
git clone https://github.com/IBM/elasticsearch-nlp-ml-tutorial.git
cd elasticsearch-nlp-ml-tutorial/terraform
```
{: pre}

## Install the infrastructure
{: #nlp-ml-tutorial-install-infra}
{: step}

Provision your {{site.data.keyword.databases-for-elasticsearch}} instance.

1. On your machine, create a document that is named `terraform.tfvars`, with the following fields:

   ```sh
   ibmcloud_api_key = "<your_api_key_from_step_1>"
   region = "<your_region>"
   es_password  = "<make_up_a_password>"
   ```
   {: pre}

   The `terraform.tfvars` document contains variables that you might want to keep secret so it is ignored by the GitHub repository.
   {: .note}

1. Install the infrastructure with the following command:

   ```sh
   terraform init
   terraform apply --auto-approve
   ```
   {: pre}

   The Terraform script outputs the configuration data that is needed to run the application, so copy it into the `scripts` folder:

   ```sh
   terraform output -json >../scripts/config.json
   cd ../scripts
   ```
   {: pre}

## Install Eland
{: #nlp-ml-tutorial-install-eland}
{: step}

[Eland](https://www.elastic.co/guide/en/elasticsearch/client/eland/current/overview.html){: external} is a Python Elasticsearch client for exploring and analyzing data in Elasticsearch. Install it with a command like:

```sh
   python3 -m pip install 'eland[pytorch]'
```
{: pre}

## Upload and analyze data
{: #nlp-ml-tutorial-data}

This tutorial uses a small data set of 132 articles that are obtained from the [BBC News](https://feeds.bbci.co.uk/news/uk/rss.xml){: external} and [Guardian](https://www.theguardian.com/politics/rss){: external} websites through their RSS feeds.

These have been transformed into a json file that is formatted as required by the Elasticsearch [bulk upload method](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html){: external}.

Run the `upload.sh` script, which does the following:

- Uploads the NLP model to Elasticsearch.

- Creates a data processing pipeline in Elasticsearch that takes any incoming data and analyzes it for meaningful terms.

- Uploads the pre-formatted data to Elasticsearch and passes it through the pipeline for analysis.

Since this is a demo, we connect nonsecurely to your database. For production, use [secure connections](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-best-practices).
{: .important}

To run the script, make sure you are in the `scripts` directory and use the command:

```sh
ES_PASSWORD=`cat config.json | jq -r .es_password.value`
ES_PORT=`cat config.json | jq -r .es_port.value`
ES_HOST=`cat config.json | jq -r .es_host.value`
export ES="https://admin:${ES_PASSWORD}@${ES_HOST}:${ES_PORT}"
./upload.sh
```
{: pre}

## Query your data
{: #nlp-ml-tutorial-query-data}
{: step}

You now have an index that is named `test` that has 132 records. Each of these records contains the news data (article ID, headline and summary) and an additional object called `tags`. This is where the machine learning model has inserted any people (PER), locations (LOC) or organizations (ORG) that it has found in the text.

The index also contains another object, called `ml`, which shows some of how the model works. For example, it tells you what accuracy probability it assigned to each of the terms it found.

For example, use this query to retrieve a single record to inspect:

```sh
curl -k "$ES/test/_search?size=1" | jq .
```
{: pre}

This machine learning model has generated valuable information. For example, if you run a news website, you might generate pages of tag-based content. If, for example, your users go to a page like www.mynewssite.com/tag/rishi-sunak, all your system would have to do is search by that tag in the index of news articles to retrieve a list of those that mention Rishi Sunak:

```sh
curl -kX POST -d@body.json -H "Content-Type: application/json" "$ES/test/_search" | jq .
```
{: pre}

There is a `body.json` file in the `scripts` directory that you can play around with to make different searches.

## Conclusion
{: #nlp-ml-tutorial-conclusion}

This tutorial shows how to use {{site.data.keyword.databases-for-elasticsearch}} to harness the power of machine learning to generate valuable additional information from your data. We hope you can use it as a springboard to explore other ways to augment and create value from your data.

To stop incurring charges, don't forget to remove all your deployed infrastructure. In your terraform directory, use the command:

```sh
terraform destroy
```
{: pre}
