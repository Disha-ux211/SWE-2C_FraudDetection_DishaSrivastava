# Event Storming — Real-Time Fraud Detection Platform

## 1. Purpose

Event Storming identifies the major domain events, commands, aggregates,
policies, actors, read models and external systems involved in the
Real-Time Fraud Detection Platform.

The event flow covers transaction ingestion, enrichment, fraud detection,
risk scoring, customer notification and investigation.

---

## 2. Actors

- Customer
- Fraud Analyst
- Fraud Operations Team
- System Administrator
- Compliance Officer
- Risk Manager
- Payment Network

---

## 3. External Systems

- Visa
- Mastercard
- RuPay
- Core Banking System
- Customer Mobile Application
- Notification Provider
- Regulatory Reporting System
- Identity / Authentication Provider

---

## 4. Aggregates

- Transaction
- CustomerProfile
- PaymentAccount
- FraudRule
- AnomalyModel
- FraudGraph
- RiskAssessment
- FraudCase
- Notification
- Card
- AuditRecord
- Investigation

---

# 5. Domain Events

## Transaction Lifecycle Events

1. TransactionReceived
2. TransactionValidated
3. TransactionRejected
4. TransactionAccepted
5. TransactionEnrichmentStarted
6. TransactionEnriched
7. CustomerProfileRetrieved
8. DeviceProfileRetrieved
9. MerchantProfileRetrieved
10. TransactionFeaturesGenerated
11. TransactionPersisted
12. TransactionPublished
13. TransactionProcessingStarted
14. TransactionProcessingCompleted
15. TransactionProcessingFailed

## Rule Engine Events

16. RuleEvaluationStarted
17. RuleLoaded
18. RuleCompiled
19. RuleEvaluated
20. RuleTriggered
21. RuleNotTriggered
22. RuleSignalGenerated
23. RuleScoreCalculated
24. RuleEvaluationFailed
25. RuleVersionSelected
26. RuleConfigurationUpdated

## Anomaly Detection Events

27. AnomalyDetectionStarted
28. FeaturesPreparedForML
29. MLModelLoaded
30. MLModelVersionSelected
31. AnomalyScoreGenerated
32. AnomalyDetected
33. AnomalyNotDetected
34. AnomalyThresholdExceeded
35. AnomalyDetectionCompleted
36. AnomalyDetectionFailed

## Graph Analysis Events

37. GraphAnalysisStarted
38. CustomerNodeLoaded
39. AccountNodeLoaded
40. DeviceNodeLoaded
41. MerchantNodeLoaded
42. GraphRelationshipCreated
43. GraphRelationshipUpdated
44. SuspiciousRelationshipDetected
45. FraudRingDetected
46. GraphRiskCalculated
47. GraphAnalysisCompleted
48. GraphAnalysisFailed

## Risk Assessment Events

49. RiskScoringStarted
50. RuleSignalReceived
51. AnomalySignalReceived
52. GraphSignalReceived
53. RiskScoreCalculated
54. RiskThresholdEvaluated
55. FraudDecisionMade
56. TransactionApproved
57. TransactionDeclined
58. TransactionFlaggedForReview
59. StepUpAuthenticationRequested
60. StepUpAuthenticationCompleted
61. StepUpAuthenticationFailed

## Fraud Case Events

62. FraudAlertCreated
63. FraudCaseCreated
64. FraudCaseAssigned
65. InvestigationStarted
66. InvestigationEvidenceAdded
67. InvestigationEscalated
68. InvestigationDecisionMade
69. FraudConfirmed
70. FalsePositiveConfirmed
71. FraudCaseResolved
72. FraudCaseClosed

## Customer and Card Events

73. CustomerRiskProfileUpdated
74. CustomerRiskLevelChanged
75. CardFreezeRequested
76. CardFrozen
77. CardUnfreezeRequested
78. CardUnfrozen
79. CardReplacementRequested
80. ReplacementCardIssued
81. CustomerProfileUpdated

## Notification Events

82. NotificationRequested
83. NotificationPrepared
84. NotificationSent
85. NotificationDeliveryConfirmed
86. NotificationDeliveryFailed
87. NotificationRetried

## Audit and Compliance Events

88. AuditRecordCreated
89. SecurityEventRecorded
90. ComplianceEventRecorded
91. RegulatoryReportGenerated
92. InvestigationAuditCompleted

---

# 6. Commands

