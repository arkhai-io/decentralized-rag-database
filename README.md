<div align="center">
  <img src="assets/logo.jpg" alt="Arkhai Logo" width="200"/>
  
  # Arkhai
  
  **Decentralized RAG platform for scientific literature processing and retrieval**
  
  ![Python](https://img.shields.io/badge/python-3.10+-blue.svg)
  ![License](https://img.shields.io/badge/license-MIT-yellow.svg)
</div>

---

## Overview

Arkhai is a modular RAG system enabling distributed document processing across heterogeneous vector databases with configurable conversion, chunking, and embedding strategies. Built with a multi-server architecture, it provides IPFS-backed storage, blockchain incentivization, and production-ready APIs for document ingestion and retrieval.

**Core Capabilities**
- Multi-server architecture (Light, Heavy, Database) for optimized performance
- IPFS integration for decentralized content addressing
- Pluggable processing components with version-controlled configurations
- Token-based reward system for contributor incentivization
- Real-time progress tracking and evaluation

---

## Quick Start

### Prerequisites

- Python 3.10+, Poetry
- OpenAI API, Lighthouse/IPFS
- Neo4j, PostgreSQL
- Docker (optional)

### Installation

```bash
git clone https://github.com/arkhai/decentralized-rag-database.git
cd decentralized-rag-database
bash scripts/setup.sh
poetry install
cp .env.example .env
```

Configure `.env` with required API keys and database credentials.

### Running Servers

```bash
# Start all three servers in separate terminals
bash scripts/start_light_server.sh      # Port 5001
bash scripts/start_heavy_server.sh      # Port 5002
bash scripts/start_database_server.sh   # Port 5003
```

API documentation available at `/docs` endpoint for each server.

---

## API Reference

### Light Server (Port 5001)

**`GET /api/v1/user/status`** - Monitor document processing progress for a user
```json
{
  "total_jobs": 100,
  "completed_jobs": 75,
  "completion_percentage": 75.0
}
```

### Heavy Server (Port 5002)

**`POST /api/v1/users/ingestion`** - Ingest and process PDFs from Google Drive
```json
{
  "drive_url": "https://drive.google.com/drive/folders/1ABC123...",
  "processing_combinations": [
    ["markitdown", "recursive", "bge"],
    ["openai", "semantic_split", "openai"]
  ],
  "user_email": "user@example.com"
}
```

**Processing Components:**
- Converters: `marker`, `openai`, `markitdown`
- Chunkers: `fixed_length`, `recursive`, `markdown_aware`, `semantic_split`
- Embedders: `openai`, `nvidia`, `bge`, `bgelarge`, `e5large`

### Database Server (Port 5003)

**`POST /api/v1/user/evaluate`** - Query processed document databases
```json
{
  "query": "What are the latest developments in CRISPR?",
  "user_email": "user@example.com",
  "model_name": "openai/gpt-4o-mini",
  "collections": ["optional_filter"],
  "k": 5
}
```

**`POST /api/v1/user/evaluate/aggregate`** - Query with intelligent result aggregation
```json
{
  "query": "What are the latest developments in CRISPR?",
  "user_email": "user@example.com",
  "aggregation_strategy": "hybrid",
  "top_k": 5,
  "similarity_weight": 0.7,
  "frequency_weight": 0.3
}
```

**`POST /api/v1/user/reranker`** - Rerank results using cross-encoder models
```json
{
  "user_email": "user@example.com",
  "query": "What are the latest developments in CRISPR?",
  "items": ["text1...", "text2..."],
  "model_preset": "msmarco-MiniLM-L-6-v2",
  "top_k": 10
}
```

**Metadata Structure:**

Response metadata includes IPFS content identifiers (`content_cid`, `root_cid`, `embedding_cid`) and document metadata (`title`, `authors`, `abstract`, `doi`, `publication_date`, `journal`, `keywords`, `url`).

---

## Project Structure

```
decentralized-rag-database/
├── config/        # Pipeline configurations
├── src/           # Core processing, storage, and reward modules
├── scripts/       # Execution scripts
├── contracts/     # Blockchain contract ABIs
├── docker/        # Containerization
├── tests/         # Test suites
└── erc20-token/   # Token system
```

---

## License

MIT License