# Graph-Augmented RAG using LLMSherpa: A Scalable Semantic Retrieval Service
The integration of vector embeddings with graph databases presents a transformative approach to Retrieval-Augmented Generation (RAG) systems, particularly when applied to complex structured data such as documents. This repository explores using LLM embedding models in conjunction with LLMSherpa Ingestor for parsing documents and storing embeddings in Neo4j's AuraDB. 

## Team Member Information
-- Aditya Raj (22BDS002)
-- Akash Nayak (22BDS003)

## Initialisation steps for Neo4j Aura DB:
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

### Run graph-rag/openai+llmsherpa/LayoutPDFReader_KGLoader.ipynb
Change the following in the code - 
llmsherpa_url = "http://localhost:5010/api/parseDocument?renderFormat=all"
path_to_file = File path to your local directory containing all the PDF's to ingest
Add your neo4j information (URL, Username and Password)

### Run graph-rag/openai+llmsherpa/KGEmbedding_Populate.ipynb
Add your neo4j information (URL, Username and Password)
Add your OpenAI key/ Ollama URL 
If you would like to use our Pre-Trained model use the weights found at this link - [res-summarizer](https://drive.google.com/drive/folders/1tYbMmf66UNj9tPwPKb9_vLzmH9L-ZvVn?usp=sharing), the Colab file used for pre-training is found at - t5-finetuned-model-code/t5_Small_ResSum_Training.ipynb

Look at the nodes and relationships defined on the Neo4j instance console at https://console-preview.neo4j.io/tools/explore


## Initialisation steps for AWS EC2:
Connect your Neo4j AuraDB instance to your EC2 instance. Generate your .pem (for UNIX) certificate to authorise SCP transfer from EC2. 

(If running locally) Pull the following docker image from either:
-- DockerHub - https://hub.docker.com/r/docker/genai
-- Github - https://github.com/docker/genai-stack


Execute the following commands in your local terminal to copy your code onto the EC2 Instance - 
1. SSH into the EC2 Instance from Your Local Terminal
```
ssh -i ~/aws/mykey.pem ubuntu@<YOUR_PUBLIC_IP>
```
2. Set up the instance
```
bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3-pip -y
pip3 install streamlit neo4j openai
```
3. Upload your project
```
bash
scp -i ~/aws/mykey.pem -r . ubuntu@<YOUR_PUBLIC_IP>:~/chatbot
```
On EC2's Ubuntu Terminal - 
```
bash
cd ~/chatbot
```
4. Run on EC2
Assuming your main script is app.py, run:
```
bash
streamlit run app.py --server.port 8501 --server.enableCORS false --server.enableXsrfProtection false
```
5. Access the given link provided on the EC2 Terminal
The service will be deployed on EC2, and the user can now interact with it. To monitor performance, the user may check the monitoring tab on the EC2 instance.
