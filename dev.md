graph TD

%% ===============================
%% ENDPOINT / AGENT LAYER
%% ===============================
subgraph ENDPOINTS["Endpoints & Infrastructure"]
    W1["Windows Hosts (.exe / .zip)"]
    L1["Linux Hosts (.deb / .rpm / .tar.gz)"]
    M1["macOS Hosts (.pkg / .zip)"]
    N1["Network Devices<br/>(FW / IDS / Router)"]
    C1["Cloud Platforms<br/>(AWS / GCP / Azure)"]
end

subgraph AGENT["Unified Security Agent (Go-based)"]
    A1["Log Collectors<br/>- Windows Event Logs<br/>- Sysmon<br/>- journald<br/>- auditd<br/>- macOS Unified Logs"]
    A2["Normalizer<br/>- ECS / Custom Schema<br/>- JSON Formatter"]
    A3["Local Enrichment<br/>- Host metadata<br/>- Process context"]
    A4["Response Engine (XDR)<br/>- Kill process<br/>- Isolate host<br/>- Block IP<br/>- Script execution"]
    A5["Secure Channel<br/>- mTLS<br/>- gRPC<br/>- Compression<br/>- Retry Buffer"]
end

W1 --> AGENT
L1 --> AGENT
M1 --> AGENT
N1 --> AGENT
C1 --> AGENT

%% ===============================
%% INGESTION & STREAM PIPELINE
%% ===============================
subgraph INGESTION["Data Ingestion & Streaming"]
    B1["Fluent Bit / OpenTelemetry"]
    B2["Message Broker<br/>Kafka / NATS"]
    B3["Vector.dev<br/>(Rust-based Transform)"]
    B4["Enrichment Engine<br/>- GeoIP<br/>- Threat Intel<br/>- Asset Context"]
end

AGENT --> B1
B1 --> B2
B2 --> B3
B3 --> B4

%% ===============================
%% STORAGE LAYER
%% ===============================
subgraph STORAGE["Hybrid Storage Layer"]
    S1["ClickHouse<br/>(Hot Data - Fast Queries)"]
    S2["OpenSearch<br/>(Optional Full-Text Search)"]
    S3["Object Storage<br/>(MinIO / S3 - Cold Archive)"]
end

B4 --> S1
B4 --> S2
B4 --> S3

%% ===============================
%% DETECTION & INTELLIGENCE
%% ===============================
subgraph DETECTION["Detection & Correlation Engine"]
    D1["Rule Engine<br/>(Sigma Compatible)"]
    D2["UEBA Engine<br/>(Behavior Analytics)"]
    D3["ML Anomaly Detection"]
    D4["Attack Graph Engine<br/>(Kill Chain / Lateral Movement)"]
end

S1 --> D1
S1 --> D2
S1 --> D3
S1 --> D4

%% ===============================
%% AI & ANALYTICS
%% ===============================
subgraph AI["AI Intelligence Layer"]
    AI1["Local LLM<br/>- Alert Triage<br/>- SQL Generation"]
    AI2["External LLM (Gemini / GPT)<br/>- Root Cause Analysis<br/>- Incident Summary"]
    AI3["MITRE ATT&CK Mapper"]
    AI4["Confidence & Explainability Engine"]
end

D1 --> AI
D2 --> AI
D3 --> AI
D4 --> AI

%% ===============================
%% SOAR & RESPONSE
%% ===============================
subgraph SOAR["SOAR & Automated Response"]
    R1["Playbook Engine<br/>- YAML / JSON"]
    R2["Approval Workflow<br/>- Human-in-the-loop"]
    R3["Action Dispatcher"]
end

AI --> R1
R1 --> R2
R2 --> R3
R3 --> AGENT

%% ===============================
%% THREAT INTELLIGENCE
%% ===============================
subgraph TIP["Threat Intelligence Platform"]
    T1["IOC Feeds<br/>(MISP / OpenCTI)"]
    T2["Reputation Scoring"]
    T3["IOC Expiry & Confidence"]
end

TIP --> B4
TIP --> D1

%% ===============================
%% API & ACCESS
%% ===============================
subgraph API["API & Access Layer"]
    G1["API Gateway (Go)<br/>- Auth<br/>- Rate Limit"]
    G2["RBAC / ABAC Engine"]
end

SOAR --> G1
AI --> G1
STORAGE --> G1
G1 --> G2

%% ===============================
%% UI & DASHBOARDS
%% ===============================
subgraph UI["User Interfaces"]
    U1["Admin Console<br/>- Policy<br/>- SOAR<br/>- Agent Mgmt"]
    U2["SOC Analyst Dashboard<br/>- Alerts<br/>- Hunt<br/>- Timeline"]
    U3["CISO / Executive View<br/>- Risk Score<br/>- Compliance"]
end

G2 --> U1
G2 --> U2
G2 --> U3

%% ===============================
%% SECURITY & GOVERNANCE
%% ===============================
subgraph SECURITY["Security & Compliance"]
    Z1["Log Integrity<br/>- Hashing<br/>- WORM"]
    Z2["Audit Trails"]
    Z3["Zero Trust Internal mTLS"]
end

AGENT --> Z1
G1 --> Z2
API --> Z3