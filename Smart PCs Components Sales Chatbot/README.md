# Smart PCs Components Sales Chatbot

An AI-driven sales agent built with n8n that answers product questions, checks inventory/pricing from a Google Sheet, and generates purchase or cancellation emails when appropriate. The agent enforces strict data-sourcing rules (only use the Google Sheet) and supports Arabic (Egyptian) and English.

---

## 📸 App Screen

![App Screen](Screenshots/app%20screen.png)

---

## 🚀 Features

- Public chat webhook to interact with customers (multilingual: English / Egyptian Arabic)
- Agent reads product data (price, stock, specs, warranty) **only** from a Google Sheet
- Context-aware conversation memory (50-message window)
- Guided purchase flow with required validation: Full Name + Phone + exact product name
- Generates plain-text emails for purchases and cancellations to dealer Gmail
- Safety: explicit fallback phrase in Arabic when data is missing

**Key nodes in workflow:**

- When chat message received (webhook: `8b269778-256e-4347-8496-db5544b662a2`)
- AI Agent (system prompt enforces rules & templates)
- Groq Chat Model (LM)
- Simple Memory (contextWindowLength: 50)
- Get row(s) in Google Sheets (sheet id: `1AilZ8XCRNDD4SEQk6QzFTEzJrkvfRMBAyJHLuZUyYW8`, gid: `1475226774`)
- Send a message in Gmail (sends owner emails)

---

## 🧩 Workflow Overview

1. User sends a message to the public chat webhook.
2. AI Agent receives the message, uses memory and Google Sheets to lookup products.
3. Agent replies in the same language (Arabic → Egyptian Arabic, English → English).
4. If user decides to purchase:
   - Agent asks for Full Name + Phone Number.
   - Confirms exact product name (must match sheet).
   - After both are provided, agent generates the purchase email (plain text) and triggers the Gmail node.
5. If user cancels, agent confirms and generates/sends cancellation email.

**Agent constraints (enforced by system prompt):**

- Use only sheet data for price/availability/specs
- Never invent values
- If data is missing → reply exactly:  
  `"المعلومة دي مش موجودة في قاعدة البيانات حالياً."`
- Emails are generated only after required fields are provided

---

## 🧠 AI Behavior & Prompts

- System message embeds full rules: language handling, data-source constraints, recommendation rules, purchase/cancellation flow, and plain-text email templates
- Model node: `openai/gpt-oss-120b` (via Groq Chat Model node)
- Memory: Simple Memory node (`contextWindowLength = 50`) to preserve conversation context

**Email templates (plain text):**

- **Purchase Email Template:** fills `CUSTOMER_NAME`, `PHONE_NUMBER`, `EXACT_PRODUCT_NAME`, `PRICE_FROM_SHEET`
- **Cancellation Email Template:** fills `CUSTOMER_NAME`, `PHONE_NUMBER`, `PRODUCT_NAME`

---

## 🛠️ Tech Stack

- **n8n** – orchestration
- **Groq / OpenRouter** – LLM (Groq Chat Model node)
- **Google Sheets** – product database (`spreadsheet id: 1AilZ8XCRNDD4SEQk6QzFTEzJrkvfRMBAyJHLuZUyYW8`, `sheet gid: 1475226774`)
- **Gmail** – owner notifications (`mh.magdi.nasser@gmail.com`)

---

## 📋 Google Sheets Mapping (used in workflow)

- **Spreadsheet ID:** `1AilZ8XCRNDD4SEQk6QzFTEzJrkvfRMBAyJHLuZUyYW8`  
- **Sheet (gid):** `1475226774` — "PCs Components Details"  
  **Expected columns:** Product Name, Price, Stock, Warranty, Specs, Category, Brand

> Agent expects exact product names to match user confirmations.

---

## ⚙️ Setup Instructions

1. Import `Workflow.json` into n8n.
2. Configure credentials:
   - Groq / LM provider credentials in the Groq Chat Model node
   - Google Sheets OAuth2 in the Google Sheets node
   - Gmail OAuth2 in the Gmail node
3. Verify Google Sheets spreadsheet ID and sheet gid match your product sheet
4. Publish the chat webhook (`webhook id: 8b269778-256e-4347-8496-db5544b662a2`) and activate the workflow
5. Test scenarios:
   - Ask product availability/pricing (agent must use sheet values)
   - Attempt to buy; provide name + phone + exact product name to trigger email generation
   - Test cancellation flow and verify generated cancellation email

---

## ✅ Quick Troubleshooting

- **Agent invents data:** confirm Google Sheets tool returns correct rows; check systemMessage enforces constraints
- **Emails not sent:** re-authorize Gmail OAuth2; ensure emails trigger only after name and phone are provided
- **Wrong language reply:** confirm input detection; Arabic → Egyptian Arabic mapping
- **No sheet results:** check sheet permissions and IDs

---

## 📂 Project Structure

```
Smart PCs Components Sales Chatbot/
│
├─ Screenshots/
│  └─ app screen.png
│
├─ Workflow.json
└─ README.md
```

---

## 👤 Author

Mahmoud Nasser — Automation | n8n

--- 

References
- Workflow: [Smart PCs Components Sales Chatbot/Workflow.json](Workflow.json)