# Deployment Architecture — Real-Time Fraud Detection Platform

## 1. Purpose

The deployment architecture describes how the Real-Time Fraud Detection Platform
is deployed across application, messaging, data, and observability infrastructure.

The design supports:

- High availability
- Horizontal scaling
- Fault isolation
- Real-time processing
- Secure communication
- Independent service deployment
- Observability

---

## 2. High-Level Deployment

```text
                         USERS / EXTERNAL SYSTEMS
                                  |
                                  v
                        +-------------------+
                        |   Load Balancer   |
                        +---------+---------+
                                  |
                                  v
                        +-------------------+
                        |    API Gateway    |
                        +---------+---------+
                                  |
             +--------------------+--------------------+
             |                    |                    |
             v                    v                    v
      +-------------+      +-------------+      +-------------+
      | Transaction |      | Rule Engine |      | Risk        |
      | Service     |      |             |      | Scoring     |
      +------+------+      +------+------+      +------+------+
             |                    |                    |
             |                    |                    |
             +--------------------+--------------------+
                                  |
                                  v
                        +-------------------+
                        |   Kafka Cluster   |
                        +---------+---------+
                                  |
             +--------------------+--------------------+
             |                    |                    |
             v                    v                    v
      +-------------+      +-------------+      +-------------+
      | Anomaly     |      | Graph       |      | Notification|
      | Detection   |      | Analysis    |      | Service     |
      +------+------+      +------+------+      +-------------+
             |                    |
             v                    v
       +-----------+        +-----------+
       |   Redis   |        |   Neo4j   |
       +-----------+        +-----------+
             |
             v
       +-------------+
       | PostgreSQL  |
       +-------------+

                    OBSERVABILITY
                           |
                           v
            +-------------------------------+
            | Prometheus / Grafana /        |
            | OpenTelemetry / Logging       |
            +-------------------------------+