# Service Decomposition Table — Real-Time Fraud Detection

## 1. Purpose

This document defines the proposed microservice boundaries for the
Real-Time Fraud Detection platform.

The decomposition follows the project requirements and applies these
principles:

* Business capability alignment
* Data ownership
* Team alignment
* Change frequency
* Scalability requirements
* Failure-domain isolation

The target is 8–12 independently deployable services. Each service owns
its business capability and its persistent data.

---

## 2. Service Decomposition

| # | Service                       | Bounded Context       | Primary Responsibility                                                                               | Technology         | Data Store                 | Team Ownership               |
| - | ----------------------------- | --------------------- | ---------------------------------------------------------------------------------------------------- | ------------------ | -------------------------- | ---------------------------- |
| 1 | Transaction Ingestion Service | Transaction Ingestion | Receive, validate, normalise and enrich transactions from payment channels                           | Java + Spring Boot | PostgreSQL                 | Transaction Platform Team    |
| 2 | Rule Engine Service           | Fraud Analysis        | Evaluate transactions against configurable deterministic fraud rules                                 | Java + Spring Boot | PostgreSQL                 | Fraud Rules Team             |
| 3 | Anomaly Detection Service     | Fraud Analysis        | Calculate ML-based anomaly scores for suspicious transaction behaviour                               | Python + FastAPI   | Feature Store + PostgreSQL | ML Detection Team            |
| 4 | Graph Analysis Service        | Fraud Analysis        | Analyse relationships between customers, accounts, devices and transactions to detect fraud networks | Python + FastAPI   | Neo4j                      | Graph Intelligence Team      |
| 5 | Risk Scoring Service          | Risk Scoring          | Combine rule, anomaly and graph signals into an explainable risk score and final decision            | Java + Spring Boot | PostgreSQL                 | Risk Decision Team           |
| 6 | Case Management Service       | Case Management       | Create, assign, investigate and resolve fraud investigation cases                                    | Java + Spring Boot | PostgreSQL                 | Fraud Operations Team        |
| 7 | Customer Profile Service      | Customer & Profile    | Provide customer, account and device information required for enrichment and detection               | Java + Spring Boot | PostgreSQL                 | Customer Data Team           |
| 8 | Notification Service          | Notification          | Send transaction, fraud alert and case-related notifications to customers and internal users         | Java + Spring Boot | PostgreSQL                 | Customer Communications Team |
| 9 | Audit & Compliance Service    | Audit & Compliance    | Maintain immutable audit records and support regulatory and compliance reporting                     | Java + Spring Boot | Append-only audit store    | Compliance Platform Team     |

---

## 3. Service Responsibilities

### 3.1 Transaction Ingestion Service

**Responsibility**

* Receive transactions from card networks, digital payments, UPI and other
  supported payment channels.
* Validate and normalise transaction data.
* Enrich transactions with customer and device information.
* Publish normalised transaction events.

**Primary Data**

* Transaction ID
* Amount
* Currency
* Payment channel
* Merchant information
* Customer reference
* Device information
* Transaction timestamp

**Communication**

Publishes transaction events to the event backbone and communicates with
customer/profile capabilities for enrichment.

---

### 3.2 Rule Engine Service

**Responsibility**

* Evaluate transactions against deterministic fraud rules.
* Maintain configurable rule definitions.
* Support rule versions and rule activation/deactivation.
* Produce rule evaluation results.

**Primary Data**

* Rule definitions
* Rule versions
* Rule execution results
* Rule metadata

**Communication**

Consumes transaction events and publishes rule evaluation results.

The project identifies Rule Engine as a core fraud-analysis capability and
requires it to support low-latency deterministic evaluation.

---

### 3.3 Anomaly Detection Service

**Responsibility**

* Analyse transaction behaviour using machine-learning models.
* Calculate anomaly scores.
* Identify unusual transaction patterns.
* Maintain model-related detection information.

**Primary Data**

* Detection features
* Anomaly scores
* Model version
* Model execution results

**Communication**

Consumes transaction features and publishes anomaly detection results.

The project specifically separates anomaly detection because its computational
profile may differ from deterministic rule evaluation and may require GPU
acceleration.

---

### 3.4 Graph Analysis Service

**Responsibility**

* Maintain relationships between fraud-relevant entities.
* Analyse customer, account, device and transaction relationships.
* Detect suspicious clusters and fraud networks.

**Primary Data**

* Customer nodes
* Account nodes
* Device nodes
* Transaction nodes
* Relationships
* Graph analysis results

**Technology**

Neo4j is selected because graph analysis requires a specialised graph
database.

**Communication**

Consumes transaction and entity relationship events and publishes graph
risk signals.

---

### 3.5 Risk Scoring Service

**Responsibility**

* Combine signals from rule evaluation, anomaly detection and graph
  analysis.
