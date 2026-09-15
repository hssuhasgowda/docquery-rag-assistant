# DocQuery --- Production-Style RAG System

A production-oriented Retrieval-Augmented Generation (RAG) application
for querying technical and business documents.

## Overview

DocQuery demonstrates an end-to-end AI Engineering pipeline:

-   Document ingestion
-   Document chunking
-   Embedding generation
-   Dense semantic retrieval
-   Sparse keyword retrieval
-   Hybrid search with Reciprocal Rank Fusion (RRF)
-   Cross-encoder reranking
-   Context construction
-   Grounded LLM generation
-   Source citations
-   RAG evaluation
-   FastAPI APIs
-   Dockerized deployment
-   Local LLM inference with Ollama

## Architecture

``` text
Documents
   |
   v
Ingestion / Parsing / Chunking
   |
   v
Embeddings + Metadata
   |
   v
Qdrant Vector Database
   |
   |-----------------------------|
   |                             |
   v                             v
Dense Retrieval            Sparse Retrieval
   |                             |
   |------------- RRF ------------|
                 |
                 v
          Cross-Encoder
            Reranking
                 |
                 v
          Context Builder
                 |
                 v
          Llama 3.2 3B
             (Ollama)
                 |
                 v
        Answer + Citations
```

## Technology Stack

  Area                Technology
  ------------------- ------------------------------------------
  Backend             Python, FastAPI
  RAG                 Retrieval-Augmented Generation
  Vector Database     Qdrant
  Embeddings          `sentence-transformers/all-MiniLM-L6-v2`
  Hybrid Retrieval    Dense + Sparse + RRF
  Reranking           `cross-encoder/ms-marco-MiniLM-L-6-v2`
  LLM                 Llama 3.2 3B
  Local LLM Runtime   Ollama
  Database            MySQL
  Frontend            TypeScript
  API Documentation   Swagger / OpenAPI
  Containerization    Docker Compose
  Evaluation          RAGAS
  Authentication      Microsoft Entra ID support

## Key RAG Components

### Document Ingestion

Documents are processed, chunked, embedded, and indexed in Qdrant.

Supported document types depend on the configured ingestion pipeline and
include formats such as:

-   PDF
-   DOCX
-   TXT
-   Markdown

### Hybrid Search

The query pipeline combines:

1.  Dense semantic retrieval
2.  Sparse keyword retrieval
3.  Reciprocal Rank Fusion (RRF)

This allows the system to handle both semantic questions and technical
keyword-heavy queries.

### Reranking

Retrieved candidates are reranked using:

``` text
cross-encoder/ms-marco-MiniLM-L-6-v2
```

This improves the ordering of candidate passages before generation.

### Grounded Generation

The language model receives retrieved document context and is instructed
to answer using that context. This helps reduce unsupported answers and
hallucinations.

### Citations

Responses contain references to the source documents/chunks used to
construct the answer.

## Local LLM Setup

This version is configured to run without an OpenAI API key by using
Ollama.

Configured model:

``` text
llama3.2:3b
```

Install/pull the model:

``` powershell
ollama pull llama3.2:3b
```

Verify:

``` powershell
ollama list
```

The application uses Ollama's OpenAI-compatible API from the Docker
container.

## Running Locally

### Prerequisites

Install:

-   Docker Desktop
-   Git
-   Ollama

Verify:

``` powershell
docker --version
docker compose version
ollama --version
```

### Environment Configuration

For local development, the important settings include:

``` env
AUTH_ENABLED=false
LLM_MODEL=llama3.2:3b
OPENAI_TIMEOUT_SECONDS=180
LLM_MAX_TOKENS=512
RERANKER_TOP_K=3
EMBEDDING_MODEL=all-MiniLM-L6-v2
```

Do not commit real credentials or secrets.

### Start the Application

``` powershell
docker compose up -d --build
```

Check the services:

``` powershell
docker compose ps
```

Application:

``` text
http://localhost:8000
```

Swagger:

``` text
http://localhost:8000/docs
```

Health check:

``` text
http://localhost:8000/health
```

## Adding Documents

Place a document inside the project's `docs` directory.

Example:

``` text
docs/
├── company_policy.pdf
├── product_manual.docx
├── architecture.md
└── notes.txt
```

Open Swagger:

``` text
http://localhost:8000/docs
```

Use:

``` text
POST /ingest
```

