# Innoventix Hub — Inbound AI Voice Agent System

This branch contains the standard `frontend/` and `backend/` project architecture for Innoventix Hub.

---

## 📁 Branch Structure

```
.
├── backend/             # Inbound AI Voice Agent Python backend (Pipecat + Cal.com + Supabase)
├── frontend/            # Next.js Analytics & Call Logs Dashboard
└── README.md
```

---

## 🚀 Running the Backend (`backend/`)

The `backend/` folder contains the voice agent server powered by Pipecat AI, Cartesia (STT/TTS), OpenAI/Anthropic (LLM), Telnyx (telephony), Cal.com (scheduling), and Supabase (CRM & call logs).

```powershell
cd backend
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
python bot.py
```

### Running Test Suite:
```powershell
cd backend
python -u test_scenario.py
```

For full architecture details and changelog, see `backend/PROJECT_DOCUMENTATION.md`.
