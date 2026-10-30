# KNOWX 🔍
### Ask questions. Find answers. Explore enterprise knowledge.

**KNOWX – A Hybrid Retrieval-Augmented Generation Framework for Enterprise Knowledge Management**

What if you could ask a question about a company's annual report and get an answer without manually searching through hundreds of pages?

That's the idea behind **KNOWX**.

KNOWX is an AI-powered enterprise document question-answering system that combines semantic search, keyword retrieval, reranking and a locally running Large Language Model (LLM) to find relevant information from enterprise documents and generate context-aware answers.

Instead of searching through lengthy reports, users can simply ask questions in natural language.

---

## ✨ What makes KNOWX interesting?

Enterprise documents contain valuable information, but finding the right information can take time.

Traditional keyword search may miss relevant passages when a question is phrased differently from the original document. Language models, on the other hand, can generate answers that are not supported by the available documents.

KNOWX brings retrieval and generation together.

It searches documents using two complementary approaches, combines their results, reranks the most relevant passages and passes the selected context to a local Llama 3 model.

**The goal is simple: make enterprise knowledge easier to search, understand and explore.**

## 🚀 Key Features

- **Natural Language Question Answering:** Ask questions in everyday language.
- **Semantic Search:** Find passages based on meaning using sentence embeddings.
- **Keyword Search:** Retrieve information using BM25.
- **Hybrid Retrieval:** Combine semantic and keyword search results.
- **Reciprocal Rank Fusion:** Merge ranked results from multiple retrieval methods.
- **Cross-Encoder Reranking:** Reorder candidate passages based on query relevance.
- **Local AI Inference:** Generate answers using Llama 3 8B Instruct through llama-cpp-python.
- **Source Information:** Return supporting document information with generated answers.
- **Web Interface:** Interact with the system through a browser-based interface.

## 🧠 How KNOWX Works

KNOWX follows a two-stage architecture.

### 1. Offline Stage – Building the Knowledge Base

Before users can ask questions, enterprise documents are processed and indexed.

1. **Document Collection:** Collect documents from Microsoft, NVIDIA and Infosys.
2. **Document Ingestion:** Extract text and divide documents into smaller chunks.
3. **Semantic Indexing:** Generate 384-dimensional embeddings using all-MiniLM-L6-v2 and store them in FAISS.
4. **BM25 Indexing:** Build a keyword-based index over the same text chunks.

### 2. Online Stage – Answering Questions

When a user submits a question:

1. **User Query:** Receive a natural-language question.
2. **Hybrid Retrieval:** Search using FAISS and BM25.
3. **Reciprocal Rank Fusion:** Combine the two ranked result lists.
4. **Cross-Encoder Reranking:** Rerank the retrieved passages.
5. **Context Building:** Select relevant passages and retain their source information.
6. **Answer Generation:** Pass the retrieved context to Llama 3 8B Instruct.
7. **Source Display:** Return the generated answer with supporting source information.

```text
Enterprise Documents
        |
        v
  Document Ingestion
        |
        v
   Text Chunking
        |
        +----------------------+
        |                      |
        v                      v
  Semantic Embeddings       BM25 Index
        |                      |
      FAISS                    |
        |                      |
        +----------+-----------+
                   |
                   v
          Reciprocal Rank Fusion
                   |
                   v
          Cross-Encoder Reranking
                   |
                   v
             Context Building
                   |
                   v
          Llama 3 8B Instruct
                   |
                   v
          Answer + Sources
```

## 📊 KNOWX by the Numbers

The current knowledge base was built using enterprise documents from three organisations.

| Project detail | Result |
|---|---:|
| Organisations | 3 |
| Documents processed | 9 |
| Extracted characters | 5,000,829 |
| Total text chunks | 6,256 |
| Embedding dimensions | 384 |
| Embedding model | all-MiniLM-L6-v2 |
| Language model | Llama 3 8B Instruct |
| Model quantisation | Q4_K_M |

### Dataset Breakdown

| Organisation | Number of chunks |
|---|---:|
| Microsoft | 893 |
| NVIDIA | 2,453 |
| Infosys | 2,910 |
| **Total** | **6,256** |

## 📈 Preliminary Retrieval Evaluation

We conducted an automated retrieval benchmark using 30 questions across Microsoft, NVIDIA and Infosys.

Four retrieval approaches were compared using Precision@5, Recall@5, F1-score@5, Mean Reciprocal Rank (MRR) and Normalized Discounted Cumulative Gain (nDCG@5).

