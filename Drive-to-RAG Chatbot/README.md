# Drive-to-RAG Chatbot

A production-ready Retrieval-Augmented Generation (RAG) chatbot that ingests documents from Google Drive, indexes them into Pinecone, and answers user queries using a vector-backed LLM agent.

---

## 📸 App Screen

See workflow layout and notes inside the workflow file: 
![App Screen](Screenshots/app%20screen.png)

---

## 🚀 Features

- Automated ingestion of Google Drive documents into a vector DB (Pinecone).
- Duplicate detection using Google Sheets tracking.
- Document chunking and Cohere embeddings.
- Namespace isolation per file (filename-based).
- Chat interface that routes queries to the correct namespace based on detected quarter (Q1–Q4).
- LLM-backed agent that uses vector retrieval for accurate, sourced answers.

Referenced workflow nodes:
- Trigger: [`When chat message received`](Drive-to-RAG Chatbot/Workflow.json)
- Agent: [`AI Agent`](Drive-to-RAG Chatbot/Workflow.json)
- Vector tool: [`Answer questions with a vector store`](Drive-to-RAG Chatbot/Workflow.json)
- Ingest nodes: [`Search files and folders`](Drive-to-RAG Chatbot/Workflow.json), [`Download file1`](Drive-to-RAG Chatbot/Workflow.json), [`Pinecone Vector Store`](Drive-to-RAG Chatbot/Workflow.json)

---

## 🧩 Workflow Overview

The automation splits into two main pipelines:

1. Preprocessing & Ingestion (Knowledge Base)
   - Discover files in Google Drive via [`Search files and folders`](Drive-to-RAG Chatbot/Workflow.json).
   - Filter already-processed files using the sheet record comparison (Google Sheets referenced in the workflow).
   - Download new files with [`Download file1`](Drive-to-RAG Chatbot/Workflow.json).
   - Extract text via [`Default Data Loader`](Drive-to-RAG Chatbot/Workflow.json) and chunk using [`Recursive Character Text Splitter`](Drive-to-RAG Chatbot/Workflow.json).
   - Generate embeddings with [`Embeddings Cohere`](Drive-to-RAG Chatbot/Workflow.json) and insert into Pinecone via [`Pinecone Vector Store`](Drive-to-RAG Chatbot/Workflow.json). Each file uses namespace = filename.

2. Chat / RAG Query Flow
   - Public chat webhook [`When chat message received`](Drive-to-RAG Chatbot/Workflow.json) receives user queries.
   - Field editing node [`Edit Fields2`](Drive-to-RAG Chatbot/Workflow.json) extracts quarter reference (e.g., Q1/Q2) and sets the Pinecone namespace.
   - The agent [`AI Agent`](Drive-to-RAG Chatbot/Workflow.json) uses the vector store tool [`Answer questions with a vector store`](Drive-to-RAG Chatbot/Workflow.json) and the chosen LLM to return concise, sourced answers.

---

## 🧠 AI Behavior

System prompt (from the workflow) instructs the agent to:
- Answer only using documents available in the vector DB.
- Keep responses concise and factual.
- Indicate which quarter/report the answer is sourced from.
- Return "insufficient data" if the requested info isn't present.

This behavior is implemented in the [`AI Agent`](Drive-to-RAG Chatbot/Workflow.json) node system message.

---

## 🛠️ Tech Stack

- n8n – workflow orchestration
- Google Drive – document source (`Apple Info` folder referenced in the workflow)
- Google Sheets – processed-file tracking (sheet ID used in workflow)
- Cohere Embeddings – embed-english-v3.0 / embed-english-light-v2.0
- Pinecone – vector store (index: `rag-chatbot`)
- Groq / other LLMs – language model for responses (configured nodes: [`Groq Chat Model`](Drive-to-RAG Chatbot/Workflow.json), [`Groq Chat Model1`](Drive-to-RAG Chatbot/Workflow.json))
- Optional: manual trigger & scheduled trigger for periodic ingestion

---

## 📋 Data Schema & Naming

- Pinecone index: `rag-chatbot` (configured in [`Pinecone Vector Store`](Drive-to-RAG Chatbot/Workflow.json))
- Namespace pattern: `{File_Name}` (e.g., `Q1.pdf`) — set in the workflow nodes that insert embeddings.
- Google Drive folder: referenced folder ID in the workflow (Apple Info).
- Google Sheets tracking table columns: `ID` (Drive file id), `Name` (filename).

---

## 📂 Project Structure

```
Drive-to-RAG Chatbot/
│
├─ Screenshots/
│  └─ app screen.png
│
├─ workflow.json
└─ README.md
```

---

## ⚙️ Setup Instructions

1. Import [Drive-to-RAG Chatbot/Workflow.json](Workflow.json) into n8n.
2. Configure credentials used in the workflow:
   - Google Drive OAuth2 (`Search files and folders`, `Download file`, `Download file1`).
   - Google Sheets OAuth2 (sheet used to track processed files).
   - Pinecone API (index `rag-chatbot`).
   - Cohere API (embeddings).
   - Groq / other LLM credentials if applicable.
3. Update these configuration values in the workflow nodes if necessary:
   - Google Drive folder ID (currently set to the workflow's `Apple Info` folder).
   - Google Sheet ID for processed-file tracking.
   - Pinecone index/namespace settings.
4. Test ingestion by adding a sample PDF to the Drive folder and running the manual trigger (`When clicking 'Execute workflow'`) or waiting for the scheduled run.
5. Activate the public chat webhook (`When chat message received`) and test queries such as "What were Apple's Q1 revenues?"

---

## 🔒 Notes & Best Practices

- Each file is isolated in its own Pinecone namespace — avoids cross-file contamination.
- The agent is prompt-engineered to avoid hallucination: answers must come from retrieved document chunks.
- Use the Google Sheets tracking to prevent duplicate processing.
- Keep embeddings model versions and Pinecone index settings consistent between upload and query paths.

---

## ✅ Quick Troubleshooting

- No results for query: confirm the corresponding file (e.g., Q1.pdf) exists and is indexed in Pinecone namespace `Q1.pdf`.
- Duplicate uploads: verify Google Sheets tracking table and the code node that filters processed IDs.
- Wrong quarter detected: check [`Edit Fields2`](Drive-to-RAG Chatbot/Workflow.json) regex for Q1–Q4 extraction.

---

## 👤 Author

Mahmoud Nasser — Automation | RAG Systems | n8n

---

References
- Workflow: [Drive-to-RAG Chatbot/Workflow.json](Workflow.json)
- Example README to mirror style: [AI Ticket Intake Automation/README.md](README.md)