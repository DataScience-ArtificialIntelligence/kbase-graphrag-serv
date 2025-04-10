# Graph-Augmented RAG using LLMSherpa: A Scalable Semantic Retrieval Service
The integration of vector embeddings with graph databases presents a transformative approach to Retrieval-Augmented Generation (RAG) systems, particularly when applied to complex structured data such as documents. This repository explores using LLM embedding models in conjunction with LLMSherpa Ingestor for parsing documents and storing embeddings in Neo4j's AuraDB. 

## Initialisation steps:
### LLMSherpa Ingestor Local Server Initialisation (https://github.com/nlmatics/nlm-ingestor/)
1. Run the tika server:
```
java -jar <path_to_nlm_ingestor>/jars/tika-server-standard-nlm-modified-2.9.2_v2.jar
```
2. Install the ingestor
```
!pip install nlm-ingestor
```
3. Run the ingestor
```
python -m nlm_ingestor.ingestion_daemon
```
### Run the docker file
A docker image is available via public github container registry. 

Pull the docker image
```
docker pull ghcr.io/nlmatics/nlm-ingestor:latest
```
Run the docker image mapping the port 5001 to port of your choice. 
```
docker run -p 5010:5001 ghcr.io/nlmatics/nlm-ingestor:latest-<version>
```
Once you have the server running, you can use the [llmsherpa](https://github.com/nlmatics/llmsherpa) API library to get chunks and use them for your LLM projects. Your llmsherpa_url will be:
"http://localhost:5010/api/parseDocument?renderFormat=all"

### Proceed to run graph-rag/openai+llmsherpa/LayoutPDFReader_KGLoader.ipynb
Change the following in the code - 
llmsherpa_url = "http://localhost:5010/api/parseDocument?renderFormat=all"
path_to_file = File path to your local directory containing all the PDF's to ingest



