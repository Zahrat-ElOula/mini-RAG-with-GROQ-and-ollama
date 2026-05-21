# Mini RAG with GROQ and Ollama

A Retrieval-Augmented Generation (RAG) system that enables you to ask questions about PDF documents using either GROQ or Ollama as the language model backend.

## 📋 Overview

This project implements a RAG pipeline that:
- **Loads PDF documents** from a local `data/` directory
- **Chunks documents** into manageable pieces using recursive text splitting
- **Creates vector embeddings** using HuggingFace's sentence transformers
- **Retrieves relevant context** using FAISS vector search
- **Generates answers** using either GROQ or Ollama LLMs
- **Cites sources** with accurate page references

## 🚀 Features

- **Two LLM backends:**
  - `rag_pdf_groq.py` - Uses GROQ's high-performance API
  - `rag_pdf_ollama.py` - Uses local Ollama models
- **Semantic search** across PDF documents
- **Context-aware responses** with source citations
- **French language support** for prompts and responses
- **Interactive CLI interface** for asking questions

## 📦 Installation

### 1. Clone the repository
```bash
git clone https://github.com/Zahrat-ElOula/mini-RAG-with-GROQ-and-ollama.git
cd mini-RAG-with-GROQ-and-ollama
```

### 2. Create a virtual environment (optional but recommended)
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

Required packages:
- `langchain`
- `langchain-community`
- `langchain-groq`
- `langchain-ollama`
- `langchain-huggingface`
- `python-dotenv`
- `PyPDF2`
- `faiss-cpu` (or `faiss-gpu` for GPU support)
- `sentence-transformers`

## ⚙️ Configuration

### For GROQ Backend

1. **Get your GROQ API key** from [console.groq.com](https://console.groq.com)

2. **Create a `.env` file** in the project root:
```env
GROQ_API_KEY=your_api_key_here
```

3. **Verify the model** in `rag_pdf_groq.py` (default: `llama-3.3-70b-versatile`)

### For Ollama Backend

1. **Install Ollama** from [ollama.ai](https://ollama.ai)

2. **Pull a model** (default: `qwen2.5:3b`):
```bash
ollama pull qwen2.5:3b
```

3. **Start the Ollama server**:
```bash
ollama serve
```

The server runs on `http://localhost:11434` by default.

## 📄 Setting up PDF Documents

1. Create a `data/` directory in the project root:
```bash
mkdir data
```

2. Add your PDF files to the `data/` directory:
```bash
cp your_document.pdf data/
cp another_document.pdf data/
```

The system supports multiple PDFs and will process them all.

## 🎯 Usage

### Using GROQ Backend
```bash
python rag_pdf_groq.py
```

Output:
```
Chargement des PDF...
Nombre total de pages chargees : 42
Decoupage en chunks...
Nombre total de chunks : 156
Creation des embeddings et de l'index FAISS...

Assistant RAG pret.
Tapez votre question ou 'quit' pour quitter.

Question > Qu'est-ce que le RAG ?
```

### Using Ollama Backend
```bash
python rag_pdf_ollama.py
```

Same interactive interface as above.

## 🔧 Configuration Options

Edit these variables in the Python files to customize behavior:

| Variable | Default | Description |
|----------|---------|-------------|
| `DATA_DIR` | `data` | Directory containing PDF files |
| `EMBEDDING_MODEL` | `sentence-transformers/all-MiniLM-L6-v2` | Embedding model from HuggingFace |
| `GROQ_MODEL` | `llama-3.3-70b-versatile` | GROQ model to use |
| `OLLAMA_MODEL` | `qwen2.5:3b` | Ollama model to use |
| `CHUNK_SIZE` | `800` | Characters per chunk |
| `CHUNK_OVERLAP` | `150` | Overlap between chunks for context |
| `TOP_K` | `3` | Number of chunks to retrieve for context |

## 📊 How It Works

### Processing Pipeline

```
PDFs in data/ 
    ↓
Load & Parse (PyPDFLoader)
    ↓
Split into Chunks (RecursiveCharacterTextSplitter)
    ↓
Generate Embeddings (HuggingFace)
    ↓
Store in FAISS Vector DB
    ↓
User Question
    ↓
Semantic Search (retrieve Top-K chunks)
    ↓
Format Context
    ↓
LLM Response (GROQ or Ollama)
    ↓
Display Answer + Sources
```

### Key Components

- **PyPDFLoader**: Extracts text and metadata from PDF files
- **RecursiveCharacterTextSplitter**: Intelligently chunks documents while preserving context
- **HuggingFaceEmbeddings**: Converts text to 384-dimensional vectors
- **FAISS**: Fast vector similarity search
- **LangChain**: Orchestrates the entire pipeline

## 💡 Example

```bash
Question > Expliquez le concept de vectorisation dans le NLP.

--- Reponse ---
La vectorisation dans le NLP est le processus de conversion du texte en 
représentations numériques...

Sources : Cours_NLP.pdf (page 15), chapitre_rag.pdf (page 8)

--- Chunks recuperes ---
1. Cours_NLP.pdf | page 15
   Vectorization is the process of converting text into numerical representations...
```

## ⚠️ Troubleshooting

### "Aucun PDF trouve dans le dossier 'data'"
- Ensure the `data/` directory exists
- Add PDF files to the `data/` directory
- Check file extensions are `.pdf` (lowercase)

### "La variable GROQ_API_KEY est absente"
- Create a `.env` file with your GROQ API key
- Ensure `.env` is in the project root directory
- Restart the script after adding the key

### Ollama connection error
- Ensure Ollama is running: `ollama serve`
- Check the model is installed: `ollama list`
- Verify the base URL is `http://localhost:11434`

### Slow response times
- Reduce `CHUNK_SIZE` for faster processing
- Decrease `TOP_K` to retrieve fewer chunks
- Use a smaller embedding model
- For Ollama, switch to a smaller model like `mistral:7b` or `neural-chat`

## 🔐 Security Notes

- **Never commit** `.env` files with real API keys to version control
- Add `.env` to `.gitignore`:
```bash
echo ".env" >> .gitignore
```
- Use environment variables or secret management for production

## 📝 License

This project is provided as-is for educational purposes.

## 👤 Author

**Zahrat-ElOula**

---

**Built with LangChain, GROQ, Ollama, and HuggingFace** 🚀
