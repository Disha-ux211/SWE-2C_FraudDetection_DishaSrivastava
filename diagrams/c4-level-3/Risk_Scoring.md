# C4 Level 3 — Risk Scoring Service

## 1. Purpose

The Risk Scoring Service is responsible for combining fraud signals from the
Rule Engine, Anomaly Detection Service, and Graph Analysis Service.

It produces a composite risk score and converts that score into an explainable
fraud decision.

## 2. Components

| Component | Responsibility |
|---|---|
| Risk Controller | Receives risk-scoring requests |
| Signal Collector | Collects signals from detection services |
| Rule Signal Processor | Processes Rule Engine results |
| ML Signal Processor | Processes anomaly/ML results |
| Graph Signal Processor | Processes graph-analysis results |
| Risk Aggregator | Combines multiple fraud signals |
| Decision Engine | Maps risk score to a business decision |
| Explanation Builder | Creates a human-readable decision explanation |
| Risk Repository | Stores risk scores and decisions |
| Event Publisher | Publishes final risk events |
| Policy Configuration | Stores decision thresholds and policies |

## 3. Component Diagram

```text
                    Transaction
                         │
                         ▼
              ┌─────────────────────┐
              │   Risk Controller   │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │   Signal Collector  │
              └──────┬──────┬───────┘
                     │      │
          ┌──────────┘      └─────────────┐
          ▼                               ▼
┌──────────────────┐             ┌──────────────────┐
│ Rule Signal      │             │ ML Signal        │
│ Processor        │             │ Processor        │
└────────┬─────────┘             └────────┬─────────┘
         │                                │
         │                                │
         └──────────────┬─────────────────┘
                        │
                        ▼
              ┌─────────────────────┐
              │ Graph Signal         │
              │ Processor            │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │   Risk Aggregator   │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │    Decision Engine  │
              └──────┬──────────────┘
                     │
            ┌────────┼─────────┐
            ▼        ▼         ▼
      Auto-Approve  Step-Up  Manual Review
                          │
                          ▼
                     Auto-Decline