# promptlytics
Multi-agent prompt analytics — track, cluster, and analyze user queries to continuously refine agent behaviors.

Closed-loop learning system for multi-agent orchestration
promptmirror


flowchart TD
    %% ==== USER & ORCHESTRATOR ====
    U[💬 User / Frontend<br/>(Chat UI, API, Teams, etc.)] --> O[⚙️ LangGraph Orchestrator<br/>• Query routing<br/>• Agent orchestration<br/>• Summarization logic]

    %% ==== AGENTS ====
    O --> A1[🟡 Agent 1<br/>(EmailAgent)]
    O --> A2[🟡 Agent 2<br/>(MeetingAgent)]
    O --> AN[🟡 Agent N<br/>(Other domain-specific agents)]

    %% ==== LANGSMITH LAYER ====
    A1 --> L[🧾 LangSmith Platform<br/>• Trace logs<br/>• Model metrics<br/>• Prompt registry<br/>• Evaluation datasets]
    A2 --> L
    AN --> L
    O --> L

    %% ==== PROMPT EVALUATION APP ====
    subgraph EVAL[🔍 Prompt Evaluation Application]
        direction TB
        I[📥 Data Ingestion Service<br/>• Fetch traces via LangSmith API<br/>• Normalize & store]
        F[🧠 Failure Analyzer<br/>• Detect incorrect runs<br/>• Group by agent<br/>• Cluster by scenario (Embeddings)]
        P[🧩 Prompt Evaluator Engine<br/>• Compare failures ↔ current prompt<br/>• LLM-based improvement suggestions]
        R[📚 Prompt Updater<br/>• Push improved prompt → LangSmith registry<br/>• Create new version / PR for review]
    end

    %% ==== STORAGE & SUPPORT ====
    I --> DB[(🗄️ Evaluation DB<br/>MongoDB / Postgres)]
    F --> VEC[(🧬 Vector Store<br/>FAISS / Pinecone<br/>for scenario clustering)]
    P --> LLM[(🤖 LLM API<br/>GPT-4o / Claude 3.5<br/>for prompt improvement analysis)]
    R --> L

    %% ==== DASHBOARD ====
    subgraph UI[📊 Web Dashboard (Streamlit / Next.js)]
        direction TB
        D1[🔎 View clusters, issue summaries]
        D2[📈 Compare pre/post metrics]
        D3[📝 Review & approve prompt updates]
    end

    %% ==== DATA FLOW ====
    L --> I
    F --> P
    P --> R
    EVAL --> UI
    R --> O

    %% ==== STYLING ====
    classDef user fill:#d5e8d4,stroke:#82b366,stroke-width:1px,color:#000;
    classDef orchestrator fill:#dae8fc,stroke:#6c8ebf,stroke-width:1px,color:#000;
    classDef agent fill:#fff2cc,stroke:#d6b656,stroke-width:1px,color:#000;
    classDef langsmith fill:#e1d5e7,stroke:#9673a6,stroke-width:1px,color:#000;
    classDef eval fill:#f8cecc,stroke:#b85450,stroke-width:1px,color:#000;
    classDef db fill:#f5f5f5,stroke:#666666,stroke-width:1px,color:#000;
    classDef llm fill:#ffe6cc,stroke:#cc9900,stroke-width:1px,color:#000;
    classDef dashboard fill:#cce5ff,stroke:#6c8ebf,stroke-width:1px,color:#000;

    class U user
    class O orchestrator
    class A1,A2,AN agent
    class L langsmith
    class EVAL,F,P,R,I eval
    class DB,VEC db
    class LLM llm
    class UI dashboard
