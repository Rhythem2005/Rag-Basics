# RAG Basics

A basic Retrieval-Augmented Generation (RAG) pipeline designed as an educational starting point for understanding how RAG systems process, store, and query documents.

## Project Overview

This repository demonstrates the fundamental steps of building a RAG application:
1. **Document Loading**: Ingesting different file formats (PDF, TXT, CSV, Word, Excel, JSON).
2. **Text Chunking**: Splitting large documents into smaller, manageable chunks.
3. **Embedding Generation**: Converting text chunks into vector embeddings using a sentence-transformer model.
4. **Vector Storage**: Storing and retrieving vector embeddings (demonstrated with ChromaDB and FAISS).
5. **RAG Querying & Generation**: Retrieving relevant contexts and prompting a Large Language Model (Groq LLM) to generate answers based on documents.

## Repository Structure

```text
├── README.md                  # Project overview and guide (this file)
├── requirements.txt           # Python dependencies
└── Understanding/             # Main project directory
    ├── app.py                 # Core application entry point
    ├── data/                  # Sample data directory for document ingestion
    ├── notebook/              # Jupyter Notebooks for step-by-step learning
    │   ├── data ingestion.ipynb
    │   └── pdf_loader.ipynb
    └── src/                   # Source scripts representing modular components
        ├── data_loader.py     # Loader scripts for diverse formats
        ├── embedding.py       # Embedding generation pipeline
        ├── search.py          # Search and retrieval logic
        └── vectorstore.py     # Vector store manager
```

## Setup & Getting Started

### Prerequisites

- Python 3.11+
- Virtual environment (recommended)

### Installation

1. Clone the repository and navigate to the project directory:
   ```bash
   cd RAG
   ```

2. Install the required packages:
   ```bash
   pip install -r requirements.txt
   ```

3. Set up your environment variables by creating a `.env` file in the `Understanding/` directory and adding your API key:
   ```env
   GROQ_API_KEY=your_actual_groq_api_key_here
   ```

## Running the Application

To run the sample pipeline entry point:
```bash
cd Understanding
python3 app.py
```
