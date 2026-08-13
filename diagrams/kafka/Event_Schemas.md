# Event Schemas — Real-Time Fraud Detection

## 1. Purpose

This document defines the event contracts exchanged between fraud detection
services through Apache Kafka.

All events contain a common envelope followed by an event-specific payload.

---

## 2. Common Event Envelope

```json
{
  "eventId": "uuid",
  "eventType": "string",
  "eventVersion": "1",
  "occurredAt": "2026-08-12T10:15:30Z",
  "source": "transaction-service",
  "traceId": "uuid",
  "correlationId": "uuid",
  "partitionKey": "string",
  "payload": {}
}