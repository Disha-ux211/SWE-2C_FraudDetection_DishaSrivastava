# Fraud Detection Saga — Transaction Workflow

## 1. Purpose

The Fraud Detection Saga coordinates the distributed transaction workflow across
multiple fraud-detection services.

The workflow ensures that a payment transaction can move through validation,
fraud analysis, risk scoring, and final decision handling without requiring one
large distributed database transaction.

The Saga uses events and compensating actions where required.

---

## 2. Saga Participants

| Service | Responsibility |
|---|---|
| Transaction Service | Creates and manages transaction state |
| Rule Engine | Performs deterministic fraud-rule evaluation |
| Anomaly Detection Service | Produces ML/anomaly risk signal |
| Graph Analysis Service | Produces relationship-based risk signal |
| Risk Scoring Service | Aggregates signals and produces final decision |
| Case Management Service | Creates cases for manual investigation |
| Notification Service | Sends customer notifications |
| Audit & Compliance Service | Records important events |

---

## 3. Main Saga Flow

```text
                    Transaction Received
                           |
                           v
                 +--------------------+
                 | Transaction Service|
                 +---------+----------+
                           |
                           v
                  Transaction Validated
                           |
                           v
                  Transaction Enriched
                           |
              +------------+------------+
              |            |            |
              v            v            v
        Rule Engine   Anomaly Detection Graph Analysis
              |            |            |
              v            v            v
        Rule Signal    ML Signal      Graph Signal
              |            |            |
              +------------+------------+
                           |
                           v
                  Risk Scoring Service
                           |
                           v
                    Risk Score
                           |
                           v
                    Decision Engine
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
     Auto-Approve      Step-Up          Manual Review
          |                |                |
          |                v                v
          |        Authentication       Fraud Case
          |          Requested           Created
          |                |                |
          |                v                v
          |        Authentication        Analyst
          |           Completed          Review
          |                |                |
          +----------------+----------------+
                           |
                           v
                    Final Decision
                           |
                           v
                    Audit Event