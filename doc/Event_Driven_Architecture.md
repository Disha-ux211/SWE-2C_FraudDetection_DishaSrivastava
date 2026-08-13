# Event-Driven Architecture Specification

## 1. Purpose

This document defines the event-driven architecture for the Real-Time Fraud
Detection Platform.

The architecture uses Apache Kafka as the asynchronous event backbone.

The design supports:

- Real-time fraud detection
- Loose coupling between services
- Independent scaling
- Fault isolation
- Event replay
- Auditability
- CQRS read models
- Saga-based business workflows

---

## 2. Event-Driven Architecture Overview

```text
                    Transaction Channels
                           |
                           v
                  Transaction Service
                           |
                           v
                 Kafka Event Platform
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
     Rule Engine     Anomaly Detection   Graph Analysis
          |                |                |
          +----------------+----------------+
                           |
                           v
                   Risk Scoring Service
                           |
              +------------+------------+
              |                         |
              v                         v
       Case Management             Notification
              |                         |
              +------------+------------+
                           |
                           v
                  Audit & Compliance