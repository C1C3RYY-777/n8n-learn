# n8n-learn ⚡

[![n8n](https://img.shields.io/badge/n8n-Workflow%20Automation-EA4B71?style=flat&logo=n8n)](https://n8n.io/)
[![Google Gemini](https://img.shields.io/badge/Google-Gemini%20AI-4285F4?style=flat&logo=google)](https://ai.google.dev/)
[![Discord](https://img.shields.io/badge/Discord-Alerts-5865F2?style=flat&logo=discord)](https://discord.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Repository dedicated to documenting and storing my practical learning journey, hands-on implementations, and workflow automation pipelines built with **n8n**.

---

## 📂 Workflows Catalog

| Workflow File | Category | Key Components / Nodes | Description |
| :--- | :--- | :--- | :--- |
| [`Section 2 - Sales Data Pipeline.json`](./Section%202%20-%20Sales%20Data%20Pipeline.json) | **Data Engineering** | Manual Trigger, Aggregate, Split Out, Filter, CSV, HTTP Request | End-to-end sales order processing, order totals calculation, delivered status filtering, and report generation. |
| [`Monitor Report Errors.json`](./Monitor%20Report%20Errors.json) | **DevOps & Monitoring** | Error Trigger, Edit Fields (Set), Discord Webhook | Catches workflow failures across executions and dispatches structured alerts directly to Discord. |
| [`My workflow.json`](./My%20workflow.json) | **AI & LangChain** | Chat Trigger, AI Agent, Google Gemini Chat Model, Window Memory, Data Table & HTTP Tools | Conversational AI support agent that queries customer and order data using tool calling and conversation memory. |
| [`Generating Reports.json`](./Generating%20Reports.json) | **Reporting** | Manual Trigger, Data Table, Merge Node | Aggregates customer data and prepares structured reporting outputs. |
| [`Merging Data.json`](./Merging%20Data.json) | **ETL & Data Prep** | Manual Trigger, Data Table, HTTP Request, Merge | Combines customer data tables with country webhook data for enriched profiles. |
| [`Nathan's Workflow.json`](./Nathan's%20Workflow.json) | **Data Management** | Manual Trigger, Data Table Upsert | Warehouse order processing and upsert operations into internal data tables. |
| [`projects/software_dev_team/ADMIN.json`](./projects/software_dev_team/ADMIN.json) | **Multi-Agent Systems** | Form Trigger, LangChain Agent, Gemini 3.1 Flash Lite, Postgres Memory, Postgres DB, Execute Workflow | Autonomous Tech Lead agent generating SDD specifications and orchestrating downstream QA workflows. |

---

## 🏗️ Deep-Dive Architecture & Workflows Breakdown

### 1. 📊 Sales Data Pipeline (`Section 2 - Sales Data Pipeline.json`)
A complete multi-stage data processing pipeline:
- **Ingestion**: Fetches raw sales records via authenticated HTTP endpoints (`GetSalesData`).
- **Data Normalization & Cleaning**: Splits batched arrays using `Split Out`, updates field names, and sets standardized report metadata.
- **Filtering Logic**: Evaluates order fulfillment status via `FilterDelivered` and handles exceptions (`IgnoreNonDelivered`).
- **Aggregation & Analytics**: Calculates line-item totals, groups orders using `AggregateOrders` and `Summarize`.
- **Export & Dispatch**: Converts structured objects into CSV format (`ConvertToCSV`) and transmits final reports via `SendOrders` and `SendAnalysis`.

### 2. 🚨 Error Monitoring & Alerting (`Monitor Report Errors.json`)
A resilient error handler pattern for enterprise n8n workflows:
- **Error Trigger**: Listens for failure events emitted across connected production workflows.
- **Message Builder**: Extracts workflow name (`$json.workflow.name`), error message (`$json.execution.error.message`), last failing node, and direct execution URL.
- **Notification**: Pushes real-time alerts to a dedicated Discord channel using webhook integration for immediate triage.

### 3. 🤖 AI Customer Support Agent (`My workflow.json`)
An autonomous conversational agent built on LangChain components inside n8n:
- **Chat Trigger**: Webhook interface receiving user questions in conversational format.
- **LLM Backbone**: Powered by Google Gemini (`@n8n/n8n-nodes-langchain.lmChatGoogleGemini`).
- **Memory**: Window Buffer Memory (`@n8n/n8n-nodes-langchain.memoryBufferWindow`) maintaining multi-turn context.
- **Tool Calling**:
  - `GetCustomers`: Queries customer profiles from n8n internal Data Tables.
  - `GetOrderData`: Interacts with live order database via authenticated HTTP header tool requests.

### 4. 🧠 Autonomous Software Development Team (`projects/software_dev_team/`)
An enterprise multi-agent engineering architecture implementing **Spec-Driven Development (SDD)** & **Design by Contract (DbC)**:
- **Intake Trigger**: Form trigger capturing project documentation and raw stakeholder ideas.
- **Tech Lead Agent**: Powered by Gemini 3.1 Flash Lite and LangChain to draft structured SDD contracts with functional requirements, BDD scenarios, invariants, and acceptance criteria.
- **State & Memory**: PostgreSQL session memory (`n8n_chat_ADMIN_historial`) and relational persistence (`projects` table).
- **Sub-workflow Handoff**: Automates downstream delegation to tester and QA workflows (`Pasar al testeador`).
- *See the comprehensive guide in [`projects/software_dev_team/README.md`](./projects/software_dev_team/README.md).*

---

## 🚀 How to Import and Run in n8n

### Option A: Via n8n Graphical Interface
1. Open your n8n instance (Cloud or Self-Hosted).
2. Go to **Workflows** > **Add Workflow**.
3. Click the `...` (options menu) in the top-right corner.
4. Select **Import from File...** and choose any `.json` workflow file from this repository.
5. Alternatively, copy the raw JSON content and paste it directly onto the n8n canvas using `Ctrl + V` (`Cmd + V` on Mac).

### Option B: Via n8n CLI
```bash
# Import a specific workflow
n8n import:workflow --input="./Section 2 - Sales Data Pipeline.json"

# Import all workflows in batch
for %f in (*.json) do n8n import:workflow --input="%f"
```

---

## 🔑 Required Credentials & Configuration

When importing these workflows, configure the following credentials within n8n:

| Credential Type | Workflows Used | Description |
| :--- | :--- | :--- |
| **Google Gemini (PaLM) API** | `My workflow.json`, `ADMIN.json` | API key from Google AI Studio for Gemini chat and Tech Lead agent completion. |
| **PostgreSQL** | `projects/software_dev_team/ADMIN.json` | Database connection for relational project state (`projects`) and LangChain chat memory. |
| **Discord Webhook** | `Monitor Report Errors.json` | Channel webhook URL for automated incident notification. |
| **HTTP Header Auth** | `My workflow.json`, `Section 2` | Custom assessment / API tokens for data retrieval endpoints. |
| **n8n Internal Data Tables** | `Nathan's Workflow`, `Merging Data`, `Generating Reports` | Data Table schemas for `Orders` and `Customers`. |

---

## 🗺️ Learning Milestones & Roadmap

- [x] **Foundations**: Core n8n interface, execution triggers, data tables.
- [x] **Data Manipulation**: Merging sources, aggregation, data filtering, and formatting.
- [x] **Advanced Pipelines**: End-to-end sales processing, CSV transformations, automated reporting.
- [x] **Error Handling & Resiliency**: Error triggers, webhook alerts, and exception handling.
- [x] **AI & Autonomous Agents**: LangChain integration, LLM tool-calling, window memory buffers.
- [x] **Sub-workflows & Multi-Agent Teams**: Modular orchestration, PostgreSQL state tracking, SDD/DbC agents.
- [ ] **Custom Community Nodes**: Extending n8n capabilities with custom TypeScript nodes.

---

## 📁 Repository Structure

```text
n8n-learn/
├── assets/
│   └── {E4DEFB35-F8A0-459C-82D2-007BE8129497}.png  # Workflow screenshots & diagrams
├── projects/
│   └── software_dev_team/                           # Autonomous Software Development Squad
│       ├── ADMIN.json                               # Tech Lead & SDD Specification generator
│       ├── README.md                                # Project guide & setup instructions
│       ├── schema.sql                               # PostgreSQL tables & trigger definitions
│       ├── docs/
│       │   └── pipeline_architecture.md             # Mermaid diagrams & agent state machine
│       ├── prompts/
│       │   └── tech_lead_system_prompt.md           # System prompt for Tech Lead agent
│       └── specs/
│           └── sdd_template.json                    # Formal SDD contract template
├── Generating Reports.json                          # Customer report generation workflow
├── Merging Data.json                                # Multi-source data merging workflow
├── Monitor Report Errors.json                       # Global error monitoring & Discord alerts
├── My workflow.json                                 # AI Support Agent (LangChain + Gemini)
├── Nathan's Workflow.json                           # Order upsert & warehouse workflow
├── Section 2 - Sales Data Pipeline.json             # Sales ETL pipeline & CSV report generator
└── README.md                                        # Project documentation & workflows guide
```
