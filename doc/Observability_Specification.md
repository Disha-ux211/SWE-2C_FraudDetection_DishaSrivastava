# Observability Specification — Real-Time Fraud Detection Platform

## 1. Purpose

Observability provides visibility into the health, performance and behaviour
of all services in the Real-Time Fraud Detection Platform.

The platform uses:

- Prometheus for metrics
- Grafana for dashboards
- OpenTelemetry for distributed tracing
- Centralized application logs

---

## 2. Metrics

The following metrics are collected from all services.

### Transaction Metrics

- Transactions received
- Transactions processed
- Transactions rejected
- Transactions approved
- Transactions declined
- Transactions requiring review
- Transaction processing latency

### Fraud Detection Metrics

- Rule evaluations
- Rules triggered
- Anomaly detections
- Graph analyses
- Fraud decisions
- Average risk score
- High-risk transaction count

### Kafka Metrics

- Messages published
- Messages consumed
- Consumer lag
- Processing failures
- Retry count
- Dead-letter messages

### Service Metrics

- Request count
- Error rate
- Response latency
- CPU usage
- Memory usage
- Instance availability

---

## 3. Distributed Tracing

OpenTelemetry provides distributed tracing across the transaction workflow.

Example:

```text
API Gateway
     |
     v
Transaction Service
     |
     +----> Rule Engine
     |
     +----> Anomaly Detection
     |
     +----> Graph Analysis
     |
     v
Risk Scoring
     |
     +----> Case Management
     |
     +----> Notification