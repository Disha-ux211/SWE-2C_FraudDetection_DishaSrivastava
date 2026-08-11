# C4 Level 1 — System Context

## Real-Time Fraud Detection Microservices Platform

### Purpose

The Real-Time Fraud Detection Platform analyses financial transactions in
real time and determines whether a transaction is legitimate, suspicious,
or fraudulent.

The platform uses rule-based detection, anomaly detection, graph-based
relationship analysis, and risk scoring.

---

## System Context

```text
                    +----------------------+
                    |      Customers       |
                    |  / Banking Users     |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Transaction Channels |
                    | Web / Mobile / POS   |
                    +----------+-----------+
                               |
                               v
        +------------------------------------------------+
        |                                                |
        |   REAL-TIME FRAUD DETECTION PLATFORM            |
        |                                                |
        |  - Transaction Processing                      |
        |  - Rule Engine                                 |
        |  - Anomaly Detection                           |
        |  - Graph Analysis                              |
        |  - Risk Scoring                                |
        |  - Case Management                             |
        |  - Notifications                               |
        |                                                |
        +----------------------+-------------------------+
                               |
              +----------------+----------------+
              |                                 |
              v                                 v
   +----------------------+          +----------------------+
   | Banking / Payment    |          | External Intelligence|
   | Systems              |          | & Risk Data Sources  |
   +----------------------+          +----------------------+

                               |
                               v
                    +----------------------+
                    | Fraud Analysts /     |
                    | Compliance Teams     |
                    +----------------------+