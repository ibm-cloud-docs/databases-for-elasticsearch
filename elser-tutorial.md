---

copyright:
  years: 2024
lastupdated: "2024-01-16"

keywords: machine learning, elasticsearch, artificial intelligence, ai, model, vector search

subcollection: databases-for-elasticsearch

content-type: tutorial
account-plan: paid
completion-time: 2hrs

---

{{site.data.keyword.attribute-definition-list}}

# Use ELSER, Elastic's Natural Language Processing model
{: #elser-embeddings-elasticsearch}
{: toc-content-type="tutorial"}
{: toc-completion-time="2hrs"}

[Elastic Learned Sparse EncodeR (ELSER)](https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-elser.html){: external} is a natural language processing (NLP) model trained by Elastic that enables you to perform semantic search by using sparse vector representation. Instead of literal matching on search terms, semantic search retrieves results based on the intent and the contextual meaning of a search query.

In this tutorial you provision an instance of {{site.data.keyword.databases-for-elasticsearch}} Platinum and apply the ELSER model to a body of text and see how it enhances the quality of search results.

This is the second of three tutorials exploring the capabilities of Elasticsearch around [Machine learning]((https://www.ibm.com/topics/machine-learning)){: external}. Machine Learning is a branch of artificial intelligence (AI) and computer science that focuses on the use of data and algorithms to imitate the way that humans learn, gradually improving its accuracy. By using statistical methods, algorithms are trained to make classifications or predictions, and to uncover key insights in data mining projects.

See the other tutorials in this Elasticsearch machine learning series:
- [Use Elasticsearch vector search capabilities](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-vector-search-elasticsearch)
- [Use machine learning models with Elasticsearch to tag content](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-nlp-ml-tutorial)

{{site.data.keyword.databases-for-elasticsearch}} is a paid-for service, so following this tutorial will incur charges.
{: note}

## Getting Productive
{: #elser-embeddings-elasticsearch-before-start}

To begin the provisioning process, install some must-have productivity tools:

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - To codify and provision infrastructure.
- [Python](https://www.python.org/downloads/){: external}
- [jq](https://jqlang.github.io/jq/download/){: external} - To process configuration files.

## Obtain an API key to deploy infrastructure to your account
{: #elser-embeddings-elasticsearch-obtain-key}
{: step}

[Create an {{site.data.keyword.cloud_notm}} API key](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: .important}

## Clone the project
{: #elser-embeddings-elasticsearch-clone-project}
{: step}

Clone the project from the [GitHub repository](https://github.com/IBM/elasticsearch-ml-elser-tutorial){: external}.

```sh
git clone https://github.com/IBM/elasticsearch-ml-elser-tutorial.git
```
{: pre}

## Install the Elasticsearch cluster
{: #elser-embeddings-elasticsearch-install-infra}
{: step}

1. Navigate into the terraform folder of the cloned project.

   ```sh
   cd elasticsearch-ml-elser-tutorial/terraform
   ```
   {: pre}

1. On your machine, create a document that is named `terraform.tfvars`, with the following fields:

   ```sh
    ibmcloud_api_key = "<your_api_key_from_step_1>"
    region = "<your_region>"
    elastic_password = "<make-up-a-password>"
   ```
   {: pre}

   The `terraform.tfvars` document contains variables that you might want to keep secret, so it is excluded from the public Github repository.
   {: important}

1. Install the infrastructure with the following command:

   ```sh
   terraform init
   terraform apply --auto-approve
   ```
   {: pre}

Finally export the database access URL to your terminal environment (it will be required by subsequent steps)

   ```sh
    terraform output --json
    export ES="<the url value obtained from the output>"
    cd ..
   ```
   {: pre}

## Install the ELSER model
{: #elser-embeddings-elasticsearch-create-index}
{: step}

The ELSER model needs to be installed and started before you can use it. First install it by typing:

```sh
curl -kX PUT "$ES/_ml/trained_models/.elser_model_1?pretty" -H 'Content-Type: application/json' -d'
{
  "input": {
	"field_names": ["text_field"]
  }
}
'
```
{: pre}

Next, start it by typing:

```sh
curl -kX POST "$ES/_ml/trained_models/.elser_model_1/deployment/_start?deployment_id=for_search&pretty"
```
{: pre}

You may have to wait a few minutes for the model to finish installing before the start command can execute.
{: note}

## Create an index and mappings for your data
{: #elser-embeddings-elasticsearch-create-index}
{: step}

You should be in the root of your project folder. In the terminal type:

```sh
curl -kXPUT -H"Content-Type: application/json" -d@mapping.json $ES/test_data
```
{: pre}

This creates an index called `test_data` in your ES instance. It will also create some explicit [mappings](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html). This is the way Elastic has of defining how a document, and the fields it contains, are stored and indexed. The mapping definition is stored in the `mapping.json` file. It defines a field called `ml.tokens` which will be used to store all the additional data (tokens) that will be created my the machine learning algorithm when it analyses the data you upload. That data will be contained in a field called `text`, which is also explicitly defined in the mapping document.

## Create an ingest pipeline for analysing your data
{: #elser-embeddings-elasticsearch-create-pipeline}
{: step}

The ELSER model comes pre-installed in all Platinum deployments of {{site.data.keyword.databases-for-elasticsearch}}. All you have to do is create a [pipeline](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html) that uses it to analyze your incoming data.

```sh
curl -kX PUT -H"Content-Type: application/json" -d@pipeline.json $ES/_ingest/pipeline/elser-v1-test
```
{: pre}

The `pipeline.json` document has the pipeline definition. It describes what processing will happen to documents when they are uploaded. In this case it defines the use of the ELSER model and tells ES to put any resulting data in the `ml.tokens` field of the index.

This is a very simple pipeline. For example it does not deal with any ingest errors, just discards them. But it is sufficient for the purposes of this demo.

## Upload your data
{: #elser-embeddings-elasticsearch-upload-data}
{: step}

The `import.json` document contains data from  the `msmarco-passagetest2019-top1000` data set, which is a subset of the [MS MARCO](https://microsoft.github.io/MSMARCO-Passage-Ranking/) Passage Ranking data set. It consists of 200 queries, each accompanied by a list of relevant text passages. All unique passages, along with their IDs, have been extracted from that data set and made ready for [bulk upload](https://www.elastic.co/guide/en/elasticsearch/reference/8.11/docs-bulk.html) to Elasticsearch.

In this step, you upload the data to Elasticsearch and pass it through the ELSER analyzer as it uploads.

Doing Natural Language Processing analysis is compute-intensive, so this process will take multiple hours (up to 12 hours). We will be doing the bulk upload in batches of 100 entries to ensure that the pipeline buffer is not overrun.

So first, split the `import.json` document into smaller documents of 100 entries each (every entry is two lines in the document):

```sh
split -l 200 -a 4  import.json
```
{: pre}

This creates several hundred small documents. Import them by running the `upload.sh` script:

```sh
./upload.sh
```
{: pre}

For each uploaded document, the ELSER model tries to infer what the text is about, and will generate a group of words that it thinks are relevant, along with a relevancy score. Here's one example of an ingested document:

```json
{
  "_index": "test_data",
  "_id": "1",
  "_version": 3,
  "_seq_no": 1310,
  "_primary_term": 1,
  "found": true,
  "_source": {
    "text": "This is the definition of RNA along with examples of types of RNA molecules. This is the definition of RNA along with examples of types of RNA molecules. RNA Definition",
    "ml": {
      "tokens": {
        "molecules": 0.9468944,
        "cell": 0.31727117,
        "type": 0.1881346,
        "lab": 0.14050934,
        "example": 0.27423903,
        "strand": 0.11950072,
        "dna": 0.82435495,
        "protein": 0.25066674,
        "term": 0.5622088,
        "definition": 0.21340553,
        "nuclear": 0.03731725,
        "different": 0.026590228,
        "element": 0.42168307,
        "genetic": 0.36145726,
        "types": 0.7244048,
        "characteristics": 0.24549837,
        "adam": 0.15298073,
        "rna": 1.949169,
        "organism": 0.27345642,
        "gene": 0.57350045,
        "substance": 0.006454099,
        "mrna": 1.002312,
        "bond": 0.096653655,
        "structure": 0.1670035,
        "genome": 0.24130683,
        "sequence": 0.21949387,
        "q": 0.028950738,
        "unit": 0.1926791,
        "examples": 1.0590855,
        "material": 0.034060754,
        "chemical": 0.454947,
        "science": 0.20523745,
        "biological": 0.47201967,
        "molecule": 0.92732555,
        "atom": 0.021480415,
        "word": 0.2971402
      },
      "model_id": ".elser_model_1"
    }
  }
}
```
{: codeblock}

You can see that it has created relationships beyond the actual words on the text. For example, it has infered that this snippet is about `science`, `genes` and `dna`.

Sit back and relax. Your import will take a bit of time.

## Search your dataset and compare results
{: #elser-embeddings-elasticsearch-search-dataset}
{: step}

Once your dataset is uploaded, you are ready to test it, in particular the difference that the ELSER NLP processor can make to the quality of your search results.
The following search query will make use the generated tokens to return results that appear to be most relevant:

```sh
curl -k -H"Content-Type: application/json" -d@elserquery.json $ES/test_data/_search | jq .
```
{: pre}

Compare that with a query that does NOT use the tokens and is simply searching over the original text:

```sh
curl -k -H"Content-Type: application/json" -d@query.json $ES/test_data/_search | jq .
```
{: pre}

You can try other queries by changing the text in the `query.json` and `elserquery.json` files. Try things like:

`what is the best exercise for stiff limbs?`

or

`explain how the US president is elected`

In all these cases, the ELSER-enhanced results are much more relevant.

Of course, that does not apply to all searches. A search for `taylor swift`, for example, will produce similar results on both. So it all depends on the type of search and the data in your index. But you can see that many searches could be significantly improved by applying the ELSER ML model.

## Tear down your infrastructure
{: #elser-embeddings-elasticsearch-tear-down}
{: step}

Your {{site.data.keyword.databases-for-elasticsearch}} incurs charges. After you finish this tutorial, you can remove all the infrastructure by going to the `terraform` directory of the project and using the command:

```sh
terraform destroy
```
{: pre}

## Ready for more?
{: #elser-embeddings-elasticsearch-next-steps}
{: step}

ELSER is just one ML model. There are many others and with the {{site.data.keyword.databases-for-elasticsearch}} Platinum Plan you can easily deploy others and analyze your data using them instead. We have put together a tutorial that [shows you how to do just that](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-nlp-ml-tutorial)
