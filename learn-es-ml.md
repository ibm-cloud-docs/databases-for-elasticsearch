---
copyright:
  years: 2024
lastupdated: "2024-01-22"

keywords: elasticsearch enterprise, elasticsearch platinum, machine learning, artificial intelligence, vector embedding, ml

subcollection: databases-for-elasticsearch

---

{{site.data.keyword.attribute-definition-list}}

# Elasticsearch, machine learning, and AI
{: #es-ml-ai}

At the heart of artificial intelligence (AI) and computer science lies machine learning (ML). It's where computers learn and adapt by using data and algorithms, just like humans.

In recent decades, technological progress in storage and processing power has paved the way for ML-based innovations. However, the game-changer arrived in 2023 with the introduction of ChatGPT, an AI chatbot that captivated the world. Today, ML's potential is sought after by businesses across the board.

Luckily for most enterprises, much of this can be achieved with existing technologies, such as Elasticsearch. Elasticsearch, a robust search-capable database, is well-suited to seamlessly incorporate essential ML features sought by enterprises embracing new technologies. Elastic have been doing just that, packing their Elasticsearch product with ML capabilities.

## Storing Data
{: #es-ml-ai-storing-data}

Machine learning algorithms try to make sense of unstructured data, like videos or images, by turning these assets into sets of numbers called [vector embeddings](https://www.elastic.co/what-is/vector-embedding){: external}. Once an asset like an image is transformed into a set of embeddings it can be stored in a database like any other data. Elasticsearch has a specific data type (dense vector type) for these embeddings.

Different ML algorithms (known as models) will analyze and transform data in different ways. ML models specialize in different types of data and tasks. For a full list of Elastic stack supported models, see [Compatible third party NLP models](https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-model-ref.html){: external}. For a comprehensive list of open source ML models, see [Hugging Face](https://huggingface.co/models){: external}.

## Querying Data
{: #es-ml-ai-querying-data}

Once inside the database, you can make sense of these assets by using Elasticsearch’s [vector search](https://www.elastic.co/what-is/vector-search){: external}. Given a search “term” (for example a vector embedding of the picture of a bird or a car), the search engine finds the vector embeddings in its data set that are mathematically closer, the “known nearest neighbor” or [kNN](https://www.elastic.co/blog/introducing-approximate-nearest-neighbor-search-in-elasticsearch-8-0){: external}. This produces a list of birds or cars that look similar to the search “term”.

## How Databases for Elasticsearch can help on your AI journey
{: #es-ml-ai-help}

### Enterprise Plan
{: #es-ml-ai-help-enterprise}

If you want to only store and search vector embeddings, the Enterprise Plan of {{site.data.keyword.databases-for-elasticsearch}} (which deploys Elastic’s Basic version) may suffice for you. This plan supports the Dense Vector data type as well as the various flavors of vector search that Elastic offers.

Using this plan means that you have to generate the actual embeddings somewhere else and then upload them to the database, as the Enterprise Plan does not support the generation of embeddings.

The Enterprise Plan gives you more flexibility at the cost of higher complexity in your AI data pipeline.

### Platinum Plan
{: #es-ml-ai-help-platinum}

For a richer set of functions, the Platinum Plan of {{site.data.keyword.databases-for-elasticsearch}} (which deploys Elastic’s Platinum version) may be what you need. With the Platinum Plan you get access to all the features of the Enterprise Plan but also the ability to generate the embeddings themselves, either by using Elastic’s own ML model [ELSER](https://www.elastic.co/guide/en/machine-learning/current/ml-nlp-elser.html){: external} (ELastic Sparse EncodeR), or by using any of the many open source ML models that [Elastic supports](https://www.elastic.co/search-labs/blog/articles/may-2023-launch-machine-learning-models){: external}.

The Platinum Plan provides a one-stop shop for generating, storing, and searching for vector embeddings.

### Machine learning tutorial series
{: #es-ml-ai-help-ml-tutorials}

If you need some inspiration to get you started, we've put together a tutorial series on how to use Elastic's ML abilities with third-party models:
- [Use Elasticsearch vector search capabilities](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-vector-search-elasticsearch)
- [Use ELSER, Elastic's Natural Language Processing model](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-elser-embeddings-elasticsearch)
- [Use machine learning models with Elasticsearch to tag content](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-nlp-ml-tutorial)

## Plans and Pricing
{: #es-ml-ai-plans-pricing}

Visit our [plans](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-elastic-offerings){: external} and [pricing](https://cloud.ibm.com/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-pricing){: external} pages for more details on features and costs.
