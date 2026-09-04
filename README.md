# Enterprise RAG Architecture & Governance Control Map

This diagram maps the technical flow of a Retrieval-Augmented Generation (RAG) system, overlaid with critical AI Governance checkpoints.

```mermaid
graph TD
    %% Define Styles
    classDef tech fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef gov fill:#e1f5fe,stroke:#0288d1,stroke-width:3px,stroke-dasharray: 5 5;
    classDef risk fill:#ffebee,stroke:#c62828,stroke-width:2px;

    %% User Interaction
    User((End User)) -->|Submits Prompt| API_Gateway
    
    %% Ingestion Pipeline
    subgraph Data Ingestion & Processing
        Raw_Data[Raw Enterprise Data] -->|Extract & Chunk| Data_Preprocessing
        Data_Preprocessing -->|Embed & Index| Vector_DB[(Vector Database)]
    end

    %% Retrieval & Generation Pipeline
    subgraph RAG Execution Pipeline
        API_Gateway -->|1. Query Vector DB| Vector_DB
        Vector_DB -->|2. Retrieve Context| Context_Window
        Context_Window -->|3. Prompt Engineering| LLM((Large Language Model))
        LLM -->|4. Generate Response| Output_Filter
    end

    %% Governance & Control Layer (The Pattern 2 Focus)
    subgraph AI Governance & Control Layer
        Gov_Ingestion[Control 1: Data Lineage & PII Scrubbing]:::gov
        Gov_Input[Control 2: Input Guardrails / Prompt Injection Check]:::gov
        Gov_Output[Control 3: Output Hallucination & Toxicity Filter]:::gov
        Gov_Monitor[Control 4: Continuous Drift & Usage Monitoring]:::gov
    end

    %% Mapping Controls to Pipeline
    Data_Preprocessing -.-> Gov_Ingestion
    API_Gateway -.-> Gov_Input
    Output_Filter -.-> Gov_Output
    LLM -.-> Gov_Monitor

    Output_Filter -->|Final Answer| User
    
    %% Risk Nodes
    Risk_Data(Data Poisoning / PII Leak):::risk
    Risk_Input(Prompt Injection):::risk
    Risk_Output(Hallucination / Bias):::risk
    Risk_Monitor(Model Drift):::risk

    Gov_Ingestion --- Risk_Data
    Gov_Input --- Risk_Input
    Gov_Output --- Risk_Output
    Gov_Monitor --- Risk_Monitor# AI-Lifecycle-Governance-Map.