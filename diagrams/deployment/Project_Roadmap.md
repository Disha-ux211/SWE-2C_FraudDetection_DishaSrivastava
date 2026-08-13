# Project Roadmap — Real-Time Fraud Detection Platform

## 1. Project Objective

Build a scalable, real-time fraud detection platform capable of processing
transactions, evaluating fraud rules, detecting behavioural anomalies,
analysing fraud relationships and producing risk-based decisions.

---

## 2. Roadmap

### Phase 1 — Domain Understanding

- Define the business domain
- Create the domain glossary
- Analyse the existing monolith
- Identify bounded contexts
- Define service responsibilities

### Phase 2 — Architecture Design

- Create C4 Level 1 system context
- Create C4 Level 2 container diagram
- Create C4 Level 3 service diagrams
- Define service decomposition
- Define persistence strategy
- Define SLA requirements

### Phase 3 — Event-Driven Architecture

- Perform Event Storming
- Identify domain events
- Identify commands
- Identify aggregates
- Identify policies
- Define Kafka topics
- Define event schemas
- Define event-driven architecture
- Define Saga workflows

### Phase 4 — API and Service Contracts

- Define REST APIs
- Create OpenAPI specification
- Define internal gRPC interfaces
- Create Protobuf contracts
- Define service-to-service communication

### Phase 5 — Data Architecture

- PostgreSQL for transactional data
- Redis for caching and real-time features
- Neo4j for fraud relationships
- Define persistence responsibilities
- Define CQRS read models

### Phase 6 — Deployment Architecture

- Containerize services with Docker
- Define Kubernetes deployments
- Define Kubernetes services
- Define service scaling
- Define health checks
- Define deployment strategy

### Phase 7 — Security and Resilience

- Define authentication
- Define authorization
- Define API security
- Define encryption requirements
- Define retry mechanisms
- Define circuit breakers
- Define failure handling
- Define idempotency
- Define audit and compliance controls

### Phase 8 — Observability

- Define application metrics
- Define structured logging
- Define distributed tracing
- Define Prometheus monitoring
- Define Grafana dashboards
- Define operational alerts

### Phase 9 — Operations and Recovery

- Define disaster recovery strategy
- Define backup and restoration procedures
- Define incident response
- Define operational runbooks
- Define architecture decisions

### Phase 10 — Final Review

- Verify architecture consistency
- Verify API contracts
- Verify event schemas
- Verify diagrams
- Verify deployment artifacts
- Verify documentation
- Verify repository structure
- Prepare final presentation
- Perform final submission review

---

## 3. Current Completion Status

| Area | Status |
|---|---|
| Domain Glossary | Complete |
| Bounded Context Map | Complete |
| Monolith Analysis | Complete |
| SLA Specification | Complete |
| Persistence Strategy | Complete |
| Service Decomposition | Complete |
| C4 Level 1 | Complete |
| C4 Level 2 | Complete |
| C4 Level 3 | Complete |
| Event Storming | Complete |
| Kafka Topology | Complete |
| Event Schemas | Complete |
| Saga Workflows | Complete |
| Event-Driven Architecture | Complete |
| OpenAPI Contract | Complete |
| Protobuf/gRPC Contract | Complete |
| Docker Deployment Artifacts | Complete |
| Kubernetes Deployment Artifacts | Complete |
| Security & Resilience | Complete |
| Observability | Complete |
| Deployment Documentation | Complete |
| Disaster Recovery | Pending |
| ADRs | Pending |
| Operational Runbook | Pending |
| AI Usage Documentation | Pending |
| Error Detection Documentation | Pending |
| Final Presentation | Pending |

---

## 4. Final Goal

The final repository should contain a consistent architecture package covering:

- Domain architecture
- Service architecture
- Event-driven architecture
- API contracts
- Data architecture
- Deployment architecture
- Security
- Resilience
- Observability
- Operations
- Disaster recovery
- Architecture decisions
- Final project documentation

The roadmap is used to track completion and ensure that no major project
deliverable is missed before submission.