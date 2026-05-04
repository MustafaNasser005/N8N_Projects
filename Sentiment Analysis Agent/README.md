# Sentiment Analysis Agent

A lightweight, end-to-end sentiment analysis workflow built with n8n, Airtable, an open-source LLM (via OpenRouter), and Gmail. Users submit a sentence via a public form; the system stores the submission in Airtable, calls an LLM to predict sentiment (Positive / Negative / Neutral) with a confidence score (0–1), updates the Airtable record, and emails the user the result.

---

## 📸 App Screen

See flow layout inside the workflow JSON: Sentiment Analysis 
![App Screen](Screenshots/app%20screen.png)

---

## 🚀 Features

- Public form for sentence submission (no auth).
- Automatic record creation in Airtable.
- LLM-based sentiment prediction returning strict JSON: { "sentiment", "confidence_score" }.
- Airtable record update with prediction and score.
- Email notification sent to the submitter with the result.
- Uses an open-source friendly model via OpenRouter for cost-effective inference.

Referenced workflow nodes:
- Trigger: `On form submission`
- Storage: `Create a record`, `Update record` (Airtable)
- LLM: `Basic LLM Chain`, `OpenRouter Chat Model`
- Email: `Send a message`
- Merge: `Merge` node to combine LLM output with Airtable data

---

## 🧩 Workflow Overview

1. User submits the "Sentiment Analysis Form" (`On form submission`).
2. n8n creates an Airtable record (`Create a record`) with Username, Email, and Sentence.
3. The LLM chain (`Basic LLM Chain`) is invoked with the sentence; the prompt instructs the model to return ONLY valid JSON with "sentiment" and "confidence_score".
4. LLM output is merged back with the Airtable record (`Merge`), then the record is updated (`Update record`) to store prediction and score.
5. A notification email is sent to the user (`Send a message`) with the sentiment and confidence.

---

## 🧠 AI Behavior & Prompting

- Model role: Sentiment analysis assistant.
- Required output: Strict JSON only, with keys:
  - sentiment: "Positive" | "Negative" | "Neutral"
  - confidence_score: float between 0 and 1
- Prompt enforces no extra text or markdown so n8n can parse the JSON reliably.
- Model used in the workflow: openai/gpt-oss-20b:free (via OpenRouter Chat Model). Swap model via the `OpenRouter Chat Model` node if needed.

---

## 🛠️ Tech Stack

- n8n – workflow orchestration
- Airtable – submissions storage (base: N8N Data; table: Sentiment Analysis Records)
- OpenRouter – access to open-source LLM (open-source model used: openai/gpt-oss-20b:free)
- Gmail – send result emails

---

## 📋 Airtable Schema (used in workflow)

- Username — Text
- Email — Email
- Sentence — Long text
- Sentiment Prediction — Single select (Positive / Negative / Neutral)
- Confidence Score — Number (0–1)
- Date Submitted — DateTime (optional)

The workflow maps form fields into these columns and updates Sentiment Prediction and Confidence Score after LLM inference.

---

## 📂 Project Structure

```
Sentiment Analysis Agent/
│
├─ Screenshots/
│  └─ app screen.png
│
├─ workflow.json
└─ README.md
```

---

## ⚙️ Setup Instructions

1. Import Free Sentiment Analysis Agent/Workflow.json into your n8n instance.
2. Configure credentials:
   - Airtable Personal Access Token (set in `Create a record` and `Update record` nodes).
   - OpenRouter API key (set in `OpenRouter Chat Model` node).
   - Gmail OAuth2 (set in `Send a message` node).
3. Verify Airtable base & table IDs in the Airtable nodes match your workspace (the workflow uses a configured base/table).
4. Optionally change the LLM model in `OpenRouter Chat Model` if you need a different size or provider.
5. Activate the form trigger (`On form submission`) and test by submitting a sentence.
6. Confirm Airtable record is created, updated with sentiment and confidence, and that the email is delivered.

---

## ✅ Quick Troubleshooting

- LLM returns non-JSON or extra text:
  - Tighten the prompt in `Basic LLM Chain` to reiterate "Return ONLY valid JSON".
  - Use the chain's output parser or simple JSON-cleaning steps before the `Update record` node.
- No Airtable write:
  - Check Airtable token permissions and base/table IDs.
- Email not sent:
  - Re-authorize Gmail OAuth2 credentials and verify sender limits.
- Low or inconsistent confidence:
  - Try a different model or add few-shot examples in the prompt to improve calibration.

---

## 🔒 Notes & Best Practices

- Enforce strict JSON output for reliability in automation.
- Monitor model costs and latency; prefer smaller/cheaper models for high-volume usage.
- Sanitize user input if exposing the workflow publicly.
- Use Airtable record IDs to correlate LLM output back to the correct submission.

---

## 👤 Author

Mahmoud Nasser — Automation | n8n | LLM Integrations

--- 

References
- Workflow: [Sentiment Analysis Agent/Workflow.json](Workflow.json)