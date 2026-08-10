# C4 Level 3 — Anomaly Detection Service

## 1. Purpose

The Anomaly Detection Service identifies unusual transaction behaviour using
machine-learning models and real-time transaction features.

It replaces the legacy platform's 15-minute batch ML scoring approach with
real-time inference.

## 2. Components

| Component | Responsibility |
|---|---|
| Inference Controller | Receives anomaly-scoring requests |
| Feature Retrieval Component | Retrieves real-time and historical features |
| Feature Engineering Component | Prepares model input features |
| Model Manager | Manages active and candidate models |
| Model Inference Component | Performs real-time ML inference |
| Model Explainability Component | Produces explanations for model predictions |
| Feature Cache | Provides low-latency feature access |
| Model Registry | Stores model versions and metadata |
| Score Publisher | Publishes anomaly scores to Kafka |
| Monitoring Component | Tracks model and inference performance |

## 3. Component Diagram

```text
                     Transaction
                          │
                          ▼
               ┌──────────────────────┐
               │ Inference Controller │
               └──────────┬───────────┘
                          │
                          ▼
               ┌──────────────────────┐
               │ Feature Retrieval    │
               │ Component            │
               └──────────┬───────────┘
                          │
                 ┌────────┴────────┐
                 ▼                 ▼
        ┌─────────────────┐  ┌──────────────────┐
        │ Feature Cache   │  │ Feature Store /  │
        │     Redis       │  │ Historical Data  │
        └────────┬────────┘  └────────┬─────────┘
                 │                    │
                 └──────────┬─────────┘
                            ▼
                 ┌──────────────────────┐
                 │ Feature Engineering  │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Model Inference      │
                 │ Component            │
                 └──────────┬───────────┘
                            │
                            ▼
                      ML Anomaly Score
                            │
                  ┌─────────┴──────────┐
                  ▼                    ▼
       ┌────────────────────┐  ┌──────────────────┐
       │ Explainability     │  │ Score Publisher  │
       │ Component          │  │                  │
       └─────────┬──────────┘  └────────┬─────────┘
                 │                      │
                 ▼                      ▼
           Explanation               Kafka
                 │
                 ▼
          Risk Scoring Service


              MODEL MANAGEMENT

┌──────────────────────┐
│ Model Registry       │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Model Manager        │
└──────────┬───────────┘
           │
     ┌─────┴──────┐
     ▼            ▼
 Champion      Challenger
 Model          Model


              MONITORING

┌──────────────────────┐
│ Monitoring Component │
└──────────┬───────────┘
           │
           ├── Inference latency
           ├── Model performance
           ├── Feature drift
           ├── Model drift
           └── Error rate