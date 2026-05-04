# Apple Financial RAG Agent

A sophisticated **Retrieval-Augmented Generation (RAG) system** built using **n8n** for analyzing Apple's quarterly financial reports. This workflow automates document processing, vector storage, and provides an intelligent chatbot interface for financial analysis.

---

## 📸 App Screen

![App Screen](Screenshots/app%20screen.png)

---

## 🚀 Features

* **Automated Document Processing**: Scans Google Drive for new Apple financial reports (Q1-Q4 PDFs)
* **Smart Duplicate Detection**: Compares with Google Sheets to avoid reprocessing existing files
* **Vector Database Integration**: Stores document embeddings in Pinecone with namespace isolation
* **RAG-Powered Chatbot**: Answers questions about Apple's financials using relevant document context
* **Scheduled & Manual Triggers**: Both scheduled updates and on-demand execution
* **Document Chunking & Embedding**: Uses Cohere embeddings and recursive text splitting

---

## 🧩 Workflow Overview

The automation is split into **three main sections**:

### 1️⃣ Preprocessing Pipeline
1. **Schedule Trigger** (hourly) or **Manual Trigger** initiates the workflow
2. **Google Drive** searches for files in the "Apple Info" folder
3. **Google Sheets** fetches already processed file records
4. **JavaScript Code Node** filters out previously processed files
5. **Google Sheets** appends new file records for tracking
6. **File Download** retrieves new PDFs from Google Drive

### 2️⃣ Vector Database Upload
1. **Loop Over Items** processes each new file
2. **Default Data Loader** extracts text from PDFs
3. **Recursive Text Splitter** divides documents into manageable chunks
4. **Cohere Embeddings** generates vector embeddings
5. **Pinecone Vector Store** stores embeddings with namespace based on filename

### 3️⃣ RAG Chatbot Agent
1. **Chat Trigger** receives user questions via webhook
2. **Field Editing** extracts quarter reference from queries (Q1, Q2, Q3, Q4)
3. **AI Agent** with specialized system prompt for financial analysis
4. **Vector Store Tool** retrieves relevant document chunks
5. **Groq Chat Model** (GPT-OSS-120B) generates accurate, context-aware responses

---

## 🧠 AI Behavior

The AI agent is configured to:

* Act as a specialized Apple financial analyst
* Use **only** information from the vector database (quarterly reports)
* Provide concise, precise answers with specific numbers and facts
* Indicate which quarter's report the information comes from
* State "insufficient data" when information is unavailable
* Avoid speculation or external knowledge

This ensures **factual accuracy** and **traceable sourcing** for all financial insights.

---

## 🛠️ Tech Stack

* **n8n** – Workflow automation platform
* **Google Drive API** – Document storage and retrieval
* **Google Sheets API** – Process tracking and deduplication
* **Pinecone** – Vector database for semantic search
* **Cohere Embeddings** – Text embedding generation
* **Groq (GPT-OSS-120B)** – High-speed LLM for responses
* **JavaScript** – Custom logic for file filtering

---

## 📋 Data Schema

### Google Sheets Tracking Table
| Column | Type | Description |
|--------|------|-------------|
| ID | Text | Google Drive File ID |
| Name | Text | Filename (e.g., Q1.pdf, Q2.pdf) |

### Pinecone Vector Store
* **Index**: `rag-chatbot`
* **Namespace**: `{filename}` (e.g., Q1.pdf, Q2.pdf)
* **Metadata**: Includes `Apple_Report` field with source filename
* **Embeddings**: Cohere embed-english-v3.0 (upload) and embed-english-light-v2.0 (query)

---

## 📂 Project Structure

```
Apple Financial RAG Agent/
│
├─ Screenshots/
│  └─ app screen.png
│
├─ workflow.json
└─ README.md
```

---

## ⚙️ Setup Instructions

1. Import the workflow JSON into **n8n**
2. Configure credentials:
   * **Google Drive OAuth2** – For accessing Apple financial reports
   * **Google Sheets OAuth2** – For tracking processed files
   * **Cohere API** – For generating embeddings
   * **Pinecone API** – For vector database operations
   * **Groq API** – For LLM responses
3. Update configuration:
   * Google Drive folder ID (currently: `1r6gnOj2YRMphjG3sDOtW8XdMJArfjHh3`)
   * Google Sheet ID (currently: `1AilZ8XCRNDD4SEQk6QzFTEzJrkvfRMBAyJHLuZUyYW8`)
   * Pinecone index name (currently: `rag-chatbot`)
4. Test with sample Apple quarterly reports in PDF format
5. Activate the webhook for chatbot interface

---

## 🔒 Notes

* **Data Isolation**: Each quarter's reports are stored in separate Pinecone namespaces
* **Avoids Reprocessing**: JavaScript logic prevents duplicate file processing
* **Context-Aware Responses**: AI only answers based on uploaded financial reports
* **Audit Trail**: Google Sheets provides clear tracking of processed documents
* **Secure Access**: All APIs use OAuth2 or secure API keys

---

## 📌 Use Cases

* **Financial Analysts**: Quick insights into Apple's quarterly performance
* **Investors**: Rapid analysis of earnings reports and financial metrics
* **Research Teams**: Comparative analysis across quarters
* **Corporate Strategy**: Historical performance tracking
* **AI/ML Demos**: Production-ready RAG implementation example

---

## 👤 Author

**Mahmoud Nasser**

Automation | AI Workflows | n8n | RAG Systems | Financial Analysis

---

## 🚀 Extension Opportunities

This foundation can be extended with:

* **Multi-Company Support**: Add other tech companies (Microsoft, Google, Amazon)
* **Comparative Analysis**: Cross-quarter and cross-year comparisons
* **Alert System**: Notify when specific metrics hit thresholds
* **Dashboard Integration**: Connect to data visualization tools
* **Multi-Format Support**: Process earnings call transcripts, SEC filings
* **Sentiment Analysis**: Add tone and sentiment scoring to responses

---

## 🔍 Sample Queries

* "What were Apple's Q1 2024 revenues?"
* "Compare iPhone sales between Q2 and Q3 2023"
* "What was the gross margin in Q4 2023?"
* "Show me the services growth in Q2 2024"
* "What risks were mentioned in the Q3 earnings report?"

The system will provide accurate, sourced answers from the relevant quarterly reports.