# 📧 Email Automation Workflow with n8n + Ollama + Google Sheets

This workflow automates email summarization and draft creation using **n8n**, **Ollama (Llama3)**, and **Google Sheets**.

---

## 🧩 Features

1. **Gmail Trigger**  
   - Captures new incoming emails automatically.

2. **Ollama (Llama3) via HTTP Request**  
   - Summarizes the email text in 3 lines using the Llama3 model.

3. **Code Node**  
   - Parses the Ollama output and structures it for Google Sheets and Gmail draft.

4. **Google Sheets Append**  
   - Logs:
     - Date
     - Sender Email
     - Email Subject
     - Snippet
     - Summarized content  

5. **Gmail Draft Node**  
   - Creates a draft reply email with the summarized text.

---

## 🛠️ Setup Instructions

### 1️⃣ Add Credentials

- **Gmail OAuth2**: Connect your Gmail account in n8n.
- **Google Sheets OAuth2**: Connect your Google account and ensure access to the desired sheet.

### 2️⃣ Import Workflow

- Copy the JSON file (`email_automation_workflow.json`) into n8n:
  - In n8n → click **Import Workflow** → paste JSON → save.

### 3️⃣ Configure Nodes

- **Google Sheets Node**:
  - Spreadsheet ID → Copy from Google Sheet URL (`https://docs.google.com/spreadsheets/d/SPREADSHEET_ID/edit`)
  - Sheet Name → Tab name in your spreadsheet (e.g., `Summary`)
  - Columns → Map these fields:
    | Column Name | Value |
    |-------------|-------|
    | Date        | `{{$json["Date"]}}` |
    | Email       | `{{$json["Email"]}}` |
    | Subject     | `{{$json["Subject"]}}` |
    | Snippet     | `{{$json["Snippet"]}}` |
    | Summary     | `{{$json["Summary"]}}` |

- **Gmail Draft Node**:
  - Subject → `=Re: {{$json["Subject"]}}`
  - Message → `=Hi, Here’s a short summary of your email: {{$json["Summary"]}} Best, n8n Assistant`
  - Send To → `={{$json["Email"]}}`

### 4️⃣ Activate Workflow

- Turn on the workflow to start capturing emails, summarizing them, and creating drafts.

---

## 📊 Example Output

| Date | Email | Subject | Snippet | Summary |
|------|-------|---------|---------|---------|
| 2025-10-08 | naukrialerts@naukri.com | Job Alert | Take a free course today with Design School. | Here is a summary of the email in 3 lines: Take a free course with Design School today. No additional details or information provided. The email appears to be a promotional message encouraging you to sign up for a course. |

---

## ⚡ Notes

- Make sure Ollama Llama3 is running locally (`http://127.0.0.1:11434`).
- Ensure n8n has permission to access Gmail and Google Sheets.
- You can customize the summary length in the HTTP Request node.

---

## 📂 Files

- `email_automation_workflow.json` → n8n workflow JSON
