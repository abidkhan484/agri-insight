# 🤖 G. Local AI Assistant — No API Key, No Cost

## Overview

Run a small AI assistant for farming queries directly on your laptop or a Raspberry Pi 5 — completely offline, no Anthropic/OpenAI billing. Farmers type questions in Bangla → get answers from your own ZBNF knowledge base.

## Problem It Solves

Farmers have specific, contextual questions ("My rice leaves are turning yellow after transplanting, what should I do in ZBNF?"). A generic Google search gives chemical farming advice. A local AI trained on YOUR ZBNF knowledge base gives relevant, ZBNF-aligned answers.

## How It Works (RAG — Retrieval Augmented Generation)

```
[Your ZBNF documents (markdown/text files)]
    ↓ one-time indexing
[Vector database (ChromaDB / FAISS — free, local)]
    ↓
[User asks: "ধানের পাতা হলুদ হচ্ছে কেন?"]
    ↓
[RAG retrieves relevant ZBNF paragraphs from your docs]
    ↓
[Local LLM (Llama 3 / Mistral / Gemma 2 via Ollama)]
    ↓
[Answer: "This is likely nitrogen deficiency. In ZBNF, increase
  Jeevamrutha application frequency to every 7 days instead of 15.
  Also apply cow urine spray (1:10 dilution) as foliar feed."]
```

## Tech Stack

| Component | Technology | Cost |
|---|---|---|
| Local LLM runtime | [Ollama](https://ollama.ai) — runs open models locally | Free |
| Models | Llama 3 (8B), Mistral 7B, or Gemma 2 (2B for low-end hardware) | Free |
| RAG framework | LlamaIndex or LangChain (Python, open-source) | Free |
| Vector store | ChromaDB (local, file-based) or FAISS | Free |
| UI | [Jan.ai](https://jan.ai) (desktop UI) or simple Flask/Streamlit web UI | Free |
| Knowledge base | Your own ZBNF markdown docs (this entire docs/ folder!) | Free |

## Hardware Requirements

| Setup | Min RAM | Works? |
|---|---|---|
| Decent laptop (8GB+ RAM) | 8 GB | ✅ Runs Gemma 2B or Mistral 7B (quantized) |
| Good laptop (16GB+ RAM) | 16 GB | ✅ Runs Llama 3 8B smoothly |
| Raspberry Pi 5 (8GB) | 8 GB | ✅ Runs Gemma 2B (slower but works) |
| Old/low-end laptop (4GB) | 4 GB | ⚠️ Use TinyLlama 1.1B — very basic answers |

## Knowledge Base Structure

Feed these documents to the RAG system:

```
docs/
├── initial-findings.md          # Core ZBNF principles and methods
├── pest-management/             # All pest control knowledge
├── multilayer-farming/          # Crop combinations and layout
├── phases/                      # Season-by-season guidance
└── technology/                  # Tech tool documentation
```

The RAG system chunks these documents, creates embeddings, and retrieves relevant sections when a farmer asks a question.

## Key Design Decisions

- **RAG over fine-tuning**: Fine-tuning a model requires GPU and time. RAG just indexes your existing docs and retrieves relevant sections — much simpler and the knowledge base is easy to update.
- **Ollama over manual model setup**: Ollama handles model downloading, quantization, and serving with a single command (`ollama run llama3`).
- **Bangla support**: Most open models handle Bangla reasonably well. For better Bangla, use multilingual models like Gemma 2.
- **No internet after setup**: Once the model and docs are downloaded, everything runs offline.

## Setup Steps (High Level)

1. Install Ollama: `curl -fsSL https://ollama.ai/install.sh | sh`
2. Pull a model: `ollama pull gemma2:2b` (or `llama3:8b` for better quality)
3. Install Python dependencies: `pip install llama-index chromadb`
4. Index your ZBNF docs into ChromaDB
5. Build a simple query interface (CLI or web UI)
6. Ask questions → get ZBNF-grounded answers

## Complexity

🟡 Intermediate to 🔴 Advanced — 3–5 days for a working RAG setup.

## References

- [Ollama](https://ollama.ai/)
- [Jan.ai](https://jan.ai/)
- [LlamaIndex docs](https://docs.llamaindex.ai/)
- [ChromaDB](https://www.trychroma.com/)
