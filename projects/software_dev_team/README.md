# 🤖 Autonomous Software Development Team (n8n Multi-Agent System)

An automated engineering organization powered by **n8n**, **LangChain**, **Google Gemini**, and **PostgreSQL**. This project coordinates specialized autonomous agents to guide software features from an initial concept to validated specifications and automated testing.

---

## 🌟 Highlights

- **Spec-Driven Development (SDD)**: Replaces informal prompting with formal contracts containing functional/non-functional requirements, BDD scenarios, invariants, and acceptance criteria.
- **Design by Contract (DbC)**: Guarantees strict preconditions, postconditions, and system invariants before any code is generated.
- **Shift-Left QA Gatekeeper**: Implements `TESTEADOR_1`, an autonomous QA Architect that audits specifications against 4 pillars (Invariants, BDD edge cases, Requirements, and Acceptance Criteria).
- **Automated Revision Feedback Loop**: If an SDD specification fails QA audit, it is automatically routed back to the ADMIN agent for up to 2 revisions before falling back to `MAX_RETRIES_REACHED`.
- **Persistent State & Memory**: Leverages PostgreSQL for relational project state storage and LangChain multi-turn chat memory.

---

## 📂 Project Structure

```text
projects/software_dev_team/
├── ADMIN.json                          # Main Tech Lead & SDD Specification Generator workflow
├── TESTEADOR_1.json                    # Shift-Left QA Architect & Gatekeeper workflow
├── README.md                           # Project guide and overview
├── schema.sql                          # PostgreSQL schema for projects and chat history
├── docs/
│   └── pipeline_architecture.md        # Architecture diagrams, state machine, and data flow
├── prompts/
│   ├── tech_lead_system_prompt.md      # Tech Lead system prompt and rules
│   └── qa_architect_system_prompt.md   # QA Gatekeeper audit criteria and system prompt
└── specs/
    ├── sdd_template.json               # Sample SDD specification output
    └── qa_report_template.json         # Sample QA audit report JSON output
```

---

## ⚙️ Prerequisites & Credentials

To run these workflows in n8n, configure the following:

| Service / Credential | Type in n8n | Workflows Used | Description |
| :--- | :--- | :--- | :--- |
| **PostgreSQL** | `postgres` | `ADMIN.json`, `TESTEADOR_1.json` | Access to the database hosting `projects`, `n8n_chat_ADMIN_historial`, and `n8n_chat_histories`. |
| **Google Gemini API** | `googlePalmApi` | `ADMIN.json`, `TESTEADOR_1.json` | API key providing access to Gemini language models. |

---

## 🚀 Quick Setup & Workflow Execution

1. **Deploy Database Schema**:
   Execute [`schema.sql`](./schema.sql) in your PostgreSQL instance:
   ```bash
   psql -h <host> -U <user> -d <database> -f schema.sql
   ```

2. **Import Workflows**:
   - In your n8n workspace, import both [`ADMIN.json`](./ADMIN.json) and [`TESTEADOR_1.json`](./TESTEADOR_1.json).
   - Ensure `ADMIN.json` references the workflow ID of `TESTEADOR_1.json` in the "Pasar al testeador" node (`pt7VA4G5OSWc711z`).
   - Ensure `TESTEADOR_1.json` references `ADMIN.json` in the "Call 'ADMIN'" revision node (`JrZ6oIdoykRFgsxs`).

3. **Run Pipeline**:
   - Open the web form exposed by the `Archivo de proyecto` trigger node in `ADMIN.json`.
   - Submit a raw project description or upload initial requirement documents.
   - The **ADMIN** agent generates the formal SDD spec (`SPEC_DRAFT`) and dispatches `TESTEADOR_1`.
   - **TESTEADOR_1** audits the document across all 4 pillars:
     - If approved (score >= 75 and 0 invariant violations), status becomes `SPEC_APPROVED`.
     - If rejected, it records `SPEC_REJECTED` and triggers an automatic revision loop back to ADMIN.
