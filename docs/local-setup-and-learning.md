# DocQuery RAG System — Local Setup and Learning Notes

## 1. Project Overview

DocQuery is a Retrieval-Augmented Generation (RAG) system that retrieves relevant document content and uses a language model to generate answers grounded in that content.

## 2. Local Development Environment

- Operating system: Windows
- Container runtime: Docker Desktop with Docker Compose
- API framework: FastAPI
- Vector database: Qdrant
- Relational database: MySQL
- Local language model runtime: Ollama
- Language model tested: llama3.2:3b

## 3. RAG Architecture

The project documents describe three main pipelines:

### Ingestion Pipeline
Loads documents, splits them into chunks, generates embeddings, and stores the resulting information for retrieval.

### Query Pipeline
Processes a question, retrieves relevant document chunks using hybrid search, combines rankings with Reciprocal Rank Fusion (RRF), reranks results, and generates an answer with source references.

### Evaluation Pipeline
Uses RAGAS to evaluate aspects of RAG response quality.

## 4. Local Setup and Verification

The following steps were completed during local setup:

- Configured environment variables in a local .env file.
- Validated the Docker Compose configuration.
- Built and started the application services.
- Verified the API health endpoint.
- Opened the FastAPI Swagger UI at http://localhost:8000/docs.
- Connected the application container to the locally running Ollama service.
- Ingested documents from docs/sample.
- Successfully queried the API and received an answer with source references.

## 5. Local Model Configuration

The application was configured to use llama3.2:3b through Ollama.

The model runs locally, so the test query did not require a paid hosted LLM API. The application's displayed cost is an estimate and should not be interpreted as an actual Ollama API charge.

## 6. Configuration and Security Notes

- The local .env file contains environment-specific configuration and secrets and must not be committed.
- Authentication was disabled for local testing.
- This configuration is intended only for local development. Authentication and access controls must be reviewed before any public deployment.
- Generated local files, including openapi-local.json, are excluded from Git.

## 7. Learning Outcomes

Through this setup, I practised running a containerized RAG application, configuring a local LLM, ingesting documents, testing FastAPI endpoints, and tracing the flow from document retrieval to answer generation.

## 8. Next Steps

- Build a React interface that calls the existing query API.
- Test retrieval relevance and source quality with additional questions.
- Review error handling, configuration, and security before considering deployment.
