---
id: data.relational
title: Relational data stores
domain: data
owner: Architecture Guild
version: "1.3"
status: approved
checklist_for: [data-store]
---
# Relational data stores

Use a relational store when the data has relationships you query across, needs
joins or ad hoc queries, or needs multi-row transactions. For lookups by key, use
`data.key-value` instead.

## Approved engines
- **Amazon Aurora DSQL.** The default for new serverless relational workloads. No
  capacity to size and no connections to pool.
- **Amazon Aurora Serverless v2 (PostgreSQL-compatible).** Use it when you need
  triggers or PL/pgSQL, pessimistic locking, or transactions that change more than
  3,000 rows.

Record which engine you chose and why in an ADR. Any other engine needs an ADR
approved by the Architecture Guild.

## Rules
- **MUST** run in the team's approved region, encrypted at rest with KMS and in
  transit with TLS 1.2 or higher.
- **MUST** use IAM authentication or credentials from AWS Secrets Manager. No
  passwords in code, config or environment variables.
- **MUST** enable automated backups with point-in-time recovery, with a retention
  period that matches the data's stated retention, not longer.
- **DSQL: MUST** connect with short-lived IAM auth tokens and retry transactions
  that fail with a write conflict.
- **Aurora: SHOULD** call the database from Lambda through the RDS Data API, or
  through RDS Proxy when results exceed the Data API limits.
- **SHOULD** set the minimum capacity to the lowest value the workload tolerates, so
  idle cost stays low.

## Design checklist
- Why relational and not key-value, in one sentence.
- The tables, their keys and the queries that matter.
- How Lambda connects, and how write conflicts or connection limits are handled.
- Backup, point-in-time recovery and how long each is kept.
- How deleted personal data leaves backups within the retention period.
