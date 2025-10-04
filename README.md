# promptlytics
Multi-agent prompt analytics — track, cluster, and analyze user queries to continuously refine agent behaviors.

Closed-loop learning system for multi-agent orchestration
promptmirror

flowchart TD
    U[💬 User / Frontend<br/>(Chat UI, API, Teams, etc.)] --> O[⚙️ LangGraph Orchestrator<br/>• Query routing<br/>• Agent orchestration<br/>• Summarization logic]

    O --> A1[🟡 Agent 1<br/>(EmailAgent)]
    O --> A2[🟡 Agent 2<br/>(MeetingAgent)]
    O --> AN[🟡 Agent N<br/>(Other domain-specific agents)]

    A1 --> L[🧾 LangSmith Platform<br/>• Trace logs<br/>• Model metrics<br/>• Prompt registry<br/>• Evaluation datasets]
    A2 --> L
    AN --> L
    O --> L

    subgraph EVAL[🔍 Prompt Evaluation Application]
        direction TB
        I[📥 Data Ingestion<br/>• Fetch traces via LangSmith API]
        F[🧠 Failure Analyzer<br/>• Detect incorrect runs<br/>• Cluster by scenario]
        P[🧩 Prompt Evaluator Engine<br/>• Suggest improved prompts]
        R[📚 Prompt Updater<br/>• Push new version to LangSmith]
    end

    I --> DB[(🗄️ Evaluation DB)]
    F --> VEC[(🧬 Vector Store)]
    P --> LLM[(🤖 LLM API)]
    R --> L

    subgraph UI[📊 Web Dashboard (Streamlit / Next.js)]
        D1[View clusters]
        D2[Review prompt updates]
    end

    L --> I
    F --> P
    P --> R
    EVAL --> UI
    R --> O
