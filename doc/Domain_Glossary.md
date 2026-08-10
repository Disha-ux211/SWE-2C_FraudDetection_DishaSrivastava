# Fraud Detection Domain Glossary

## 1. Fraud
Fraud is an intentional attempt to obtain money, goods, services, or other benefits through deception or unauthorized activity.

## 2. Card-Present (CP) Fraud
Fraud involving a physical payment card being used at a point-of-sale terminal.

## 3. Card-Not-Present (CNP) Fraud
Fraud involving transactions where the physical card is not presented, such as online purchases.

## 4. PAN
Primary Account Number. It is the card number associated with a payment card.

## 5. BIN
Bank Identification Number. The initial portion of a card number used to identify the issuing institution and card characteristics.

## 6. MCC
Merchant Category Code. A code used to classify a merchant according to its business category.

## 7. AFA
Additional Factor Authentication. An additional authentication step such as an OTP, biometric verification, or challenge question.

## 8. OTP
One-Time Password. A temporary authentication code used to verify a user during a transaction or login.

## 9. Tokenisation
The replacement of sensitive card data with a non-sensitive token that can be used instead of the original value.

## 10. Chargeback
A payment reversal initiated through the card payment process, commonly associated with disputed or fraudulent transactions.

## 11. False Positive
A legitimate transaction incorrectly classified as fraudulent.

## 12. False Positive Rate
The percentage of legitimate transactions incorrectly flagged as fraudulent.

## 13. Fraud Detection Rate
The percentage of confirmed fraudulent transactions correctly identified by the fraud detection system.

## 14. Rule Engine
A deterministic detection component that evaluates transactions against configurable fraud rules.

## 15. Fraud Rule
A configurable condition or set of conditions used to identify potentially fraudulent behaviour.

## 16. Rule ID
A unique identifier assigned to an individual fraud detection rule.

## 17. Rule Version
A specific version of a fraud rule maintained as the rule evolves over time.

## 18. Rule Lifecycle
The stages through which a rule passes, such as creation, testing, approval, activation, modification, and retirement.

## 19. Rule Simulation
Testing a rule against historical or sample transactions before activating it in production.

## 20. Anomaly Detection
A fraud detection technique that identifies transaction behaviour that differs significantly from expected patterns.

## 21. ML Score
A numerical risk signal produced by a machine-learning model based on transaction features.

## 22. Feature
An input attribute used by a fraud detection model, such as transaction amount, location, device, or transaction frequency.

## 23. Feature Store
A system used to store and serve machine-learning features for real-time or batch model inference.

## 24. Model Drift
A change in data or behavioural patterns that causes a machine-learning model's performance to deteriorate over time.

## 25. Champion Model
The currently preferred production machine-learning model.

## 26. Challenger Model
A candidate model evaluated against the current champion before potentially becoming the production model.

## 27. SHAP
SHapley Additive exPlanations. A technique for explaining how individual model features contribute to a model prediction.

## 28. Graph Analysis
Fraud detection based on relationships between entities such as cards, customers, devices, addresses, merchants, and IP addresses.

## 29. Fraud Graph
A graph representing entities and their relationships to identify suspicious networks or fraud rings.

## 30. Fraud Ring
A group of connected entities coordinating fraudulent activity.

## 31. Graph Node
An entity represented as a node in the fraud graph, such as a customer, card, device, IP address, or merchant.

## 32. Graph Relationship
A connection between two graph entities, such as shared device, shared address, or transaction relationship.

## 33. Community Detection
Graph analysis used to identify densely connected groups of entities that may represent fraud rings.

## 34. Centrality
A graph-analysis measurement describing the importance or influence of a node within a network.

## 35. PageRank
A centrality technique that identifies influential nodes based on their connections within a graph.

## 36. Betweenness Centrality
A graph metric that identifies nodes acting as bridges between different parts of a network.

## 37. Path Analysis
Analysis of the relationship path between entities to determine their degree of separation.

## 38. Subgraph Matching
Detection of known suspicious relationship patterns by searching for matching structures within the fraud graph.

