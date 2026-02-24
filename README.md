# 🧠 MCP RAG System using n8n + Qdrant + Groq

This repository contains two n8n workflows that together implement a **Retrieval-Augmented Generation (RAG)** system based on the **Model Context Protocol (MCP)** architecture:

* ✅ **MCP Server** (Handles ingestion + retrieval)
* ✅ **MCP Client** (Handles chat + AI orchestration)

The system enables:

* 📄 PDF document ingestion
* 🧬 Embedding generation using HuggingFace
* 🗂️ Vector storage and retrieval using Qdrant
* 🤖 AI-powered chat responses via Groq
* 🌐 Real-time web search using Tavily

---

# 📌 Architecture Overview

```
User Uploads PDF → Text Processing → Embeddings → Qdrant Vector DB
                                                      ↑
User Chat → AI Agent → Retrieval + Web Search → Response Generation
```

The system is split into two logical components:

| Component  | Responsibility                                  |
| ---------- | ----------------------------------------------- |
| MCP Server | Document ingestion + Vector database management |
| MCP Client | AI chat interface + tool orchestration          |

---

# 🚀 Workflow 1: MCP Server (Active)

This workflow acts as the **RAG ingestion server**.

## 🔹 Key Components

### 1️⃣ MCP Server Trigger

* Entry point for external events
* Listens on a defined MCP endpoint
* Enables external systems to query the vector database

---

### 2️⃣ On Form Submission (PDF Upload)

* Allows users to upload PDF files
* Triggers ingestion pipeline
* Starts document processing flow

---

### 3️⃣ Default Data Loader

* Processes all binary input data
* Extracts text from uploaded PDFs
* Uses simple text splitting mode
* Automatically determines document loading logic

---

### 4️⃣ Embeddings (HuggingFace Inference)

Model used:

```
sentence-transformers/distilbert-base-nli-mean-tokens
```

* Converts document chunks into embeddings
* Generates vector representations
* Prepares data for storage in Qdrant

---

### 5️⃣ Qdrant Vector Store (Ingestion)

Collection:

```
MCP_RAG
```

* Stores embeddings in Qdrant
* Maintains metadata
* Ensures consistency with retrieval configuration

---

### 6️⃣ Qdrant Vector Store (Retrieval Mode)

Configuration:

* Mode: `retrieve-as-tool`
* Top K matches: `4`
* Returns document metadata
* Used during query time to fetch relevant context

---

### 📝 Sticky Notes

The workflow includes documentation inside n8n:

* **MCP Server Overview**
* **RAG Ingestion Pipeline**

These guide users on how ingestion and retrieval are structured.

---

# 🤖 Workflow 2: MCP Client (Inactive)

This workflow handles AI-driven conversations and tool orchestration.

## 🔹 Entry Point

### Trigger: When Chat Message Received

* Activates when a user sends a chat message
* Passes input to the AI Agent

---

## 🧠 AI Agent (Core Orchestrator)

The AI Agent:

* Processes user input
* Decides which tool to use
* Maintains conversation context
* Generates final responses

### Tools Connected to AI Agent

| Tool               | Purpose              |
| ------------------ | -------------------- |
| Groq Chat Model    | Response generation  |
| Simple Memory      | Context management   |
| MCP Client         | RAG database access  |
| Tavily Search Tool | Real-time web search |

---

## 🔹 Groq Chat Model

Model used:

```
openai/gpt-oss-120b
```

* Generates chat responses
* Uses retrieved context + web results
* Integrated through Groq

---

## 🔹 Simple Memory

* Context window length: `15`
* Maintains recent interaction history
* Enables multi-turn conversations

---

## 🔹 MCP Client Node

* Connects to MCP endpoint
* Queries MCP Server for vector retrieval
* Used for Model Context Protocol queries

---

## 🔹 Tavily Search Tool

* HTTP POST request-based tool
* Retrieves real-time web information
* Used when:

  * Question requires recent updates
  * External data is needed
  * RAG database lacks context

---

# 🔄 How the System Works

## 📄 Document Ingestion Flow

1. User uploads PDF
2. Text extracted via Default Data Loader
3. Text split into chunks
4. Embeddings generated (HuggingFace)
5. Stored in Qdrant (`MCP_RAG` collection)

---

## 💬 Chat Processing Flow

1. User sends message
2. AI Agent evaluates intent
3. If MCP-related → Query Qdrant via MCP Client
4. If real-time data needed → Use Tavily
5. Combine results
6. Generate response via Groq model
7. Maintain conversation context (Simple Memory)

---

# 🏗️ Tech Stack

* **n8n** – Workflow orchestration
* **Qdrant** – Vector database
* **HuggingFace** – Embedding generation
* **Groq** – LLM inference
* **Tavily API** – Web search
* **Model Context Protocol (MCP)** – Retrieval framework

---

# 📂 Collection Details

**Vector Collection Name:**

```
MCP_RAG
```

**Retrieval Configuration:**

* Top K: 4
* Includes metadata
* Tool-based retrieval mode

---

# 🎯 Key Features

✅ PDF-based RAG ingestion
✅ Embedding generation via HuggingFace
✅ Vector storage in Qdrant
✅ AI tool orchestration
✅ Real-time web search
✅ Context-aware multi-turn chat
✅ Modular MCP-based design

---

# 🔮 Future Improvements

* Enable MCP Client workflow
* Add authentication layer
* Improve chunking strategy (semantic splitting)
* Add monitoring & logging
* Support additional file formats
* Add streaming responses

---

# 📖 Summary

This project demonstrates a complete **end-to-end RAG system built in n8n**, combining:

* Document ingestion
* Vector storage
* AI agent orchestration
* Real-time web augmentation

It serves as a modular foundation for building:

* AI knowledge assistants
* MCP-compliant RAG systems
* Enterprise document search platforms
* Context-aware chatbots

---

If you found this useful, feel free to ⭐ the repository.
