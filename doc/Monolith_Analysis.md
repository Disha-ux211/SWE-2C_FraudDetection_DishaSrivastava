# Legacy Monolith Analysis

## 1. Executive Summary

ShieldPay Financial Services is a mid-tier credit card issuer processing approximately
2.4 million transactions per day across India, Southeast Asia, and the Middle East.

Its existing fraud detection platform is a Java monolith deployed on bare-metal
servers in a co-located Mumbai data centre.

The system was originally developed in 2012 and has grown to approximately
1.2 million lines of Java code.

The current architecture is no longer suitable for the required scale, real-time
fraud detection, rapid rule changes, advanced analytics, or resilience requirements.

## 2. Current Architecture

The legacy platform has the following characteristics:

- Java monolithic application
- Bare-metal deployment
- Co-located Mumbai data centre
- Approximately 1.2 million lines of Java code
- More than 500 fraud rules hard-coded into Java switch statements
- Single Oracle 19c database
- Approximately 45 TB of database data
- Batch ML scoring every 15 minutes
- Logistic regression model
- Model trained approximately six months ago
- WAR-based deployment on Apache Tomcat 8.5
- Manual deployment process
- Four-hour maintenance window every Saturday
- No service mesh
- No containerisation
- No database read replicas
- No cross-region database replication

## 3. Business Capabilities

| Functional Area | Business Capability |
|---|---|
| Transaction processing | Transaction Ingestion |
| Transaction normalisation | Transaction Enrichment |
| Fraud switch statements | Fraud Rule Evaluation |
| Batch ML scoring | Anomaly Detection |
| Fraud case records | Case Management |
| Customer information | Customer Profile |
| Audit records | Audit and Compliance |
| Fraud alerts | Notification |
| Reporting | Regulatory Reporting |
| Database operations | Data Persistence |
| Payment integrations | Payment Network Integration |
| Analyst workflow | Fraud Investigation |

## 4. External Integrations

The target architecture must account for the following external systems:

### Payment Networks
- Visa
- Mastercard
- RuPay

### Banking Systems
- Core Banking System

The Core Banking System provides real-time balance and account-status information.

### Customer Applications
- Customer mobile application

The mobile application receives notifications and supports step-up authentication.

### Third-Party Enrichment Services
- Device fingerprinting providers
- IP geolocation providers
- Velocity-data providers

### Regulatory Systems
- RBI reporting systems
- Other jurisdiction-specific regulatory reporting systems

### Legacy Platform
The legacy monolith remains an integration point during migration through an
Anti-Corruption Layer.

## 5. Major Architectural Pain Points

### 5.1 Single Database Bottleneck

All transactions, customer profiles, fraud cases, audit logs, and reporting data
are stored in one Oracle database.

This creates a major scalability and availability dependency.

### 5.2 Hard-Coded Fraud Rules

More than 500 rules are embedded in Java switch statements.

Changing a fraud rule therefore requires application changes and deployment.

This prevents fraud analysts from rapidly adapting to new attack patterns.

### 5.3 Batch ML Scoring

The ML model runs every 15 minutes rather than during transaction processing.

This creates a detection gap where fraudulent transactions can be completed before
the next scoring cycle.

### 5.4 Outdated Model

The existing model is a logistic regression model trained approximately six months
ago.

This limits the system's ability to adapt to evolving fraud behaviour.

### 5.5 No Graph Analysis

The legacy platform does not provide relationship-based analysis.

Consequently, relationships between cards, customers, devices, addresses,
merchants, and IP addresses cannot be efficiently analysed as fraud networks.

### 5.6 Manual Deployments

Deployments require a four-hour maintenance window every Saturday.

This slows delivery of fraud detection improvements and increases operational risk.

### 5.7 Single Point of Failure

The Oracle database has no read replicas and no cross-region replication.

A database failure therefore represents a major availability risk.

### 5.8 Limited Fault Isolation

Because the application is monolithic, failures or resource problems in one
functional area can affect unrelated functionality.

### 5.9 Scaling Limitations

The monolith requires scaling of the complete application even when only one
functional area requires additional capacity.

### 5.10 Limited Deployment Flexibility

The system cannot independently deploy the rule engine, ML engine, graph engine,
or risk-scoring functionality.

## 6. Crisis Drivers

In Q3 2025, ShieldPay experienced a 340% increase in fraud attempts.

Three major threats exposed weaknesses in the existing architecture:

1. AI-generated synthetic identities
2. A third-party payment aggregator data breach resulting in exposed card data
3. Fraudsters exploiting the 15-minute batch-scoring window

Fraud losses increased from 12 crore INR per quarter to 53 crore INR in Q3 2025.

The false-positive rate increased from 0.3% to 2.1%, and customer complaints
tripled.

## 7. Required Architectural Direction

The monolith should be progressively decomposed into independently deployable
microservices.

The recommended migration approach is the Strangler Fig pattern.

An Anti-Corruption Layer should isolate the new domain model from legacy data
models during migration.

The new platform should introduce:

- Real-time transaction processing
- Rule-based detection
- ML-based anomaly detection
- Graph-based fraud detection
- Composite risk scoring
- Event-driven communication
- Independent data ownership
- Containerisation
- Service mesh
- API Gateway
- Observability
- Multi-region disaster recovery

## 8. Target-State Principles

1. Each service owns a focused business capability.
2. Services should be independently deployable.
3. Critical transaction-path communication should use low-latency synchronous calls.
4. Asynchronous business events should use Kafka.
5. Sensitive cardholder data must be protected.
6. Fraud decisions must be explainable.
7. Detection components should scale independently.
8. The architecture must support gradual migration from the legacy monolith.

## 9. Conclusion

The existing ShieldPay monolith cannot reliably support the required scale,
real-time fraud detection, rapid fraud-rule changes, advanced graph analysis,
or modern operational practices.

The proposed microservices architecture addresses these limitations by separating
fraud detection capabilities, introducing event-driven communication, polyglot
persistence, independent deployment, and real-time detection layers.