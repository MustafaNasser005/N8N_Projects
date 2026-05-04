# Hotel Reservation Automation

A minimal n8n workflow that accepts hotel reservation requests via a public form and stores them in Airtable. Designed for quick demos or as a starter template for booking automations.

---

## 📸 App Screen

![App Screen](Screenshots/app%20screen.png)

---

## 🚀 Features

- Public form for guest name and room selection (Single / Deluxe / Suite).
- Automatic record creation in Airtable with mapped fields.
- Simple, low-friction workflow suitable for prototypes and demos.

---

## 🧩 Workflow Overview

1. User submits the "Hotel Reservation" form (node: On form submission).
2. The workflow maps form fields into Airtable (node: Create a record):
   - Name ← "Your Name"
   - Room ← first selected option from "Choose your room?"
3. A new record is created in the configured Airtable base/table.

Key nodes (from Workflow.json):
- On form submission (formTrigger)
- Create a record (airtable)

---

## 📋 Airtable Mapping (from workflow)

- Airtable Base ID: appcsdWUfP7GXdK2q
- Airtable Table ID: tblWvko0ZFBeyofTh

Field mapping:
- Name = {{ $json['Your Name'] }}
- Room = {{ $json['Choose your room?'][0] }}

Note: "Reservation Date" exists in schema but is marked removed in the workflow schema.

---

## ⚙️ Setup Instructions

1. Import Hotel Reservation Automation/Workflow.json into n8n.
2. Configure credentials:
   - Airtable Personal Access Token in the `Create a record` node.
3. Verify the Airtable base/table IDs in the node match your Airtable workspace (defaults above).
4. Activate the form trigger (publish webhook) and test by submitting the form.
5. Confirm a new record appears in Airtable with Name and Room populated.

---

## ✅ Quick Troubleshooting

- No record in Airtable:
  - Verify Airtable token and that the base/table IDs match.
  - Check n8n execution logs for errors.
- Room not populated correctly:
  - The form uses a checkbox; the workflow takes the first selected option (index 0). Adjust mapping for multi-select needs.
- Form not accessible:
  - Ensure the form trigger webhook is published and reachable from the client.

---

## 📂 Project Structure

```
Hotel Reservation Automation/
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

References
- Workflow: [Hotel Reservation Automation/Workflow.json](Workflow.json)