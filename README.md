# Intelligent-Drug-Repurposing-Platform-RepurposeAI
# 🧬 RepurposeAI — Drug Repurposing Intelligence Platform

> **Autonomous AI research platform that orchestrates multi-domain pharmaceutical intelligence to discover drug repurposing opportunities in real time.**

[![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-3.0-000000?logo=flask)](https://flask.palletsprojects.com)
[![Claude AI](https://img.shields.io/badge/Claude-AI%20Synthesis-CC785C?logo=anthropic)](https://console.anthropic.com)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

---

## 📌 What Is This?

RepurposeAI is an intelligent research orchestration platform that accepts a molecule name and autonomously investigates its repurposing potential across **four independent research domains** simultaneously — clinical trials, patent landscape, market data, and regulatory filings. It then synthesizes cross-domain insights using Claude AI into a single traceable innovation report.

**The core insight:** No single database tells the full story. A drug may have strong clinical evidence but a crowded patent space, or high market usage but no active trials — these contradictions are invisible unless you look at all domains at once. RepurposeAI finds those gaps.

---

## 🎯 Core Capabilities

| Capability | Description |
|---|---|
| **Query Decomposition** | Single molecule input → 5 parallel domain-specific subtasks |
| **Multi-Source Retrieval** | Live data from ClinicalTrials.gov, PubChem, OpenFDA, PubMed |
| **Context Continuity** | Clinical findings enrich downstream market and regulatory queries |
| **Contradiction Detection** | Flags cross-domain conflicts (e.g. strong science + no commercialisation) |
| **AI Synthesis** | Claude generates a structured JSON report with traceable citations |
| **Confidence Scoring** | Evidence-weighted 0–100 score across all 4 domains |
| **Follow-up Q&A** | Ask natural language questions about the generated report |
| **Batch Analysis** | Compare up to 5 molecules simultaneously, ranked by confidence |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         USER INPUT: molecule name                    │
└────────────────────────────┬────────────────────────────────────────┘
                             │
                    ┌────────▼────────┐
                    │   Orchestrator   │  app.py — coordinates all stages
                    │   (Flask API)    │
                    └──────┬──────────┘
                           │ Stage 1: Parallel dispatch
          ┌────────────────┼────────────────────────────┐
          │                │                │            │            │
   ┌──────▼─────┐  ┌───────▼──────┐  ┌─────▼──────┐  ┌─▼──────────┐  ┌─▼────────┐
   │  Clinical  │  │   Patents    │  │   Market   │  │ Regulatory │  │  PubMed  │
   │  Agent     │  │   Agent      │  │   Agent    │  │   Agent    │  │  Agent   │
   │clinical.py │  │ patents.py   │  │  market.py │  │regulatory.py│  │pubmed.py │
   │            │  │              │  │            │  │            │  │          │
   │ ClinTrials │  │   PubChem    │  │  OpenFDA   │  │  OpenFDA   │  │  NCBI    │
   │    .gov    │  │ Google Pat.  │  │  NDC + AE  │  │  Labels    │  │ PubMed   │
   └──────┬─────┘  └───────┬──────┘  └─────┬──────┘  └─┬──────────┘  └─┬────────┘
          │                │                │            │               │
          └────────────────┴────────────────┴────────────┴───────────────┘
                                            │
                              Stage 2: Context extraction
                           ┌────────────────▼────────────────┐
                           │        context_memory.py         │
                           │  Extract clinical signals →      │
                           │  Enrich downstream queries       │
                           └────────────────┬────────────────┘
                                            │
                              Stage 3: Cross-domain synthesis
                           ┌────────────────▼────────────────┐
                           │          synthesizer.py          │
                           │   Claude AI — structured JSON    │
                           │   report with citations          │
                           └────────────────┬────────────────┘
                                            │
                     ┌──────────────────────┼──────────────────────┐
                     │                      │                       │
          ┌──────────▼────────┐  ┌──────────▼────────┐  ┌─────────▼────────┐
          │    scorer.py      │  │  contradiction.py  │  │   followup.py    │
          │  Confidence 0-100 │  │  Cross-domain flag │  │  Q&A on report   │
          └───────────────────┘  └────────────────────┘  └──────────────────┘
                                            │
                           ┌────────────────▼────────────────┐
                           │     Unified Innovation Report    │
                           │  + Source citations + Confidence │
                           └─────────────────────────────────┘
```

---

## 📁 Project Structure

```
repurposeai/
├── backend/
│   ├── app.py                     ← Flask API server + pipeline orchestrator
│   └── modules/
│       ├── __init__.py
│       ├── clinical.py            ← ClinicalTrials.gov agent
│       ├── patents.py             ← PubChem + Google Patents agent
│       ├── market.py              ← OpenFDA NDC + adverse events agent
│       ├── regulatory.py          ← FDA drug labels agent
│       ├── pubmed.py              ← PubMed repurposing literature agent
│       ├── synthesizer.py         ← Claude AI synthesis engine
│       ├── scorer.py              ← Evidence-weighted confidence scorer
│       ├── contradiction.py       ← Cross-domain conflict detector
│       ├── context_memory.py      ← Context continuity layer
│       └── followup.py            ← Interactive Q&A on reports
├── frontend/
│   ├── templates/
│   │   └── index.html             ← Single-page application
│   └── static/
│       ├── css/style.css          ← All styling
│       └── js/main.js             ← Frontend logic
├── .env                           ← API keys (never commit this)
├── requirements.txt               ← Python dependencies
├── run.sh                         ← Mac/Linux one-click starter
└── run.bat                        ← Windows one-click starter
```

---

## 🚀 Quick Start (3 Steps)

### Step 1 — Get your API key (free)

Go to [console.anthropic.com](https://console.anthropic.com) → Sign up → Create API key → Copy it.

> **No key?** The app still works — it fetches all real data but shows a placeholder for the AI synthesis section.

### Step 2 — Configure `.env`

```env
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxx
```

### Step 3 — Run

**Mac / Linux:**
```bash
chmod +x run.sh
./run.sh
```

**Windows:**
```
Double-click run.bat
```

**Manual:**
```bash
pip install -r requirements.txt
cd backend
python app.py
```

Open **http://localhost:5000** in your browser.

---

## 🔬 API Reference

All endpoints accept and return JSON.

### `POST /analyze`
Run the full pipeline on a single molecule.

```json
// Request
{ "molecule": "metformin" }

// Response
{
  "molecule": "metformin",
  "clinical": { "total_found": 42, "trials": [...] },
  "patents": { "total_patents": 3, "compound_info": {...} },
  "market": { "products_found": 5, "adverse_event_reports": 84321 },
  "regulatory": { "approvals": [...], "warnings": [...] },
  "pubmed": { "total_found": 120, "papers": [...] },
  "report": { "executive_summary": "...", "strategic_recommendation": {...} },
  "confidence": { "total": 82, "label": "High confidence", "breakdown": {...} },
  "contradictions": [...],
  "elapsed_seconds": 4.2
}
```

### `POST /compare`
Side-by-side analysis of two molecules.

```json
{ "molecule1": "aspirin", "molecule2": "ibuprofen" }
```

### `POST /batch`
Analyze up to 5 molecules, returned sorted by confidence score.

```json
{ "molecules": ["metformin", "sildenafil", "thalidomide"] }
```

### `POST /followup`
Ask a natural language question about a previously generated report.

```json
{
  "question": "What are the key regulatory risks?",
  "context": { "molecule": "metformin", "report": {...} }
}
```

### `GET /health`
Returns API key status and server health.

---

## 🧩 Module Deep-Dive

### `clinical.py` — ClinicalTrials.gov Agent
Queries the ClinicalTrials.gov v2 API for all studies where the molecule appears as an intervention. Returns trial ID, status (recruiting/completed), phase, conditions, and sponsor for up to 10 trials.

**Source:** `https://clinicaltrials.gov/api/v2/studies`
**Auth:** None required

---

### `patents.py` — PubChem Patent Agent
Resolves the molecule to a PubChem CID, retrieves its IUPAC name, molecular formula, and weight, then fetches associated patent IDs. Links each patent to Google Patents for full-text review.

**Sources:** PubChem REST API, Google Patents
**Auth:** None required

---

### `market.py` — OpenFDA Market Agent
Searches the FDA NDC database for approved products containing the molecule (brand names, manufacturers, dosage forms) and queries the FDA adverse event database as a proxy for real-world usage volume.

**Sources:** `api.fda.gov/drug/ndc.json`, `api.fda.gov/drug/event.json`
**Auth:** None required

---

### `regulatory.py` — FDA Label Agent
Fetches structured drug label data: approved application numbers, current indications and usage text, warnings, and contraindications from the OpenFDA label endpoint.

**Source:** `api.fda.gov/drug/label.json`
**Auth:** None required

---

### `pubmed.py` — Scientific Literature Agent
Searches PubMed for papers on drug repurposing or new indications for the molecule. Returns title, authors, journal, year, and direct PubMed links for up to 8 papers.

**Source:** NCBI E-utilities (esearch + esummary)
**Auth:** None required

---

### `context_memory.py` — Context Continuity Layer
After clinical data is retrieved, this module extracts key signals (conditions being investigated, whether active/late-phase trials exist) and builds an enriched context string. This is passed to the AI synthesizer so cross-domain reasoning is grounded in the actual clinical findings — not generic prompting.

```
Clinical: "Found trials for Type 2 Diabetes, CKD, Cancer"
         ↓
Context: "Conditions found: Type 2 Diabetes, CKD, Cancer.
          Has recruiting trials: True. Has late-phase: True."
         ↓
Synthesizer receives enriched cross-domain context
```

---

### `synthesizer.py` — Claude AI Synthesis Engine
Sends all domain data plus the cross-domain context to Claude. Instructs the model to return a structured JSON report covering executive summary, unmet needs, pipeline status, patent landscape, market potential, and a strategic recommendation with PURSUE / INVESTIGATE FURTHER / LOW PRIORITY verdict.

Falls back to a structured mock report if no API key is configured, so raw data is always displayed.

---

### `scorer.py` — Confidence Scoring Engine
Evidence-weighted scoring across four domains (out of 100):

| Domain | Max | Key signals |
|---|---|---|
| Clinical | 35 | Trial count, recruiting status, late-phase presence |
| Patents | 25 | Compound identification, patent density |
| Market | 25 | Products listed, adverse event volume |
| Regulatory | 15 | Approvals count, indications, warnings |

Labels: **High confidence** (≥75) · **Moderate** (≥50) · **Low** (≥25) · **Insufficient data** (<25)

---

### `contradiction.py` — Conflict Detection Engine
After synthesis, this module compares signals across domains and flags inconsistencies that would not be visible from any single source:

| Flag type | Example |
|---|---|
| `CLINICAL vs MARKET` | Late-phase trials exist but near-zero market usage |
| `MARKET vs CLINICAL` | High market usage but no active recruiting trials |
| `REGULATORY vs STRATEGY` | AI says PURSUE but FDA label has safety warnings |
| `PIPELINE GAP` | Zero trials but high adverse event volume |
| `PATENT vs STRATEGY` | AI says PURSUE but 8+ patents found |
| `REGULATORY vs CLINICAL` | Approved drug but all trials still early-phase |

---

## 📊 Data Sources

| Domain | Source | API Endpoint | Key |
|---|---|---|---|
| Clinical Trials | ClinicalTrials.gov | `clinicaltrials.gov/api/v2/studies` | None |
| Patent Data | PubChem NCBI | `pubchem.ncbi.nlm.nih.gov/rest/pug` | None |
| Market Data | OpenFDA NDC | `api.fda.gov/drug/ndc.json` | None |
| Adverse Events | OpenFDA FAERS | `api.fda.gov/drug/event.json` | None |
| Drug Labels | OpenFDA Labels | `api.fda.gov/drug/label.json` | None |
| Literature | PubMed NCBI | `eutils.ncbi.nlm.nih.gov/entrez` | None |
| AI Synthesis | Anthropic Claude | `anthropic.com` | Required |

All data sources except Claude AI are free and require no authentication. The platform can run in demo mode without an API key, displaying all real data with a placeholder for the AI synthesis section.

---

## 🧪 Demo Molecules

| Molecule | Why it's interesting |
|---|---|
| **Metformin** | Active cancer repurposing research; rich clinical + market data |
| **Aspirin** | Huge historical dataset; well-documented repurposing for cardiology |
| **Sildenafil** | Famous repurposing story (cardiovascular → erectile dysfunction) |
| **Thalidomide** | Controversial rehabilitation; repurposed for multiple myeloma |
| **Ibuprofen** | Rich regulatory and adverse event data; Alzheimer's repurposing interest |

---

## ⚙️ Configuration

All configuration lives in `.env`:

```env
# Required for AI synthesis
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxx

# Alternative: OpenRouter (supports multiple models)
OPENROUTER_API_KEY=sk-or-v1-xxxxxxxxxxxxxxxxx
```

> ⚠️ Never commit `.env` to version control. It is listed in `.gitignore`.

---

## 🛠️ Development Setup

```bash
# Clone the repository
git clone https://github.com/yourname/repurposeai.git
cd repurposeai

# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate          # Mac/Linux
venv\Scripts\activate.bat         # Windows

# Install dependencies
pip install -r requirements.txt

# Set up environment
cp .env.example .env
# Edit .env and add your API key

# Run development server
cd backend
python app.py
```

---

## 🔒 Security Notes

- API keys are loaded from `.env` via `python-dotenv` and never exposed to the frontend
- Molecule input is sanitized (max 100 characters, stripped whitespace)
- All external API calls use timeouts (15–40 seconds) to prevent hanging
- CORS is enabled for local development; restrict in production

---

## 🗺️ Roadmap

- [ ] PDF export of innovation reports
- [ ] Persistent report history with SQLite
- [ ] Drug–drug interaction cross-referencing
- [ ] Structure-based search (SMILES / InChI input)
- [ ] Email alerts for newly recruiting trials
- [ ] Integration with DrugBank and ChEMBL

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🏆 Acknowledgements

Built for the **Developer Student Community Hackathon 2025**.

Data provided by:
- [ClinicalTrials.gov](https://clinicaltrials.gov) — U.S. National Library of Medicine
- [PubChem](https://pubchem.ncbi.nlm.nih.gov) — NCBI / NIH
- [OpenFDA](https://open.fda.gov) — U.S. Food and Drug Administration
- [PubMed](https://pubmed.ncbi.nlm.nih.gov) — NCBI / NIH
- [Anthropic Claude](https://anthropic.com) — AI synthesis

---

<p align="center">
  <strong>RepurposeAI</strong> — turning fragmented pharmaceutical data into traceable innovation intelligence.
</p>
