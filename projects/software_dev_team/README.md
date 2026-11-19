# 🤖 Autonomous Software Development Team (n8n Multi-Agent System)

An automated engineering organization powered by **n8n**, **LangChain**, **Google Gemini**, and **PostgreSQL**. This project coordinates specialized autonomous agents to guide software features from an initial concept to validated specifications and automated testing.

---

## 🌟 Highlights

- **Spec-Driven Development (SDD)**: Replaces informal prompting with formal contracts containing functional/non-functional requirements, BDD scenarios, invariants, and acceptance criteria.
- **Design by Contract (DbC)**: Guarantees strict preconditions, postconditions, and system invariants before any code is generated.
- **Persistent State & Memory**: Leverages PostgreSQL for relational project state storage and LangChain multi-turn chat memory.
- **Modular Pipeline Orchestration**: Decoupled agent workflows communicating via n8n `executeWorkflow` sub-workflow execution nodes.

---

## 📂 Project Structure

```text
projects/software_dev_team/
├── ADMIN.json                          # Main Tech Lead & SDD Specification Generator workflow
├── README.md                           # Project guide and overview
├── schema.sql                          # PostgreSQL schema for projects and chat history
├── docs/
│   └── pipeline_architecture.md        # Architecture diagrams, state machine, and data flow
├── prompts/
│   └── tech_lead_system_prompt.md      # Tech Lead system prompt and rules
└── specs/
    └── sdd_template.json               # Sample SDD specification output
```

---

## ⚙️ Prerequisites & Credentials

To run this workflow in n8n, configure the following:

| Service / Credential | Type in n8n | Description |
| :--- | :--- | :--- |
| **PostgreSQL** | `postgres` | Access to the database hosting the `projects` table and `n8n_chat_ADMIN_historial` memory. |
| **Google Gemini API** | `googlePalmApi` | API key providing access to `models/gemini-3.1-flash-lite`. |

---

## 🚀 Quick Setup

1. **Deploy Database Schema**:
   Execute [`schema.sql`](./schema.sql) in your PostgreSQL instance:
   ```bash
   psql -h <host> -U <user> -d <database> -f schema.sql
   ```

2. **Import Workflow**:
   - In your n8n workspace, navigate to **Workflows** > **Add Workflow**.
   - Import [`ADMIN.json`](./ADMIN.json) or paste its JSON content.
   - Attach your `googlePalmApi` and `postgres` credentials to the corresponding nodes.

3. **Run Pipeline**:
   - Open the web form exposed by the `Archivo de proyecto` trigger node.
   - Submit a raw project description or upload initial requirement documents.
   - The **ADMIN** agent generates the formal SDD spec, persists it in Postgres with state `SPEC_DRAFT`, and dispatches the downstream testing and QA workflows.
