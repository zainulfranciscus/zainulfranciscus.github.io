# **Cloud & AI Engineering Portfolio**

Welcome to my project showcase\! This page documents two production-ready engineering architectures that demonstrate modern cloud infrastructure, event-driven serverless design, and stateful Generative AI workflows.

## **📂 Featured Projects**

| Project | Core Domain | Key Technologies |
| :---- | :---- | :---- |
| **[1\. Intelligent Customer Support Agent](https://www.google.com/search?q=%23-1-intelligent-customer-support-agent)** | AI Orchestration & Agentic Workflows | Google Agent Development Kit (ADK), Gemini, Python, Pydantic, Telemetry |
| [**2\. Serverless Event-Driven Document Pipeline**](https://www.google.com/search?q=%23-2-serverless-event-driven-document-processing-pipeline) | Cloud Infrastructure & Event-Driven Systems | Google Cloud Platform (GCS, Pub/Sub, Cloud Run, BigQuery), Python, Docker |

## **🧠 1\. Intelligent Customer Support Agent**

An enterprise-grade, stateful ReAct (Reasoning and Acting) agent built using the **Google Agent Development Kit (ADK)** and orchestrated via a directed graph workflow. It securely classifies incoming customer queries, coordinates downstream specialized agents, and logs detailed traces.

### **Architecture Workflow**

graph TD  
    START(\[START\]) \--\> preprocess\_node\[preprocess\_node\]  
    preprocess\_node \--\> classifier\_agent\[classifier\_agent\]  
    classifier\_agent \--\> router\_node\[router\_node\]  
    router\_node \--\>|ctx.route \== 'shipping'| shipping\_faq\_agent\[shipping\_faq\_agent\]  
    router\_node \--\>|ctx.route \== 'unrelated'| decline\_agent\[decline\_agent\]

### **Key Engineering Features**

* **Stateful Graph Execution:** Uses shared session states (WorkflowState) and structured Pydantic schemas (Classification) to manage dynamic context safely across nodes.  
* **Intelligent Routing:** Features a specialized router agent backed by gemini-3.1-flash-lite that enforces structural outputs and routes traffic to specialized sub-agents.  
* **Extensible Tool Integrations:** Ready to bind custom python functions to agents to access external systems (e.g., query database tracking tables).  
* **Built-in Telemetry:** Integrates directly with OpenTelemetry, Cloud Trace, BigQuery, and Cloud Logging for deep observability.

### **Quick Start & Commands**

This project uses uv for lightning-fast Python dependency management.

\# Setup CLI skills  
uvx google-agents-cli setup

\# Run the local agent playground (with hot-reloading)  
agents-cli playground

\# Run unit and integration test suite  
uv run pytest tests/unit tests/integration

## **⚡ 2\. Serverless Event-Driven Document Processing Pipeline**

A highly resilient, serverless, and completely decoupled ingest-and-process architecture built on **Google Cloud Platform (GCP)**. It automates OCR simulations, extracts semantic metadata, and ensures idempotent stream insertions.

### **Infrastructure Architecture**

graph TD  
    A\[User / Upload\] \--\>|Upload Document| B(GCS Ingestion Bucket)  
    B \--\>|OBJECT\_FINALIZE Event| C\[Pub/Sub Notification Topic\]  
    C \--\>|Push Subscription with OIDC| D\[Cloud Run Processor Service\]  
    D \--\>|1. Stream file & Simulate OCR| B  
    D \--\>|2. Check duplicates & Insert| E\[(BigQuery Dataset.Table)\]  
    D \--\>|Failure after 5 retries| F\[Dead Letter Queue Topic\]

### **Key Engineering Features**

* **True Serverless Compute:** Leverages Google Cloud Run to process incoming notifications on demand, minimizing idle container costs.  
* **Secure Pub/Sub Decoupling:** Employs Pub/Sub push subscription protected via OpenID Connect (OIDC) service account authentication.  
* **Strong Idempotency Guarantees:** Protects against duplicate deliveries (inherent to at-least-once pub/sub semantics) by using the Cloud Storage generation key to reject duplicates before streaming metadata to BigQuery.  
* **Built-in Resilience:** Configured with a Dead Letter Queue (DLQ) to quarantine poisoned or unparseable messages after 5 retries.

### **Deployment & Verification**

The pipeline is completely defined and easily provisioned using automated infrastructure scripts.

\# Provision Cloud Storage, Pub/Sub, Cloud Run, IAM policies, and BigQuery schemas  
./deploy.sh

\# Upload a test file to GCS to trigger the event pipeline  
gcloud storage cp test\_contract.txt gs://ingest-bucket-doc-pipeline-\[SUFFIX\]/

\# Verify the metadata stream directly in BigQuery  
bq query \--use\_legacy\_sql=false \\  
  'SELECT bucket, filename, generation, word\_count, tags, ocr\_preview FROM \`document\_pipeline.processed\_metadata\` LIMIT 10'

## **🛠️ Skills & Architectural Paradigms Demonstrated**

* **Agentic AI Systems:** Agent-driven architectures, structured JSON routing, tool execution, and prompt engineering using Google's modern ADK.  
* **Cloud Architecture & Infrastructure:** Serverless microservices, managed databases, secure service-to-service IAM bindings, and streaming ingestion.  
* **Reliability Engineering:** Automated unit/integration testing, telemetry tracking, idempotency controls, and DLQ exception handling.  
* **Modern Python Tooling:** Poetry, uv, FastAPI, Pydantic, and GCP client SDKs.