| Metric | FAISS | BM25 | Hybrid RRF | Cross-Encoder |
|---|---:|---:|---:|---:|
| Precision@5 | 0.1333 | 0.1933 | 0.1800 | **0.2000** |
| Recall@5 | 0.6667 | 0.9667 | 0.9000 | **1.0000** |
| F1-score@5 | 0.2222 | 0.3222 | 0.3000 | **0.3333** |
| MRR | 0.4522 | 0.7500 | 0.6483 | **0.9833** |
| nDCG@5 | 0.5053 | 0.8049 | 0.7122 | **0.9877** |

**What do these results tell us?**

In this preliminary benchmark, Cross-Encoder reranking achieved the highest scores across all five retrieval metrics. BM25 also performed strongly, particularly in recall.

*Evaluation note:* These are preliminary known-source retrieval results based on automatically generated questions and one known relevant source chunk per question. They are not independently validated answer-accuracy measurements. Human-labelled relevance evaluation and answer-generation quality evaluation remain future work.

## 🛠️ Technology Stack

| Component | Technology |
|---|---|
| Programming language | Python |
| Embedding model | all-MiniLM-L6-v2 |
| Semantic search | FAISS |
| Keyword retrieval | BM25 |
| Rank fusion | Reciprocal Rank Fusion |
| Reranking | Cross-Encoder |
| Language model | Meta Llama 3 8B Instruct |
| Model format | GGUF, Q4_K_M |
| Local inference | llama-cpp-python |
| Backend | FastAPI |
| Frontend | HTML, CSS, JavaScript |
| API server | Uvicorn |

## 💻 Running KNOWX

### Prerequisites

- Python environment configured for the project.
- Project dependencies installed.
- FAISS and BM25 indexes available.
- Quantised Llama 3 GGUF model file available at the configured model path.

### Clone the repository

```bash
git clone https://github.com/Manaswini-118/KLH-AIDS-2026-27-2420080029-Knowx.git

cd KLH-AIDS-2026-27-2420080029-Knowx
```

### Activate the project environment

On Windows PowerShell:

```powershell
.\hf-venv\Scripts\Activate.ps1
```

If you have not created the environment on your machine, configure it using your project's Python version and install the required dependencies first.

### Start the application

```bash
python -m uvicorn app.app:app --reload
```

Open your browser and visit:

```text
http://127.0.0.1:8000
```

*Note: The model file and required index files must be available locally. The virtual environment itself is not included in the repository.*

## 🖥️ Development Environment

KNOWX was developed and tested on:

- Operating System: Windows 11
- Processor: AMD Ryzen 5 8640HS
- RAM: 16 GB
- Storage: SSD
- Graphics: Integrated AMD Radeon Graphics
- Model execution: CPU only
- CPU threads: 6
- Context window: 4096 tokens

The quantised GGUF model is executed locally using llama-cpp-python, without NVIDIA CUDA GPU acceleration.

## 📁 Project Structure

```text
KNOWX/
│
├── app/
│   ├── app.py
│   └── static/
│       ├── app.js
│       ├── index.html
│       └── style.css
│
├── data/
│   ├── raw/
│   └── processed/
│
├── models/
│   ├── semantic_index.faiss
│   ├── semantic_metadata.json
│   ├── bm25_index.pkl
│   └── llama3-8b-q4/
│
├── src/
│   ├── pipeline.py
│   ├── hybrid_search.py
│   └── reranker.py
│
├── evaluation_results/
│
└── README.md
```

*This is a simplified overview. Some generated files and local model files may not be included in the GitHub repository.*

## 🔮 Future Improvements

KNOWX has a working retrieval and answer-generation pipeline, but there is room to improve it further.

Planned improvements include:

- Building a human-labelled evaluation dataset.
- Evaluating answer quality and source relevance.
- Comparing different retrieval configurations and local language models.
- Measuring response time and memory consumption.
- Expanding the enterprise document collection.
- Adding document highlighting and more detailed source attribution.
- Improving document access controls and deployment readiness.

## 👥 Project Team

**Team KNOWX**

- B. Manaswini
- G. Babitha
- G. Rohan
- P. Adithya Ram 

**Project Guide:** Dr. K. Swanthana

**Institution:** K L (Deemed to be) University

**Academic Year:** 2026–2027

## 🎓 About the Project

KNOWX was developed as a B.Tech specialization mini-project in Artificial Intelligence and Data Science.

The project gave us practical experience in document processing, NLP, information retrieval, vector search, RAG architectures, local LLM inference and full-stack integration.

---



If you find the project interesting, feel free to explore the implementation and its workflow.

**KNOWX – Making enterprise knowledge easier to find, understand and use.**
