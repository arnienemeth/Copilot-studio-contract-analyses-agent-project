# 🧠 Contract Intelligence — Azure AI Multi-Agent System + Copilot Studio Agent

> An agentic AI system that reads contracts (PDF/DOCX), extracts structured data, classifies by total contract value, performs weighted risk assessment, and generates interactive dashboards — all powered by Azure OpenAI GPT-4o-mini and Azure Document Intelligence. Extended with a Microsoft Copilot Studio conversational agent for business user access.

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![Azure OpenAI](https://img.shields.io/badge/Azure_OpenAI-GPT--4o--mini-0078D4?logo=microsoftazure&logoColor=white)
![Azure Document Intelligence](https://img.shields.io/badge/Azure-Document_Intelligence-0078D4?logo=microsoftazure&logoColor=white)
![Azure Blob Storage](https://img.shields.io/badge/Azure-Blob_Storage-0078D4?logo=microsoftazure&logoColor=white)
![Copilot Studio](https://img.shields.io/badge/Microsoft-Copilot_Studio-742774?logo=microsoft&logoColor=white)
![Power Automate](https://img.shields.io/badge/Power-Automate-0066FF?logo=microsoft&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📸 Screenshots

### Interactive Dashboard
The system generates a fully interactive HTML dashboard with risk distribution charts, TCV breakdown, a filterable contracts table, and detailed risk findings per contract.

> After running the pipeline, open `output/dashboard.html` in your browser.

### Natural Language Query Mode
Ask questions about your contracts in plain English:
```
📝 Your question: Show me all high-risk contracts over 500k
📝 Your question: Which vendor has the worst liability terms?
📝 Your question: Compare termination clauses across all contracts
```

### High-Risk Email Alerts
Automatically flags high-risk contracts and generates a formatted HTML alert report with risk summaries and top findings.

### Copilot Studio Agent
A conversational agent built in Microsoft Copilot Studio providing business users with a natural language chat interface to the same 12 contracts — deployable to Microsoft Teams.

---

## 🎯 What Problem Does This Solve?

Organizations manage hundreds of contracts across vendors, clients, and departments. Manual review is slow, inconsistent, and expensive. Key risks — unfavorable termination clauses, low liability caps, vendor-friendly IP ownership, auto-renewal traps — often go unnoticed until it's too late.

**Contract Intelligence automates this entire workflow:**

| Manual Process | This System |
|---|---|
| Lawyer reads 20-page contract (~45 min) | AI extracts all data in ~15 seconds |
| Risk depends on who reviews it | Consistent weighted scoring (0-100) every time |
| Excel tracker updated monthly | Real-time dashboard with filters and charts |
| "I think this clause is risky" | Explainable findings: "+20 pts: No termination for convenience" |
| Email chains about flagged contracts | Automated high-risk alerts |
| Lawyers needed to answer basic questions | Business users query contracts via chat in Teams |

---

## 🏗️ Full Solution Architecture

The project is built in **two complementary layers** — a code-first Python pipeline for heavy processing, and a low-code Copilot Studio agent for business user access:

```
┌─────────────────────────────────────────────────────────────────┐
│                    FULL SOLUTION ARCHITECTURE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   CODE-FIRST LAYER (Python + Azure)                              │
│                                                                  │
│   📄 PDF/DOCX Input                                              │
│        │                                                         │
│   ┌────▼────────┐                                                │
│   │ ORCHESTRATOR │ — coordinates pipeline, handles retries       │
│   └────┬────────┘                                                │
│        │                                                         │
│   ┌────▼──────┐  ┌────────────┐  ┌──────────┐                  │
│   │ INGESTION │→ │ EXTRACTION │→ │   RISK   │                  │
│   │   AGENT   │  │   AGENT    │  │  AGENT   │                  │
│   │ Azure Doc │  │ GPT-4o-mini│  │ Rules +  │                  │
│   │ Intel +   │  │ Structured │  │ LLM deep │                  │
│   │ Fallback  │  │ JSON output│  │ analysis │                  │
│   └───────────┘  └────────────┘  └────┬─────┘                  │
│                                        │                         │
│              ┌─────────────────────────┤                         │
│              │                         │                         │
│   ┌──────────▼──────┐    ┌────────────▼──────┐                 │
│   │  📊 Dashboard    │    │   ☁️ Azure Blob    │                 │
│   │  HTML + Charts  │    │   risk-low/medium/ │                 │
│   │  JSON + CSV     │    │   risk-high/       │                 │
│   └─────────────────┘    └───────────────────┘                 │
│                                        │                         │
│  ─────────────────────────────────────────────────────────────  │
│                                        │                         │
│   LOW-CODE LAYER (Microsoft Copilot Studio)                      │
│                                        ▼                         │
│                     ┌──────────────────────────┐                │
│                     │   CONTRACT REVIEW AGENT   │                │
│                     │                           │                │
│                     │  📚 Knowledge Sources:    │                │
│                     │    12 Contract PDFs/DOCX  │                │
│                     │    Analysis Report JSON   │                │
│                     │                           │                │
│                     │  🗂️ Custom Topics:         │                │
│                     │    Contract Risk Summary  │                │
│                     │    TCV Analysis           │                │
│                     │    Clause Comparison      │                │
│                     │    Quick Summary          │                │
│                     │    Escalate to Legal      │                │
│                     │                           │                │
│                     │  ⚡ Agent Flows:           │                │
│                     │    Log Query → Dataverse  │                │
│                     │    Email Risk Alert       │                │
│                     └────────────┬─────────────┘                │
│                                  │                               │
│                                  ▼                               │
│                     ┌────────────────────────┐                  │
│                     │    END USERS            │                  │
│                     │  Microsoft Teams / Web  │                  │
│                     └────────────────────────┘                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Why Two Layers?

| Dimension | Python + Azure Pipeline | Copilot Studio Agent |
|---|---|---|
| **Target user** | Developer / data engineer | Business analyst / procurement team |
| **Input** | Runs on all 12 contracts at once | Answers questions one at a time |
| **Customization** | Unlimited (pure code) | Within platform features |
| **Risk scoring** | Custom weighted model (0-100) | AI-generated from indexed documents |
| **Output** | JSON, CSV, HTML dashboard | Conversational chat answers |
| **Deployment** | CLI / scripts | Microsoft Teams / web channel |
| **Best for** | Nightly batch processing | Daily business user Q&A |

**In a real enterprise:** the Python pipeline runs nightly, processes new contracts, and uploads results. Business users then query everything through the Copilot Studio agent in Teams — no code required on their end.

---

## 🤖 Copilot Studio Agent — Low-Code Layer

### What Was Built

A full conversational agent in Microsoft Copilot Studio using the same 12 vendor contracts as knowledge sources. Business users can ask natural language questions and get instant answers — no technical expertise required.

### Topics (Conversation Flows)

| Topic | Trigger Phrases | What It Does |
|---|---|---|
| **Welcome / Greeting** | Hello, Hi, Start | Introduces the agent and its capabilities |
| **Contract Risk Summary** | Risk analysis, What are the risks, High risk vendors | Analyzes risk level for a specific vendor contract |
| **TCV Analysis** | Total contract value, How much, Contract value, Budget | Returns TCV, category, and duration for a contract |
| **Clause Comparison** | Compare clauses, Termination terms, Side by side | Compares a specific clause across multiple vendors |
| **Quick Summary** | Summarize, Give me an overview, Key terms | Returns a concise structured summary of any contract |
| **Escalation to Legal** | Escalate, I need legal review, Send to legal | Collects concern details and priority, prepares escalation summary |

### Agent Flows (Automation)

**Flow 1 — Log Contract Query to Dataverse**
Every query is automatically logged to a Dataverse table (`ContractAnalysesTable`) for audit purposes:
- Contract name queried
- Question text asked
- Risk level detected
- Timestamp (`utcNow()`)

**Flow 2 — Send Email Risk Alert**
When a user escalates a contract, the flow sends a formatted email via Outlook:
- To: legal team recipient
- Subject: dynamically includes risk level
- Body: contract summary and escalation reason

### Knowledge Sources

All 12 vendor contracts uploaded directly to the agent:
```
CTR-2024-001_BrightDesk_Supplies_Ltd.pdf
CTR-2024-002_Maya_Chen_Design_Studio.docx
CTR-2024-003_TechServe_Solutions_GmbH.pdf
CTR-2024-004_PixelWave_Digital_Agency.docx
CTR-2024-005_CloudMatrix_Inc.pdf
CTR-2024-006_DriveForce_Leasing_BV.docx
CTR-2024-007_ShieldNet_Cybersecurity_Ltd.pdf
CTR-2024-008_RoboFlow_Industrial_Solutions.docx
CTR-2024-009_Pinnacle_Business_Systems_AG.pdf
CTR-2024-010_ConnectAll_Telecom_plc.docx
CTR-2024-011_Apex_Digital_Transformation_Ltd.pdf
CTR-2024-012_StratoSphere_Cloud_Partners.docx
```

The `analysis_report.json` from the Azure pipeline is also uploaded — so the agent knows both the raw contract text AND the pre-computed risk analysis.

---

## ⚖️ Risk Scoring Model (Python Pipeline)

The system uses a **transparent, weighted scoring model** — not a black-box classification. Every risk finding includes the exact reason and point value.

### Risk Levels

| Score | Level | Action |
|---|---|---|
| 0-29 | 🟢 **LOW** | Standard terms, proceed normally |
| 30-59 | 🟡 **MEDIUM** | Notable concerns, review recommended |
| 60-100 | 🔴 **HIGH** | Significant risks, legal review required |

### Scoring Categories

| Category | Max Points | What It Checks |
|---|---|---|
| **Liability** | 25 | Cap below 50% of TCV, excluded consequential damages |
| **Termination** | 20 | No exit clause, 6+ month notice, high exit fees |
| **Auto-renewal** | 15 | Short notice period, uncapped price increases |
| **Data handling** | 15 | Vendor uses data for AI training, unspecified jurisdictions |
| **IP ownership** | 10 | Vendor owns all custom work, no license-back |
| **Indemnification** | 10 | One-sided (client bears all liability) |
| **Penalties** | 5 | SLA credits capped below 20%, no liquidated damages |

### Example Finding

```
⚠️  CATEGORY:    Termination
    SEVERITY:    High (+20 points)
    FINDING:     No termination for convenience clause found. Client
                 has no exit path without vendor breach.
    CLAUSE REF:  Missing section
```

---

## 💰 TCV Classification

Contracts are automatically sorted into value buckets:

| Bucket | Range |
|---|---|
| `below_100k` | < $100,000 |
| `100k_to_250k` | $100,000 — $249,999 |
| `250k_to_500k` | $250,000 — $499,999 |
| `500k_to_750k` | $500,000 — $749,999 |
| `750k_to_1m` | $750,000 — $999,999 |
| `above_1m` | $1,000,000+ |

If the contract lists monthly payments and duration, the system calculates the total automatically.

---

## 🛠️ Tech Stack

### Python Pipeline

| Component | Technology | Purpose |
|---|---|---|
| **Language** | Python 3.10+ | Core application |
| **LLM** | Azure OpenAI GPT-4o-mini | Data extraction + risk analysis |
| **Document Parsing** | Azure Document Intelligence (F0) | PDF/DOCX text extraction |
| **Cloud Storage** | Azure Blob Storage (LRS) | Organized contract storage by risk level |
| **Dashboard** | HTML + Chart.js (CDN) | Interactive visualization |
| **API Calls** | Python stdlib (`urllib`) | Zero SDK dependency for Azure APIs |
| **Local Fallback** | `pypdf` + `python-docx` | Extraction when Azure is unavailable |

### Copilot Studio Layer

| Component | Technology | Purpose |
|---|---|---|
| **Agent Platform** | Microsoft Copilot Studio | Conversational agent authoring |
| **Knowledge** | Copilot Studio file upload | Indexes 12 contracts + analysis JSON |
| **Topics** | Custom conversation flows | Structured Q&A for each use case |
| **Automation** | Power Automate (Agent flows) | Dataverse logging + Outlook email |
| **Database** | Microsoft Dataverse | Audit log of all queries |
| **Deployment** | Microsoft Teams / web | Business user access channel |

### Design Decisions (Python Pipeline)

- **No LangChain / LangGraph** — built from scratch for full control and transparency
- **No Azure SDK** — pure REST API calls using Python stdlib, reducing dependencies to near zero
- **JSON mode** — GPT-4o-mini forced to return structured JSON, validated with automatic retry logic
- **Dual extraction** — Azure Document Intelligence as primary, local libraries as automatic fallback
- **Hybrid risk scoring** — fast rule-based pre-checks combined with deep LLM analysis for nuance

---

## 💵 Cost Analysis

| Service | Pricing | Cost for 12 Contracts |
|---|---|---|
| Azure OpenAI (GPT-4o-mini) | ~$0.15/1M input, $0.60/1M output tokens | ~$0.01 |
| Document Intelligence (F0) | Free tier: 500 pages/month | $0.00 |
| Blob Storage (LRS) | ~$0.02/GB/month | ~$0.01 |
| Copilot Studio | Power Platform Developer (free) | $0.00 |
| **Total** | | **< $0.05** |

The entire project runs well within Azure's free tier and a $5 budget.

---

## 📁 Project Structure

```
contract-intelligence/
│
├── main.py                         # Entry point (run this)
├── .env.example                    # Environment config template
├── requirements.txt                # Python dependencies
│
├── agents/                         # AI Agents
│   ├── orchestrator.py             # Pipeline coordinator + report generator
│   ├── ingestion_agent.py          # Document parsing (Doc Intelligence + fallback)
│   ├── extraction_agent.py         # GPT-4o-mini structured data extraction
│   └── risk_agent.py               # Weighted risk scoring (rules + LLM)
│
├── models/
│   └── contract.py                 # Contract data model, TCV buckets, risk enums
│
├── tools/
│   ├── llm_client.py               # Azure OpenAI REST client (no SDK needed)
│   ├── document_reader.py          # Document Intelligence + local fallback
│   ├── dashboard.py                # Interactive HTML dashboard generator
│   ├── query_interface.py          # Natural language Q&A with GPT-4o-mini
│   └── email_alerts.py             # High-risk contract email alert system
│
├── storage/
│   └── blob_manager.py             # Azure Blob Storage REST client
│
├── contracts/                      # 12 test vendor contracts (PDF + DOCX)
│
└── output/                         # Generated after running the pipeline
    ├── dashboard.html              # Interactive dashboard
    ├── analysis_report.json        # Full structured results
    ├── contracts_export.csv        # Spreadsheet-ready export
    └── high_risk_alert.html        # Email alert report
```

---

## 🚀 Getting Started (Python Pipeline)

### Step 1 — Clone the Repository

```bash
git clone https://github.com/arnienemeth/contract-intelligence.git
cd contract-intelligence
```

### Step 2 — Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 3 — Set Up Azure Resources

You need 3 Azure services. All fit within the free tier.

#### 3a. Azure OpenAI Service
1. [Azure Portal](https://portal.azure.com) → Create resource → search **"Azure OpenAI"**
2. Create with **Standard S0** pricing tier
3. Go to [Azure AI Foundry](https://oai.azure.com) → Deployments → Deploy **GPT-4o-mini**
4. Copy your **Endpoint** and **KEY 1** from Resource Management → Keys and Endpoint

#### 3b. Azure Document Intelligence
1. Azure Portal → Create resource → search **"Document Intelligence"**
2. Select **Free F0** tier (500 pages/month)
3. Copy your **Endpoint** and **KEY 1**

#### 3c. Azure Blob Storage
1. Azure Portal → Create resource → search **"Storage account"**
2. Select **LRS** redundancy (cheapest option)
3. After creation, go to Containers → create 4 containers:
   - `incoming`
   - `risk-low`
   - `risk-medium`
   - `risk-high`
4. Copy the **Connection string** from Security + networking → Access keys

### Step 4 — Configure Environment

```bash
cp .env.example .env
```

Open `.env` and paste your Azure credentials:

```env
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com/
AZURE_OPENAI_KEY=your-key-here
AZURE_OPENAI_DEPLOYMENT=gpt-4o-mini
AZURE_OPENAI_API_VERSION=2024-10-21

AZURE_DOC_INTEL_ENDPOINT=https://your-resource.cognitiveservices.azure.com/
AZURE_DOC_INTEL_KEY=your-key-here

AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=...
AZURE_STORAGE_CONTAINER_INCOMING=incoming
AZURE_STORAGE_CONTAINER_RISK_LOW=risk-low
AZURE_STORAGE_CONTAINER_RISK_MEDIUM=risk-medium
AZURE_STORAGE_CONTAINER_RISK_HIGH=risk-high
```

### Step 5 — Test Connections

```bash
python main.py --test
```

Expected output:
```
🔌 TESTING AZURE CONNECTIONS
1️⃣  Azure OpenAI... ✅ Connected
2️⃣  Document Intelligence... ✅ Connected
3️⃣  Blob Storage... ✅ Connected
✅ Connection tests complete!
```

### Step 6 — Run the Full Analysis

```bash
python main.py
```

### Step 7 — Explore Results

```bash
# Open interactive dashboard
start output\dashboard.html          # Windows
open output/dashboard.html           # macOS

# Natural language query mode
python main.py --query

# Regenerate dashboard only
python main.py --dashboard
```

---

## 💬 Query Mode

```bash
python main.py --query
```

| Example Question | What It Returns |
|---|---|
| "Show me all high-risk contracts" | Lists contracts with risk score 60+ |
| "Which vendor has the worst liability terms?" | Compares liability clauses across vendors |
| "What contracts over 500k are high risk?" | Filters by TCV and risk level |
| "Summarize risks for CloudMatrix" | Detailed breakdown for a specific vendor |
| "Compare termination clauses of top 3 by TCV" | Side-by-side clause comparison |
| "What are the biggest red flags overall?" | Aggregated findings ranked by severity |

---

## 📊 Sample Console Output

```
######################################################################
  CONTRACT INTELLIGENCE PIPELINE
  Found 12 contracts to process
  Started: 2026-02-24 14:30:00
######################################################################

[1/12]
======================================================================
📋 PROCESSING: CTR-2024-001_BrightDesk_Supplies_Ltd.pdf
======================================================================

  🔍 [IngestionAgent] Extracted 2,341 words (14,892 chars)
  🧠 [ExtractionAgent] TCV: $45,000 | Vendor: BrightDesk Supplies Ltd
  ⚖️  [RiskAgent] Risk: LOW (score: 0/100) — 0 findings

...

######################################################################
  ANALYSIS COMPLETE
######################################################################

  Processed: 12 contracts  |  Failed: 0
  Total TCV: $6,102,000

  📁 TCV DISTRIBUTION
  below_100k    2    100k_to_250k   2    250k_to_500k   2
  500k_to_750k  2    750k_to_1m     2    above_1m       2

  ⚖️  RISK DISTRIBUTION
  🟢 LOW: 2    🟡 MEDIUM: 6    🔴 HIGH: 4
```

---

## 📧 Email Alerts (Optional)

```env
ALERT_EMAIL_ENABLED=true
ALERT_SMTP_SERVER=smtp.gmail.com
ALERT_SMTP_PORT=587
ALERT_SENDER_EMAIL=your.email@gmail.com
ALERT_SENDER_PASSWORD=your-app-password
ALERT_RECIPIENT_EMAIL=recipient@company.com
```

> For Gmail, use an [App Password](https://myaccount.google.com/apppasswords).

---

## ✅ What's Left to Complete (Copilot Studio)

If you're following this project as a build guide, here's the remaining checklist for the Copilot Studio layer:

- [ ] Upload all 12 contracts as Knowledge sources in the agent
- [ ] Upload `output/analysis_report.json` as an additional knowledge source
- [ ] Verify all 6 topics have correct trigger phrases and variable mappings
- [ ] Complete "Log Contract Query" agent flow (add inputs to trigger → Dataverse → respond)
- [ ] Complete "Send Contract Risk Email" agent flow (add inputs → Outlook V2 → respond)
- [ ] Connect Log flow to Contract Risk Summary topic via "Add a tool"
- [ ] Connect Email flow to Escalation to Legal topic via "Add a tool"
- [ ] Run all 8 test scenarios in the Copilot Studio test panel
- [ ] Capture portfolio screenshots (agent overview, topics list, test conversations, flow designer)
- [ ] Record a 2–3 minute demo video (Windows: Win+G)

---

## 🔮 Future Enhancements

- [ ] **Contract comparison** — diff clauses across vendors side by side
- [ ] **Expiration tracking** — 30/60/90 day alerts for upcoming renewals
- [ ] **RAG retrieval** — "How does this compare to previous contracts with this vendor?"
- [ ] **Streamlit web UI** — browser-based interface instead of CLI
- [ ] **Multi-language support** — contracts in German, French, Spanish
- [ ] **Azure Functions trigger** — auto-process contracts uploaded to Blob Storage
- [ ] **Teams deployment** — publish Copilot Studio agent to Microsoft Teams channel
- [ ] **CI/CD pipeline** — automated testing and deployment

---

## 🏆 What This Demonstrates

| Skill | Evidence |
|---|---|
| **AI Architecture** | Designed multi-agent system from scratch |
| **Cloud Engineering** | Azure OpenAI, Document Intelligence, Blob Storage |
| **Python Development** | 2,500+ lines of production-quality code |
| **Low-Code / No-Code** | Copilot Studio agent with topics, flows, knowledge |
| **Business Thinking** | Solved a real procurement/legal problem end-to-end |
| **Data Engineering** | Risk scoring model, TCV classification, JSON/CSV output |
| **Integration Design** | Connected code-first pipeline with low-code chat interface |
| **Documentation** | Architecture diagrams, build guides, this README |

---

## 📄 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
