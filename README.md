# 🔒 SSL Certificate Monitor (n8n Workflow)

This repository contains an **automated SSL/TLS certificate monitor** built for [n8n](https://n8n.io).  
It checks the expiry dates of multiple domains and automatically sends an **email alert** when any certificate has **≤ 40 days** remaining.

---

## 🚀 Features
- ✅ Checks SSL expiry for any list of domains  
- ✅ Uses `openssl` to extract certificate issue/expiry dates  
- ✅ Calculates remaining validity (`daysLeft`)  
- ✅ Sends **automated email alerts** when a certificate is close to expiry  
- ✅ Lightweight & serverless — runs entirely inside n8n  
- ✅ Compatible with **n8n v1.40 +** (tested up to v1.113.x)

---

## 🧩 Workflow Overview

| Step | Node | Purpose |
|------|------|----------|
| 1 | **Schedule Trigger** | Runs the workflow daily at a specific hour |
| 2 | **Domain List** | Defines the list of domains to check |
| 3 | **Extract Domain** | Parses domain JSON and loops through each entry |
| 4 | **Check SSL Dates Finder** | Executes `openssl` to pull issue & expiry dates |
| 5 | **SSL Age Calculator** | Computes certificate age and days left |
| 6 | **Is SSL ≤ 40 Days Left?** | Conditional check for expiring certificates |
| 7 | **Send Email Alert** | Sends an alert email to configured recipients |

---

## ⚙️ Configuration

### 1️⃣ Prerequisites
- Running **n8n instance** (v1.40 +)
- SMTP credentials (e.g. Gmail, AWS SES, Mailgun)
- OpenSSL installed inside the n8n container (already present in official images)

---

### 2️⃣ Environment Variables (if needed)
If using Gmail SMTP inside Docker, ensure:
```bash
N8N_EMAIL_MODE=smtp
N8N_SMTP_HOST=smtp.gmail.com
N8N_SMTP_PORT=587
N8N_SMTP_USER=example@gmail.com
N8N_SMTP_PASS=your_app_password


### 2️⃣ Environment Variables
```bash
Copy the script (ssl_certificate_email_alert_script.json) and paste into the n8n workflow
