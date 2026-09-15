# Innoventix Hub — Inbound AI Voice Agent & Operations Dashboard

An end-to-end, production-grade AI voice telephony platform and real-time operations dashboard for **Innoventix Hub**.

The platform combines a low-latency voice pipeline (answering calls, booking appointments, capturing warm leads, handling support escalations, and executing cross-sell pitches) with a full Next.js analytics dashboard for live monitoring and lead management.

---

## 📁 Repository Architecture

```
InnoventixHub_Inbound-Voice-Agent/
├── backend/                 # Inbound Voice Agent (Python, Pipecat AI, Cal.com, Supabase, Telnyx)
└── frontend/                # Real-Time Operations Dashboard (Next.js, TypeScript, Tailwind/CSS)
```

---

## ✨ System Features

### 📞 Voice Agent Backend (`backend/`)

- **Low-Latency Cascade Voice Pipeline**: Cartesia STT (Ink-Whisper) $\rightarrow$ OpenAI GPT / Anthropic Claude LLM $\rightarrow$ Cartesia TTS (Sonic) $\rightarrow$ Telnyx PSTN Audio over 8kHz WebSockets.
- **Automated Telephony & Tunneling**: `auto_tunnel.py` auto-spawns a `cloudflared` tunnel on server start and programmatically updates the Telnyx TeXML Application Voice URL.
- **Direct Cal.com Scheduling Integration**: Direct REST API calls (`cal_com.py`) to query open slots and book meetings without N8N middleware.
- **Branded Email Confirmations**: Fires branded confirmation emails with Google Meet video links via Resend (`email_sender.py`).
- **Pre-Greeting CRM Context Lookup**: Looks up caller ID in Supabase (`booking_db.py`) before the greeting, enabling personalized cross-sell pitches for existing customers.
- **4 Core Call Scenarios + Escalations**:
  1. *Info / General Support*: Pure knowledge-base Q&A.
  2. *Sales / Purchase Interest*: Live meeting booking.
  3. *Cross-Sell*: Context-driven pitches for existing clients.
  4. *Support Escalation*: Technical issue meeting booking with specific issue topics.
  5. *Warm Lead Capture*: Logs hesitant callers into Supabase CRM (`leads` table).
  6. *Direct Booking Link Email*: Email-only booking links without touching calendar.
  7. *Human Transfer*: Immediate emergency handoff (`transfer_to_human`).
- **Comprehensive Testing Tools**:
  - `python -u test_scenario.py`: Executes all 7 end-to-end test scenarios against live Cal.com & Supabase APIs.
  - `python test_console_chat.py`: Interactive terminal testing with full live tool execution.

---

### 📊 Operations Dashboard Frontend (`frontend/`)

- **Overview Dashboard (`/`)**: Real-time 24-hour activity summary (Total Calls, Bookings, Warm Leads, Dropped Calls), hourly call volume charts, and recent call list.
- **Call Logs & Transcripts (`/calls`)**: Complete searchable call history with duration, classification outcomes, latency logs, and full turn-by-turn conversation transcripts.
- **Booked Meetings Schedule (`/bookings`)**: Centralized view of all sales and support meetings booked by the voice agent.
- **Warm Leads CRM (`/leads`)**: Lead tracking interface to update lead statuses (`warm`, `contacted`, `converted`, `cold`).
- **Protected Access (`/login`)**: Passcode authentication with Next.js middleware session protection.

---

## 🚀 Quick-Start Guide

### 1. Backend Setup (`backend/`)

```powershell
cd backend
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

Fill in your API keys in `backend/.env`:
- `TELNYX_API_KEY`, `TELNYX_ACCOUNT_SID`, `TELNYX_TEXML_APP_ID`
- `CARTESIA_API_KEY`, `CARTESIA_VOICE_ID`
- `OPENAI_API_KEY` (or `ANTHROPIC_API_KEY`)
- `CAL_API_KEY`, `CAL_BOOKING_URL`
- `SUPABASE_URL`, `SUPABASE_SERVICE_KEY`
- `RESEND_API_KEY`, `RESEND_FROM_ADDRESS`
- `TIMEZONE` (e.g. `Asia/Karachi` or `Europe/London`)

#### Start the Voice Agent:
```powershell
python bot.py
```

#### Run Automated Test Suite:
```powershell
python -u test_scenario.py
```

---

### 2. Frontend Dashboard Setup (`frontend/`)

```bash
cd frontend
npm install
cp .env.local.example .env.local
```

Fill in your configuration in `frontend/.env.local`:
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
DASHBOARD_PASSCODE=your-passcode
```

#### Start the Development Server:
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🗄️ Database Schema Setup (Supabase)

Run the SQL script from `backend/innoventix_schema.sql` in your Supabase SQL Editor to create the required tables:

- **`call_logs`**: Stores call transcripts, durations, outcomes, and `timestamptz` timestamps.
- **`meetings`**: Audit trail of booked sales & support meetings.
- **`leads`**: CRM warm leads captured by the agent.
- **`customers`**: Customer database for pre-greeting cross-sell context lookup.

---

## 🌿 Git Branches

- **`main`**: Production release code (Frontend + Backend).
- **`testing`**: Staging environment for test iterations.