Example:

``` json
{
  "path": "docs/company_policy.pdf"
}
```

The API returns an ingestion task ID.

Check the ingestion status using:

``` text
GET /ingest/{task_id}
```

Successful ingestion returns a status similar to:

``` json
{
  "task_id": "example-task-id",
  "status": "done",
  "chunks": 10,
  "deleted": 0,
  "error": null
}
```

## Querying Documents

Use:

``` text
POST /query
```

Example:

``` json
{
  "query": "What is the architecture of this system?"
}
```

The pipeline is:

``` text
Question
   |
   v
Query Processing
   |
   v
Dense + Sparse Retrieval
   |
   v
RRF Fusion
   |
   v
Cross-Encoder Reranking
   |
   v
Context Construction
   |
   v
Llama 3.2 3B
   |
   v
Answer + Sources
```

## Main API Endpoints

  Method   Endpoint                             Purpose
  -------- ------------------------------------ --------------------------
  GET      `/health`                            Application health
  GET      `/config`                            Frontend configuration
  POST     `/ingest`                            Ingest documents
  GET      `/ingest/{task_id}`                  Check ingestion status
  POST     `/query`                             Query the RAG system
  POST     `/query/stream`                      Streaming query response
  GET      `/conversations`                     List conversations
  GET      `/conversations/{conversation_id}`   Retrieve conversation
  DELETE   `/conversations/{conversation_id}`   Delete conversation
  POST     `/feedback`                          Report document feedback
  GET      `/feedback`                          List reported documents
  POST     `/feedback/resolve`                  Resolve a report

## Example Workflow

``` text
1. Start Docker services
2. Place a document in docs/
3. Call POST /ingest
4. Wait until the task status is done
5. Call POST /query
6. Retrieve the grounded answer
7. Inspect the source citations
```

## Why This Is an AI Engineering Project

This project goes beyond a basic chatbot and demonstrates practical RAG
engineering:

-   Document processing
-   Chunking
-   Embedding models
-   Vector databases
-   Dense retrieval
-   Sparse retrieval
-   Hybrid search
-   Reciprocal Rank Fusion
-   Cross-encoder reranking
-   Context construction
-   LLM integration
-   Local LLM deployment
-   Grounded generation
-   Source citations
-   RAG evaluation
-   FastAPI
-   Docker
-   MySQL
-   TypeScript frontend
-   Authentication support

## Resume Description

### AI-Powered Enterprise Document RAG System

Built a production-oriented Retrieval-Augmented Generation system for
querying technical and business documents. Implemented document
ingestion, chunking, embedding generation, Qdrant vector storage,
dense+sparse hybrid retrieval with Reciprocal Rank Fusion, cross-encoder
reranking, grounded LLM generation using Llama 3.2 3B via Ollama, and
source citations. Containerized the application with Docker Compose and
exposed the RAG pipeline through FastAPI APIs and a web interface.

## Interview Explanation

> I built a production-oriented RAG system for querying enterprise
> documents. Documents are ingested, chunked, embedded, and stored in
> Qdrant. For a user query, the system performs both dense semantic
> retrieval and sparse keyword retrieval, combines the results using
> Reciprocal Rank Fusion, reranks the candidates with a cross-encoder,
> and sends the most relevant context to a local Llama 3.2 model running
> through Ollama. The final response includes source citations so the
> answer can be traced back to the original documents.

## Project Structure

``` text
docquery/
├── src/
│   └── docquery/
│       ├── api/
│       ├── generate/
│       ├── ingest/
│       ├── retrieve/
│       └── config.py
├── frontend/
├── docs/
├── Dockerfile
├── docker-compose.yml
├── .env
└── README.md
```

## Security

Never commit:

``` text
.env
API keys
passwords
tokens
Azure credentials
database credentials
private documents
```

Use environment variables or a proper secret-management system for
sensitive configuration.

## Future Improvements

-   Improve local LLM model selection
-   Add retrieval metrics such as Recall@K and MRR
-   Expand RAGAS evaluation
-   Improve PDF/OCR handling
-   Add stronger metadata filtering
-   Improve streaming responses
-   Add conversation memory
-   Add automated document ingestion
-   Add observability and tracing
-   Add CI/CD
-   Deploy to a cloud environment

## Author

**Venu**

AI Engineer \| Generative AI \| Machine Learning \| LLMs \| RAG
