# SLA Specification

## 1. Overview

The fraud detection platform must support real-time transaction processing with
defined latency, availability, throughput, and recovery objectives.

The following SLA targets are based on the requirements specified for the
target architecture.

## 2. Service Level Objectives

| Metric | Target |
|---|---|
| Transaction API availability | 99.99% |
| Fraud decision availability | 99.99% |
| Transaction ingestion latency | p95 < 50 ms |
| End-to-end fraud decision latency | p95 < 200 ms |
| Rule Engine latency | p95 < 50 ms |
| Anomaly Detection latency | p95 < 100 ms |
| Graph Analysis latency | p95 < 100 ms |
| Risk Scoring latency | p95 < 50 ms |
| Kafka event processing | Near real-time |
| Recovery Point Objective | ≤ 5 minutes |
| Recovery Time Objective | ≤ 30 minutes |

## 3. Availability Requirements

Critical transaction-path services must target 99.99% availability.

These services include:

- Transaction Processing
- Fraud Rules
- Anomaly Detection
- Graph Analysis
- Risk Scoring

High availability should be achieved through:

- Multiple service instances
- Kubernetes deployment
- Health checks
- Automatic restart
- Load balancing
- Horizontal scaling
- Multi-availability-zone deployment
- Circuit breakers
- Retry policies

## 4. Latency Requirements

The fraud decision must be generated quickly enough to participate in real-time
transaction authorization.

The target is:

```text
Transaction Request
        ↓
Validation
        ↓
Parallel Detection
 ┌──────┼────────┐
 ↓      ↓        ↓
Rules   ML      Graph
 └──────┼────────┘
        ↓
   Risk Scoring
        ↓
    Final Decisions