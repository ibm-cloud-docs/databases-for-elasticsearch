---
copyright:
  years: 2024
lastupdated: "2024-01-02"

keywords: elasticsearch enterprise, elasticsearch platinum, machine learning, artificial intelligence, vector embedding

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Elasticsearch, Machine Learning, and AI
{: #es-ml-ai}

At the heart of artificial intelligence (AI) and computer science lies machine learning (ML). It's where computers learn and adapt using data and algorithms, just like humans.

Over the last couple of decades, the technological advances in storage and processing power have enabled some innovative products based on ML, but this evolution was turbo-charged in 2023 with the launch of ChatGPT, the AI-driven chatbot that took the world by storm. Now everyone, in every business, wants to harness the power of ML.

Luckily for most enterprises, a lot of this can be achieved with existing technologies, such as Elasticsearch. As a database with powerful search capabilities, Elasticsearch is ideally placed to integrate many of the ML features that are now required by enterprises that want to harness the power of these new technologies. Elastic have been doing just that, packing their Elasticsearch product with ML capabilities.

## Storing Data
{: #es-ml-ai-storing-data}

Machine Learning algorithms try to make sense of unstructured data, like videos or images, by turning these assets into sets of numbers called [vector embeddings](https://www.elastic.co/what-is/vector-embedding){: external}. Once an asset like an image is transformed into a set of embeddings it can be stored in a database like any other data.  Elasticsearch has a specific data type (dense vector type) for these embeddings.

Different ML algorithms (known as models) will analyze and transform data in different ways. ML models specialize in different types of data and tasks. For a comprehensive list of open-source ML models, see [Hugging Face](https://huggingface.co/models){: external}.

## Querying Data
{: #es-ml-ai-querying-data}

Once inside the database, you can make sense of these assets by using Elasticsearch’s [vector search](https://www.elastic.co/what-is/vector-search){; external}. Given a search “term” (for example a vector embedding of the picture of a bird or a car), the search engine will find the vector embeddings in its dataset that are mathematically closer, the “known nearest neighbor” or [kNN](https://www.elastic.co/blog/introducing-approximate-nearest-neighbor-search-in-elasticsearch-8-0) {: external}. This will produce a list of birds or cars that look similar to the search “term”.

## How Databases for Elasticsearch can help on your AI journey
{: #es-ml-ai-help}

### Enterprise Plan
{: #es-ml-ai-help-enterprise}

If you just want to store and search vector embeddings, the Enterprise Plan of {{site.data.keyword.databases-for-elasticsearch}} (which deploys Elastic’s Basic version) may suffice for you. This plan supports the Dense Vector data type as well as the various flavors of vector search that Elastic offers.

Using this plan means that you will have to generate the actual embeddings somewhere else and then upload them to the database, as the Enterprise Plan does not support the generation of embeddings.

The Enterprise Plan gives you more flexibility at the cost of higher complexity in your AI data pipeline.

### Platinum Plan
{: #es-ml-ai-help-platinum}

For a richer set of functionality, the Platinum Plan of {{site.data.keyword.databases-for-elasticsearch}} (which deploys Elastic’s Platinum version) may be what you need. With the Platinum Plan you get access to all the features of the Enterprise Plan but also the ability to generate the embeddings themselves, either by using Elastic’s own ML model [ELSER](https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-elser.html){: external} (ELastic Sparse EncodeR), or by using any of the many open-source ML models that [Elastic supports](https://www.elastic.co/search-labs/blog/articles/may-2023-launch-machine-learning-models){: external}.

The Platinum Plan provides a one-stop shop for generating, storing and searching for vector embeddings.

If you need some inspiration to get you started, we have put together this [simple tutorial](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-nlp-ml-tutorial){: external} on how to use Elastic’s ML capabilities with a third-party ML model.

Please visit our [plans](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-elastic-offerings){: external} and [pricing](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-pricing){: external} pages for more details on features and costs.