## 39. Synthetic Identity
A fabricated identity created by combining real and/or fictitious identity attributes for fraudulent purposes.

## 40. Money Mule
An individual or account used to receive, transfer, or move funds associated with fraudulent activity.

## 41. SIM Swap
A technique where control of a victim's mobile number is fraudulently transferred to another SIM, potentially allowing interception of authentication messages.

## 42. Enumeration Attack
An attack in which large numbers of payment credentials or transaction attempts are tested to determine which credentials are valid.

## 43. Velocity Check
A fraud detection technique that evaluates the number or frequency of transactions within a defined time window.

## 44. Behavioural Anomaly
Transaction behaviour that significantly differs from a customer's established or expected behaviour.

## 45. Device Fingerprinting
Identification of a device using characteristics that help associate transactions with a particular device.

## 46. IP Geolocation
Determination of an approximate geographic location associated with an IP address.

## 47. Risk Score
A numerical representation of the estimated fraud risk associated with a transaction.

## 48. Composite Risk Score
A final score created by combining signals from multiple fraud detection engines.

## 49. Risk Scoring Service
The service responsible for combining Rule Engine, ML/Anomaly Detection, and Graph Analysis signals into a final risk score.

## 50. Auto-Approve
A decision in which a transaction is automatically allowed because its risk score falls within the approved range.

## 51. Step-Up Authentication
A decision requiring additional authentication before allowing a transaction to proceed.

## 52. Manual Review
A decision that routes a transaction to a fraud analyst for investigation.

## 53. Auto-Decline
A decision that automatically blocks a transaction because its risk score indicates high risk.

## 54. Fraud Case
A record representing a transaction or group of transactions requiring investigation.

## 55. Case Management
The workflow used by fraud analysts to investigate, assign, update, and resolve fraud cases.

## 56. Audit Trail
An immutable record of important system and user actions, including fraud decisions and configuration changes.

## 57. Real-Time Fraud Detection
Fraud analysis performed during or immediately around transaction processing rather than through delayed batch processing.

## 58. Kafka
An event-streaming platform used to transport transaction and fraud-related events between distributed services.

## 59. Consumer Group
A group of Kafka consumers that collectively process messages from Kafka topics.

## 60. Dead-Letter Queue (DLQ)
A destination for messages that cannot be successfully processed after the configured retry attempts.

## 61. Correlation ID
An identifier used to trace a transaction across multiple services and asynchronous events.

## 62. Microservice
An independently deployable service responsible for a focused business capability.

## 63. Bounded Context
A clearly defined domain boundary containing its own business concepts, rules, and terminology.

## 64. API Gateway
The external entry point that handles authentication, authorization, rate limiting, routing, validation, and other cross-cutting concerns.

## 65. Service Mesh
An infrastructure layer responsible for service-to-service communication, security, traffic management, and observability.

## 66. mTLS
Mutual Transport Layer Security. A security mechanism in which both communicating services authenticate each other and encrypt communication.

## 67. Rate Limiting
Controlling the number of requests allowed from a client, IP address, endpoint, merchant, or the entire system within a defined period.

## 68. Circuit Breaker
A resilience mechanism that temporarily stops requests to an unhealthy service to prevent cascading failures.

## 69. Observability
The ability to understand system behaviour through metrics, logs, and distributed traces.

## 70. RPO
Recovery Point Objective. The maximum acceptable amount of data loss measured in time following a failure.

## 71. RTO
Recovery Time Objective. The maximum acceptable time required to restore a service after a failure.

## 72. PCI DSS
Payment Card Industry Data Security Standard. A security framework governing the protection of payment card data.

## 73. RBI
Reserve Bank of India. India's central banking authority and a key regulatory authority relevant to the Indian fraud-detection environment.

## 74. GDPR
General Data Protection Regulation. A European data protection framework relevant when processing data of applicable European individuals.

## 75. Fraud Decision Explainability
The ability to provide a human-readable explanation describing why a transaction received a particular fraud decision.