# Configure Ollama LLM With StirlingPdfAiEngine
This guide will help you set up the Ollama LLM with the StirlingPdfAiEngine for processing PDF documents. Follow the steps below to ensure a smooth integration.

### Ollama LLM Setup
1. **Install Ollama LLM**: install the Ollama LLM on your system. You can find the installation instructions on the [Ollama official website](https://ollama.com/download).<br>
Linux
```bash
curl -fsSL https://ollama.com/install.sh | sh
```
2. **Verify Installation**: After installation, verify that Ollama LLM is correctly installed by running:
```bash
ollama --version
```
3. **Install model**: for lite and fast processing, install the
- `llama3.2` model — for AI operations such as document analysis, summarization, and chat.<br>
- `nomic-embed-text` — for generating embeddings, useful for semantic search and document retrieval (RAG).
```bash
ollama pull llama3.2
ollama pull nomic-embed-text
```
4. **Run Ollama LLM**: Start the Ollama LLM server to enable communication with the StirlingPdfAiEngine.
```bash
ollama run llama3.2
```
5. **Test the LLM**: You can test the LLM by sending a simple prompt to ensure it responds correctly.
```bash
ollama list
curl http://localhost:11434
# Ollama is running
```

### StirlingPdfAiEngine Setup
1. Open Stirling PDF in your browser and go to Settings > Server > AI Engine in connection settings set `run your own engine` keep the default settings. eg."AI engine URL:http://localhost:5001".<br>

**Models & Providers**:
- Model Provider: ollama
- Ollama Base URL: http://localhost:11434
- Fast Model Name: llama3.2
- Smart Model Name: llama3.2

**Documents & RAG**:
- Embedding provider: Ollama
- Embedding model: nomic-embed-text
- Embedding base URL: http://localhost:11434

2. **Save Settings**: After configuring the settings, click on "Save" to apply the changes.
3. **Install the AI Engine prerequisites**:
```bash
sudo apt update
sudo apt install -y git curl build-essential
```
install `uv`
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc
```
stirling pdf source code
```bash
git clone https://github.com/Stirling-Tools/Stirling-PDF.git
cd Stirling-PDF
```
Install the Task runner
```bash
curl -1sLf 'https://dl.cloudsmith.io/public/task/task/setup.deb.sh' | sudo -E bash
sudo apt install task

```
verifiy the task is installed
```bash
task --version
task --list
```
From the Stirling-PDF repository root, run:
```bash
task engine:install
task engine:prepare
```
then config `.env.local` in engine folder with the following content:
```bash
# ==========================================
# Ollama connection (OpenAI-compatible API)
# ==========================================
OPENAI_BASE_URL=http://localhost:11434/v1
OLLAMA_BASE_URL=http://localhost:11434
OPENAI_API_KEY=ollama

# ==========================================
# Chat models
# ==========================================
STIRLING_SMART_MODEL=openai-chat:llama3.2
STIRLING_FAST_MODEL=openai-chat:llama3.2

CHAT_PROVIDER=ollama

STIRLING_SMART_MODEL_MAX_TOKENS=8192
STIRLING_FAST_MODEL_MAX_TOKENS=2048

# ==========================================
# Embedding model
# ==========================================
STIRLING_RAG_EMBEDDING_MODEL=ollama:nomic-embed-text

# ==========================================
# Document storage
# ==========================================
STIRLING_DOCUMENTS_BACKEND=sqlite

# ==========================================
# Java integration
# ==========================================
STIRLING_ALLOW_CONFIG_PUSH=false
```
there was some problem i faced after going back and forth with ai got some solution to fix it, in source code
`/Stirling-PDF/engine/src/stirling/agents/pdf_questions.py`
`/Stirling-PDF/engine/src/stirling/documents/embedder.py`

4. **Run the AI Engine**: Start the StirlingPdfAiEngine to enable processing of PDF documents.
```bash
task engine:dev
```
5. **Test the Integration**: Open Stirling PDF settings test the connection. and check with ai agent. by selecting a PDF document and asking questions related to its content. The AI should respond accurately based on the document's information. 
