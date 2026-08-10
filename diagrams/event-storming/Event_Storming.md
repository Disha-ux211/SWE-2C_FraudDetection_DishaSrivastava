# Event Storming — Real-Time Fraud Detection Platform

## 1. Purpose

Event Storming is used to identify the major business events, commands,
actors, policies, aggregates, and external systems involved in the real-time
fraud detection domain.

The event flow below focuses on the transaction-to-fraud-decision journey.

---

## 2. Primary Business Flow

```text
Customer initiates payment
          |
          v
[Transaction Received]
          |
          v
[Transaction Validated]
          |
          v
[Transaction Enriched]
          |
          +-------------------+
          |                   |
          v                   v
[Rules Evaluated]      [Features Retrieved]
          |                   |
          v                   v
[Rule Signal Generated] [Anomaly Score Generated]
          |                   |
          |                   |
          +---------+---------+
                    |
                    v
          [Graph Analysis Started]
                    |
                    v
          [Graph Risk Calculated]
                    |
                    v
          [Risk Score Calculated]
                    |
                    v
          [Fraud Decision Made]
                    |
        +-----------+-----------+-------------+
        |           |           |             |
        v           v           v             v
  Auto-Approve   Step-Up    Manual Review  Auto-Decline
                 Auth