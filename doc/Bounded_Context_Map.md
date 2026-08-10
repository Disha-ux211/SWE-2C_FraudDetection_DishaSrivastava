# Bounded Context Map

## 1. Overview

The target fraud detection platform is divided into bounded contexts based on
distinct business capabilities.

Each bounded context owns its domain model and communicates with other contexts
through well-defined APIs and domain events.

## 2. Core Bounded Contexts

| Bounded Context | Primary Responsibility | Main Data |
|---|---|---|
| Transaction Processing | Receive, validate and enrich transactions | Transactions |
| Fraud Rules | Evaluate configurable fraud rules | Rules, rule versions |
| Anomaly Detection | Identify behavioural anomalies using ML | Features, model scores |
| Graph Analysis | Identify suspicious relationships and fraud rings | Graph entities and relationships |
| Risk Scoring | Combine detection signals and produce final risk score | Risk scores |
| Customer Profile | Maintain customer behavioural and profile information | Customer profiles |
| Case Management | Manage fraud investigations | Cases, decisions |
| Notification | Send transaction and fraud-related notifications | Notification records |
| Audit & Compliance | Maintain audit records and regulatory evidence | Audit events |
| Reporting | Provide operational and regulatory reporting | Reporting read models |

## 3. Context Relationships

### Transaction Processing → Fraud Rules

The Transaction Processing context sends transaction information to the Fraud
Rules context for deterministic fraud evaluation.

### Transaction Processing → Anomaly Detection

Transaction features are provided to the Anomaly Detection context for real-time
machine-learning scoring.

### Transaction Processing → Graph Analysis

Relevant transaction entities and relationships are supplied to the Graph
Analysis context.

### Fraud Rules → Risk Scoring

The Rule Engine produces rule-match signals that are consumed by Risk Scoring.

### Anomaly Detection → Risk Scoring

The ML/Anomaly Detection service produces an anomaly score and explanation
signals for Risk Scoring.

### Graph Analysis → Risk Scoring

Graph Analysis provides relationship-based fraud signals and graph scores.

### Customer Profile → Detection Services

Customer profile and behavioural information is made available to the detection
services as required.

### Risk Scoring → Case Management

High-risk transactions can result in fraud cases being created for manual review.

### Risk Scoring → Notification

Risk decisions can trigger customer notifications or step-up authentication
flows.

### All Core Contexts → Audit & Compliance

Important business and security actions generate audit events.

## 4. Context Map

```text
                         ┌─────────────────────┐
                         │  Customer Profile   │
                         └──────────┬──────────┘
                                    │
                                    ▼
┌─────────────────────┐      ┌─────────────────────┐
│ Transaction         │─────▶│    Fraud Rules      │
│ Processing          │      └──────────┬──────────┘
└──────────┬──────────┘                 │
           │                            │
           ├──────────────────────┐     │
           │                      │     │
           ▼                      ▼     ▼
┌─────────────────────┐  ┌─────────────────────┐
│ Anomaly Detection   │  │   Graph Analysis    │
└──────────┬──────────┘  └──────────┬──────────┘
           │                        │
           │                        │
           └────────────┬───────────┘
                        ▼
              ┌─────────────────────┐
              │    Risk Scoring     │
              └──────────┬──────────┘
                         │
                ┌────────┼─────────┐
                ▼        ▼         ▼
        ┌────────────┐ ┌────────┐ ┌──────────────┐
        │Case Mgmt   │ │Notify  │ │Audit &       │
        │            │ │        │ │Compliance    │
        └────────────┘ └────────┘ └──────────────┘
                                      │
                                      ▼
                              ┌──────────────┐
                              │  Reporting   │
                              └──────────────┘