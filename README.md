# N8N Projects Portfolio

A comprehensive collection of production-ready n8n automation workflows and AI agent implementations. Each project demonstrates end-to-end automation patterns, LLM integration, vector databases, and multi-channel notifications.

---

## 📂 Projects Overview

### 1. **Drive-to-RAG Chatbot**
A production-ready Retrieval-Augmented Generation (RAG) chatbot that ingests documents from Google Drive, indexes them into Pinecone, and answers user queries using a vector-backed LLM agent.

![Drive-to-RAG Chatbot](Drive-to-RAG%20Chatbot/screenshots/app%20screen.png)

- **Tech Stack:** Google Drive, Cohere Embeddings, Pinecone, Groq LLM
- **Features:** Automated document ingestion, namespace isolation, quarter-based routing, LLM-backed agent
- **Use Case:** Document Q&A, financial report analysis
- **[→ Read Full README](Drive-to-RAG%20Chatbot/README.md)**

---

### 2. **Sentiment Analysis Agent**
A lightweight sentiment analysis workflow that accepts user submissions via public form, runs LLM inference on sentences, and returns sentiment predictions (Positive/Negative/Neutral) with confidence scores.

![Free Sentiment Analysis Agent](Sentiment%20Analysis%20Agent/Screenshots/app%20screen.png)

- **Tech Stack:** OpenRouter (open-source LLM), Airtable, Gmail, n8n
- **Features:** Public form submission, LLM-based sentiment classification, strict JSON output, email notifications
- **Use Case:** Content moderation, feedback analysis, sentiment tracking
- **[→ Read Full README](Free%20Sentiment%20Analysis%20Agent/README.md)**

---

### 3. **Hotel Reservation Automation**
A minimal webhook workflow that accepts hotel reservation requests via a public form and stores them in Airtable. Designed for quick demos or as a starter template for booking automations.

![Hotel Reservation Automation](Hotel%20Reservation%20Automation/Screenshots/app%20screen.png)

- **Tech Stack:** n8n Form Trigger, Airtable
- **Features:** Public form, automatic record creation, simple field mapping
- **Use Case:** Hotel bookings, event registration, demo templates
- **[→ Read Full README](Hotel%20Reservation%20Automation/README.md)**

---

### 4. **RAG Knowledge Agent v1**
A Retrieval-Augmented Generation agent that ingests documents from a Google Drive folder, indexes them into Pinecone, and serves concise, source-backed answers via a public chat webhook. Tuned for Apple quarterly reports.

![RAG Knowledge Agent v1](RAG%20Knowledge%20Agent%20v1/Screenshots/app%20screen.png)

- **Tech Stack:** Google Drive, Cohere Embeddings, Pinecone, Google Gemini
- **Features:** Automatic file ingestion, metadata tagging, vector retrieval (topK=20), session memory, sourced answers
- **Use Case:** Financial document Q&A, knowledge base chatbots, report analysis
- **[→ Read Full README](RAG%20Knowledge%20Agent%20v1/README.md)**

---

### 5. **Apple Financial RAG Agent**
An enterprise-grade RAG agent specialized for Apple financial document analysis. Ingests Apple earnings reports, 10-K filings, and quarterly presentations from Google Drive, embeds them with Cohere, stores in Pinecone with document-level metadata, and provides financial analysts with precise, source-cited answers via a secure chat interface.

![Apple Financial RAG Agent](Apple%20Financial%20RAG%20Agent/Screenshots/app%20screen.png)

- **Tech Stack:** Google Drive, Cohere Embeddings (embed-english-v3.0), Pinecone (index: apple-financial), Claude / Google Gemini LLM
- **Features:** Financial document ingestion, dual-namespace strategy (by-quarter + by-document-type), metadata tagging (filing type, fiscal year, quarter), advanced retrieval (topK=30 for multi-source answers), conversation memory with financial context preservation, source citations (page numbers, section references)
- **Use Case:** Financial analyst support, earnings call Q&A, regulatory filing research, investor relations automation
- **[→ Read Full README](Apple%20Financial%20RAG%20Agent/README.md)**

---

### 6. **AI Ticket Intake Automation**
An end-to-end AI-powered IT ticket intake and notification system. Users submit support requests via a public form; the system stores submissions in Airtable, uses Google Gemini to generate intelligent summaries and auto-categorize tickets by priority/component, and sends formatted notifications to both the user and IT management.

![AI Ticket Intake Automation](AI%20Ticket%20Intake%20Automation/Screenshots/app%20screen.png)

