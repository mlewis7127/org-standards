---
id: data.relational
title: Relational data stores
domain: data
owner: Architecture Guild
version: "1.2"
status: approved
checklist_for: [data-store]
---

# Relational data stores

Use a relational store when the data has relationships you query across, needs
joins or ad hoc queries, or needs multi-row transactions. For lookups by key, use
`data.key-value` instead.

## Approved engine
- **Amazon Aurora Serverless v2 (PostgreSQL-compatible).** It scales capacity with
  load, so you don't size for peak.

Any other engine needs an ADR approved by the Architecture Guild.

## Rules
- **MUST** run in the team's approved region, encrypted at rest with KMS and in
  transit with TLS 1.2 or higher.
- **MUST** use IAM authentication or credentials from AWS Secrets Manager. No
  passwords in code, config or environment variables.
- **MUST** enable automated backups with point-in-time recovery, with a retention
  period that matches the data's stated retention, not longer.
- **SHOULD** connect from Lambda through RDS Proxy, so a burst of invocations
  doesn't exhaust database connections.
- **SHOULD** set the minimum capacity to the lowest value the workload tolerates, so
  idle cost stays low.

## Design checklist
- Why relational and not key-value, in one sentence.
- The tables, their keys and the queries that matter.
- Peak connections, and how they are pooled.
- Backup, point-in-time recovery and how long each is kept.
- How deleted personal data leaves backups within the retention period.
