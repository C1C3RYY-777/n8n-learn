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
