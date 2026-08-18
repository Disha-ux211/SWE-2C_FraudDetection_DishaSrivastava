# Operational Runbook — Real-Time Fraud Detection Platform

## 1. Purpose

This runbook provides operational procedures for monitoring, diagnosing and
recovering the Real-Time Fraud Detection Platform.

---

## 2. Service Health Check

Check the health of:

- API Gateway
- Transaction Service
- Rule Engine
- Anomaly Detection Service
- Graph Analysis Service
- Risk Scoring Service
- Customer Profile Service
- Case Management Service
- Notification Service
- Audit & Compliance Service

Verify:

- Service availability
- Error rate
- Response latency
- CPU usage
- Memory usage
- Pod health

---

## 3. Transaction Processing Issue

### Symptoms

- Transactions are not being processed.
- Transaction latency increases.
- Transaction failures increase.

### Actions

1. Check API Gateway health.
2. Check Transaction Service health.
3. Check Kafka producer status.
4. Check Kafka consumer lag.
5. Check PostgreSQL connectivity.
6. Check service logs.
7. Check recent deployments.
8. Restart unhealthy pods if required.
9. Verify transaction processing after recovery.

---

## 4. Kafka Consumer Lag

### Symptoms

- Events are delayed.
- Consumer lag increases continuously.

### Actions

1. Identify the affected topic.
2. Identify the affected consumer group.
3. Check consumer health.
4. Check processing latency.
5. Check for repeated consumer errors.
6. Scale consumers if required.
7. Inspect retry and dead-letter topics.
8. Verify lag returns to normal.

---

## 5. Rule Engine Failure

### Symptoms

- Rule evaluations fail.
- Rule signals are not generated.

### Actions

1. Check Rule Engine health.
2. Check application logs.
3. Verify rule configuration.
4. Check Kafka connectivity.
5. Retry failed events.
6. Verify rule evaluation results.
7. Monitor fraud decision latency.

---

## 6. Anomaly Detection Failure

### Symptoms

- ML scores are not generated.
- Anomaly detection latency increases.

### Actions

1. Check Anomaly Detection Service.
2. Check model availability.
3. Check model version.
4. Check feature availability.
5. Review service logs.
6. Retry failed detection requests.
7. Route transactions according to configured fallback policy.
8. Verify anomaly scores are being generated.

---

## 7. Graph Analysis Failure

### Symptoms

- Graph risk scores are unavailable.
- Fraud relationship analysis fails.

### Actions

1. Check Graph Analysis Service.
2. Check Neo4j connectivity.
3. Verify graph database health.
4. Check recent graph events.
5. Retry failed analysis.
6. Verify graph risk calculation.

---

## 8. Risk Scoring Failure

### Symptoms

- Final risk scores are not generated.
- Fraud decisions are delayed.

### Actions

1. Check Risk Scoring Service.
2. Check Rule Engine output.
3. Check Anomaly Detection output.
4. Check Graph Analysis output.
5. Inspect Kafka topics.
6. Retry failed events.
7. Apply the configured safe fallback policy if required.
8. Verify risk decisions after recovery.

---

## 9. Database Failure

### PostgreSQL

1. Check database connectivity.
2. Check database health.
3. Check connection pool usage.
4. Check available storage.
5. Verify replica status.
6. Restore or fail over if required.
7. Verify transaction consistency.

### Neo4j

1. Check Neo4j availability.
2. Check database connectivity.
3. Verify graph integrity.
4. Restore backup if required.
5. Replay relationship events if required.

### Redis

1. Check Redis availability.
2. Check memory usage.
3. Check connection errors.
4. Restart or fail over if required.
5. Rebuild cache from source data where necessary.

---

## 10. Notification Failure

### Symptoms

- Customer notifications are delayed or not delivered.

### Actions

1. Check Notification Service.
2. Check notification provider connectivity.
3. Inspect notification retry queue.
4. Check dead-letter messages.
5. Retry failed notifications.
6. Verify delivery status.

---

## 11. Fraud Case Backlog

### Symptoms

- Manual-review cases increase rapidly.
- Analysts cannot process cases within the expected time.

### Actions

1. Check fraud detection rate.
2. Check rule trigger rates.
3. Check anomaly detection results.
4. Check risk-score thresholds.
5. Check analyst assignment.
6. Escalate critical cases.
7. Investigate sudden changes in fraud volume.

---

## 12. Dead-Letter Queue Procedure

When messages enter a dead-letter topic:

1. Identify the source topic.
2. Identify the failure reason.
3. Inspect the event.
4. Determine whether the failure is transient or permanent.
5. Correct the underlying issue.
6. Replay the event if safe.
7. Verify successful processing.
8. Record the incident.

---

## 13. High Fraud Rate Alert

### Symptoms

Fraud decisions suddenly increase above normal levels.

### Actions

1. Check current transaction volume.
2. Check rule trigger rates.
3. Check anomaly scores.
4. Check graph-risk signals.
5. Check recent rule changes.
6. Check model version changes.
7. Check geographic concentration.
8. Escalate to Fraud Operations if required.

---

## 14. Security Incident

If suspicious system activity is detected:

1. Identify the affected service.
2. Preserve relevant logs and audit records.
3. Restrict affected access where required.
4. Rotate compromised credentials.
5. Review authentication and authorization logs.
6. Investigate affected transactions.
7. Notify the appropriate security team.
8. Record the incident.

---

## 15. Deployment Failure

If a deployment causes failures:

1. Check deployment status.
2. Check pod health.
3. Review application logs.
4. Compare metrics before and after deployment.
5. Stop rollout if necessary.
6. Roll back to the previous stable version.
7. Verify service health.
8. Record the incident.

---

## 16. Incident Severity

| Severity | Description | Example |
|---|---|---|
| P1 | Critical platform failure | Fraud detection unavailable |
| P2 | Major service degradation | Risk scoring unavailable |
| P3 | Limited service impact | Notification delays |
| P4 | Minor issue | Non-critical dashboard issue |

---

## 17. Incident Closure

An incident can be closed after:

- Service health is restored.
- Transactions are processing normally.
- Kafka lag is stable.
- Databases are healthy.
- Fraud decisions are being generated.
- No critical alerts remain.
- Relevant stakeholders have been notified.
- The incident has been documented.

---

## 18. Post-Incident Review

Every major incident should record:

- Incident summary
- Start time
- End time
- Root cause
- Services affected
- Customer impact
- Recovery actions
- Corrective actions
- Preventive actions

The review should result in improvements to the architecture, monitoring or
operational procedures where appropriate.