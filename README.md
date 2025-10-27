---

````markdown
# 🔒 SSL Certificate Monitor (n8n Workflow)

This repository contains an **automated SSL/TLS certificate monitor** built for [n8n](https://n8n.io).  
It checks the expiry dates of multiple domains and automatically sends an **email alert** when any certificate has **≤ 40 days** remaining.

---

## 🚀 Features
- ✅ Checks SSL expiry for any list of domains  
- ✅ Uses `openssl` to extract certificate issue/expiry dates  
- ✅ Calculates remaining validity (`daysLeft`)  
- ✅ Sends **automated email alerts** before expiry  
- ✅ Lightweight & serverless — runs entirely inside n8n  
- ✅ Compatible with **n8n v1.40+** (tested up to v1.113.x)

---

## 🧩 Workflow Overview

| Step | Node | Purpose |
|------|------|----------|
| 1 | **Schedule Trigger** | Runs the workflow daily at a specific hour |
| 2 | **Domain List** | Defines the list of domains to check |
| 3 | **Extract Domain** | Parses domain JSON and loops through each entry |
| 4 | **Check SSL Dates Finder** | Executes `openssl` to pull issue & expiry dates |
| 5 | **SSL Age Calculator** | Computes certificate age and remaining days |
| 6 | **Is SSL ≤ 40 Days Left?** | Filters certificates nearing expiration |
| 7 | **Send Email Alert** | Sends an alert email to configured recipients |

---

## ⚙️ Configuration

### 1️⃣ Prerequisites
Before importing the workflow, ensure you have:
- A running **n8n instance** (v1.40 or higher)
- Working **SMTP credentials** (e.g., Gmail, AWS SES, Mailgun)
- **OpenSSL** available inside the n8n container (included by default in official images)

---

### 2️⃣ Environment Variables
If you are using **Gmail SMTP** inside a Docker environment, configure these variables in your `.env` or container settings:

```bash
N8N_EMAIL_MODE=smtp
N8N_SMTP_HOST=smtp.gmail.com
N8N_SMTP_PORT=587
N8N_SMTP_USER=example@gmail.com
N8N_SMTP_PASS=your_app_password
````

> ⚠️ **Note:** Gmail now requires an **App Password** for SMTP access if 2FA is enabled.

---

## 📥 Workflow Import Instructions

### 1. Copy or Download the Workflow

Copy the contents of the workflow file:

```
ssl_certificate_email_alert_script.json
```

### 2. Import into n8n

In your **n8n dashboard**:

1. Go to **Workflows → Import from File**
2. Paste the workflow JSON or upload the `.json` file
3. Click **Import**

### 3. Configure Email Credentials

1. Open the **Send Email Alert** node
2. Select or create an SMTP credential (e.g., `example@gmail.com SMTP`)
3. Save the changes

### 4. Update Domain List

Inside the **Domain List** node, replace the example domains:

```json
[
  { "domain": "a.example.com" },
  { "domain": "b.example.com" },
  { "domain": "c.example.com" }
]
```

### 5. Activate the Workflow

Enable the workflow toggle in n8n.
It will now run daily at the scheduled time and email alerts when SSL certificates are close to expiration.

---

## 📧 Example Email Output

**Subject:**

```
SSL Expiry Alert: a.example.com — 25 days left
```

**Body:**

```
Hello Team,

The SSL certificate for a.example.com will expire soon.
Remaining validity: 25 days.

Issued on: 2025-09-15T11:00:00Z
Expires on: 2025-11-10T11:00:00Z

Please review renewal schedule.

--  
Regards,  
n8n SSL Monitor
```

---

## 📂 Repository Structure

```
.
├── ssl_certificate_email_alert_script.json   # n8n workflow definition
└── README.md                                 # Documentation (this file)
```

---

## 🧠 Troubleshooting

| Issue                     | Possible Cause                         | Solution                                                       |
| ------------------------- | -------------------------------------- | -------------------------------------------------------------- |
| `Unknown alias: obj`      | Outdated expression syntax             | Ensure `$json['field']` syntax is used (fixed in this version) |
| Email not sent            | SMTP misconfiguration                  | Check n8n → Credentials → SMTP settings                        |
| Empty SSL results         | Domain unreachable or port 443 blocked | Verify DNS and network access                                  |
| Workflow doesn’t activate | Syntax conflict or missing node        | Reimport the updated JSON from this repo                       |

---

## 🛡️ License

This project is released under the **MIT License**.
You are free to use, modify, and distribute it for both personal and commercial purposes.

---
