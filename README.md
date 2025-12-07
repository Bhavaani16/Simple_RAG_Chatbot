Here is a comprehensive `README.md` file tailored for your GitHub repository. It synthesizes the technical setup instructions, the project report details, and the evaluation metrics into a professional format.

-----

# 🌌 Big Bang Cosmology RAG Agent

A Retrieval-Augmented Generation (RAG) system capable of answering complex questions about the Big Bang theory, cosmological history, and scientific evidence. This project utilizes a local LLM running via Docker and includes both a Jupyter-based chat interface and a web-based evaluation agent.

## 📋 Project Overview

This project implements a local RAG pipeline designed to ingest, chunk, and retrieve information from a specific corpus of scientific documents related to Big Bang Cosmology. It prevents hallucinations by grounding answers in retrieved context and handles out-of-scope queries gracefully.

**Key Features:**

  * **Local Inference:** Runs entirely offline using Dockerized Ollama (Mistral 7B).
  * **Dual Interfaces:**
      * 🐍 **Jupyter Notebook:** Interactive chat with history.
      * 🌐 **Web UI:** An `index.html` evaluation agent for testing specific queries.
  * **Optimized Retrieval:** Uses a specific chunking strategy (500 chars) to maximize context relevance.

-----

## 🏗️ Architecture & Methodology

### Data Corpus

The knowledge base consists of six text files covering:

  * Core definitions and History (Friedmann, Hubble, Lemaître).
  * The "Four Pillars" of evidence (Hubble's Law, CMB, etc.).
  * Timeline of the Universe (Planck epoch to present).
  * Unresolved problems (Dark matter, Horizon problem).

### [cite_start]RAG Pipeline Configuration [cite: 10, 11, 12, 13]

  * **Chunking Strategy:** Fixed-size overlapping chunks.
  * **Chunk Size:** 500 characters.
  * **Overlap:** 50 characters.
  * **Rationale:** This overlap ensures that sentences split across chunks retain context, improving vector retrieval accuracy.

-----

## 🚀 Installation & Setup

### Prerequisites

  * [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running.
  * Python 3.x.

### 1\. Build the Docker Image

The project uses a custom Dockerfile to pre-load the Ollama model.

```bash
# Build the image containing Mistral 7B
docker build -f dockerfile -t ollama-mistral-img .
```

### 2\. Run the Container

Start the container. Use the GPU command if you have NVIDIA hardware for faster inference.

**CPU Only:**

```bash
docker run -d --rm --name ollama-mistral-offline -p 127.0.0.1:11434:11434 ollama-mistral-img
```

**With GPU Support:**

```bash
docker run -d --rm --gpus all --name ollama-mistral-offline -p 127.0.0.1:11434:11434 ollama-mistral-img
```

### 3\. Verify Connection

Ensure the API is accessible:

```bash
curl http://127.0.0.1:11434/api/tags
```

-----

## 💻 Usage

### Option A: Jupyter Chat Interface

1.  Open `chat_interface.ipynb` in VS Code or Jupyter Lab.
2.  Install dependencies if needed: `pip install ipywidgets requests`.
3.  Run all cells to initialize the UI.
4.  Chat with the model directly in the notebook.

### Option B: Web Evaluation Agent

1.  Open `index.html` in any modern web browser.
2.  Enter a query (e.g., *"What are the four pillars of evidence?"*).
3.  View the retrieved context chunks, inference time, and the generated answer.

### Option C: Low Resource Mode (Gemma 1B)

[cite_start]If your hardware struggles with Mistral 7B, you can use the smaller Gemma model[cite: 3].

1.  Build the small model image:
    ```bash
    docker build -f optional_small_model_dockerfile -t ollama-gemma-img .
    ```
2.  Run the container:
    ```bash
    docker run -d --name ollama-gemma-offline -p 127.0.0.1:11434:11434 ollama-gemma-img
    ```
3.  Update `MODEL_NAME` in the Python scripts to `"gemma3:1b-it-qat"`.

-----

## 📊 Evaluation Results

The system was evaluated against a test suite of factual, conceptual, and out-of-scope questions.

| Metric | Result | Description |
| :--- | :--- | :--- |
| **Hit Rate** | **85.7%** | [cite_start]6/7 queries successfully retrieved relevant context[cite: 21]. |
| **Avg Latency** | **0.23s** | [cite_start]Average time for the pipeline to process[cite: 21]. |
| **Source Usage** | **3 Files** | Primary sources: *Big Bang.txt, Evidence-Big Bang.txt, Timeline-Big Bang.txt*. |

### Example Success

  * **Query:** "What are the four main pillars of evidence supporting the Big Bang theory?"
  * [cite_start]**Result:** Correctly identified Expansion, CMB, Light Element Abundances, and Structure Formation[cite: 24].

### Guardrails (Out-of-Scope)

  * **Query:** "What happens if you cheat?"
  * [cite_start]**Result:** The system correctly identified that no context existed and refused to answer, preventing hallucination[cite: 25, 26].

-----

## 📂 Project Structure

```text
├── 📓 chat_interface.ipynb           # Interactive Python Chat UI
├── 🐳 dockerfile                     # Main Docker build (Mistral 7B)
├── 🐳 optional_small_model_dockerfile # Low-resource Docker build (Gemma 1B)
├── 📄 docker_commands.md             # Cheat sheet for Docker operations
├── 🌐 index.html                     # Web-based Evaluation UI
├── 📊 evaluation_results.json        # Raw metrics data
├── 📝 RAG Report.docx                # Detailed project documentation
└── 📂 data/                          # Source text files (Big Bang corpus)
```

-----

## 👥 Credits

  * **Dataset:** Big Bang Cosmology Text Corpus
  * **Tools:** Ollama, Docker, Python

-----

*Created for the RAG Implementation Project.*
