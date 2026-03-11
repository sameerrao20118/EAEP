# Hyper-Personalized Retail Banking Experience v2

## 1. Problem Statement
Current banking apps are reactive, generic, and fragmented. Customers often receive late warnings, limited financial coaching, and little context around spending, subscriptions, or upcoming cashflow issues. Although banks already hold rich customer and transaction data, that data is underused and not connected into timely, event-driven experiences.

Customers now expect proactive, personalized support based on their age group, spending habits, subscriptions, and financial behavior. They want precise insights, early warnings, and relevant nudges through channels they already use.

This gap has measurable business impact:
- 67% of customers face at least one monthly financial surprise.
- 30% do not receive early warning on upcoming issues.
- GBP 1.4 billion is lost each year to forgotten subscriptions.
- 57% of Gen Z prefer apps that coach them rather than simply inform them.

If banks fail to respond, they risk higher support volumes, weaker engagement, and customer churn to more adaptive competitors. In a fast-changing UK banking market, a rapid turnaround is required to protect the existing base and attract newer segments such as Gen Z.

## 2. Conceptual Diagram
```mermaid
flowchart TD
    U[Mobile and Web Channels] --> G[API Gateway]
    G --> O[Experience Orchestrator]
    G --> C[Consent Service]
    G --> A[Identity Provider]

    O --> I[Insights Service]
    O --> X[Actions Service]
    O --> S[Subscriptions Service]
    O --> N[Notification Service]
    O --> K[Event Bus]

    K --> F[Stream Enrichment]
    F --> T[Feature Store]
    T --> M[Model Service]
    T --> R[Rules Engine]

    M --> O
    R --> O

    B[Core Banking Systems] --> E[Ingestion Connectors]
    P[Open Banking Providers] --> E
    D[CRM and Customer Data] --> E
    E --> K

    K --> L[Lakehouse]
    L --> T
```

## 3. Interface Diagram
```mermaid
flowchart LR
    APP[Client Apps] --> GW[API Gateway]
    GW --> AUTH[Identity Provider]
    GW --> CONSENT[Consent Service]
    GW --> ORCH[Experience Orchestrator]

    ORCH --> INSIGHTS[Insights API]
    ORCH --> ACTIONS[Actions API]
    ORCH --> SUBS[Subscriptions API]
    ORCH --> NOTIFY[Notification API]
    ORCH --> BUS[Event Bus]

    BUS --> ENRICH[Enrichment]
    ENRICH --> FEATURES[Feature Store]
    FEATURES --> MODELS[Model API]
    FEATURES --> RULES[Rules API]

    CORE[Core Banking] --> INGEST[Ingest API]
    OB[Open Banking] --> INGEST
    CRM[CRM] --> INGEST
    INGEST --> BUS
```

## 4. API Surface
- `GET /v1/users/{id}/insights`
  Returns personalized insights with message, reason, severity, and confidence.

- `GET /v1/users/{id}/cashflow/forecast`
  Returns a short-term cashflow prediction and upcoming risk indicators.

- `GET /v1/users/{id}/subscriptions`
  Returns detected recurring payments, renewal patterns, and risk flags.

- `POST /v1/users/{id}/subscriptions/{subscriptionId}/cancel`
  Starts a subscription cancellation or partner handoff flow.

- `POST /v1/users/{id}/actions/transfer`
  Moves funds between eligible accounts to reduce shortfall risk.

- `GET /v1/users/{id}/notifications`
  Returns notification history across push, in-app, and email.

- `POST /v1/consents`
  Creates or updates customer consent for personalization, channels, and data usage.

- `GET /v1/consents/{consentId}`
  Returns consent scope, status, and audit metadata.
