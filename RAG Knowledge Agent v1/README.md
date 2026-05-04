# RAG Knowledge Agent v1

A Retrieval-Augmented Generation (RAG) agent that ingests documents from a designated Google Drive folder, indexes them into Pinecone, and serves concise, source-backed answers via a public chat webhook. This instance is tuned to answer questions about Apple quarterly reports stored in the indexed namespace.

---

## 📸 App Screen

![App Screen](Screenshots/app%20screen.png)

---

## 🚀 Features

- Automatic ingestion of files uploaded to a monitored Google Drive folder.
- Text extraction and metadata tagging (filename -> apple-filename).
- Chunking and embeddings via Cohere (embed-english-light-v2.0).
- Vector storage in Pinecone index `rag-chatbot` under namespace `apple-info`.
- Public chat webhook that routes queries to an LLM agent which uses the vector store tool for sourced answers.
- Session memory (short window) to keep conversational context.

Key nodes (from Workflow.json):
- Google Drive Trigger: watches folder "Apple Info"
- Download file: downloads new files
- Default Data Loader: extracts text and sets metadata
- Embeddings Cohere: generates embeddings
- Pinecone Vector Store: inserts vectors into index `rag-chatbot` (namespace `apple-info`)
- When chat message received: public webhook
- AI Agent: system-prompted agent configured to answer from indexed docs
- Answer questions with a vector store: vector retrieval tool
- Google Gemini Chat Model: model for agent responses

---

## 🧩 Workflow Overview

1. Google Drive Trigger watches the specific folder (folder ID: 1r6gnOj2YRMphjG3sDOtW8XdMJArfjHh3) for new files.
2. New files are downloaded by the Download file node.
3. Default Data Loader extracts text, attaches metadata (apple-filename = original filename), and emits document chunks.
4. Embeddings Cohere converts chunks to embeddings (model: embed-english-light-v2.0).
5. Pinecone Vector Store inserts embeddings into index `rag-chatbot` under namespace `apple-info`.
6. Users send queries to the public chat webhook (When chat message received).
7. AI Agent uses the "Answer questions with a vector store" tool (topK=20) plus Google Gemini for concise, sourced answers. Agent system message restricts answers to the indexed documents and requests "insufficient data" when appropriate.

---

## 🧠 AI Behavior & Prompting

- System message: agent specialized to analyze Apple quarterly financial reports; must answer only using retrieved document content; be concise; include source / quarter when possible; return "insufficient data" if request cannot be supported by indexed docs.
- Models used:
  - Google Gemini Chat Model (models/gemini-2.0-flash-001) — for generation
  - Cohere embeddings (embed-english-light-v2.0) — for vectorization

---

## 🛠️ Tech Stack

- n8n – orchestration
- Google Drive – document source (folder: Apple Info)
- Cohere – embeddings
- Pinecone – vector database (index: rag-chatbot)
- Google Gemini (PaLM) – LLM for agent responses

---

## 📋 Data Schema & Naming

- Pinecone index: rag-chatbot
- Namespace: apple-info
- Document metadata:
  - apple-filename: original Drive filename
- Retrieval: topK set to 20 in the vector store tool

---

## 📂 Project Structure

```
RAG Knowledge Agent v1/
│
├─ Screenshots/
│  └─ app screen.png
│
├─ Workflow.json
└─ README.md
```

---

## ⚙️ Setup Instructions

1. Import Workflow.json into n8n.
2. Configure credentials:
   - Google Drive OAuth2 (Drive Trigger, Download file).
   - Cohere API key (Embeddings Cohere).
   - Pinecone API key and ensure index `rag-chatbot` exists.
   - Google PaLM / Gemini API (Google Gemini Chat Model).
3. Verify the Google Drive folder ID in the Google Drive Trigger points to your "Apple Info" folder (or change to your folder).
4. Confirm Pinecone namespace configuration (current namespace: `apple-info`) or set the namespace per-file as desired.
5. Test ingestion: upload a sample PDF/Doc to the Drive folder and observe workflow run and Pinecone insertion.
6. Test chat: send a request to the public webhook (When chat message received) such as "What were Apple's Q1 revenues?" and validate the agent returns sourced data or "insufficient data".

---

## ✅ Quick Troubleshooting

- No vectors in Pinecone:
  - Confirm download node successfully retrieves file content.
  - Check Default Data Loader is producing ai_document output.
  - Ensure Embeddings Cohere node has valid API key and returns embeddings.
  - Verify Pinecone index exists and credentials are correct.
- Agent returns hallucinated content:
  - Check agent system prompt to enforce use of retrieved documents only.
  - Validate the vector tool is attached to the agent and that retrievals are returned to the model (topK may be adjusted).
- Wrong namespace or empty results:
  - Confirm namespace `apple-info` used on insertion and query sides match.
  - Inspect metadata (apple-filename) to ensure correct file-based segmentation.
- Failure to trigger on Drive upload:
  - Re-check Drive trigger folder ID and OAuth scopes (drive.readonly / drive.file as needed).

---

## 👤 Author

Mahmoud Nasser — Automation | RAG Systems | n8n

---

References
- Workflow: [RAG Knowledge Agent v1/Workflow.json](Workflow.json)