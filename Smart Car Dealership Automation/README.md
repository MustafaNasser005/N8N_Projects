# Smart Car Dealership Automation

A lightweight n8n webhook workflow that records car sales and notifies the dealer. Incoming POST requests to a public webhook trigger an email notification and append a row to a Google Sheet for sales tracking.

---

## 📸 App Screen

![App Screen](Screenshots/app%20screen.png)

---

## 🚀 Features

- Public webhook endpoint to receive sale events (JSON POST).
- Email notification sent to dealer on each sale (Gmail).
- Sale details appended to a Google Sheet (sales log).
- Minimal, easy-to-deploy workflow for integration with POS systems or webhooks.

Key nodes:
- Webhook: public POST endpoint
- Send a message: Gmail notification to dealer
- Append row in sheet: Google Sheets sales log

---

## 🧩 Workflow Overview

1. External system POSTS sale JSON to the webhook path (method: POST).
2. n8n forwards the payload to the Gmail node which sends an email to the configured dealer address.
3. n8n appends a new row to the designated Google Sheet with buyer, price, car type, and date.

### Expected POST Body

```json
{
  "buyer": "Jane Doe",
  "car": "Model S",
  "price": "75000"
}
```

### Example `curl` Command

```bash
curl -X POST "https://<n8n-host>/webhook/a52637bf-3991-4358-801b-6188ecf43aeb" \
  -H "Content-Type: application/json" \
  -d '{"buyer":"Jane Doe","car":"Model S","price":"75000"}'
```

---

## 📋 Node / Integration Details

### Webhook
- **HTTP Method:** POST  
- **Path:** `a52637bf-3991-4358-801b-6188ecf43aeb`

### Send a Message (Gmail)
- **To:** `mh.magdi.nasser@gmail.com`  
- **Subject:** `NEW Car Sale!`  
- **Body Template:** uses request body fields:  
  - Car: `{{ $json.body.car }}`  
  - Price: `{{ $json.body.price }}`  
  - Buyer: `{{ $json.body.buyer }}`

### Append Row in Sheet (Google Sheets)
- **Spreadsheet ID:** `1AilZ8XCRNDD4SEQk6QzFTEzJrkvfRMBAyJHLuZUyYW8`  
- **Sheet (gid):** `1092208228` (Car Sales)  
- **Columns Appended:**  
  - Name: `{{ $json.body.buyer }}`  
  - Price: `{{ $json.body.price }}`  
  - Type: `{{ $json.body.car }}`  
  - Date: `{{ $today.format('yyyy-MM-dd') }}`

---

## ⚙️ Setup Instructions

1. Import Smart Car Dealership/Workflow.json into n8n.
2. Configure credentials:
   - Gmail OAuth2 in the `Send a message` node.
   - Google Sheets OAuth2 in the `Append row in sheet` node.
3. Publish the webhook (make sure the workflow is active) or expose n8n to receive external requests.
4. Test with the example curl payload above and confirm:
   - Dealer receives the notification email.
   - A new row appears in the Google Sheet.

---

## ✅ Quick Troubleshooting

- No email delivered:
  - Re-authorize Gmail OAuth2 credentials.
  - Check Gmail quota / sending limits.
  - Inspect n8n execution logs for the Gmail node error details.

- Row not appended to sheet:
  - Verify Google Sheets credentials and that the spreadsheet ID + sheet gid are correct.
  - Confirm the sheet has editable access for the configured account.

- Webhook not reachable:
  - Ensure n8n is reachable from the client network.
  - Confirm the workflow is active and the webhook path matches the request URL.

---

## 📂 Project Structure

```
Smart Car Dealership/
│
├─ Screenshots/
│  └─ app screen.png
│
├─ workflow.json
└─ README.md
```

---

## 👤 Author

Mahmoud Nasser — Automation | n8n

---
