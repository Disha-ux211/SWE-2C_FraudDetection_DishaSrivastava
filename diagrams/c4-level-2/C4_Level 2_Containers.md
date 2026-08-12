# C4 Level 2 — Container Diagram

## 1. Purpose

The C4 Level 2 view decomposes the Real-Time Fraud Detection Platform into
major containers/services and shows how they communicate.

## 2. Containers

| Container | Responsibility | Technology |
|---|---|---|
| API Gateway | External API entry point, authentication, routing and rate limiting | Spring Cloud Gateway |
| Transaction Service | Receives and validates transactions | Java / Spring Boot |
| Rule Engine | Evaluates configurable fraud rules | Java / Spring Boot |
| Anomaly Detection Service | Performs ML-based anomaly detection | Python / FastAPI |
| Graph Analysis Service | Performs relationship and fraud-ring analysis | Java / Spring Boot + Neo4j |
| Risk Scoring Service | Combines detection signals and produces final risk score | Java / Spring Boot |
| Customer Profile Service | Provides customer profile and behavioural information | Java / Spring Boot |
| Case Management Service | Creates and manages fraud investigation cases | Java / Spring Boot |
| Notification Service | Sends customer notifications and authentication requests | Java / Spring Boot |
| Audit & Compliance Service | Records auditable business and security events | Java / Spring Boot |
| Kafka Event Platform | Transports asynchronous events between services | Apache Kafka |
| PostgreSQL | Stores relational transactional data | PostgreSQL |
| Redis | Provides low-latency cache and real-time feature storage | Redis |
| Neo4j | Stores fraud graph relationships | Neo4j |
| Observability Platform | Metrics, logs and distributed traces | Prometheus / Grafana / OpenTelemetry |

## 3. Container Relationships

```text
                         ┌──────────────────┐
                         │    Customer      │
                         └────────┬─────────┘
                                  │
                              HTTPS / REST
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │    API Gateway      │
                       └──────────┬──────────┘
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │ Transaction Service │
                       └──────┬──────┬───────┘
                              │      │
                  ┌───────────┘      └────────────┐
                  ▼                               ▼
        ┌─────────────────┐             ┌──────────────────┐
        │   Rule Engine   │             │ Customer Profile │
        └────────┬────────┘             └──────────────────┘
                 │
                 │
        ┌────────┴─────────────┐
        │                      │
        ▼                      ▼
┌──────────────────┐   ┌──────────────────┐
│ Anomaly Detection│   │  Graph Analysis  │
│ Service          │   │     Service      │
└────────┬─────────┘   └────────┬─────────┘
         │                      │
         │                      ▼
         │               ┌──────────────┐
         │               │    Neo4j     │
         │               └──────────────┘
         │
         ▼
┌────────────────────────────────────────┐
│          Risk Scoring Service          │
└───────────────────┬────────────────────┘
                    │
             Risk Decision
                    │
          ┌─────────┼──────────┐
          ▼         ▼          ▼
 ┌──────────────┐ ┌────────┐ ┌───────────────┐
 │ Case Mgmt    │ │Notify  │ │ Audit &       │
 │ Service      │ │Service │ │ Compliance    │
 └──────────────┘ └────────┘ └───────────────┘


        ASYNCHRONOUS EVENT COMMUNICATION

┌─────────────────────────────────────────────────────┐
│                  Kafka Event Platform               │
└───────┬────────────┬─────────────┬──────────────────┘
        │            │             │
        ▼            ▼             ▼
   Rule Engine   ML Service   Graph Analysis
        │            │             │
        └────────────┼─────────────┘
                     ▼
              Risk Scoring


        PERSISTENCE

┌──────────────────┐   ┌──────────────────┐
│   PostgreSQL     │   │      Redis       │
│ Transactional    │   │ Cache / Features │
└──────────────────┘   └──────────────────┘

                    ┌──────────────────┐
                    │      Neo4j       │
                    │  Fraud Graph     │
                    └──────────────────┘


        OBSERVABILITY

┌──────────────────────────────────────────────────┐
│ Prometheus + Grafana + OpenTelemetry             │
└──────────────────────────────────────────────────┘
             ▲             ▲             ▲
             │             │             │
       Metrics         Logs/Traces     Alerts
             │             │             │
             └─────────────┼─────────────┘
                           │
                    All Services
                    ## 4. Communication Contracts

### 4.1 Synchronous gRPC Communication

The following synchronous interactions use internal gRPC communication.

| Source | Target | RPC Method | Purpose |
|---|---|---|---|
| Transaction Service | Customer Profile Service | `GetProfile` | Retrieve customer and profile information |
| Transaction Service | Rule Engine | `Evaluate` | Evaluate transaction against fraud rules |
| Risk Scoring Service | Customer Profile Service | `GetProfile` | Retrieve profile information required for risk scoring |
| Risk Scoring Service | Rule Engine | `Evaluate` | Request rule evaluation when required |
| API Gateway | Transaction Service | `CreateTransaction` | Submit a transaction |
| API Gateway | Case Management Service | `GetCases` | Retrieve investigation cases |
| API Gateway | Risk Scoring Service | `GetRiskScore` | Retrieve the risk score for a transaction |

### 4.2 Asynchronous Kafka Communication

The following interactions use asynchronous Kafka events.

| Producer | Consumer | Kafka Topic | Purpose |
|---|---|---|---|
| Transaction Service | Rule Engine | `transaction-events` | Publish validated transactions |
| Transaction Service | Anomaly Detection Service | `transaction-events` | Provide transaction data for anomaly detection |
| Transaction Service | Graph Analysis Service | `transaction-events` | Update graph relationships |
| Rule Engine | Risk Scoring Service | `rule-events` | Publish rule evaluation results |
| Anomaly Detection Service | Risk Scoring Service | `anomaly-events` | Publish anomaly scores |
| Graph Analysis Service | Risk Scoring Service | `graph-events` | Publish graph analysis results |
| Risk Scoring Service | Case Management Service | `risk-decision-events` | Create cases for review-required decisions |
| Risk Scoring Service | Notification Service | `risk-decision-events` | Notify customers about risk decisions |
| Case Management Service | Notification Service | `case-events` | Publish case lifecycle changes |
| All Services | Audit & Compliance Service | `audit-events` | Record auditable business and security events |

### 4.3 External API Communication

External clients communicate with the API Gateway using HTTPS/REST.

Example external endpoints include:

```text
POST /api/v1/transactions
GET  /api/v1/transactions/{id}/risk-score
GET  /api/v1/cases
PUT  /api/v1/cases/{id}/decision
GET  /api/v1/rules
POST /api/v1/rules
GET  /api/v1/dashboard/metrics

The endpoint requirements above are directly specified in the project document. :contentReference[oaicite:1]{index=1}

### One more thing

Your existing Level 2 diagram is fine as a **high-level visual**, so don't rewrite the whole thing.

Just add the communication-contract section above, save it, and then we'll commit the update.

**Don't push yet.** Tell me **“updated”** after you've added it.