- **Tech Stack:** n8n, Airtable, Google Gemini 2.5 Flash, Gmail
- **Features:** Public IT request form (no auth), automatic ticket creation, AI-generated summaries, priority detection, structured JSON output from LLM, dual-email workflow (user confirmation + management alert)
- **Use Case:** IT helpdesk automation, ticket triage, SLA tracking, workflow automation demos
- **[→ Read Full README](AI%20Ticket%20Intake%20Automation/README.md)**

---

### 7. **Smart Car Dealership Automation**
A lightweight webhook workflow that records car sales and notifies the dealer. Incoming POST requests trigger email notifications and append rows to a Google Sheet for sales tracking.

![Smart Car Dealership Automation](Smart%20Car%20Dealership%20Automation/Screenshots/app%20screen.png)

- **Tech Stack:** n8n Webhook, Gmail, Google Sheets
- **Features:** Public webhook endpoint, email notifications, sales log in Google Sheets
- **Use Case:** POS system integration, sales notifications, lead tracking
- **[→ Read Full README](Smart%20Car%20Dealership%20Automation/README.md)**

---

### 8. **Smart PCs Components Sales Chatbot**
An AI-driven sales agent that answers product questions, checks inventory/pricing from a Google Sheet, and generates purchase or cancellation emails. Supports English and Egyptian Arabic with strict data-sourcing rules.

![Smart PCs Components Sales Chatbot](Smart%20PCs%20Components%20Sales%20Chatbot/Screenshots/app%20screen.png)

- **Tech Stack:** Groq LLM, Google Sheets, Gmail, n8n
- **Features:** Multilingual chat (EN/AR), inventory lookup, guided purchase flow, email generation, context memory (50-message window)
- **Use Case:** E-commerce sales automation, customer support, product inquiry handling
- **[→ Read Full README](Smart%20PCs%20Components%20Sales%20Chatbot/README.md)**

---

## 🛠️ Common Tech Stack

| Technology | Purpose | Used In |
|------------|---------|---------|
| **n8n** | Workflow orchestration | All projects |
| **Google Drive** | Document source | Drive-to-RAG, RAG Knowledge Agent v1 |
| **Google Sheets** | Data storage & lookup | Free Sentiment Analysis, Smart Car Dealership, Smart PCs Chatbot |
| **Airtable** | Structured database | Free Sentiment Analysis, Hotel Reservation |
| **Gmail** | Email notifications | All projects (except Hotel Reservation) |
| **Pinecone** | Vector database | Drive-to-RAG, RAG Knowledge Agent v1 |
| **Cohere** | Embeddings generation | Drive-to-RAG, RAG Knowledge Agent v1 |
| **Groq / OpenRouter** | LLM inference | Free Sentiment Analysis, Smart PCs Chatbot |
| **Google Gemini** | LLM inference | RAG Knowledge Agent v1 |

---

## 🚀 Quick Start

### Prerequisites
- n8n instance (cloud or self-hosted)
- API keys/OAuth credentials for services used in each workflow
- (Optional) Vector DB access (Pinecone) for RAG projects
- (Optional) Spreadsheet/Database for data storage

### Setup Steps (Generic)

1. **Clone or import workflows:**
   - Each project folder contains a `Workflow.json` file
   - Import into your n8n instance via UI or CLI

2. **Fix Git remote if needed:**
   - If `origin` already exists, update it instead of adding a new remote.
   - Run:
     ```powershell
     git remote set-url origin https://github.com/MustafaNasser005/N8N_Projects.git
     git push -u origin main
     ```
   - Use `git remote -v` to verify the remote URL.

3. **Configure credentials:**
   - Update OAuth2 / API keys in each workflow node
   - Verify data source IDs (Google Drive folders, Airtable bases, Pinecone indexes, etc.)

3. **Publish webhooks (if applicable):**
   - Activate the workflow
   - Test public endpoints (forms, chat, webhooks)

4. **Monitor & iterate:**
   - Check execution logs
   - Adjust prompts, field mappings, or LLM models as needed

---

## 📋 Architecture Patterns

### Pattern 1: Form → Database → Notification
Used in: **Hotel Reservation, Free Sentiment Analysis**
- Public form submission → data stored in Airtable/Sheets → email confirmation

### Pattern 2: Document Ingestion → Vector Store → RAG Agent
Used in: **Drive-to-RAG Chatbot, RAG Knowledge Agent v1**
- Monitor Google Drive → extract text → embed with Cohere → store in Pinecone → query via LLM agent

### Pattern 3: Webhook → Processing → Multi-channel Notification
Used in: **Smart Car Dealership**
- POST request → process payload → send email + append to sheet

### Pattern 4: Chat Agent with External Data Lookup
Used in: **Smart PCs Components Sales Chatbot**
- Chat webhook → LLM agent → Google Sheets lookup tool → decision logic → email generation

