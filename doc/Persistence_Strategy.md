# Persistence Strategy

## 1. Overview

The target fraud detection platform uses polyglot persistence so that each
bounded context can use a storage technology appropriate for its workload.

The design avoids a single shared database and assigns data ownership to the
services that manage each business capability.

## 2. Persistence Technologies

| Technology | Primary Purpose |
|---|---|
| PostgreSQL | Transactional and relational data |
| Redis | Low-latency cache and real-time feature data |
| Neo4j | Fraud relationship and graph analysis |
| Kafka | Durable event streaming and asynchronous communication |

## 3. PostgreSQL

PostgreSQL is used for structured transactional data that requires strong
consistency and relational querying.

### Suitable Data

- Transaction records
- Customer profiles
- Fraud cases
- Rule metadata
- Rule versions
- Audit metadata
- Decision records

### Characteristics

- ACID transactions
- Relational constraints
- Indexing
- Read replicas
- Backup and recovery
- High availability configuration

## 4. Redis

Redis is used where very low-latency access is required.

### Suitable Data

- Real-time velocity counters
- Frequently accessed customer features
- Session information
- Temporary risk information
- Rate-limiting counters
- Frequently accessed cache entries

Redis data should have appropriate TTL values so that temporary information does
not grow indefinitely.

## 5. Neo4j

Neo4j is used for relationship-oriented fraud analysis.

### Graph Entities

Examples include:

- Customer
- Card
- Account
- Device
- IP Address
- Merchant
- Address
- Phone Number

### Example Relationships

```text
Customer ──USES──> Card
Customer ──USES──> Device
Customer ──USES──> IP
Customer ──LIVES_AT──> Address
Card ──TRANSACTED_AT──> Merchant
Device ──CONNECTED_TO──> IP
Customer ──OWNS──> Account