# Kafka Topic Topology — Real-Time Fraud Detection

## 1. Purpose

Apache Kafka is the asynchronous event backbone for the fraud detection
platform.

The topology supports real-time transaction processing, independent
consumer scaling, replay, fault isolation and auditability.

---

## 2. Topic Design

| Topic | Partitions | Replication | Retention | Key | Consumer Groups |
|---|---:|---:|---|---|---|
| fraud.transactions.raw | 12 | 3 | 30 days | card_number_hash | rule-engine, anomaly-detection, graph-analysis |
| fraud.transactions.enriched | 12 | 3 | 30 days | transaction_id | rule-engine, anomaly-detection, graph-analysis |
| fraud.rules.evaluated | 8 | 3 | 14 days | transaction_id | risk-scoring |
| fraud.anomalies.detected | 8 | 3 | 14 days | transaction_id | risk-scoring |
| fraud.graphs.analysed | 8 | 3 | 14 days | transaction_id | risk-scoring |
| fraud.risk.decisions | 8 | 3 | 30 days | transaction_id | case-management, notification |
| fraud.cases.events | 6 | 3 | 30 days | case_id | notification, audit-compliance |
| fraud.notifications | 6 | 3 | 7 days | customer_id | notification |
| fraud.audit.events | 6 | 3 | infinite | event_id | audit-compliance |

---

## 3. Topic Naming Convention

Topics follow:

domain.entity.action

Examples:

fraud.transactions.raw
fraud.transactions.enriched
fraud.rules.evaluated
fraud.anomalies.detected
fraud.graphs.analysed
fraud.risk.decisions
fraud.cases.events
fraud.notifications
fraud.audit.events

---

## 4. Transaction Lifecycle

```text
fraud.transactions.raw
          |
          v
fraud.transactions.enriched
          |
     +----+----+
     |         |
     v         v
   Rules    Anomaly Detection
     |         |
     v         v
fraud.rules.evaluated
fraud.anomalies.detected
     \         /
      \       /
       v     v
    Graph Analysis
          |
          v
fraud.graphs.analysed
          |
          v
    Risk Scoring
          |
          v
fraud.risk.decisions
       /       \
      v         v
   Cases    Notifications
      |         |
      v         v
fraud.cases.events
      |
      v
fraud.audit.events