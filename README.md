# 🔐 AI-Powered SSL Certificate Checker — n8n DevOps Portfolio Project 6

An automated SSL certificate monitoring workflow built with n8n that checks 
expiry dates for multiple websites daily, uses Groq AI to assess risk levels 
and generate action plans, and logs everything to Google Sheets for a permanent audit trail.

---

## 📸 Workflow Overview

![Workflow Canvas](screenshots/workflow-canvas.png)

---

## 🧰 Tech Stack

| Tool | Purpose |
|---|---|
| **n8n** | Workflow automation engine |
| **JavaScript (Code Nodes)** | Website list, SSL data, data formatting |
| **Groq AI (LLaMA 3.3-70b)** | Risk assessment and action plan generation |
| **Google Sheets** | Permanent audit log of all SSL checks |
| **Google Cloud OAuth2** | Secure authentication to Google APIs |

---

## 🗺️ Architecture

```
Schedule Trigger (daily at 8am)
        ↓
Code Node — define list of websites to monitor
        ↓
Code Node — fetch/simulate SSL certificate data
        ↓
HTTP Request — send to Groq AI for risk assessment
        ↓
Code Node — map data to Google Sheets column names
        ↓
Google Sheets — append row to audit log
```

---

## ⚙️ How It Works

### 1. Schedule Trigger
Fires every day at 8am automatically — no manual intervention needed.

### 2. Website List (Code Node)
Defines the list of domains to check:
```javascript
const websites = [
  'thepresentdevopsengineer.cloud',
  'google.com',
  'github.com',
  'amazon.com',
  'cloudflare.com',
  'linkedin.com',
  'netflix.com'
];
```

### 3. SSL Data (Code Node)
Fetches or simulates SSL certificate expiry data including expiry date and days remaining until expiry.

### 4. Groq AI Risk Assessment (HTTP Request)
Sends SSL data to Groq AI which returns a Risk Level (Critical/High/Medium/Low) and a one sentence action plan per website.

### 5. Data Formatting (Code Node)
Maps all data to exact Google Sheets column names for clean logging.

### 6. Google Sheets Logging
Appends a new row for each website with all SSL data and AI analysis.

---

## 📊 Risk Level Classification

| Days Remaining | Risk Level | Action Required |
|---|---|---|
| 120+ days | Low | Monitor, renew 30 days before expiry |
| 30-120 days | Medium | Schedule renewal soon |
| 7-30 days | High | Renew immediately |
| 0-7 days | Critical | Emergency renewal required |
| Negative | Expired | Certificate has expired |

---

## 🛠️ Setup Instructions

### Prerequisites
- n8n instance (self-hosted or cloud)
- Groq API key ([console.groq.com](https://console.groq.com))
- Google account with Sheets access
- Google Cloud project with Sheets API and Drive API enabled

### Steps

**1. Clone this repo**
```bash
git clone https://github.com/YOUR_USERNAME/n8n-ssl-certificate-checker.git
```

**2. Import the workflow**
- Open your n8n instance
- Click "Import from file"
- Select `workflow.json`

**3. Configure Credentials**
- Add Header Auth credential for Groq API
- Add Google Sheets OAuth2 credential
- Enable Google Sheets API and Drive API in Google Cloud Console

**4. Create Google Sheet**
- Create a sheet named `SSL Certificate Monitor`
- Add headers: `Website | Expiry Date | Days Remaining | Risk Level | AI Action Plan | Checked At`
- Copy the Spreadsheet ID from the URL

**5. Activate the workflow**
- Toggle to Active — runs automatically every day at 8am

---

## 📁 Project Structure

```
n8n-ssl-certificate-checker/
├── workflow.json
├── screenshots/
│   ├── workflow-canvas.png
│   ├── website-list-output.png
│   ├── ssl-data-output.png
│   ├── groq-ai-response.png
│   ├── data-reshape-output.png
│   ├── google-sheets-data.png
│   └── workflow-active.png
└── README.md
```

---

## 🔐 Security Best Practices

- All credentials stored in n8n Credential Manager
- Google Sheets access via OAuth2
- Groq API key stored as Header Auth credential
- Workflow JSON contains no hardcoded secrets

---

## 💡 Potential Improvements

- Replace simulated SSL data with real certificate checking API
- Add Slack alerts for Critical and High risk certificates
- Add email notifications for expired certificates
- Track historical trends in a separate sheet
- Monitor multiple environments (staging, production)

---

## 🧠 Key Concepts Learned

- Google Cloud OAuth2 — enabling APIs and creating credentials
- n8n credential manager — secure credential storage
- Data reshaping — mapping between different data structures
- AI risk assessment — using LLMs for operational decisions
- Google Sheets as a database — spreadsheets for audit logging

---

## 👨‍💻 Author

**The Present DevOps Engineer**
🌐 [thepresentdevopsengineer.cloud](https://thepresentdevopsengineer.cloud)

---

## 📜 License

MIT License
