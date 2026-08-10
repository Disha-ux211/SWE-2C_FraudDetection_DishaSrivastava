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