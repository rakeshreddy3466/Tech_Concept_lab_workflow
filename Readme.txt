Framtidskassan – Enterprise Agentic Architecture
Innovation in Motion | Tech Concept Lab Prototype

Framtidskassan is an enterprise-grade prototype designed to revolutionize how Swedish social insurance (Försäkringskassan) applications are processed. By deploying an autonomous "AI Workforce," this system decouples administrative friction from final legal assessments, solving the "Human Bottleneck" and eliminating latency traps.

The AI-First Paradigm
Rather than forcing citizens to navigate rigid digital forms and complex legal statutes (Socialförsäkringsbalken), Framtidskassan utilizes a Neural Orchestration Layer to map natural human language directly to strict operational frameworks.

The system acts as a tireless, secure digital co-worker—gathering exact evidence, validating identities, and auditing documents via Vision AI before a human caseworker ever touches the file.

System Architecture
The architecture is built on a Zero-Trust, air-gapped model ensuring Citizen PII is never exposed directly from the client browser to public LLMs.

1. The Front-End (index.html)
A dynamic, state-driven customer portal built with HTML/CSS/JS matching the official FKDS (Försäkringskassan Design System). It captures unstructured "Human Reality Input" and securely converts physical documents into Base64 payloads.

2. The Neural Orchestration Layer (n8n)
The core middleware handling secure routing, API air-gapping, and dynamic persona loading.

Agent 1: Semantic Triage (Framtidskassan Semantic Router.json): Guards the front door. Analyzes user intent and maps it to 1 of 8 specific benefit streams. If the input is ambiguous, it halts the system and triggers a dynamic Clarification UI, preventing AI hallucination.

Agent 2: The Specialist Co-worker (FK Carrier Agent.json): A "shapeshifting" cognitive worker. Once intent is verified, n8n injects the exact statutory rules (e.g., SFB 13 kap.) into its prompt. It conducts a contextual interview, asks for specific evidence, and utilizes Vision AI to audit uploaded medical and employer certificates.

The "JSON Sniper": A custom JavaScript logic firewall that intercepts LLM outputs, strips away conversational anomalies (like markdown backticks), and ensures only sanitized, strictly formatted data passes through.

3. Immutable Persistence (Supabase)
A PostgreSQL database acting as the system's ledger. It durably stores verified citizen data, documents, and the exact AI reasoning trail for complete auditability.

4. Augmented Governance (caseworker.html)
The Human-in-the-Loop dashboard. The pre-audited, perfectly structured dossier is pushed here. Efficiency scales infinitely via AI, but final legal accountability remains firmly in human hands. Includes automated Decision Letter generation.

Repository Structure
index.html: The citizen-facing application portal and AI chat interface.

caseworker.html: The secure internal dashboard for human review and final decision-making.

Framtidskassan Semantic Router.json: The n8n workflow for Agent 1 (Intent Classification).

FK Carrier Agent.json: The n8n workflow for Agent 2 (Interviewing, OCR Validation, and DB interaction).

framtidskassan_presentation.html: The interactive, scroll-snapping executive pitch deck.

Setup & Installation
To run this prototype locally, you will need to configure the middleware and database connections.

Prerequisites
An active n8n instance (Local, Cloud, or Docker).

An Anthropic API Key (for Claude 3 Haiku/Sonnet).

A Supabase project.

Step 1: Database Setup (Supabase)
In your Supabase SQL editor, ensure you have the following tables created:

customers (Columns: personnummer, full_name, email, phone, employer, employment_type, monthly_income, employment_start)

applications (Columns: id, personnummer, status, agent_assessment, chat_log, caseworker_notes, created_at)

documents (Columns: id, personnummer, doc_type, filename, file_data, mime_type, created_at)

Step 2: Orchestration Layer Setup (n8n)
Open your n8n instance.

Go to Workflows -> Import from File.

Import Framtidskassan Semantic Router.json and FK Carrier Agent.json.

Open the HTTP Request nodes in both workflows and replace the placeholder x-api-key with your actual Anthropic API Key.

In FK Carrier Agent.json, update the Supabase HTTP Request nodes with your Supabase URL and Service Role Key.

Activate both workflows and copy their Production Webhook URLs.

Step 3: Front-End Configuration
Open index.html in a text editor.

Locate the Webhook configuration variables at the top of the <script> tag:

JavaScript
const WEBHOOK_URL = 'YOUR_CARRIER_AGENT_WEBHOOK_URL';
const CLASSIFY_WEBHOOK_URL = 'YOUR_SEMANTIC_ROUTER_WEBHOOK_URL'; 
Replace them with the URLs generated from n8n.

Update the SUPABASE_URL and SUPABASE_KEY variables in both index.html and caseworker.html with your project's anon/public credentials.

Step 4: Run the Application
You cannot run the HTML files simply by double-clicking them due to CORS and Base64 upload restrictions. You must serve them over a local server.
If you have Python installed:

run:
python3 -m http.server 8000

To run the and check in the system
Supabase key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImttZ3l1d21peHNkeHRweG9ycnpjIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzQ2ODkzNjQsImV4cCI6MjA5MDI2NTM2NH0.y9D9WTdSHnfRZ4MdJU8Lw0Wvgr5je1NH7Kr5Es9558Q
Supabase_url: https://kmgyuwmixsdxtpxorrzc.supabase.co
Claude API key: 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
API name: x-api-key