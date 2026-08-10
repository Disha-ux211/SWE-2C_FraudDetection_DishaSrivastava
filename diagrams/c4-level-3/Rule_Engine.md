# C4 Level 3 — Rule Engine

## 1. Purpose

The Rule Engine evaluates payment transactions against configurable fraud
detection rules.

It replaces the legacy monolith's hard-coded Java switch statements with
centrally managed, versioned, and independently deployable fraud rules.

## 2. Components

| Component | Responsibility |
|---|---|
| Rule Controller | Receives rule-management and evaluation requests |
| Rule Evaluation Component | Evaluates active rules against transaction data |
| Rule Repository | Stores rule definitions and metadata |
| Rule Version Manager | Maintains rule versions and lifecycle |
| Rule Cache | Provides low-latency access to active rules |
| Rule Validator | Validates rule syntax and configuration |
| Rule Simulator | Tests rules against sample or historical transactions |
| Rule Event Publisher | Publishes rule evaluation events |
| Audit Component | Records rule changes and important actions |

## 3. Component Diagram

```text
                    Transaction
                         │
                         ▼
              ┌─────────────────────┐
              │   Rule Controller   │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │ Rule Evaluation     │
              │ Component           │
              └──────────┬──────────┘
                         │
             ┌───────────┴───────────┐
             ▼                       ▼
    ┌─────────────────┐     ┌─────────────────┐
    │   Rule Cache    │     │ Rule Repository │
    │     Redis       │     │   PostgreSQL    │
    └────────┬────────┘     └─────────────────┘
             │
             ▼
    ┌─────────────────────┐
    │ Active Rule Set     │
    └──────────┬──────────┘
               │
               ▼
    ┌─────────────────────┐
    │ Rule Evaluation     │
    └──────────┬──────────┘
               │
               ▼
       Rule Match Signals
               │
       ┌───────┴────────┐
       ▼                ▼
┌──────────────┐  ┌───────────────┐
│ Event        │  │ Audit         │
│ Publisher    │  │ Component     │
└──────┬───────┘  └───────────────┘
       │
       ▼
     Kafka


          RULE MANAGEMENT FLOW

┌─────────────────────┐
│ Rule Administrator  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Rule Controller     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Rule Validator      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Rule Version        │
│ Manager             │
└──────┬──────────────┘
       │
       ├──────────────► Rule Repository
       │
       ├──────────────► Rule Cache
       │
       └──────────────► Audit Component


          RULE SIMULATION

┌─────────────────────┐
│ Historical / Sample │
│ Transactions        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Rule Simulator      │
└──────────┬──────────┘
           │
           ▼
     Simulation Results