* Calculate the overall risk score.
* Produce an explainable fraud decision.
* Classify transactions as approved, declined or requiring review.

**Primary Data**

* Rule signals
* Anomaly scores
* Graph signals
* Risk score
* Final decision
* Decision explanation

**Communication**

Consumes detection signals and publishes the final risk decision.

The project requires the detection signals to be combined through a risk
scoring engine producing explainable, audit-ready decisions.

---

### 3.6 Case Management Service

**Responsibility**

* Create fraud investigation cases.
* Assign cases to fraud analysts.
* Track investigation status.
* Record investigation decisions.
* Resolve fraud cases.

**Primary Data**

* Fraud case
* Case priority
* Analyst assignment
* Investigation notes
* Case status
* Resolution decision

**Communication**

Consumes review-required fraud decisions and publishes case lifecycle
events.

---

### 3.7 Customer Profile Service

**Responsibility**

* Maintain customer and account information needed by fraud detection.
* Provide device and customer profile information.
* Support transaction enrichment.

**Primary Data**

* Customer profile
* Account information
* Device profile
* Customer risk attributes

**Communication**

Provides profile information to transaction processing and detection
services through APIs or internal gRPC.

The project explicitly identifies `CustomerProfileService` with
`GetProfile` and `UpdateProfile` operations for internal service
communication.

---

### 3.8 Notification Service

**Responsibility**

* Notify customers about relevant transaction and fraud decisions.
* Send internal fraud alerts.
* Send case-related notifications.
* Track notification delivery status and retries.

**Primary Data**

* Notification request
* Notification type
* Recipient reference
* Delivery status
* Retry information

**Communication**

Consumes fraud decision and case events and communicates with supported
notification channels.

---

### 3.9 Audit & Compliance Service

**Responsibility**

* Record fraud decisions and important system activities.
* Maintain an immutable audit trail.
* Support regulatory investigations and compliance reporting.
* Preserve evidence required for fraud decisions.

**Primary Data**

* Audit event
* Actor
* Resource
* Action
* Before/after state
* Trace ID
* Integrity hash
* Previous hash

**Storage**

An append-only storage design is used to protect audit records from
unauthorised modification.

The project specifically requires fraud decisions, rule changes, model
deployments and configuration changes to be captured in the audit trail.

---

## 4. Communication Summary

The services communicate using synchronous APIs/gRPC where an immediate
response is required and asynchronous events through a message broker for
decoupled processing.

The project identifies REST/gRPC and asynchronous messaging such as Apache
Kafka or RabbitMQ as supported communication approaches.

### Main Event Flow

```text
Payment Channels
       |
       v
Transaction Ingestion
       |
       +------------------+
       |                  |
       v                  v
 Rule Engine       Anomaly Detection
       |                  |
       +--------+---------+
                |
                v
        Graph Analysis
                |
                v
        Risk Scoring
                |
        +-------+-------+
        |               |
        v               v
   Notification   Case Management
        |               |
        +-------+-------+
                |
                v
        Audit & Compliance
```

---

## 5. Service Boundary Decisions

### Transaction Ingestion vs Fraud Detection

Transaction ingestion is separated from detection because transaction
processing must remain available even when an individual detection engine
fails.

### Rule Engine vs Anomaly Detection

Rules require deterministic, low-latency execution, while ML anomaly
detection can have different computational and scaling requirements.

### Anomaly Detection vs Graph Analysis

ML scoring and graph processing use different processing models and
datastores, so they are independently deployable.

### Detection Services vs Risk Scoring

Detection services produce individual signals, while Risk Scoring combines
those signals and owns the final decision.

### Risk Scoring vs Case Management

Risk scoring is an automated real-time decision capability, whereas case
management supports human investigation workflows.

### Operational Services vs Audit

Audit and compliance have independent retention, integrity and access-control
requirements and therefore require an isolated service boundary.

These boundaries support the project's required business capability,
scalability and failure-isolation heuristics.

---

## 6. Target Architecture

The proposed architecture contains **9 independently deployable services**:

1. Transaction Ingestion Service
2. Rule Engine Service
3. Anomaly Detection Service
4. Graph Analysis Service
5. Risk Scoring Service
6. Case Management Service
7. Customer Profile Service
8. Notification Service
9. Audit & Compliance Service

This falls within the project's required target of **8–12 services**.

---

## 7. Design Principles

* Each service represents a focused business capability.
* Each service owns its persistent data.
* Services communicate through defined APIs and events.
* Services can be deployed and scaled independently.
* Detection engines are isolated into separate failure domains.
* Technology choices reflect the workload of each capability.
* Audit and compliance information is kept separately from operational
  transaction data.
* The architecture supports real-time fraud decisions while maintaining
  scalability and fault isolation.