---

## 🧠 AI & LLM Integration

All AI-powered projects enforce **strict prompt engineering:**

- **Data sourcing:** Agents only use provided tools (vector store, sheets) — no hallucination
- **Output formatting:** Enforce JSON or structured text (no markdown unless needed)
- **Language handling:** Detect user language and respond accordingly
- **Fallback behavior:** Clear error messages when data is unavailable

Example system prompts focus on:
- Role definition (e.g., "You are a sales agent")
- Rules (e.g., "Use only sheet data for pricing")
- Templates (e.g., email body format)
- Safety (e.g., "Return 'insufficient data' if not found")

---

## 📊 Use Cases by Industry

### E-Commerce & Retail
- Smart PCs Components Sales Chatbot (inventory, sales automation)
- Hotel Reservation Automation (booking)

### Finance & Reports
- Drive-to-RAG Chatbot (quarterly report Q&A)
- RAG Knowledge Agent v1 (document analysis)

### Content & Sentiment
- Free Sentiment Analysis Agent (feedback, moderation)

### Automotive & Services
- Smart Car Dealership Automation (sales tracking, lead notifications)

---

## 🔒 Security & Best Practices

1. **Credentials:**
   - Use n8n's credential manager (never hardcode API keys)
   - Rotate tokens regularly
   - Scope OAuth permissions to minimum required

2. **Data Privacy:**
   - Sanitize user inputs before processing
   - Use Airtable/Google Sheets row-level access controls
   - Monitor PII handling in LLM prompts

3. **Reliability:**
   - Enable error workflows for failed executions
   - Log important decisions (purchases, submissions)
   - Set rate limits on public webhooks

4. **Prompt Safety:**
   - Enforce strict JSON validation before parsing LLM output
   - Avoid passing raw user input to LLM without sanitization
   - Test edge cases (empty data, special characters, non-English input)

---

## 📈 Scalability Tips

- **High-volume webhooks:** Add request queuing (n8n PRO feature)
- **Large documents:** Increase chunk size in text splitter; monitor embedding costs
- **Many concurrent chats:** Use connection pooling for database; monitor memory
- **Multiple namespaces:** Plan Pinecone index sharding; version embeddings models

---

## 🤝 Contributing & Customization

Each project is modular and designed for extension:

- **Add new LLM models:** Swap Groq, OpenRouter, or local models in agent nodes
- **Expand data sources:** Add new Google Sheets, Airtable tables, or document folders
- **Custom prompts:** Edit system messages in AI Agent nodes to change behavior
- **New integrations:** Connect to Slack, Microsoft Teams, Discord, etc., for multi-channel support

---

## 📚 Project Structure

```
N8N Projects/
│
├─ AI Ticket Intake Automation/
│  ├─ Workflow.json
│  ├─ README.md
│  └─ Screenshots/
│
├─ Apple Financial RAG Agent/
│  ├─ Workflow.json
│  ├─ README.md
│  └─ Screenshots/
│
├─ Drive-to-RAG Chatbot/
│  ├─ Workflow.json
│  ├─ README.md
│  └─ Screenshots/
│
├─ Sentiment Analysis Agent/
│  ├─ Workflow.json
│  ├─ README.md
│  └─ Screenshots/
│
├─ Hotel Reservation Automation/
│  ├─ Workflow.json
│  ├─ README.md
│  └─ Screenshots/
│
├─ RAG Knowledge Agent v1/
│  ├─ Workflow.json
│  ├─ README.md
│  └─ Screenshots/
│
├─ Smart Car Dealership Automation/
│  ├─ Workflow.json
│  ├─ README.md
│  └─ Screenshots/
│
├─ Smart PCs Components Sales Chatbot/
│  ├─ Workflow.json
│  ├─ README.md
│  └─ Screenshots/
│
└─ README.md (this file)
```

---

## 👤 Author

**Mahmoud Nasser**

Automation | AI Workflows | n8n | LLM Integration | RAG Systems

---

## 📝 License

These workflows and automations are provided as-is for educational and commercial use. Modify and extend as needed for your use cases.

---

## 🔗 Resources

- **n8n Documentation:** https://docs.n8n.io
- **n8n Community:** https://community.n8n.io
- **Pinecone Docs:** https://docs.pinecone.io
- **Cohere API:** https://docs.cohere.com
- **Google Cloud APIs:** https://cloud.google.com/docs
- **Groq API:** https://console.groq.com

---

## 📞 Support & Questions

For issues, questions, or feature requests on any project:
1. Check the individual project README for troubleshooting
2. Review n8n execution logs
3. Verify API credentials and data source IDs
4. Test with sample data before production deployment

---

**Last Updated:** December 13, 2025
