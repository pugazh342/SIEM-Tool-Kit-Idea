```mermaid
graph TD
    %% Log Ingestion Layer
    subgraph Ingestion_Layer [Data Ingestion & Sources]
        A1[Endpoints/Servers] --> B[Fluent Bit / OTel Agent]
        A2[Network Devices] --> B
        A3[Cloud Logs - AWS/GCP] --> B
        A4[Firewalls/IDPS] --> B
    end

    %% Streaming & Transformation
    subgraph Pipeline_Layer [Stream Processing]
        B --> C{NATS / Kafka Broker}
        C --> D[Vector.dev - Rust Transformer]
        D --> E{Data Enrichment}
        E -- GeoIP/Threat Intel --> F[Normalized JSON]
    end

    %% Storage Layer
    subgraph Storage_Layer [Hybrid Storage Engine]
        F --> G[(ClickHouse - Hot Data)]
        F --> H[(MinIO/S3 - Cold Archive)]
    end

    %% Intelligence & AI Layer
    subgraph Intelligence_Layer [AI-Correlation & Response Engine]
        G <--> I[Gemini Pro - Root Cause Analysis]
        G <--> J[Local LLM - Triage & SQL Gen]
        I & J --> K[SOAR Playbook Executor]
    end

    %% Interface & RBAC
    subgraph Interface_Layer [Unified Dashboard]
        K --> L{API Gateway - Go}
        L --> M[Admin Portal]
        L --> N[Analyst Dashboard]
    end

    %% Privileges
    subgraph RBAC [Access Control]
        M -- Full Config / SOAR Edit --> L
        N -- Read / Search / Soft-Response --> L
    end

    %% Styling
    style G fill:#f96,stroke:#333,stroke-width:2px
    style I fill:#4285F4,stroke:#333,stroke-width:2px
``
    style B fill:#85C1E9
    style M fill:#E74C3C
    style N fill:#2ECC71
