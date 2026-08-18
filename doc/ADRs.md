# Architecture Decision Records (ADRs)

## ADR-001: Use Microservices Architecture

### Status
Accepted

### Context

The Real-Time Fraud Detection Platform contains multiple independent
capabilities including transaction processing, rule evaluation, anomaly
detection, graph analysis, risk scoring and case management.

These capabilities have different scaling and technology requirements.

### Decision

Use a microservices architecture with independently deployable services.

### Consequences

Positive:

- Independent scaling
- Independent deployment
- Clear service ownership
- Fault isolation
- Technology flexibility

Trade-offs:

- Increased operational complexity
- Distributed communication
- Distributed tracing and monitoring are required

---

## ADR-002: Use Apache Kafka for Event-Driven Communication

### Status
Accepted

### Context

Fraud detection requires multiple services to process transaction information
independently and asynchronously.

### Decision

Use Apache Kafka as the event backbone between services.

### Consequences

Positive:

- Asynchronous processing
- High throughput
- Event replay
- Loose coupling
- Independent consumer scaling

Trade-offs:

- Kafka infrastructure must be operated
- Event schema evolution must be managed
- Consumer lag must be monitored

---

## ADR-003: Use PostgreSQL for Transactional Data

### Status
Accepted

### Context

The platform requires reliable storage for transactions, customer data,
fraud cases and other relational business information.

### Decision

Use PostgreSQL as the primary relational database.

### Consequences

PostgreSQL provides:

- ACID transactions
- Relational integrity
- Reliable persistence
- Mature backup and recovery capabilities

---

## ADR-004: Use Redis for Low-Latency Data

### Status
Accepted

### Context

Real-time fraud detection requires fast access to frequently used data and
real-time features.

### Decision

Use Redis for caching and low-latency feature storage.

### Consequences

Positive:

- Very low latency
- Reduced database load
- Suitable for frequently accessed features

Trade-off:

Redis should not be the sole source of truth for critical transactional data.

---

## ADR-005: Use Neo4j for Fraud Relationship Analysis

### Status
Accepted

### Context

Fraud detection requires analysing relationships between customers,
accounts, devices and merchants.

Relational queries become difficult to maintain for complex relationship
analysis.

### Decision

Use Neo4j as the graph database for fraud relationships.

### Consequences

Neo4j supports:

- Relationship traversal
- Fraud-ring detection
- Suspicious connection analysis
- Graph-based risk signals

---

## ADR-006: Use REST and gRPC for Service Communication

### Status
Accepted

### Context

The platform requires external APIs as well as efficient internal
service-to-service communication.

### Decision

Use REST/OpenAPI for external APIs and gRPC/Protobuf for selected internal
service communication.

### Consequences

REST provides a widely accessible external interface.

gRPC provides efficient strongly typed internal communication.

Both API contracts must be versioned and maintained.

---

## ADR-007: Use CQRS for Analytics Read Models

### Status
Accepted

### Context

Operational transaction processing and analytical dashboards have different
query and performance requirements.

### Decision

Use CQRS with materialised read models for analytics.

### Consequences

Positive:

- Optimized dashboard queries
- Separation of write and read workloads
- Independent read-model scaling
- Near-real-time analytics

Trade-off:

Read models are eventually consistent with source events.

---

## ADR-008: Use Kubernetes for Deployment

### Status
Accepted

### Context

The platform consists of multiple independently scalable services.

### Decision

Use Kubernetes for container orchestration.

### Consequences

Kubernetes provides:

- Service discovery
- Horizontal scaling
- Health checks
- Pod recovery
- Rolling deployments
- Load balancing

Operational complexity is increased and requires appropriate platform
monitoring.

---

## ADR-009: Use OpenTelemetry for Distributed Tracing

### Status
Accepted

### Context

A single transaction can pass through multiple services before a fraud
decision is produced.

### Decision

Use OpenTelemetry for distributed tracing.

### Consequences

Trace and correlation identifiers allow operators to follow a transaction
across the complete processing workflow.

---

## ADR-010: Use Saga for Distributed Business Workflows

### Status
Accepted

### Context

Fraud investigation and card-blocking workflows span multiple services and
cannot rely on a single distributed database transaction.

### Decision

Use Saga-based workflows with compensating actions.

### Consequences

The platform can coordinate long-running workflows while avoiding distributed
database transactions.

Compensation logic must be explicitly designed and tested.