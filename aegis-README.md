# AEGIS — AI Evidence & Governance Integrity System

> **AI governance fails at the enforcement layer — not the policy layer.**
> AEGIS is the bridge.

[![IAPP AIGP Aligned](https://img.shields.io/badge/IAPP-AIGP%20v2.1%20Aligned-00d4c8?style=flat-square)](https://iapp.org/certify/aigp/)
[![EU AI Act](https://img.shields.io/badge/EU%20AI%20Act-Articles%209--17%20%C2%B7%20Annex%20IV-a78bfa?style=flat-square)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689)
[![SR 11-7](https://img.shields.io/badge/SR%2011--7-Model%20Risk%20Management-f59e0b?style=flat-square)](https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm)
[![NIST AI RMF](https://img.shields.io/badge/NIST-AI%20RMF%20GOVERN%201.4-10b981?style=flat-square)](https://airc.nist.gov/RMF)
[![GCP Cloud Run](https://img.shields.io/badge/GCP-Cloud%20Run%20%C2%B7%20Always%20Free-4285F4?style=flat-square)](https://cloud.google.com/run)
[![License: MIT](https://img.shields.io/badge/License-MIT-white?style=flat-square)](LICENSE)

---

## The problem this solves

**96% of financial institutions have AI governance policies. Only 53% have turned them into technical controls.**

That 43-point gap is where regulatory exposure lives. A firm with a policy document and no enforcement mechanism cannot answer a regulator's simplest question: *"Show me the audit trail for this model."*

This is not a hypothetical. The EU AI Act's full enforcement deadline is **August 2026**. The FCA's model risk rules have been in effect since **May 2024**. And more than half of organisations have not completed the minimum prerequisite for compliance: knowing what AI systems they operate.

AEGIS was designed to answer that question — automatically, completely, in plain language, in under 4 minutes.

---

## What AEGIS is

AEGIS is an **open-source reference implementation** of an AI governance enforcement platform for regulated financial institutions. It operationalises three foundational governance policies into a live, agentic system that:

- **Enforces** mandatory documentation standards at the point of intake — no incomplete submission reaches classification
- **Classifies** every AI system by EU AI Act risk tier (Prohibited / High-Risk / Limited / Minimal) using Annex III logic, with a plain-language explanation of why
- **Tracks** human oversight — who reviewed, what they verified, when, linked to the specific artifact version
- **Assembles** a complete, regulator-ready Annex IV evidence bundle on demand — the kind that answers an FCA Section 165 notice
- **Reports** five governance KPIs in real time: audit trail completeness, EU AI Act classification coverage, policy-to-control translation rate, human oversight coverage, and mean time to compliance evidence

**Every decision the system makes is explained in plain language. No black boxes. No unexplained classifications.**

---

## Live demo

🔗 **[View the design documentation →](https://your-github-username.github.io/aegis)**

The design doc site walks through the full system: problem statement with cited research, anchor client profile, user persona, customer journey map, C4 architecture diagrams (Levels 1–3), five Architecture Decision Records, and a fully scripted simulation of four governance scenarios — all running in the browser, zero API cost.

---

## The four scenarios

The simulation demonstrates AEGIS across four real governance situations:

| Scenario | What happens | Time |
|---|---|---|
| **Regulator audit request** | FCA Section 165 arrives. AEGIS assembles the complete Annex IV evidence bundle from Firestore — intake record, risk classification, oversight log, version history, policy citations. | < 4 min |
| **High-risk model intake** | New model submitted. 12 mandatory fields validated. EU AI Act Annex III §5(b) classification at 94% confidence. Oversight requirements set. Audit record written. | < 90s |
| **Incomplete intake — blocked** | 3 mandatory fields missing. Intake Agent blocks at the gate. No downstream agent fires. Field-level rejection with plain-language explanation. | < 5s |
| **HITL escalation** | Classification confidence 0.61 — below the 0.80 autonomous threshold. System cannot decide. Compliance Officer notified with full brief. One-click resolve or escalate to Risk Committee. | < 20s + human |

---

## Architecture

AEGIS is a genuine four-layer agentic system — not a workflow tool with AI bolted on.

```
L1  Presentation & Experience    HTML/CSS/JS · GitHub Pages (docs) · Cloud Run (app)
L2  Agent Orchestration          Python 3.12 · FastAPI · 5 specialist agents
L3  MLOps & Intelligence         Gemini 2.0 Flash · Policy-as-Context · Prompt registry
L4  Infrastructure               GCP Cloud Run · Firestore · Cloud Storage · £0 ongoing
```

### The five agents

| Agent | Responsibility | Policy enforced |
|---|---|---|
| **Intake Agent** | Validates all mandatory fields. Blocks incomplete submissions before any downstream processing. | Model Documentation Minimum Standards §5 · EU AI Act Annex IV |
| **Classification Agent** | Assigns EU AI Act risk tier using Annex III logic via Gemini 2.0 Flash. Returns tier + plain-language rationale. | EU AI Act Articles 6–7 · Annex III |
| **Oversight Agent** | Determines reviewer requirements by risk tier. Approval is a system event — timestamped, reviewer-identified, immutable. | Human Oversight Documentation Standard · EU AI Act Article 14 |
| **Audit Agent** | Assembles complete Annex IV evidence bundle on demand. The agent that answers the FCA's Section 165 notice. | EU AI Act Annex IV · FCA SYSC · SR 11-7 |
| **Explainability Agent** | Wraps every preceding decision in a plain-language "why" — what was decided, which policy clause triggered it, what the human must do next. | IAPP AIGP Explainability Principle · EU AI Act Article 13 |

### Architecture Decision Records

Five ADRs document every major design choice — see the [Architecture page](https://your-github-username.github.io/aegis/architecture.html) for the full records with options considered and consequences:

| ADR | Decision | Rationale |
|---|---|---|
| ADR-001 | Cloud Run over App Engine or Cloud Functions | Serverless, zero idle cost, 2M requests/month free, Docker-portable |
| ADR-002 | FastAPI over Flask or Django | Native Pydantic validation, auto OpenAPI docs, async support |
| ADR-003 | Server-side Gemini API key | Zero user friction, Google rate limiting as cost control, key never in browser |
| ADR-004 | Policy-as-Context over vector database | Gemini's 1M token context window fits all three policies; no chunk boundary errors; policy changes take effect immediately |
| ADR-005 | Firestore over Cloud SQL | Schema-flexible for evolving governance records; always-free tier; no VPC connector cost |

---

## The three governing policies

AEGIS enforces three governance policies authored under IAPP AIGP guidance. All are included in this repository under `/policies`.

| Policy | ID | Scope |
|---|---|---|
| AI Content Working Standard | `AI-CWS-001` | Governs use of AI tools in producing documentation — disclosure, review obligations, prohibited uses |
| Human Oversight Documentation Standard | `HO-DOC-001` | Defines human oversight requirements for AI-assisted content and model output documentation |
| Model Documentation Minimum Standards | `DOC-GOV-001` | Mandatory fields, approval workflow, and publication gate for all AI model documentation |

All three policies are subservient to existing organisational compliance obligations and are aligned with SR 11-7, NIST AI RMF, and EU AI Act requirements.

---

## Regulatory mapping

| Requirement | AEGIS control | Evidence produced |
|---|---|---|
| EU AI Act Art. 9 — Risk management | Classification Agent assigns Annex III risk tier | Risk classification record · Firestore |
| EU AI Act Art. 11 + Annex IV — Technical documentation | Intake Agent enforces all Annex IV mandatory fields | Completed intake record · timestamped |
| EU AI Act Art. 13 — Transparency | Explainability Agent wraps every decision | Plain-language decision record · every event |
| EU AI Act Art. 14 — Human oversight | Oversight Agent gates approval by named, qualified reviewer | Oversight record · reviewer identity · timestamp |
| SR 11-7 §IV — Model documentation | Intake Agent enforces SR 11-7-aligned mandatory fields | Model documentation with all required fields |
| NIST AI RMF GOVERN 1.4 — Accountability | Every agent event timestamped and Firestore-persisted | Live compliance dashboard · 5 KPIs |
| IAPP AIGP — Human accountability | Oversight Agent requires named reviewer at every gate | Named reviewer linked to specific artifact version |
| FCA / PRA model risk rules (May 2024) | Audit Agent assembles complete evidence chain on demand | Annex IV bundle · regulator-ready |

---

## The five governance KPIs

The dashboard tracks these metrics in real time:

| KPI | Target | What it measures |
|---|---|---|
| **Audit Trail Completeness** | 100% | AI artifacts with a full, reconstructable evidence chain in Firestore |
| **EU AI Act Risk Classification Coverage** | 100% | AI systems classified by Annex III risk tier |
| **Policy-to-Control Translation Rate** | 100% | Policy requirements with a corresponding technical enforcement control — closes the 43pt industry gap |
| **Human Oversight Coverage** | 100% | High-risk AI outputs with a recorded, named human review before publication |
| **Mean Time to Compliance Evidence (MTCE)** | < 24h | Submission to regulator-ready audit package — current: 4m 12s vs 9.3 days manual |

---

## Tech stack

| Component | Technology | Cost |
|---|---|---|
| Frontend | HTML5 · CSS3 · Vanilla JS | Free · GitHub Pages |
| API backend | Python 3.12 · FastAPI · Pydantic | Free · Cloud Run always-free |
| Agent framework | Custom Python — 5 typed agent functions | Free · open source |
| LLM | Gemini 2.0 Flash | Free · Google AI Studio free tier |
| Policy store | Google Cloud Storage | Free · 5GB always-free |
| Audit database | Cloud Firestore | Free · 50K reads/day always-free |
| Container host | Google Cloud Run | Free · 2M requests/month always-free |
| Design doc site | GitHub Pages | Free · always |
| **Total ongoing cost** | | **£0** |

---

## Repository structure

```
aegis/
├── docs/                     # Design documentation site (GitHub Pages)
│   ├── index.html            # Overview — problem, anchor client, persona, journey map
│   ├── architecture.html     # C4 diagrams (L1–L3), ADRs, tech stack, regulatory mapping
│   ├── simulation.html       # Four scripted governance scenarios
│   └── dashboard.html        # Five KPIs, model registry, audit feed, scorecard
│
├── app/                      # Functional application
│   ├── main.py               # FastAPI entry point
│   ├── orchestrator.py       # Master orchestrator agent
│   ├── agents/
│   │   ├── intake.py         # Intake Agent — field validation
│   │   ├── classification.py # Classification Agent — EU AI Act Annex III
│   │   ├── oversight.py      # Oversight Agent — approval gate
│   │   ├── audit.py          # Audit Agent — evidence assembly
│   │   └── explainability.py # Explainability Agent — plain-language wrapper
│   ├── models/               # Pydantic schemas for all agent I/O
│   ├── prompts/              # Prompt registry — versioned, auditable
│   └── Dockerfile            # Cloud Run deployment
│
├── policies/                 # The three governing policy documents
│   ├── AI_Content_Working_Standard_v0_1.docx
│   ├── Human_Oversight_Documentation_Standard_v0_1.docx
│   └── Model_Documentation_Minimum_Standards_v0_1.docx
│
├── .github/
│   └── workflows/
│       └── deploy.yml        # GitHub Actions — deploy docs to GitHub Pages
│
└── README.md
```

---

## Running the design documentation site locally

No build step. No dependencies. Open the file.

```bash
git clone https://github.com/your-username/aegis.git
cd aegis/docs
open index.html   # macOS
# or: python -m http.server 8000 — then open localhost:8000
```

---

## Running the functional app locally

**Prerequisites:** Python 3.12+, a Google AI Studio API key (free at [aistudio.google.com](https://aistudio.google.com)), GCP project with Firestore enabled.

```bash
# Clone and install
git clone https://github.com/your-username/aegis.git
cd aegis/app
pip install -r requirements.txt

# Set environment variables
export GEMINI_API_KEY="your-google-ai-studio-key"
export GCP_PROJECT_ID="your-gcp-project-id"
export FIRESTORE_COLLECTION="aegis_audit_log"

# Run
uvicorn main:app --reload --port 8000
```

The FastAPI app exposes:
- `POST /intake` — Submit a new AI model for governance processing
- `GET /audit/{model_id}` — Retrieve the complete audit package for a model
- `GET /registry` — List all models in the governance registry
- `GET /dashboard` — Live governance metrics
- `GET /docs` — Auto-generated OpenAPI documentation (FastAPI built-in)

---

## Deploying to Google Cloud Run

```bash
# Build and push container
gcloud builds submit --tag gcr.io/YOUR_PROJECT/aegis-app

# Deploy to Cloud Run
gcloud run deploy aegis-app \
  --image gcr.io/YOUR_PROJECT/aegis-app \
  --platform managed \
  --region europe-west2 \
  --allow-unauthenticated \
  --set-env-vars GEMINI_API_KEY=your-key,GCP_PROJECT_ID=your-project
```

Runs within the Cloud Run always-free tier at portfolio traffic volumes. No ongoing cost.

---

## Anchor client

AEGIS was designed around **Meridian Capital Group** — a fictional but architecturally realistic reference institution: a UK-headquartered asset manager with operations in Frankfurt and New York, regulated by the FCA, BaFin, and operating under SEC oversight, with £180B AUM and 34 AI models in production across risk, trading, and client reporting functions.

The design documentation uses Meridian's governance situation — including a real trigger scenario (an FCA Section 165 request with no retrievable audit trail) — to ground every architectural decision in a concrete, credible financial services context.

---

## About the author

AEGIS was designed and built by an **IAPP AI Governance Professional (AIGP)**-certified practitioner with experience in technical documentation governance within financial services environments, including ML model documentation for trading-floor-adjacent systems.

This project is a portfolio demonstration of:
- Designing and implementing an AI governance enforcement system from first principles
- Authoring three governance policies aligned to SR 11-7, NIST AI RMF, and EU AI Act
- Applying the C4 architecture model and Architecture Decision Records to a governance system
- Building a genuinely agentic, four-layer system with policy-as-context and explainability at every decision point
- Deploying production-grade tooling on zero infrastructure budget

---

## Contributing

Issues and pull requests welcome. Areas where contributions would be most valuable:

- Additional EU AI Act risk classification test cases
- Prompt engineering improvements for the Classification Agent
- Additional regulatory framework mappings (MAS, APRA, OCC)
- Integration examples for Confluence, Jira, or ServiceNow
- Unit tests for all five agent functions

---

## Licence

MIT. Use it, fork it, build on it. If you use AEGIS in a real governance program, a GitHub star and a note in the issues would be appreciated — it helps evidence real-world adoption.

---

## References

- ADR.org / "From Principles to Practice: A Benchmark Study in AI Governance" (2026)
- EU AI Act — Regulation (EU) 2024/1689 — Articles 9–17, Annex III, Annex IV
- Federal Reserve SR 11-7 — Guidance on Model Risk Management (2011)
- NIST AI Risk Management Framework 1.0 (2023)
- IAPP AI Governance Professional Body of Knowledge v2.1 (February 2026)
- FCA / Bank of England — AI in Financial Services joint research (November 2024)
- IBM Cost of a Data Breach Report (2025)
- Cloud Security Alliance — EU AI Act High-Risk Deadline: Enterprise Readiness Gap (March 2026)
- Aveni.ai — AI Governance in UK Financial Services (May 2026)
- Wisconsin Bankers Association — The AI Governance Gap (March 2026)

---

*AEGIS is a portfolio and reference implementation. It is not legal or compliance advice. All regulatory citations are provided for architectural context. Consult qualified legal and compliance professionals before implementing governance controls in a regulated environment.*
