# promptlytics
Multi-agent prompt analytics — track, cluster, and analyze user queries to continuously refine agent behaviors.

Closed-loop learning system for multi-agent orchestration
promptmirror



```mermaid
flowchart TD
    %% ==== USER & ORCHESTRATOR ====
    U[User / Frontend - Chat UI, API, Teams] --> O[LangGraph Orchestrator: Query routing, Agent orchestration, Summarization logic]

    %% ==== AGENTS ====
    O --> A1[Agent 1 - EmailAgent]
    O --> A2[Agent 2 - MeetingAgent]
    O --> AN[Agent N - Other domain-specific agents]

    %% ==== LANGSMITH LAYER ====
    A1 --> L[LangSmith Platform: Trace logs, Model metrics, Prompt registry, Evaluation datasets]
    A2 --> L
    AN --> L
    O --> L

    %% ==== PROMPT EVALUATION APP ====
    subgraph EVAL[Prompt Evaluation Application]
        direction TB
        I[Data Ingestion Service: Fetch traces via LangSmith API]
        F[Failure Analyzer: Detect incorrect runs, Group by agent, Cluster by scenario]
        P[Prompt Evaluator Engine: Compare failures vs current prompt, Generate improvement suggestions]
        R[Prompt Updater: Push improved prompt to LangSmith registry, Create new version for review]
    end

    %% ==== STORAGE & SUPPORT ====
    I --> DB[(Evaluation DB - MongoDB or Postgres)]
    F --> VEC[(Vector Store - FAISS or Pinecone)]
    P --> LLM[(LLM API - GPT-4o or Claude 3.5)]
    R --> L

    %% ==== DASHBOARD ====
    subgraph UI[Web Dashboard - Streamlit or Next.js]
        direction TB
        D1[View clusters and issue summaries]
        D2[Compare metrics]
        D3[Review and approve prompt updates]
    end

    %% ==== DATA FLOW ====
    L --> I
    F --> P
    P --> R
    EVAL --> UI
    R --> O