| Command | Aggregate | Resulting Event |
|---|---|---|
| SubmitTransaction | Transaction | TransactionReceived |
| ValidateTransaction | Transaction | TransactionValidated |
| EnrichTransaction | Transaction | TransactionEnriched |
| RetrieveCustomerProfile | CustomerProfile | CustomerProfileRetrieved |
| RetrieveFeatures | Transaction | TransactionFeaturesGenerated |
| EvaluateRules | FraudRule | RuleEvaluationStarted |
| TriggerRule | FraudRule | RuleTriggered |
| GenerateAnomalyScore | AnomalyModel | AnomalyScoreGenerated |
| DetectAnomaly | AnomalyModel | AnomalyDetected |
| AnalyseGraph | FraudGraph | GraphAnalysisStarted |
| CalculateGraphRisk | FraudGraph | GraphRiskCalculated |
| CalculateRiskScore | RiskAssessment | RiskScoreCalculated |
| MakeFraudDecision | RiskAssessment | FraudDecisionMade |
| ApproveTransaction | Transaction | TransactionApproved |
| DeclineTransaction | Transaction | TransactionDeclined |
| RequestStepUpAuthentication | Transaction | StepUpAuthenticationRequested |
| CreateFraudAlert | FraudCase | FraudAlertCreated |
| CreateCase | FraudCase | FraudCaseCreated |
| AssignCase | FraudCase | FraudCaseAssigned |
| StartInvestigation | Investigation | InvestigationStarted |
| AddEvidence | Investigation | InvestigationEvidenceAdded |
| EscalateInvestigation | Investigation | InvestigationEscalated |
| ConfirmFraud | Investigation | FraudConfirmed |
| ConfirmFalsePositive | Investigation | FalsePositiveConfirmed |
| ResolveCase | FraudCase | FraudCaseResolved |
| CloseCase | FraudCase | FraudCaseClosed |
| FreezeCard | Card | CardFrozen |
| UnfreezeCard | Card | CardUnfrozen |
| ReplaceCard | Card | ReplacementCardIssued |
| UpdateCustomerRisk | CustomerProfile | CustomerRiskProfileUpdated |
| SendNotification | Notification | NotificationSent |
| RetryNotification | Notification | NotificationRetried |
| CreateAuditRecord | AuditRecord | AuditRecordCreated |
| RecordComplianceEvent | AuditRecord | ComplianceEventRecorded |
| GenerateRegulatoryReport | AuditRecord | RegulatoryReportGenerated |

---

# 7. Policies

## Fraud Detection Policy

IF the risk score exceeds the configured threshold,

THEN the transaction is flagged for review or declined according to the
configured risk policy.

---

## High Anomaly Policy

IF the anomaly score exceeds the configured threshold,

THEN generate an anomaly signal for Risk Scoring.

---

## Fraud Ring Policy

IF graph analysis identifies a suspicious fraud network,

THEN increase the graph risk score and send the signal to Risk Scoring.

---

## Step-Up Authentication Policy

IF the transaction has medium or elevated risk and additional verification
is required,

THEN request step-up authentication.

---

## Manual Review Policy

IF the final risk decision is REVIEW,

THEN create a fraud investigation case and assign it to a fraud analyst.

---

## Card Freeze Policy

IF fraud is confirmed,

THEN freeze the associated card.

---

## False Positive Policy

IF the investigation determines that the transaction is legitimate,

THEN unfreeze the card when applicable and close the fraud case.

---

## Notification Policy

IF a transaction is declined, flagged, or requires customer action,

THEN send an appropriate customer notification.

---

## Audit Policy

IF a security, fraud or compliance action occurs,

THEN create an immutable audit record.

---

# 8. Primary Transaction Flow

```text
Customer
   |
   v
SubmitTransaction
   |
   v
TransactionReceived
   |
   v
TransactionValidated
   |
   v
TransactionEnriched
   |
   +-------------------+-------------------+
   |                   |                   |
   v                   v                   v
Rule Evaluation   Anomaly Detection   Graph Analysis
   |                   |                   |
   v                   v                   v
RuleSignal        AnomalyScore        GraphRisk
   |                   |                   |
   +-------------------+-------------------+
                       |
                       v
                RiskScoringStarted
                       |
                       v
                RiskScoreCalculated
                       |
                       v
                 FraudDecisionMade
                       |
          +------------+-------------+
          |            |             |
          v            v             v
       APPROVE       REVIEW       DECLINE
                       |
                       v
                 FraudCaseCreated
                       |
                       v
                Analyst Assignment
                       |
                       v
                  Investigation
                       |
             +---------+---------+
             |                   |
             v                   v
       Fraud Confirmed    False Positive
             |                   |
             v                   v
        Card Frozen        Card Unfrozen
             |                   |
             +---------+---------+
                       |
                       v
                  Case Resolved
                       |
                       v
                   Notification
                       |
                       v
                   Audit Record