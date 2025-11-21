# AI-JAT — AI-Driven Job Application Toolkit 🚀

**Automate finding jobs, generate tailored cover letters, and get a ready Excel + email — all from a single web form.**

---

## 📖 Overview & Problem Solved

Job hunting is repetitive: you search dozens of job postings, read long descriptions, tailor cover letters for each role, and then track every application. That takes hours and is easy to lose momentum on.

**AI-JAT automates that loop.**

From a simple one-page web form, this tool:
1.  Scrapes relevant LinkedIn job postings.
2.  Reads and parses each job description.
3.  Scores matches against your short resume/skills.
4.  Generates a tailored cover letter per job using LLMs.
5.  Produces a consolidated spreadsheet and emails the packaged toolkit to you.

This workflow is built for **developers and automation users** who want to accelerate outreach without losing personalization. Instead of manually copying job text and drafting letters, AI-JAT delivers a clean Excel with the top 10 job matches plus unique cover letters and direct application links — ready to review and use.

---

## ✨ Core Features & Technology Stack

| Feature | Description | Technology Used (n8n Nodes) |
| :--- | :--- | :--- |
| **1. One-page Web Form** | Collects user input (Name, Keywords, Location, Resume, etc.) to trigger the workflow. | `n8n-nodes-base.formTrigger` |
| **2. LinkedIn Search URL Builder** | Converts form fields into a specific LinkedIn jobs search URL with filters. | `n8n-nodes-base.code` |
| **3. Fetch & Parse Listings** | Requests search results, extracts links (top 10), and iterates to parse Title, Company, Description, etc. | `httpRequest`, `html`, `splitOut`, `splitInBatches` |
| **4. AI Scoring & Generation** | Computes a match score vs. user skills and generates a personalized cover letter (JSON). | `langchain.agent`, `lmChatGoogleGemini`, `lmChatGroq` |
| **5. Data Staging** | Creates a Google Sheet, appends/updates rows with job data/cover letters, and exports as XLSX. | `googleSheets` (Create, Append, Update), `httpRequest` (Export) |
| **6. Email Delivery** | Builds an HTML email, attaches the generated XLSX toolkit, sends to the user, and cleans up files. | `gmail`, `googleSheets` (Delete) |
| **7. Reliability Controls** | Throttling to avoid rate limits, data normalization, and aggregation. | `wait`, `set`, `aggregate` |



---

## 🚀 Demo & Workflow Visuals

* **Live Demo:** [Click Here](https://kpn8n.duckdns.org/form/c882030e-8e30-48b1-abbe-45d51e6f9e13)

### Workflow Diagram
<img width="1920" height="1080" alt="Screenshot 2025-11-21 at 15 29 45" src="https://github.com/user-attachments/assets/dd02b354-41c8-463b-8171-25448aa8ad8a" />

### Final Output Example
<img width="1925" height="593" alt="Screenshot 2025-11-21 at 15 24 01" src="https://github.com/user-attachments/assets/80d06a56-4b64-41e0-a49b-f723a3c77469" />

---

## 🛠️ Setup and Installation Guide

### Files
* **Workflow File:** `AI-JAT.json` (Import this into your n8n instance).

### 1) Import the workflow into n8n
1.  Open your n8n editor UI (self-hosted or cloud).
2.  Click **Import** → choose **File** → select `AI-JAT.json`.
3.  Confirm import. The workflow is named **AI-JAT**.

### 2) Make the Form publicly reachable
The `formTrigger` node exposes a webhook.
* **Self-hosted:** Ensure your instance is reachable (public IP + port or via reverse proxy/DuckDNS).
* **Sample Link Structure:** `https://your-n8n-domain.com/form/<webhook-id>`

### 3) Configure Credentials
You must add the following credentials in your n8n "Credentials" section:

| Credential / API Key | Purpose |
| :--- | :--- |
| **Google Sheets OAuth2** | To create, append, export, and delete the run-specific spreadsheet. |
| **Gmail OAuth2** | To send the final email with the XLSX attachment to the user. |
| **Google Palm / Gemini API** | Primary LLM backend for generating cover letters (`lmChatGoogleGemini`). |
| **Groq API Key** | Secondary LLM backend for scoring and generation validation. |

### 4) Node Configuration Checks
After importing, check these specific nodes:
* **Google Sheets Nodes:** Ensure `Create`, `Append`, and `HTTP Request (Export)` are using your saved Google Sheets credentials.
* **Gmail Node:** Ensure it uses your saved Gmail credentials.
* **Make Linkden URL (Code Node):** If you changed form field names, update the variable references here.
* **Wait Node:** If you are hitting LinkedIn rate limits, increase the wait time.

### 5) Test Run
1.  Open the public form URL.
2.  Submit a test entry with your own email.
3.  Monitor the execution in the n8n Editor.
4.  **Success:** You receive an email with an XLSX attachment containing 10 analyzed jobs.

---

## 🎯 Execution — How to run

1.  **Activate:** Ensure the workflow is set to `Active` in n8n.
2.  **Access:** Open the web form link.
3.  **Input:**
    * **Details:** Name, Email, Keywords, Location, Job Type.
    * **Description:** Paste your resume summary or skill set in "Your Description in Short".
    * **Preferences:** Check "Easy Apply" if desired.
4.  **Process:** The bot will scrape LinkedIn, , analyze descriptions using AI, and compile your toolkit.
5.  **Receive:** Check your inbox for **"{{ Name }}, Your Job Toolkit is Ready!"** and download the Excel file.

---

## 🤝 Contribution & Support

Contributions, improvements, and bug fixes are welcome!

1.  **Fork** this repository.
2.  **Create a branch** for your changes.
3.  **Open a PR** with a description and screenshots.
4.  **Star** the repository if you find AI-JAT useful ⭐.

**Troubleshooting:**
If you need help with webhook exposure or credential errors, please open an Issue describing your environment (Self-hosted/Cloud) and error logs.
