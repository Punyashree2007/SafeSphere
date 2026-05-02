# 🛡️ SafeHer Network
### AI-Powered Predictive Safety & Coordinated Response System
**Hackathon MVP** · React + Node.js + Firebase Firestore

---

## 📁 Project Structure

```
safeher-network/
├── backend/
│   ├── server.js              # Express API + escalation logic
│   ├── firebaseAdmin.js       # Firebase Admin SDK init
│   ├── package.json
│   ├── .env.example
│   └── serviceAccountKey.json ← YOU ADD THIS (see Firebase setup)
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── UserDashboard.jsx
│   │   │   ├── ResponderDashboard.jsx
│   │   │   ├── AuthorityDashboard.jsx
│   │   │   ├── Navbar.jsx
│   │   │   └── Badges.jsx
│   │   ├── hooks/
│   │   │   └── useVoiceTrigger.js
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   ├── firebase.js
│   │   ├── api.js
│   │   └── styles.css
│   ├── index.html
│   ├── vite.config.js
│   ├── package.json
│   └── .env.example
│
├── FIREBASE_SETUP.md
└── README.md (this file)
```

---

## ⚡ Quick Start

### Prerequisites
- Node.js v18+
- npm v9+
- A Firebase project (see FIREBASE_SETUP.md)

---

## 🔧 Installation

### 1. Clone / unzip the project
```bash
cd safeher-network
```

### 2. Set up Firebase
Follow **FIREBASE_SETUP.md** completely before continuing.

### 3. Backend Setup
```bash
cd backend
npm install

# Copy env file and edit it if needed
cp .env.example .env

# Place serviceAccountKey.json from Firebase in this folder
# backend/serviceAccountKey.json
```

### 4. Frontend Setup
```bash
cd ../frontend
npm install

# Create .env from example and fill in your Firebase web config
cp .env.example .env
# Edit .env with your Firebase values
```

---

## 🚀 Running the App

Open **two terminal windows**:

### Terminal 1 — Backend
```bash
cd backend
npm run dev
# Starts on http://localhost:5000
```

### Terminal 2 — Frontend
```bash
cd frontend
npm run dev
# Starts on http://localhost:3000
```

Open http://localhost:3000 in your browser.

---

## 🎯 Demo Flow

1. **User Dashboard** (http://localhost:3000/)
   - Press the red **SOS button** → alert created in Firestore
   - Click **▶ Start Listening** → say "help" or "emergency" → SOS auto-triggers
   - Watch the AI Risk badge (🔴 HIGH after 8PM, 🟢 LOW daytime)

2. **Responder Dashboard** (http://localhost:3000/responder)
   - Alert appears within 3 seconds (auto-poll)
   - Click **✅ Accept** → status becomes "accepted"
   - Click **🏛️ Report to Authority** → routes case to authority

3. **Escalation** (automatic)
   - If no one accepts within **20 seconds** → status → "escalated"
   - Backend console prints: `🚨 ESCALATED TO AUTHORITIES`

4. **Authority Dashboard** (http://localhost:3000/authority)
   - Shows ONLY reported cases
   - Highlights escalated cases with priority banners

---

## 🔌 API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/health` | Health check |
| GET | `/api/risk` | Current AI risk level |
| GET | `/api/alerts` | All alerts (newest first) |
| GET | `/api/alerts/authority` | Only reported alerts |
| POST | `/api/sos` | Create SOS alert |
| PATCH | `/api/alerts/:id/accept` | Responder accepts alert |
| PATCH | `/api/alerts/:id/report` | Report to authority |

### POST /api/sos body:
```json
{ "userId": "user_abc12", "triggeredBy": "voice" }
```

### PATCH /api/alerts/:id/accept body:
```json
{ "responderName": "Officer Singh" }
```

---

## ⚙️ Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` | 5000 | Backend port |
| `ESCALATION_DELAY_MS` | 20000 | Escalation timeout (ms) |

---

## 🧠 AI Risk Logic

Rule-based (no ML models):
- Hour 20–23 or 0–5 → **HIGH RISK** 🔴
- All other hours → **LOW RISK** 🟢

---

## 🎤 Voice Keywords

The browser SpeechRecognition API listens for:
- `"help"`
- `"save me"`
- `"emergency"`
- `"sos"`
- `"danger"`

Works in Chrome, Edge, and Safari. Firefox has limited support.

---

## 🛠️ Tech Stack

| Layer | Tech |
|-------|------|
| Frontend | React 18 + Vite + React Router v6 |
| Backend | Node.js + Express |
| Database | Firebase Firestore |
| Voice | Web SpeechRecognition API |
| Styling | Custom CSS (no Tailwind dependency) |

---

## 🏆 Hackathon Features Checklist

- [x] SOS Button
- [x] Voice Trigger (keyword detection)
- [x] Smart Escalation (20s timer)
- [x] Responder Dashboard (live polling)
- [x] Accept Alert
- [x] Report to Authority
- [x] Authority Dashboard (reported-only feed)
- [x] AI Risk Detection (rule-based)
- [x] Real-time UI updates (3s polling)
- [x] Status badges + risk labels
- [x] Timestamps on all events
