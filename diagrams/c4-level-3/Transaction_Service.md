# C4 Level 3 — Transaction Service

## 1. Purpose

The Transaction Service is responsible for receiving, validating, enriching,
and initiating fraud analysis for payment transactions.

It is part of the critical real-time transaction path.

## 2. Components

| Component | Responsibility |
|---|---|
| Transaction Controller | Receives external transaction requests |
| Validation Component | Validates transaction structure and required fields |
| Enrichment Component | Adds customer, device, location, and merchant information |
| Transaction Processor | Coordinates transaction processing |
| Fraud Orchestration Component | Initiates fraud detection |
| Transaction Repository | Persists transaction state |
| Event Publisher | Publishes transaction events to Kafka |
| Idempotency Component | Prevents duplicate transaction processing |

## 3. Component Diagram

```text
                 Transaction Request
                         │
                         ▼
              ┌─────────────────────┐
              │ Transaction         │
              │ Controller          │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │ Validation          │
              │ Component           │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │ Idempotency         │
              │ Component           │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │ Enrichment          │
              │ Component           │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │ Transaction         │
              │ Processor           │
              └──────┬──────┬───────┘
                     │      │
              ┌──────┘      └─────────────┐
              ▼                           ▼
    ┌──────────────────┐        ┌──────────────────┐
    │ Fraud            │        │ Transaction      │
    │ Orchestration    │        │ Repository       │
    └────────┬─────────┘        └────────┬─────────┘
             │                           │
             ▼                           ▼
       Detection Services             PostgreSQL
             │
             ▼
       ┌──────────────┐
       │ Event        │
       │ Publisher    │
       └──────┬───────┘
              │
              ▼
           Kafka