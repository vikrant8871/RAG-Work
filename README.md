# RAG Statistic based model

A minimal Retrieval-Augmented Generation (RAG) project demonstrating how to index local documents, create embeddings, persist a vector store, and answer questions using an LLM with retrieved context.

## Project Structure

```
RAG Work/
├─ data/
│  ├─ pdf/
│  │  └─ Statistic_For_Data_Science_1704226429.pdf
│  ├─ text_files/
│  │  ├─ machine_learning.txt
│  │  └─ python_intro.txt
│  └─ vector_store/             # Persisted Chroma/FAISS-like store
│     ├─ chroma.sqlite3
│     └─ ...
├─ notebook/
│  ├─ document.ipynb            # Basic document handling / exploration
│  └─ RAG_pipeline.ipynb        # End-to-end RAG pipeline walkthrough
├─ Source/
│  └─ __init__.py               # Package placeholder (optional extensions)
├─ main.py                      # CLI / script entry-point for RAG demo
├─ requirements.txt             # Python dependencies
├─ .env                         # Environment variables (API keys, config)
└─ README.md                    # This file
```

## Features

- Load documents from PDF and plain text.
- Chunk and embed documents using an embedding model.
- Persist embeddings to a local vector store (Chroma).
- Retrieve top-k relevant chunks for a user query.
- Generate answers with an LLM grounded in retrieved context.
- Jupyter notebooks to explore and iterate on the pipeline.

## Prerequisites

- Python 3.10+
- A supported LLM provider/API key if you intend to run generation (e.g., Groq, OpenAI, or local). For Groq, set `GROQ_API_KEY` in `.env`.
- Basic build tools for installing Python packages.

## Setup

1) Clone the repository (or open the folder).

2) Create a virtual environment and activate it.
- Windows (cmd):
  - python -m venv .venv
  - .venv\Scripts\activate

3) Install dependencies:
- pip install -r requirements.txt

4) Configure environment variables by creating a `.env` file in the project root:
- Example:
  GROQ_API_KEY=your_api_key_here
  MODEL=llama-3.1-8b-instant

Note: If you previously used a decommissioned model (e.g., `gemma2-9b-it`), update `MODEL` to a currently supported one. Refer to your provider docs.

## Data

Place your source documents in:
- data/pdf/ for PDFs
- data/text_files/ for plaintext files

The vector database persists to `data/vector_store/` so you don’t need to rebuild embeddings every run.

## Usage

### Option A: Run the script

- python main.py

Typical flow implemented by `main.py`:
- Load and preprocess documents from `data/`.
- Build/refresh embeddings and persist to `data/vector_store/`.
- Accept a user query and retrieve top-k relevant chunks.
- Call the LLM with retrieved context and print the answer.

Environment-driven configuration (commonly read from `.env`):
- GROQ_API_KEY: API key for Groq (if used).
- MODEL: Chat model identifier, e.g., `llama-3.1-8b-instant`.
- Any additional flags you add to `main.py`.

### Option B: Explore notebooks

Open the notebooks in `notebook/`:
- notebook/RAG_pipeline.ipynb: End-to-end RAG pipeline with step-by-step cells.
- notebook/document.ipynb: Document loading/inspection examples.

Run cells sequentially to build the index and test queries.

## Troubleshooting

- BadRequestError: model decommissioned
  - Cause: The selected model (e.g., `gemma2-9b-it`) is no longer supported by the provider.
  - Fix: Update your model to a supported one, e.g., `llama-3.1-8b-instant` or `llama-3.1-70b-versatile` and retry.

- Missing API key or auth error
  - Ensure `.env` contains a valid key (e.g., `GROQ_API_KEY`) and the environment is loaded.

- Slow or expensive generations
  - Use a smaller/faster model, reduce context, or lower the number of retrieved chunks.

- Vector store not found / blank answers
  - Ensure documents exist under `data/`.
  - Re-run the indexing step to (re)create `data/vector_store/`.

## Extending

- Swap embedding model or LLM provider via configuration.
- Add new loaders/parsers for additional file types.
- Implement reranking to improve retrieval quality.
- Add evaluation (e.g., RAGAS) to measure fidelity and grounding.
- Wrap `main.py` as a web/API service (FastAPI/Streamlit/Gradio).

## Requirements

See requirements.txt. Typical stack includes:
- chromadb (vector store)
- sentence-transformers or similar embedding models
- langchain / llama_index (if used)
- groq (if using Groq LLM API)
- python-dotenv
- jupyter


## Acknowledgments

- Groq for LLM serving and deprecation guidance.
- Open-source libraries enabling the RAG workflow.
