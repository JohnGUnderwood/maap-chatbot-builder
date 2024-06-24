# MongoDB MAAP Chatbot Framework

The MongoDB MAAP Chatbot Framework is a set of libraries that you can use to build your RAG Application
using MongoDB and [Atlas Vector Search](https://www.mongodb.com/docs/atlas/atlas-vector-search/vector-search-overview/). and associated MAAP partners

The repo offers the flexibility to its user to set up the rag application by simiply configuring a yaml file(details see below). The repo offers the users the flexibilty to choose from various option available through partners program. The following modules of rag are made configurable
1. Data loaders
2. Embedding Models
3. Chat LLM Models
4. Post query Reranker

# Setup and Running Demo Video: https://www.youtube.com/watch?v=-r824BdVZt0
# Steps to run the application
1. Clone the project to you machine install dependencies
## Installation

```
cd chabot
npm build:local
cd examples/partnerproduct
npm install
```

2. Configure RAG application
# Configuration
```
ingest:
  - source: 'pdf'
    source_path: '<file_path>'
    chunk_size: 2000
    chunk_overlap: 200
embedding:
    class_name: Nomic-v1.5
vector_store:
    connectionString: '<you_mdb_connection_string>'
    dbName: '<db_name>'
    collectionName: 'embedded_content'
    embeddingKey: 'embedding'
    textKey: 'text'
    numCandidates: 150
    minScore: 0.1 
    vectorSearchIndexName: 'vector_index'
llms:
    class_name: Fireworks
    model_name: 'accounts/fireworks/models/mixtral-8x22b-instruct'
    temprature: ''
    top_p: ''
    top_k: ''


``` 
Also make a copy of the `examples/partnerproduct/example.env` as `.env` to folder where you are running and added the API Keys | URL | Connection Strings | other secrets used in your application.  

## Embedding Model 
    Instantiation of Embedding Classes:
    
    * For 'VertexAI', it instantiates GeckoEmbedding without any parameters.
    * For 'Azure-OpenAI-Embeddings', it creates an instance of AzureOpenAiEmbeddings, passing in an object with properties like modelName, deploymentName, apiVersion, and azureOpenAIApiInstanceName, all derived from parsedData.embedding.
    * The 'Cohere' case instantiates CohereEmbeddings with a modelName parameter.
    * 'Titan' leads to the creation of a TitanEmbeddings instance without parameters.
    * Both 'Nomic-v1' and 'Nomic-v1.5' cases instantiate their respective classes, NomicEmbeddingsv1 and NomicEmbeddingsv1_5, without parameters.
## LLM Model
    Each case in the switch corresponds to a different class name (VertexAI, OpenAI, Anyscale, Fireworks, Anthropic, Bedrock). If the class_name matches one of these cases, a new instance of the corresponding class is created and returned. The constructor of each class is called with an object containing a modelName, which is also obtained from parsedData.llms.model_name. modelName corresponds to the LLM Model that is being used as part of the demo

## Data Loaders
    For each case, a new instance of the corresponding loader class (WebLoader, PdfLoader, SitemapLoader, DocxLoader, ConfluenceLoader) is created and configured with parameters extracted from the data object. These parameters typically include the path or URL to the data source (source_path), and may also include settings for how the data should be chunked (chunkSize, chunkOverlap). The newly created loader instance is then added to the dataloaders array for later processing.

    * WebLoader: Used for loading data from web pages. It is initialized with a URL.
    * PdfLoader: Used for loading data from PDF files. It is initialized with a file path.
    * SitemapLoader: Used for loading data from sitemaps. It is initialized with a URL to the sitemap.
    * DocxLoader: Used for loading data from DOCX documents. It is initialized with a file path.
    * ConfluenceLoader: Used for loading data from Confluence spaces. It is initialized with space names and Confluence connection details.


3. Once configured you can use the yaml file you just created say as in example `examples/partnerproduct/src/config_1.yaml`
## Ingest Data
```
npm install
npm run ingest ./src/config_1.yaml
```

## Run the server
```
npm run start ./src/config_1.yaml
```
4. You can start your UI client application by running the following command
## Start your application UI
```
cd ui
npm install
npm run start
```
Your application ui will be running at `http://localhost:3000`

