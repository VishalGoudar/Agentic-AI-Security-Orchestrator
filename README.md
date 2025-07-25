# 🛡️ Agentic AI Security Orchestrator on GCP
An intelligent system leveraging Agentic AI and Google Cloud Platform to automate security alert triage and remediation.

## 📖 Overview
This project implements an Agentic AI orchestrator that automates security alert triage and basic remediation on Google Cloud using OpenAI/Vertex AI + MCP.

- Triage security alerts via LLM
- Takes remediation actions (e.g., revoke IAM, disable service accounts)
- Uses MCP + Cloud Run for secure tool integration

## 🚀 Features

- Agentic triage pipeline: alert-triage.py Cloud Function for initial alert processing.
- Automated IAM remediation: remediate-action.py Cloud Function for taking corrective actions.
- Secure pipeline using Model Context Protocol (MCP server): A custom FastAPI application acting as a secure intermediary for LLM interactions.
- GCP deployment via Terraform: Infrastructure as Code for reliable and repeatable deployments.

## 💡 Architecture

### 📊 Architecture Diagram
(docs/architecture.png)

### Explanation of the Above Flow ☝️:
Here-
- Security Command Center (SCC): A security alert is generated by SCC.
- Logs Router Sink: The Alert is routed via a Logs Router Sink to a dedicated Pub/Sub.
- Alert-triage Cloud Function: Subscribes to scc-alerts(pub/sub topic), receives the aleart, and extracts relevant context.
- MCP Server (Cloud Run): The alert-triage function sends the extracted context to the MCP server endpoint.
- LLMs (OpenAI/Vertex AI): The MCP server interacts with LLMs, providing the alert details and a prompt to determine severity and recommended actions.
- MCP Decisioning: The MCP server processes the LLM's response and returns a structured decision (e.g., severity label: "HIGH", recommended action: "disable IAM").
- GCP IAM API: The recommended action makes IAM API calls to perform the prescribed remediation/action (e.g., disable service account, revoke roles).
- Logging & Monitoring: All steps are logged to Cloud Logging, and actions are auditable in IAM Audit Logs and Security Command Center.

### Model Context Protocol (MCP) Integration and Decisioning
The MCP server acts as a critical component in this architecture, ensuring secure and controlled interaction with the Large Language Model (LLM) and providing a structured decision-making layer.

### What is MCP?
In this project, the "Model Context Protocol (MCP)" refers to a custom-built service (deployed on Cloud Run) that:
- Acts as a proxy and orchestrator for LLM interactions.
- Enforces context management: Ensures sensitive data is handled securely and only necessary context is sent to the LLM.
- Provides structured prompting: Formulates precise prompts to guide the LLM's reasoning for security tasks.
- Performs response parsing and validation: Interprets the LLM's natural language output into structured, actionable decisions.
- Implements decision logic: Applies business rules, thresholds, and sanity checks on the LLM's recommendations before making a final decision.

## 🛠️ Prerequisites
- GCP project ID.
- Service Account for the Cloud Functions and Cloud Run with appropriate roles.
- OpenAI API Key or Vertex AI (Google Cloud) setup for LLM access.
- Service account with Logging Viewer + Functions Invoker roles
