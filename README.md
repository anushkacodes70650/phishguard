[README (2).md](https://github.com/user-attachments/files/35067855/README.2.md)
# 🛡️ PhishGuard

**Phishing detection and threat intelligence platform** — scan URLs, messages and domains, get an explainable risk score, and export forensic PDF reports.

> *"Think Before You Click."*

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![Tests](https://img.shields.io/badge/tests-15%20passing-brightgreen)

---

## Overview

PhishGuard is a full-stack web application that helps users spot phishing links, scam messages and suspicious domains. Detection is done by a **heuristic and rule-based engine** that explains *why* something is flagged, so results are easy to understand and verify.

## Features

### 🔗 URL Scanner
- Shannon-entropy check to catch randomly generated (DGA-style) domains
- Punycode / look-alike character (homoglyph) detection
- Brand-impersonation heuristics (PayPal, Apple, Microsoft, Chase, Netflix, …)
- Subdomain stacking and open-redirect pattern checks
- SSRF protection: loopback, private-range and cloud-metadata addresses are blocked

### 💬 Message Scanner
- Detects urgency, threats, credential requests, prize/lottery lures and business-email-compromise patterns
- Highlights the suspicious phrases directly in the message
- Extracts embedded links and scans them too

### 🌐 Domain Intelligence
- DNS lookups (A, MX, NS) and SPF policy check
- TLS certificate inspection (issuer, subject, validity, expiry warnings)

### 📡 Threat Intelligence
- Indicator search with confidence scoring
- Honest provider-status reporting; optional external API keys via `.env`

### 📄 PDF Reports
- Generate a forensic PDF report for any completed scan (ReportLab)

### 🎓 Awareness Academy
- Short modules on phishing, smishing (SMS), quishing (QR codes) and BEC, each with a quiz and badge

### 👤 Accounts & Admin
- JWT authentication, user profile and settings, scan history with search, filters and CSV/JSON export
- Role-based admin console (user management, system telemetry)

---

## Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 18, TypeScript, Vite, Tailwind CSS, Framer Motion, Three.js, Recharts, Axios, React Router |
| **Backend** | Python, FastAPI, SQLAlchemy, SQLite, Pydantic v2, python-jose (JWT), bcrypt, ReportLab, dnspython, httpx |
| **Testing** | PyTest |

---

## Getting Started

### Prerequisites
- Python 3.10+
- Node.js 18+ and npm

### 1. Clone the repository
```bash
git clone https://github.com/anushkascodes70650/phishguard.git
cd phishguard
```

### 2. Start the backend
```bash
cd backend
python -m venv venv
# Windows:  venv\Scripts\activate
# macOS/Linux:  source venv/bin/activate
pip install -r requirements.txt

# optional: copy the example config and edit it
cp .env.example .env

python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```
- API: http://127.0.0.1:8000
- Interactive docs: http://127.0.0.1:8000/docs

The SQLite database is created and seeded automatically on first start.

> On Windows you can use `py` instead of `python`.

### 3. Start the frontend
Open a second terminal:
```bash
cd frontend
npm install
npm run dev
```
Open http://localhost:5173

---

## Demo Accounts

Seeded automatically for **local development only**:

| Role | Email | Password |
|---|---|---|
| Analyst | `analyst@phishguard.ai` | `Analyst@PhishGuard2026!` |
| Administrator | `admin@phishguard.ai` | `Admin@PhishGuard2026!` |

---

## Configuration

Copy `backend/.env.example` to `backend/.env`:

| Variable | Purpose |
|---|---|
| `PHISHGUARD_SECRET_KEY` | Secret used to sign JWTs — **set your own long random value** |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | Token lifetime (default 7 days) |
| `DATABASE_URL` | Database connection string (default: local SQLite file) |
| `VIRUSTOTAL_API_KEY`, `GOOGLE_SAFE_BROWSING_KEY`, `URLHAUS_API_KEY`, `PHISHTANK_API_KEY` | Optional external intelligence providers. The app works without them. |

---

## Running the Tests

```bash
cd backend
python -m pytest tests -v
```

The suite (15 tests) covers URL heuristics, SSRF protection, message analysis, JWT authentication and admin access control.

---

## API Overview

Base path: `/api` — full details in [`docs/API_DOCUMENTATION.md`](docs/API_DOCUMENTATION.md).

| Area | Endpoints |
|---|---|
| Auth | `POST /auth/register`, `POST /auth/login`, `GET /auth/me` |
| Scanning | `POST /scan/url`, `POST /scan/message`, `POST /domain/analyze` |
| History | `GET /history`, `GET /history/export` |
| Dashboard | `GET /dashboard/overview`, `GET /dashboard/analytics` |
| Reports | `POST /reports/generate/{scan_id}`, `GET /reports/{report_id}/download` |
| Awareness | `GET /awareness/modules`, `POST /awareness/quiz` |
| Admin | `GET /admin/overview`, `GET /admin/users`, `PUT /admin/users/{user_id}/role` |

---

## Project Structure

```
phishguard/
├── backend/
│   ├── app/
│   │   ├── api/            # Route handlers (auth, scan, history, reports, admin, …)
│   │   ├── core/           # Settings and security helpers
│   │   ├── database/       # Session setup and seed data
│   │   ├── detection/      # URL, message and domain detection engines
│   │   ├── intelligence/   # Threat-intel provider service
│   │   ├── models/         # SQLAlchemy models
│   │   ├── reports/        # PDF generator
│   │   ├── schemas/        # Pydantic schemas
│   │   └── main.py         # FastAPI entry point
│   ├── tests/              # PyTest suite
│   └── requirements.txt
├── frontend/
│   └── src/
│       ├── components/     # Reusable UI (gauges, scanner, modals, navbar)
│       ├── context/        # Auth context
│       ├── pages/          # Dashboard, scanners, history, reports, admin, …
│       ├── services/       # API client
│       └── types/
└── docs/                   # Setup, architecture and API documentation
```

---

## Security Notes

- Change `PHISHGUARD_SECRET_KEY` before any real deployment.
- The demo accounts are for local testing; remove or change them before deploying.
- CORS is open (`*`) for development convenience — restrict it to your frontend's origin in production.
- PhishGuard uses heuristics and is meant as an aid, not a replacement for enterprise-grade security tooling.

## Documentation

- [Setup guide](docs/SETUP.md)
- [Architecture](docs/ARCHITECTURE.md)
- [API reference](docs/API_DOCUMENTATION.md)

---

Made by [@anushkascodes70650](https://github.com/anushkascodes70650)
