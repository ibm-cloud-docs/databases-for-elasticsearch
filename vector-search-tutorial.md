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

# Learn how to use Elasticsearch vector search capabilities
{: #vector-search-elasticsearch}
{: toc-content-type="tutorial"}
{: toc-completion-time="2hrs"}

## Objectives
{: #vector-search-elasticsearch-objectives}

In this tutorial, you deploy an instance of {{site.data.keyword.databases-for-elasticsearch}} and use it to store [vector representations](https://www.elastic.co/what-is/vector-embedding){: external} of images that you are then able to search to find similarities with new, unseen, images.

These vector representations, known as embeddings, are created using Machine learning algorithms. [Machine learning]((https://www.ibm.com/topics/machine-learning)){: external} is a branch of artificial intelligence (AI) and computer science that focuses on the use of data and algorithms to imitate the way that humans learn, gradually improving its accuracy. By using statistical methods, algorithms are trained to make classifications or predictions, and to uncover key insights in data mining projects.

These learning algorithms are known as “models”. In this tutorial we make use of one such model, [OpenAI's CLIP](https://github.com/openai/CLIP){: external}. CLIP (Contrastive Language-Image Pre-Training) is a neural network trained on a variety of (image, text) pairs.

Traditionally, finding similarities between images, something that is relatively straightforward to the human eye, has been difficult for a computer to do. Machine Learning has transformed this field of search.

This is the first in a series of three tutorials exploring the AI capabilities of Elasticsearch. The second one is [here]() and the third one is [here]().

{{site.data.keyword.databases-for-elasticsearch}} is a paid-for service, so following this tutorial will incur charges.
{: important}

## Getting productive
{: #vector-search-elasticsearch-get-productive}

To begin the provisioning process, install some must-have productivity tools:

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - To codify and provision infrastructure.
- [Python](https://www.python.org/downloads/){: external}

You also need a dataset of images that provide the corpus for doing similarity search. For example, you might have a dataset of car images or of bird images. You typically need thousands of these images. We cannot provide one here because of copyright restrictions on images.

In this tutorial you won't upload the images themselves to your database. The vector representations will be calculated locally and only those will be uploaded. In a real-life scenario you would probably have the images stored somewhere (like an Object Storage bucket) and a reference to the image location would be stored alongside the vector representation, for retrieval.
{: note}

## Obtain an API key
{: #nlp-ml-tutorial-api-key}
{: step}

[Create an {{site.data.keyword.cloud_notm}} API key](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: .important}

## Clone the project
{: #vector-search-elasticsearch-clone-project}
{: step}

Clone the project from the [GitHub repository](https://github.com/IBM/elasticsearch-ml-vector-search-tutorial){: external}.

```sh
git clone https://github.com/IBM/elasticsearch-ml-vector-search-tutorial.git
```
{: pre}

## Install the Elasticsearch cluster
{: #vector-search-elasticsearch-install-infra}
{: step}

1. Navigate into the terraform folder of the cloned project.

   ```sh
   cd elasticsearch-ml-vector-search-tutorial/terraform
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

1. Finally, export the database access URL to your terminal environment. It will be required by subsequent steps.

   ```sh
    terraform output --json
    export ES_URL="<the url value obtained from the output>"
   ```
   {: pre}

## Install dependencies
{: #vector-search-elasticsearch-install-dependencies}
{: step}

You need to install some Python dependencies:

```sh
    pip3 install elasticsearch
    pip3 install Pillow
    pip3 install imgbeddings
    pip3 install requests
   ```
   {: pre}

## Generate vector embeddings for your images
{: #vector-search-elasticsearch-generate-embeddings}
{: step}

Create a folder called `images` at the root of the project folder structure. Inside it, create one or more folders with different images. For example, if you have a dataset of cars then you may want to create folders for different types of car, for example `fordescort` and `fordcortina`. This is not strictly necessary (all images could go in a folder called `cars`), but organizing folders may make it easier to identify search matches later on.

You are ready to run the `create.py` script. In the root of the project type:

```sh
python3 create.py
```
{: pre}

This script creates an Elasticsearch index called `images` in your {{site.data.keyword.databases-for-elasticsearch}} database.

Then, it cycles through the `images` folder and for each image it finds it create a set of [embeddings](https://www.elastic.co/what-is/vector-embedding){: external} using [this open source Python package](https://github.com/minimaxir/imgbeddings){: external}, which uses the open source [CLIP model from OpenAI](https://github.com/openai/CLIP){: external}. With those embeddings and a small amount of metadata (file path and file id), the script creates a document that is then uploaded to the Elasticsearch index.

Depending on the size of your dataset, this process could take multiple hours to complete.

## Search your dataset
{: #vector-search-elasticsearch-search-dataset}
{: step}

You are now ready to test your new dataset. To do this you need to find another image of a car (if you are using cars) that is not part of your original dataset. Save this image (for example, `myimage.jpg`) to the root of the project.

Run the `search.py` script, passing in the image name:

```sh
python3 search.py myimage.jpg
```
{: pre}

The script generates a set of embeddings for the supplied image using the same algorithm as before. It attempts a [known nearest neighbor](https://www.elastic.co/guide/en/elasticsearch/reference/current/knn-search.html){: external} search on the dataset to find the closest match in the dataset to the image that was supplied in the search. It returns the details of the closest match.

```text
{"took": 4947, "timed_out": false, "_shards": {"total": 1, "successful": 1, "skipped": 0, "failed": 0}, "hits": {"total": {"value": 1, "relation": "eq"}, "max_score": 0.97320974, "hits": [{"_index": "images", "_id": "5c910de5357cb9a3f1b43e6618b141afa6666bfca8676269d5e10a14e1688819", "_score": 0.97320974, "fields": {"file_path": ["./images/Bald_Eagle/26897.jpg"], "desc": ["Bald_Eagle"]}}]}}
```
{: pre}

You can repeat this process with other images.

## Image similarity, not bird similarity
{: #vector-search-elasticsearch-similarity}
{: step}

What you have built is more of an image similarity search than a bird (or car) similarity search. The vector search algorithm is analyzing the whole image and looking for matches. An image of a warbler sitting on a tree branch might be more similar to an image of a tree sparrow sitting on a tree branch than to an image of a warbler in flight. Nevertheless, it is a powerful tool to make relationships between objects that (until recently) were difficult, if not impossible.

## Tear down your infrastructure
{: #vector-search-elasticsearch-tear-down}
{: step}

Your {{site.data.keyword.databases-for-elasticsearch}} incurs charges. After you finish this tutorial, you can remove all the infrastructure by going to the `terraform` directory of the project and using the command:

```sh
terraform destroy
```
{: pre}

##  Next Steps
{: #vector-search-elasticsearch-next-steps}

If you are ready to explore further, you can also use {{site.data.keyword.databases-for-elasticsearch}} to not only store vector embeddings but to generate them as well. That is the subject of our [second tutorial]() in this series.
