# Deployment Architecture

## 1. Overview

The Real-Time Fraud Detection Platform is deployed as a set of independently
scalable services.

The deployment architecture contains:

- API Gateway
- Transaction Service
- Rule Engine
- Anomaly Detection Service
- Graph Analysis Service
- Risk Scoring Service
- Customer Profile Service
- Case Management Service
- Notification Service
- Audit & Compliance Service
- Kafka
- PostgreSQL
- Redis
- Neo4j
- Observability components

---

## 2. Deployment Structure

```text
                         Internet
                            |
                            v
                     API Gateway
                            |
          +-----------------+-----------------+
          |                 |                 |
          v                 v                 v
 Transaction Service   Customer Profile   Case Management
          |
          +-------------------+
          |                   |
          v                   v
     Rule Engine       Anomaly Detection
          |                   |
          +---------+---------+
                    |
                    v
             Risk Scoring
                    |
          +---------+---------+
          |         |         |
          v         v         v
       Kafka      Redis     PostgreSQL
                    |
                    v
                  Neo4j

       Prometheus + Grafana + OpenTelemetry
                    |
                    v
             All Services