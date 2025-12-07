# 🔍 Semantic Image Search

A powerful semantic image search engine that leverages multimodal AI to enable intelligent image retrieval using natural language queries or visual similarity. Built with OpenAI's CLIP (via OpenCLIP), Qdrant vector database, and LangChain for advanced query processing.

## ✨ Features

- **🔤 Text-to-Image Search**: Find images by describing them in natural language
- **🖼️ Image-to-Image Search**: Discover similar images using a reference image
- **🧠 Intelligent Query Translation**: LLM-powered query optimization using Google's Gemini for better search results
- **⚡ High-Performance Vector Search**: Lightning-fast retrieval using Qdrant vector database
- **🎨 Modern Interactive UI**: Beautiful and intuitive interface built with Streamlit
- **🚀 Robust REST API**: Production-ready FastAPI backend with comprehensive logging
- **📊 Structured Logging**: JSON-based logging with structlog for better observability
- **🏗️ Scalable Architecture**: Modular design ready for production deployment

## 🛠️ Technology Stack

| Component | Technology |
|-----------|-----------|
| **Programming Language** | Python 3.12+ |
| **Embeddings** | OpenCLIP (ViT-B-32) with torch |
| **Vector Database** | Qdrant |
| **LLM Orchestration** | LangChain |
| **Query Translation** | Google Gemini 2.5 Pro |
| **Backend API** | FastAPI + Uvicorn |
| **Frontend UI** | Streamlit |
| **Logging** | structlog (JSON structured logging) |
| **Configuration** | python-dotenv |

## 📋 Prerequisites

- Python 3.12 or higher
- Qdrant instance (cloud or local)
- Google API key (for query translation)
- 4GB+ RAM (for model loading)

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/AyushPoddar351/Semantic-Image-Search.git
cd Semantic-Image-Search
```

### 2. Install Dependencies

Using **pip**:
```bash
pip install -e .
```

Or using **uv** (recommended for faster installs):
```bash
uv sync
```

### 3. Environment Configuration

Create a `.env` file in the project root:

```env
# Qdrant Configuration
QDRANT_URL=https://your-qdrant-instance.cloud
QDRANT_API_KEY=your_qdrant_api_key
QDRANT_COLLECTION=semantic-image-search

# Vector Configuration
VECTOR_SIZE=512

# CLIP Model Configuration
CLIP_MODEL_NAME=ViT-B-32
CLIP_CHECKPOINT=laion2b_s34b_b79k
DEVICE=cpu  # or 'cuda' for GPU

# LLM Configuration (Query Translation)
GOOGLE_API_KEY=your_google_api_key
GOOGLE_MODEL=gemini-2.5-pro

# Optional: OpenAI (alternative to Gemini)
# OPENAI_API_KEY=your_openai_api_key
# OPENAI_MODEL=gpt-4o-mini

# Paths
IMAGES_ROOT=./images
QUERY_IMAGE_ROOT=./data/query_images
RETRIEVED_ROOT=./data/retrieved
```

## 📦 Project Structure

```
semantic_image_search/
├── backend/                    # Core backend logic
│   ├── main.py                # FastAPI application entry point
│   ├── config.py              # Configuration management
│   ├── embeddings.py          # CLIP embedding service
│   ├── qdrant_client.py       # Qdrant client manager
│   ├── ingestion.py           # Image indexing service
│   ├── retriever.py           # Search and retrieval logic
│   ├── query_translator.py    # LLM-based query optimization
│   ├── logger/                # Structured logging setup
│   │   ├── custom_logger.py
│   │   └── __init__.py
│   └── exception/             # Custom exception handling
│       └── custom_exception.py
├── ui/                        # Streamlit frontend
│   └── app.py                 # Web interface
└── notebooks/                 # Jupyter notebooks for experiments
```

## 🏃 Usage

### Step 1: Index Your Images

First, prepare your images in a folder structure (optional category-based organization):

```
images/
├── animals/
│   ├── cat1.jpg
│   └── dog1.jpg
├── landscapes/
│   ├── mountain.jpg
│   └── beach.jpg
└── food/
    └── pizza.jpg
```

### Step 2: Start the Backend Server

```bash
uvicorn semantic_image_search.backend.main:app --reload
```

The API will be available at `http://localhost:8000`

**📚 API Documentation**: `http://localhost:8000/docs`

### Step 3: Index Images via API

```bash
curl -X POST "http://localhost:8000/ingest?folder_path=./images"
```

Or use the default path from your `.env`:

```bash
curl -X POST "http://localhost:8000/ingest"
```

### Step 4: Start the Frontend

In a new terminal:

```bash
streamlit run semantic_image_search/ui/app.py
```

The interface will open at `http://localhost:8501`

## 🔧 API Endpoints

### 1. **Ingest Images** 📥
```http
POST /ingest?folder_path=/path/to/images
```
Index a folder of images into the vector database.

### 2. **Search by Text** 🔤
```http
GET /search-text?q=cat on a sofa&k=5&category=animals&save_results=false
```
Search images using natural language.

**Parameters:**
- `q` (required): Search query
- `k` (optional): Number of results (default: 5)
- `category` (optional): Filter by category
- `save_results` (optional): Save results locally

### 3. **Search by Image** 🖼️
```http
POST /search-image
Content-Type: multipart/form-data

file: <image_file>
k: 5
category: animals
save_results: false
```
Find similar images using an uploaded reference image.

### 4. **Translate Query** 🌐
```http
GET /translate?q=show me pictures of cute dogs
```
Translate/optimize a natural language query for better CLIP retrieval.

## 💡 How It Works

1. **Indexing Phase**:
   - Images are processed through OpenCLIP to generate 512-dimensional embeddings
   - Embeddings are stored in Qdrant with metadata (filename, path, category)

2. **Search Phase**:
   - User query (text or image) is processed through the same CLIP model
   - Query embedding is compared against indexed vectors using cosine similarity
   - Top-K most similar images are retrieved

3. **Query Translation** (for text searches):
   - Natural language queries are optimized using Gemini LLM
   - Converts conversational queries to CLIP-friendly captions
   - Example: "show me cats" → "domestic cat sitting"

## 🎯 Example Usage

### Python API Client

```python
import requests

# Search by text
response = requests.get(
    "http://localhost:8000/search-text",
    params={"q": "sunset over mountains", "k": 5}
)
results = response.json()

# Search by image
with open("query_image.jpg", "rb") as f:
    response = requests.post(
        "http://localhost:8000/search-image",
        files={"file": f},
        params={"k": 5}
    )
results = response.json()
```

## 📊 Performance Optimization

- **Batch Indexing**: Images are indexed in batches for efficiency
- **Vector Quantization**: Qdrant's on-disk storage for large datasets
- **Lazy Loading**: Services initialized only when needed
- **Caching**: Singleton pattern for model and client instances

## 🐛 Troubleshooting

### Issue: "QDRANT_URL missing in environment"
**Solution**: Ensure `.env` file exists with valid Qdrant credentials

### Issue: "Failed to load CLIP model"
**Solution**: Check internet connection for model download, or verify `DEVICE` setting

### Issue: "Query translation timeout"
**Solution**: Increase timeout in `query_translator.py` or check API key validity

## 🚀 Future Enhancements

- [ ] Multi-modal search (text + image combined)
- [ ] BLIP-based image captioning for better metadata
- [ ] Re-ranking with cross-encoders
- [ ] Docker deployment configuration
- [ ] Kubernetes manifests for production
- [ ] S3/GCS storage integration
- [ ] Redis caching layer
- [ ] Monitoring with Prometheus/Grafana


⭐ If you find this project useful, please consider giving it a star on GitHub!