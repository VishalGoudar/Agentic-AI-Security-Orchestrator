# Agentic AI orchestrator for GCP security — auto-triage &amp; remediation via LM + MCP.

## 🔐 Overview
This project implements an **Agentic AI orchestrator** that automates security alert triage and basic remediation on Google Cloud using OpenAI/Vertex AI + MCP.

- Triages security alerts via LLM
- Takes remediation actions (e.g., revoke IAM, disable service accounts)
- Uses **MCP + Cloud Run** for secure tool integration

## 🚀 Features
- Agentic triage pipeline (`alert-triage.js`, `triage-agent.py`)
- Automated IAM remediation (`remediate-iam.js`, `remediation-agent.py`)
- Secure pipeline using **Model Context Protocol** (MCP server)
- GCP deployment via Terraform

## 💡 Architecture
![Architecture Diagram](architecture-diagram.png)

## 🛠️ Prerequisites
- GCP project w/ billing enabled
- Service account with Logging Viewer + Functions Invoker roles
- OpenAI or Vertex AI setup
- Install:
  ```bash
  sudo apt install tree
