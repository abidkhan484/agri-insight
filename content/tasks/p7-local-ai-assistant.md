---
title: "P7 — Local AI Assistant"
weight: 70
bookFlatSection: true
---

# 🤖 P7 — Local AI Assistant (Tool G)

## Objective

Set up a RAG (Retrieval Augmented Generation) system that runs entirely locally — Ollama LLM + ChromaDB vector store + your ZBNF docs as the knowledge base — allowing farmers to ask ZBNF questions in Bangla and get grounded, relevant answers without any API cost.

## Prerequisites

- Laptop with 8GB+ RAM (16GB recommended for Llama 3 8B)
- Python ≥ 3.10
- ZBNF documentation content finalized (this project's `docs/` folder)

## Subtasks

### Phase 1: Ollama Setup

- [ ] Install Ollama: `curl -fsSL https://ollama.ai/install.sh | sh`
- [ ] Pull model: `ollama pull gemma2:2b` (low-end) or `ollama pull llama3:8b` (recommended)
- [ ] Verify: `ollama run gemma2:2b "What is Jeevamrutha?"` returns a coherent answer
- [ ] Test Bangla: `ollama run gemma2:2b "জীবামৃত কি?"` — confirm Bangla support quality
- [ ] Document hardware performance: response time, RAM usage

### Phase 2: Knowledge Base Indexing

- [ ] Install dependencies: `pip install llama-index chromadb sentence-transformers`
- [ ] Create `indexer.py` that:
  1. Loads all `.md` files from `docs/` recursively
  2. Chunks documents (512 tokens, 50-token overlap)
  3. Generates embeddings using `sentence-transformers/all-MiniLM-L6-v2`
  4. Stores in ChromaDB (local, file-based at `./chroma_db/`)
- [ ] Run indexing: verify document count and chunk count
- [ ] Add re-indexing script for when docs are updated

### Phase 3: RAG Query Engine

- [ ] Create `query.py` with RAG pipeline:
  1. Receive user question (Bangla or English)
  2. Embed the question
  3. Retrieve top-5 relevant chunks from ChromaDB
  4. Construct prompt: system instruction + retrieved context + user question
  5. Send to Ollama API (`POST http://localhost:11434/api/generate`)
  6. Return the grounded answer
- [ ] System prompt: "You are a ZBNF farming expert for Bangladesh. Answer only from the provided context. If unsure, say so. Respond in the same language as the question."
- [ ] Add source citation: include which doc section the answer came from

### Phase 4: Web UI

- [ ] Build a simple web UI (Flask or Streamlit):
  - Chat-style input/output
  - "Sources" expandable section showing retrieved doc chunks
  - Bangla input support
  - Loading indicator during inference
- [ ] Alternative: integrate with [Jan.ai](https://jan.ai) desktop UI
- [ ] Test with 20+ common farmer questions

### Phase 5: Optimization & Testing

- [ ] Benchmark: query latency on target hardware (aim for <10s on 8GB RAM)
- [ ] Test with Bangla-only queries — document quality of Bangla responses per model
- [ ] Create a test suite of 30 questions with expected answer themes
- [ ] Tune chunk size and retrieval count (k) for best answer quality
- [ ] Document which model works best for Bangla ZBNF queries

## Acceptance Criteria

- [ ] `ollama` runs locally and responds to queries
- [ ] All ZBNF docs indexed in ChromaDB (verify count)
- [ ] RAG query returns ZBNF-grounded answers (not hallucinated)
- [ ] Bangla queries return Bangla answers with reasonable quality
- [ ] Web UI allows conversational interaction
- [ ] Full system works offline after initial setup
- [ ] Response time < 15 seconds on 8GB RAM machine

## Estimated Effort

⏱️ **3–5 days**

## Dependencies

| Dependency | Status |
|---|---|
| ZBNF documentation content finalized | Required |
| No code dependencies on other tasks | — |
| Hardware: 8GB+ RAM laptop | Required |
