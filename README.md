<div align="center">

# 🛡️ PhishGuard AI — Phishing Email Threat Analyzer 🛡️
### An AI-Powered Zero-Trust Email Threat Inspector

[![Made for Education](https://img.shields.io/badge/Purpose-Educational-blueviolet?style=for-the-badge)](#-disclaimer)
[![Frontend](https://img.shields.io/badge/Frontend-Tailwind%20CSS-38BDF8?style=for-the-badge&logo=tailwindcss&logoColor=white)](#)
[![Automation](https://img.shields.io/badge/Automation-n8n-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io/)
[![AI Engine](https://img.shields.io/badge/AI%20Engine-OpenAI%20GPT--4-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com/)
[![License](https://img.shields.io/badge/License-Educational%20Use-lightgrey?style=for-the-badge)](#-disclaimer)

**Made by [Syed Muhammad Tayyab](https://github.com/Syed-Muhammad-Tayyab) — Happy Hacking! 🎉**

</div>

---

> ⚠️ **Educational / personal use.** This tool sends submitted email content to a webhook and an AI model for analysis. Don't paste real sensitive data from third parties, and review your n8n/OpenAI usage and privacy terms before using it on real mail.

---

## 📑 Table of Contents

- [🔥 What is PhishGuard AI?](#-what-is-phishguard-ai)
- [🧱 Architecture](#-architecture)
- [📂 Repo Structure](#-repo-structure)
- [🛠️ Setup Guide](#️-setup-guide)
- [🧠 How the AI Scores Risk](#-how-the-ai-scores-risk)
- [📧 Sample Report Output](#-sample-report-output)
- [⚠️ Disclaimer](#️-disclaimer)

---

## 🔥 What is PhishGuard AI?

**PhishGuard AI** is a lightweight, single-page **email threat inspector**. A user pastes a suspicious email's subject/body into a sleek dark-mode UI, submits it, and the payload is sent to an **n8n automation workflow** that calls an **AI model (GPT)** to analyze the content for phishing, spoofing, and social-engineering indicators — then emails back a fully styled HTML incident report. 🧠🔐

Key features:

- 🎨 Modern glassmorphism UI built with Tailwind CSS, animated scanning overlay, and live status feedback
- 🔌 Zero backend code — just a single `fetch()` call to an n8n webhook
- 🤖 Structured JSON output from the AI (risk score, risk level, threat indicators, recommendations)
- 📬 Auto-generated, branded HTML email report delivered straight to the recipient's inbox

---

## 🧱 Architecture

```
┌──────────────────┐     POST (email + content)     ┌─────────────────────┐
│  index.html (UI)  │ ──────────────────────────────▶ │   n8n Webhook        │
│  Tailwind + JS     │                                 │   Workflow            │
└──────────────────┘                                   └──────────┬──────────┘
                                                                    │
                                                                    ▼
                                                        ┌─────────────────────┐
                                                        │  OpenAI GPT-4        │
                                                        │  (prompts.txt)       │
                                                        │  → JSON risk report  │
                                                        └──────────┬──────────┘
                                                                    │
                                                                    ▼
                                                        ┌─────────────────────┐
                                                        │  HTML Email Report   │
                                                        │  (email_content.txt) │
                                                        │  → sent to recipient │
                                                        └─────────────────────┘
```

---

## 📂 Repo Structure

| File | Purpose |
|---|---|
| `index.html` | The front-end UI — the form users paste suspicious email content into |
| `prompts.txt` | The system prompt that instructs the AI model how to analyze the email and the exact JSON schema it must return |
| `email_content.txt` | The HTML template used by the n8n workflow to build the styled report email sent back to the user |

---

## 🛠️ Setup Guide

### 1️⃣ Clone the repo

```bash
git clone https://github.com/Syed-Muhammad-Tayyab/Phishing-Email-Threat-Analyzer.git
cd Phishing-Email-Threat-Analyzer
```

### 2️⃣ Build the n8n workflow

Set up an **n8n** workflow with:

1. A **Webhook** node (POST) that receives `{ email, content }` from the form.
2. An **AI / OpenAI** node using the system prompt from [`prompts.txt`](./prompts.txt) to analyze `content` and return the JSON risk schema.
3. A **Send Email** node that renders [`email_content.txt`](./email_content.txt) (replace the `{{ ... }}` expressions with your workflow's actual node/field references) and sends it to the submitted `email`.

### 3️⃣ Connect the frontend to your webhook

Open `index.html` and replace the placeholder with your real n8n webhook URL:

```js
const N8N_WEBHOOK_URL = 'WEBHOOK_URL'; // ← replace with your n8n webhook endpoint
```

### 4️⃣ Run it locally

```bash
php -S 0.0.0.0:8000
# or simply open index.html directly in your browser
```

Then visit `http://localhost:8000`, paste a suspicious email, and hit **Execute Threat Analysis**. ⚔️

---

## 🧠 How the AI Scores Risk

The AI is instructed (see [`prompts.txt`](./prompts.txt)) to act as a cybersecurity specialist and return **only** a JSON object:

```json
{
  "risk_score": 85,
  "risk_level": "High Risk",
  "summary": "Concise summary of why this email is safe or dangerous.",
  "threat_indicators": [
    "Fake sense of urgency",
    "Unverified external domain link"
  ],
  "safety_recommendations": [
    "Do not click the link",
    "Report to IT/security team"
  ]
}
```

`risk_level` is always one of: `Safe`, `Low Risk`, `Moderate Risk`, `High Risk`, or `Critical Threat`.

---

## 📧 Sample Report Output

The n8n workflow turns that JSON into a branded HTML incident report (see [`email_content.txt`](./email_content.txt)) featuring:

- 🎯 Target email analyzed
- 📊 Risk score + risk level badge
- 📝 Executive summary
- 🚩 Identified threat indicators (list)
- ✅ Actionable safety recommendations (list)

---

## ⚠️ Disclaimer

This project is provided for **educational and personal productivity use**. It is not a certified security product and should not be relied on as the sole method of detecting phishing. Always verify suspicious emails through your organization's official security channels, and be mindful of what data you submit to third-party AI services.

---

<div align="center">

Made with ❤️ by **[Syed Muhammad Tayyab](https://github.com/Syed-Muhammad-Tayyab)**
📂 [View this repo](https://github.com/Syed-Muhammad-Tayyab/Phishing-Email-Threat-Analyzer)

</div>
