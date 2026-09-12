## System Architecture

```mermaid
flowchart TB

    subgraph DH["DATA HOLDER LAYER"]
        D1["Data Holder 01<br/>Local Dataset"]
        D2["Data Holder 02<br/>Local Dataset"]
        D3["Data Holder 03<br/>Local Dataset"]
        DN["Data Holder N<br/>Local Dataset"]
        P["Non-IID Partitioning<br/>IID / Dirichlet"]

        D1 --> P
        D2 --> P
        D3 --> P
        DN --> P
    end

    subgraph LT["LOCAL TRAINING"]
        R["Representation / Feature Adapter"]
        M["Detection Model"]
        T["Local Training"]

        R --> M
        M --> T
    end

    P --> R

    subgraph DP["PRIVACY PROTECTION"]
        C["Per-Sample Gradient Clipping"]
        N["Differential Privacy Noise"]
        A["PRV Privacy Accountant"]

        C --> N
        N --> A
    end

    T --> C

    subgraph SA["SECURE AGGREGATION"]
        MASK["Pairwise Zero-Sum Masking"]
        Q["Quorum / Dropout Recovery"]
        AGG["Protected Update Aggregation"]

        MASK --> Q
        Q --> AGG
    end

    N --> MASK

    subgraph RF["ROBUST FEDERATED LEARNING"]
        V["Update Validation"]
        DEF["DualDefense / Robust Aggregation"]
        GM["Global Model"]

        V --> DEF
        DEF --> GM
    end

    AGG --> V

    subgraph SL["SECURITY & ATTACK LAB"]
        INV["Gradient Inversion<br/>DLG Reconstruction"]
        POI["Byzantine Poisoning<br/>Sign Flip / ALIE / Replacement"]
        RES["Security Evaluation"]

        INV --> RES
        POI --> RES
    end

    GM --> INV
    GM --> POI
    DEF --> RES

    subgraph EV["EVALUATION ENGINE"]
        U["Utility Metrics<br/>Accuracy / F1 / Loss"]
        PR["Privacy Metrics<br/>Epsilon / Delta / Noise"]
        RB["Robustness Metrics<br/>ASR / TPR / FPR / FNR"]
        PF["Performance Metrics<br/>Latency / RAM / Bandwidth"]
        PX["Privacy / Utility / Security Analysis"]

        U --> PX
        PR --> PX
        RB --> PX
        PF --> PX
    end

    RES --> U
    A --> PR
    DEF --> RB
    AGG --> PF

    subgraph GOV["MODEL GOVERNANCE"]
        RG["Release Gate"]
        CERT["Model Certificate"]
        HOLD["Quarantine / HOLD"]

        RG --> CERT
        RG --> HOLD
    end

    PX --> RG

    subgraph OPS["MODEL & AUDIT LAYER"]
        MR["Model Registry"]
        AL["HMAC-SHA256 Audit Ledger"]
        RP["Reports & Evidence"]

        MR --> RP
        AL --> RP
    end

    CERT --> MR
    HOLD --> AL
    RG --> AL
    GM --> MR

    subgraph UI["TRACK-4 PRIVATE TRAINING CONTROL CENTER"]
        UI1["Federation Monitor"]
        UI2["Privacy Lab"]
        UI3["Attack & Defense Lab"]
        UI4["Experiment Center"]
        UI5["Governance & Audit"]
        UI6["Performance & Reports"]
    end

    UI1 --> V
    UI2 --> A
    UI3 --> RES
    UI4 --> PX
    UI5 --> RG
    UI6 --> RP

    classDef holder fill:#111827,stroke:#64748B,color:#F8FAFC,stroke-width:1px;
    classDef train fill:#172033,stroke:#60A5FA,color:#F8FAFC,stroke-width:1px;
    classDef privacy fill:#18231F,stroke:#34D399,color:#F8FAFC,stroke-width:1px;
    classDef secure fill:#241C12,stroke:#F59E0B,color:#F8FAFC,stroke-width:1px;
    classDef defense fill:#211827,stroke:#C084FC,color:#F8FAFC,stroke-width:1px;
    classDef attack fill:#27171A,stroke:#F87171,color:#F8FAFC,stroke-width:1px;
    classDef eval fill:#18181B,stroke:#A1A1AA,color:#F8FAFC,stroke-width:1px;
    classDef gov fill:#172026,stroke:#22D3EE,color:#F8FAFC,stroke-width:1px;
    classDef ops fill:#1E1B2E,stroke:#818CF8,color:#F8FAFC,stroke-width:1px;
    classDef ui fill:#18181B,stroke:#94A3B8,color:#F8FAFC,stroke-width:1px;

    class D1,D2,D3,DN,P holder;
    class R,M,T train;
    class C,N,A privacy;
    class MASK,Q,AGG secure;
    class V,DEF,GM defense;
    class INV,POI,RES attack;
    class U,PR,RB,PF,PX eval;
    class RG,CERT,HOLD gov;
    class MR,AL,RP ops;
    class UI1,UI2,UI3,UI4,UI5,UI6 ui;


Private Data
     │
     ▼
Local Data Holders
     │
     ▼
Non-IID Partitioning
     │
     ▼
Local Model Training
     │
     ▼
Per-Sample Clipping
     │
     ▼
Differential Privacy
     │
     ▼
Pairwise Secure Aggregation
     │
     ▼
Robust Update Validation
     │
     ▼
Global Detection Model
     │
     ├──────────────► Gradient Inversion Evaluation
     │
     └──────────────► Byzantine Attack Evaluation
                              │
                              ▼
                   Security + Utility Evaluation
                              │
                              ▼
                       Release Governance
                       ┌──────┴──────┐
                       ▼             ▼
                    APPROVE        HOLD
                       │             │
                       ▼             ▼
                 Model Registry   Quarantine
                       │
                       ▼
                 Audit + Reports


DATA HOLDERS
     │
     ▼
LOCAL TRAINING
     │
     ▼
DIFFERENTIAL PRIVACY
     │
     ▼
SECURE AGGREGATION
     │
     ▼
ROBUST FEDERATION
     │
     ▼
GLOBAL MODEL
     │
     ├──► INVERSION TEST
     ├──► POISONING TEST
     └──► DEFENSE EVALUATION
              │
              ▼
      PRIVACY / UTILITY / ROBUSTNESS
              │
              ▼
        RELEASE GOVERNANCE
           ┌───────┐
           │ PASS  │──► MODEL REGISTRY
           └───────┘
           ┌───────┐
           │ HOLD  │──► QUARANTINE
           └───────┘
