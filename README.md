# SentinelIQ
AI Security Copilot for Security Operations Centers (SOCs)

# SentinelIQ

**AI Security Copilot for Security Operations Centers (SOCs)**

> Turns thousands of noisy security alerts into a few clear attack stories, predicts the attacker's next move, prices the risk in rupees, and responds safely, while defending the AI itself from being tricked.

Built for the **CyberForge Hackathon** by Mitigata (Theme: Innovative Solution for AI & Cybersecurity).

---

## Table of Contents
- [Problem](#problem)
- [Our Solution](#our-solution)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Demo Flow](#demo-flow)
- [Safety and Responsible Testing](#safety-and-responsible-testing)
- [Datasets and Third-Party Tools](#datasets-and-third-party-tools)
- [Impact](#impact)
- [Team](#team)

---

## Problem

Security teams face four connected problems:

1. **Alert overload:** thousands of daily alerts, mostly false positives. Real breaches get buried and analysts burn out.
2. **Scattered evidence:** one attack appears as many separate alerts across different tools, and connecting them by hand is slow.
3. **Unknown business cost:** management and insurers need to know what an incident will cost, not read technical jargon.
4. **AI can be tricked:** attackers hide instructions in emails and logs (prompt injection) to fool AI analysts into closing real threats.

## Our Solution

SentinelIQ ingests alerts from multiple sources, enriches and scores them, correlates them into a single attack story with an interactive graph, explains it in plain English with evidence links, predicts what happens next, quantifies the financial impact, and proposes safe, approval-gated responses. A prompt-injection firewall protects the AI agents themselves.

## Key Features

### Detect and triage
- Alert ingestion from firewall, endpoint (EDR), email and authentication logs
- Enrichment with IOC lookup and MITRE ATT&CK mapping
- Risk scoring using user-behaviour baselines (for example impossible travel, new device) and asset business value
- Honeypot/canary asset that instantly escalates when touched
- Analyst feedback loop that reduces repeated false positives

### Investigate
- Cross-source correlation into one attack story, graph and timeline
- Plain-English explanation where every claim links to the raw log evidence, with a confidence indicator
- **Ask the Incident:** chat grounded only in the current incident's evidence
- **Threat actor attribution card:** MITRE ATT&CK technique overlap with known group profiles (similarity, not proof)

### Predict
- Next-step prediction and blast-radius map
- **What-if simulation:** click any node to see what an attacker could reach if it were compromised

### Respond and prevent
- Playbooks (block IP, reset credentials, isolate endpoint) with risk tiers
- Human approval for critical actions, dry-run, rollback and a full audit log (executed in a sandbox/simulator only)
- Auto-drafted Sigma-style detection rule with an approve-and-export option

### Business and insurance
- Asset tagging with downtime cost and a live **loss clock** (₹)
- CERT-In / DPDP-style incident report draft
- **Claim package assembler:** timeline, evidence, ₹ quantification and response log in one downloadable file
- Cyber insurance readiness score

### AI security
- Prompt-injection firewall for untrusted logs and emails (quarantined model, structured output only)
- Per-agent permission sandboxing
- **Self-red-team report card:** injection tests run at startup with measured results

## Architecture

```
Sources (Firewall / EDR / Email / Auth logs)
        │
        ▼
Ingest & Enrich  ──►  IOC lookup, MITRE ATT&CK mapping
        │
        ▼
Score  ──►  User behaviour baseline + asset business value
        │
        ▼
Correlate  ──►  Attack graph and timeline
        │
        ▼
AI Agents  ──►  Explain · Predict · What-if · Attribution · Chat
        │
        ▼
Respond  ──►  Approval-gated actions in a simulator + audit log

Side layers:
  • Business layer: ₹ loss clock, compliance report, claim package, readiness score
  • AI security layer: injection firewall, agent permissions, red-team scorecard
```

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, FastAPI |
| AI agents | Claude API |
| Anomaly scoring | scikit-learn |
| Knowledge base (RAG) | MITRE ATT&CK data with a vector store |
| Graph and simulation | NetworkX |
| Frontend | React with an interactive graph view (React Flow or Cytoscape) |
| Data | Public datasets plus a scripted simulated attack |

## Project Structure

```
sentineliq/
├── backend/
│   ├── app/
│   │   ├── ingest/          # log parsers and normalisation
│   │   ├── scoring/         # anomaly, user baseline, asset value
│   │   ├── correlation/     # attack graph and timeline
│   │   ├── agents/          # explain, predict, what-if, chat
│   │   ├── security/        # prompt-injection firewall, red-team tests
│   │   ├── response/        # playbooks, approval, simulator, audit log
│   │   └── business/        # ₹ model, reports, claim package
│   ├── data/                # sample and simulated datasets
│   └── requirements.txt
├── frontend/
│   ├── src/
│   └── package.json
├── simulator/               # scripted attack for demo
└── README.md
```

## Getting Started

### Prerequisites
- Python 3.10+
- Node.js 18+
- An Anthropic API key

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/sentineliq.git
cd sentineliq
```

### 2. Backend setup
```bash
cd backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Create a `.env` file in `backend/`:
```env
ANTHROPIC_API_KEY=your_api_key_here
```

Run the server:
```bash
uvicorn app.main:app --reload
```

### 3. Frontend setup
```bash
cd frontend
npm install
npm start
```

### 4. Run the attack simulation
```bash
cd simulator
python run_attack.py
```

Open the dashboard in your browser (default: `http://localhost:3000`).

## Demo Flow

1. Hundreds of noisy alerts collapse into a few incidents.
2. Open the top incident to see the attack graph, plain-English story and evidence links, with the loss clock ticking.
3. View the attribution card, then click a node for a what-if blast radius.
4. Approve a critical response and watch the audit log.
5. Ask the incident a question in chat.
6. Download the claim-ready package.
7. Closer: a phishing email with a hidden injection is blocked, and the red-team report card is shown.

## Safety and Responsible Testing

- All testing runs on **simulated or public data** inside a sandbox.
- No real-world systems are accessed, attacked or disrupted.
- No malware, exploits or harmful payloads are developed.
- Response actions execute **only in the simulator**, and critical actions require human approval.
- Any confidential material provided by the organisers is used only for the hackathon.

## Datasets and Third-Party Tools

Disclosed as required by the hackathon rules:

- **Datasets:** CICIDS2017, OTRF/Mordor security datasets, MITRE ATT&CK knowledge base, plus a simulated attack we scripted
- **APIs:** Anthropic Claude API
- **Libraries:** FastAPI, scikit-learn, NetworkX, React, React Flow / Cytoscape



## Impact

- **SOC analysts:** less noise and burnout, faster and better-informed decisions
- **Organisations:** clear rupee cost of risk, safer automation, regulator-ready reporting
- **Insurers and brokers:** structured, evidence-backed incident data and claim-ready files

Metrics such as alerts reduced, mean time to respond and injection tests blocked are measured on our demo data and reported as-is.

## Team

| Name | Role |
|---|---|
| **Niveditha** | Team Lead, AI & ML: agents, scoring, RAG, injection defence |
| **Nithin Gowda** | Backend & Frontend: pipeline, dashboard, attack graph, demo |

## License

Created for the CyberForge Hackathon. Add a license of your choice (for example MIT) before publishing.
