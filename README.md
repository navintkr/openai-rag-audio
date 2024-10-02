# RAG Audio: Pattern for RAG + Voice Using Azure AI Search and the GPT-4o Realtime API

This repo helps implement RAG support in applications that use voice as their user interface, powered by the GPT-4o realtime API for audio. 

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/Gjsi_wXnSRQ/0.jpg)](https://www.youtube.com/watch?v=Gjsi_wXnSRQ)

### 1. Pre-requisites
1. [Azure OpenAI](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesOpenAI), with 2 model deployments, one of the **gpt-4o-realtime-preview** model (Currently it's available in East US 2 and Sweden), and one for embeddings (text-embedding-ada-002)
1. [Azure AI Search](https://ms.portal.azure.com/#create/Microsoft.Search) with [Semantic Search enabled](https://learn.microsoft.com/azure/search/semantic-how-to-enable-disable)
1. [Azure Blob Storage](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)

### 2. Creating an index
Follow these steps to create a new index. We'll create a setup where once created, you can add, delete, or update your documents in blob storage and the index will automatically follow the changes.

1. Upload your documents to an Azure Blob Storage container. An easy way to do this is using the Azure Portal: navigate to the container and use the upload option to move your content (e.g. PDFs, Office docs, etc.)
1. In the Azure Portal, go to your Azure AI Search service and select "Import and vectorize data", choose Blob Storage, then point at your container and follow the rest of the steps on the screen.
1. Once the indexing process completes, you'll have a search index ready for vector and hybrid search.

For more details on ingesting data in Azure AI Search using "Import and vectorize data", here's a [quickstart](https://learn.microsoft.com/en-us/azure/search/search-get-started-portal-import-vectors).

### 3. Setting up the environment
The app needs to know which service endpoints to use for the Azure OpenAI and Azure AI Search. The following variables can be set as environment variables on the ".env" file at "app/backend/" directory with this content.
   ```
   AZURE_OPENAI_ENDPOINT=wss://<your instance name>.openai.azure.com
   AZURE_OPENAI_DEPLOYMENT=gpt-4o-realtime-preview
   AZURE_OPENAI_API_KEY=<your api key>
   AZURE_SEARCH_ENDPOINT=https://<your service name>.search.windows.net
   AZURE_SEARCH_INDEX=<your index name>
   AZURE_SEARCH_API_KEY=<your api key>
   ```
   To use Entra ID (your user when running locally, managed identity when deployed) simply don't set the keys. 

### 4. Running the app

#### Local environment
1. Install the required tools:
   - [Node.js](https://nodejs.org/en)
   - [Python >=3.11](https://www.python.org/downloads/)
      - **Important**: Python and the pip package manager must be in the path in Windows for the setup scripts to work.
      - **Important**: Ensure you can run `python --version` from console. On Ubuntu, you might need to run `sudo apt install python-is-python3` to link `python` to `python3`.
   - [Powershell](https://learn.microsoft.com/powershell/scripting/install/installing-powershell)

2. Clone the repo (`git clone https://github.com/navintkr/openai-rag-audio`)
3. Create a Python virtual environment and activate it.
4. The app needs to know which service endpoints to use for the Azure OpenAI and Azure AI Search. The following variables can be set as environment variables, or you can create a ".env" file in the "app/backend/" directory with this content.
   ```
   AZURE_OPENAI_ENDPOINT=wss://<your instance name>.openai.azure.com
   AZURE_OPENAI_DEPLOYMENT=gpt-4o-realtime-preview
   AZURE_OPENAI_API_KEY=<your api key>
   AZURE_SEARCH_ENDPOINT=https://<your service name>.search.windows.net
   AZURE_SEARCH_INDEX=<your index name>
   AZURE_SEARCH_API_KEY=<your api key>
   ```
   To use Entra ID (your user when running locally, managed identity when deployed) simply don't set the keys.  
5. Run this command to start the app:

   Windows:

   ```pwsh
   cd app
   pwsh .\start.ps1
   ```

   Linux/Mac:

   ```bash
   cd app
   ./start.sh
   ```

6. The app should now be available on http://localhost:8765