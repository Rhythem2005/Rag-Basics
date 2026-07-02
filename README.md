# 🔍 RAG Basics: Deep Dive & Architecture

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![LangChain](https://img.shields.io/badge/Framework-LangChain-emerald.svg?style=flat-square)](https://github.com/langchain-ai/langchain)
[![VectorDB](https://img.shields.io/badge/Vector%20DB-Chroma%20%7C%20FAISS-orange.svg?style=flat-square)](https://github.com/chroma-core/chroma)
[![LLM](https://img.shields.io/badge/LLM-Groq%20(Llama%203.3)-red.svg?style=flat-square)](https://groq.com/)

An educational, step-by-step implementation of a **Retrieval-Augmented Generation (RAG)** pipeline. This project breaks down how custom document collections are ingested, embedded, stored, and queried to generate context-aware answers using LLMs.

---

## 🏗️ System Architecture

The following diagram illustrates how data flows through the RAG pipeline during both **Ingestion** (offline) and **Retrieval/Generation** (online) phases:

```mermaid
graph TD
    %% Styling
    classDef Ingestion fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,color:#0d47a1;
    classDef Retrieval fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px,color:#1b5e20;
    classDef Storage fill:#fff3e0,stroke:#ef6c00,stroke-width:2px,color:#e65100;
    classDef LLM fill:#fce4ec,stroke:#c2185b,stroke-width:2px,color:#880e4f;

    %% Phase 1: Ingestion
    subgraph Ingestion_Phase ["📥 Phase 1: Data Ingestion (Offline)"]
        A[📄 Multi-Format Documents<br>PDF, TXT, CSV, DOCX, XLSX, JSON] --> B[✂️ Recursive Character Text Splitter]
        B --> C[🧠 sentence-transformers<br>all-MiniLM-L6-v2]
        C --> D[💾 Vector Store Ingestion]
    end

    %% Storage
    subgraph Vector_DB ["🗄️ Vector Databases"]
        E[(FAISS<br>Flat Index)]
        F[(ChromaDB<br>Persistent Client)]
    end
    D --> E
    D --> F

    %% Phase 2: Retrieval & Generation
    subgraph Retrieval_Phase ["⚡ Phase 2: Search & Generation (Online)"]
        G[👤 User Query] --> H[🧠 Generate Query Embedding]
        H --> I[🔍 Similarity Search<br>Cosine Similarity / Distance]
        I -->|Retrieve Top-K Contexts| J[📝 Context Constructor]
        J --> K[🧠 Prompt Assembly]
        K --> L[🤖 Groq LLM<br>Llama 3.3 70B]
        L --> M[✨ Final Answer]
    end

    %% Connections
    E -.-> I
    F -.-> I

    %% Apply Classes
    class A,B,C,D Ingestion_Phase;
    class G,H,I,J,K,M Retrieval_Phase;
    class E,F Storage;
    class L LLM;
```

---

## ⏱️ Interactive Sequence Flow

When a user asks a question, here is the exact chronological execution flow of the system:

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant App as app.py (RAGSearch)
    participant DB as Vector Database (Chroma/FAISS)
    participant LLM as Groq ChatGroq (Llama 3.3)

    User->>App: Submits Query ("What is attention mechanism?")
    Note over App: Generate Query Embedding<br/>via SentenceTransformer
    App->>DB: Query Vectors (top_k=3)
    DB-->>App: Return Top-3 Semantic Matches (Chunks + Metadata)
    Note over App: Construct context from matching chunks
    Note over App: Format Prompt Template<br/>[System instructions + Context + Query]
    App->>LLM: Send structured prompt
    LLM-->>App: Stream / Return Generated Text Answer
    App-->>User: Display response + Source Citations
```

---

## 🗄️ Vector Database Comparison

This repository is configured to demonstrate two popular vector database options:

| Feature | 🗃️ ChromaDB | ⚡ FAISS |
| :--- | :--- | :--- |
| **Type** | Full Vector Database (Server / Serverless) | Vector Indexing Library |
| **Storage** | SQLite-backed persistent directory | In-memory with file serialization (`.index`/`.faiss`) |
| **Metadata Filtering**| Built-in native filtering (SQL-like dicts) | Requires external indexing or wrapper |
| **Best Used For** | Prototyping, full RAG apps with metadata queries | High-performance similarity search on raw vectors |

---

## 📁 Repository Structure

```text
├── README.md                  # Comprehensive documentation and diagrams (this file)
├── requirements.txt           # Python dependencies
└── Understanding/             # Main project directory
    ├── app.py                 # Core application entry point
    ├── data/                  # Directory containing docs to ingest (PDFs, TXTs, etc.)
    ├── notebook/              # Step-by-step Jupyter Notebook walkthroughs
    │   ├── data ingestion.ipynb
    │   └── pdf_loader.ipynb
    └── src/                   # Core modular packages
        ├── data_loader.py     # Document loaders for all supported extensions
        ├── embedding.py       # SentenceTransformers pipeline
        ├── search.py          # RAG retrieval and execution logic
        └── vectorstore.py     # Database wrappers
```

---

## 🛠️ Installation & Setup

> [!IMPORTANT]
> Ensure you have Python 3.11+ installed before proceeding.

1. **Clone the repository:**
   ```bash
   git clone <repo-url>
   cd RAG
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure Environment Keys:**
   Create a `.env` file inside the `Understanding/` folder and paste your Groq API key:
   ```env
   GROQ_API_KEY=gsk_your_groq_api_key
   ```

---

## 🚀 Usage

To test out the RAG pipeline pipeline:
```bash
cd Understanding
python3 app.py
```

> [!TIP]
> Ensure you have placed some data files (e.g. PDFs, TXT files) inside the `Understanding/data/` folder before running the loader for the first time.
