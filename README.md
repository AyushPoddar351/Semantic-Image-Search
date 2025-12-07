# Semantic Image Search

A powerful semantic image search engine that leverages multimodal AI to enable searching for images using natural language queries or other images. This project combines state-of-the-art technologies including OpenAI's CLIP (via OpenCLIP), Qdrant vector database, and LangChain to provide accurate and fast search capabilities.

## 🚀 Features

*   **Text-to-Image Search**: Find images by describing them in natural language.
*   **Image-to-Image Search**: Find similar images by uploading a reference image.
*   **Query Translation**: Intelligent query processing and translation using LLMs to optimize search results.
*   **High-Performance Vector Search**: Utilizes Qdrant for efficient and scalable vector storage and retrieval.
*   **Modern Interactive UI**: User-friendly interface built with Streamlit.
*   **Robust API**: RESTful API backend built with FastAPI.

## 🛠️ Technology Stack

*   **Core Logic**: Python 3.12+
*   **Embeddings**: OpenCLIP (torch)
*   **Vector Database**: Qdrant
*   **Orchestration**: LangChain
*   **Backend**: FastAPI, Uvicorn
*   **Frontend**: Streamlit

## 📋 Prerequisites

Ensure you have the following installed:
*   Python 3.12 or higher
*   pip or uv (for package management)

## 📦 Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/AyushPoddar351/Semantic-Image-Search.git
    cd Prj-5-Semantic-Image-Search
    ```

2.  **Install dependencies:**
    This project uses `pyproject.toml` for dependency management.

    Using `pip`:
    ```bash
    pip install -e .
    ```

    Or using `uv` (recommended):
    ```bash
    uv sync
    ```

## ⚙️ Configuration

Ensure you have the necessary environment variables set up. Check `semantic_image_search/backend/config.py` for default values. You may need to create a `.env` file for API keys if you are using external LLM services for query translation.

## 🏃 Usage

The project consists of a backend API and a frontend UI.

### 1. Start the Backend Server

Run the FastAPI backend using Uvicorn from the root directory:

```bash
uvicorn semantic_image_search.backend.main:app --reload
```
The API will be available at `http://localhost:8000`. You can access the automatic documentation at `http://localhost:8000/docs`.

**API Endpoints:**
*   `GET /search-text`: Search images using text queries.
*   `POST /search-image`: Search images using an uploaded image.
*   `POST /ingest`: Index a folder of images.
*   `GET /translate`: Translate/optimize a search query.

### 2. Start the Frontend Application

In a new terminal, launch the Streamlit UI:

```bash
streamlit run semantic_image_search/ui/app.py
```
The web interface will open automatically in your browser (usually at `http://localhost:8501`).

## 📁 Project Structure

```
semantic_image_search/
├── backend/            # FastAPI application and core logic
│   ├── ingestion.py    # Image indexing service
│   ├── retriever.py    # Search and retrieval logic
│   ├── qdrant_client.py # Vector DB interaction
│   ├── main.py         # API entry point
│   └── ...
├── ui/                 # Streamlit frontend code
│   └── app.py
├── notebooks/          # Jupyter notebooks for experiments
└── ...
```
