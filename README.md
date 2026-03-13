# 🤖 AI Sales Intelligence Agent

> A complete, hackathon-ready AI-powered sales automation system.
> Captures leads from any source, scores them with AI, and automates follow-ups.

---

## 🏗️ Architecture

```
Multiple Lead Sources
(Website · LinkedIn · WhatsApp · Email · API · Webhook)
              ↓
    FastAPI Lead Ingestion API  (POST /lead)
              ↓
       AI Lead Analyzer
    (OpenAI GPT / Rule-based)
              ↓
    ┌─────────────────────────┐
    │  Automation Layer       │
    │  ├─ Google Sheets CRM   │
    │  └─ WhatsApp Notify     │
    └─────────────────────────┘
              ↓
    Streamlit Dashboard
```

---

## 📁 Project Structure

```
sales_manager_aiagent/
├── app.py            ← Streamlit dashboard (5 pages)
├── lead_api.py       ← FastAPI backend   (POST /lead, GET /leads, GET /stats)
├── ai_analyzer.py    ← AI lead scoring   (OpenAI + rule-based fallback)
├── automation.py     ← Google Sheets CRM + WhatsApp via CallMeBot
├── config.py         ← Central config    (reads from .env)
├── requirements.txt
├── .env.example      ← Copy to .env and fill in
└── n8n_workflow.json ← (Optional) n8n integration workflow
```

---

## ⚡ Quick Start (2 terminals)

### 1. Install dependencies
```bash
pip install -r requirements.txt
```

### 2. Configure environment
```bash
# Windows
copy .env.example .env

# macOS / Linux
cp .env.example .env

# Open .env and set DEMO_MODE=true to run without any API keys
```

### 3. Terminal 1 — Start the FastAPI API
```bash
uvicorn lead_api:app --reload --port 8000
```

You'll see:
```
INFO:     Uvicorn running on http://127.0.0.1:8000
INFO:     Application startup complete.
```

### 4. Terminal 2 — Start the Streamlit Dashboard
```bash
streamlit run app.py
```

Browser opens at **http://localhost:8501**

---

## 🎯 Features

### Streamlit Dashboard (5 pages)

| Page | Description |
|------|-------------|
| 🎯 Submit Lead | Form to submit leads manually with instant AI analysis |
| 📊 Lead Dashboard | CRM-style view of all leads with filters and details |
| 📡 Multi-Source Demo | One-click demo leads from LinkedIn, WhatsApp, Email, API |
| 📅 Book a Demo | Demo scheduling form (also creates a lead) |
| ⚙️ System Status | Health check, API test, config reference |

### FastAPI Endpoints

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/lead` | Submit and analyze a lead |
| `GET`  | `/leads` | List all leads (filterable) |
| `GET`  | `/leads/{id}` | Get one lead by ID |
| `GET`  | `/stats` | Pipeline stats (hot/warm/cold counts) |
| `GET`  | `/health` | API health check |
| `GET`  | `/docs` | Swagger interactive docs |

---

## 📡 API Usage

### Submit a lead via curl
```bash
curl -X POST http://localhost:8000/lead \
  -H "Content-Type: application/json" \
  -d '{
    "name":    "Sarah Johnson",
    "company": "Acme Corp",
    "email":   "sarah@acme.com",
    "message": "We need AI to automate our SDR workflow. Ready to buy.",
    "source":  "linkedin"
  }'
```

### Example response
```json
{
  "id": "A3F2C1B9",
  "status": "analyzed",
  "lead_score": 82,
  "conversion_probability": "High (79%)",
  "intent": "Prospect wants to automate SDR workflows with AI and is ready to purchase.",
  "recommended_strategy": "Schedule a discovery call within 24 h. Emphasize quick ROI...",
  "lead_summary": "Sarah from Acme Corp submitted a hot lead via LinkedIn...",
  "suggested_reply": "Hi Sarah, Thank you for reaching out!...",
  "automations": {
    "google_sheets": "skipped",
    "whatsapp": "skipped"
  },
  "created_at": "2026-03-13T03:49:40"
}
```

---

## 🔧 Integrations Setup

### 🧠 OpenAI (Real AI Analysis)
1. Go to https://platform.openai.com/api-keys
2. Create an API key
3. In `.env`: set `DEMO_MODE=false` and `OPENAI_API_KEY=sk-...`

### 📊 Google Sheets CRM
1. Create a Google Sheet with tab named `leads`
2. Go to Google Cloud Console → Create a Service Account
3. Download the JSON key → save as `service_account.json` in the project root
4. Share your Google Sheet with the service account email (Editor)
5. Copy the Sheet ID from the URL into `.env` as `GOOGLE_SHEET_ID`

**Google Sheet columns (Row 1 headers):**
```
name | company | email | message | source | lead_score | conversion_probability | intent | recommended_strategy | lead_summary | suggested_reply | status | created_at
```

### 💬 WhatsApp Notifications (CallMeBot — Free)
1. Send `I allow callmebot to send me messages` to **+34 603 21 25 97** on WhatsApp
2. You'll receive your API key
3. Set `WHATSAPP_PHONE` and `WHATSAPP_API_KEY` in `.env`
4. Set `DEMO_MODE=false`

No Meta developer account required!

### 🔗 n8n Integration (Optional)
1. Install n8n: `npm install -g n8n` then `n8n start`
2. Import `n8n_workflow.json` via n8n UI → Workflows → Import
3. Update your webhook URL in `.env` as `N8N_WEBHOOK_URL`
4. Configure Google Sheets and Telegram nodes in n8n

---

## 🧪 Demo Mode

**No API keys required.** Set `DEMO_MODE=true` in `.env` (default).

In demo mode:
- AI uses a smart rule-based scorer (realistic scores, not random)  
- Google Sheets write is skipped (logs a message)  
- WhatsApp send is skipped (logs a message)  
- All other features work normally  

---

## 📊 Lead Scoring Explanation

| Score | Tier | Action |
|-------|------|--------|
| 70–100 | 🔥 Hot | Immediate outreach, assign to senior SDR |
| 45–69 | 🌡️ Warm | Follow up within 48 h, send case study |
| 10–44 | ❄️ Cold | Add to nurture sequence |

**Score boosters:**
- Business email domain (not gmail/yahoo)
- Hot keywords: "urgent", "budget approved", "ready to buy", "enterprise"
- Message length (longer = more intent)
- Source: LinkedIn (+10), WhatsApp (+8), Demo (+15)

---

## 🚀 Hackathon Demo Script

1. Open **http://localhost:8501**
2. Go to `📡 Multi-Source Demo` — click all 5 source buttons to show multi-source ingestion
3. Go to `🎯 Submit Lead` — fill in a lead with "urgent" and "enterprise" in the message
4. Show the AI score, strategy, and suggested reply
5. Go to `📊 Lead Dashboard` — show all leads with filters
6. Open **http://localhost:8000/docs** — show the REST API with live testing
7. Explain: in production, Sheets + WhatsApp trigger automatically

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Streamlit |
| Backend API | FastAPI + Uvicorn |
| AI Analysis | OpenAI GPT-4o-mini / Rule-based |
| CRM Database | Google Sheets (gspread) |
| Notifications | WhatsApp via CallMeBot |
| Automation | n8n (optional) |
| Config | python-dotenv |

---

## 📄 License

MIT — built for hackathon. Use freely.
