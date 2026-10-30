---
id: data.key-value
title: Key-value data stores
domain: data
owner: Architecture Guild
version: "1.1"
status: approved
checklist_for: [data-store]
---

# Key-value data stores

Use a key-value store when every access pattern is known up front and is a lookup
by key or a query within one partition.

## Approved engine
- **Amazon DynamoDB, on-demand capacity.** It's billed per request and scales without
  capacity planning.

## Rules
- **MUST** design the keys from the access patterns: list every read and write
  before choosing a partition key and sort key.
- **MUST** encrypt with a customer-managed KMS key for tables that hold personal data.
- **MUST** enable point-in-time recovery, and set its recovery period no longer than
  the data's stated retention.
- **SHOULD** use TTL to expire items whose retention is time-based, with a scheduled
  job as a backstop where deletion has a deadline (TTL deletion isn't immediate).
- **SHOULD** keep one table per service unless access patterns clearly differ.
- **MUST NOT** scan a table on a request path.

## Design checklist
- Every access pattern, and the key or index that serves it.
- Item size and peak request rate.
- What expires, how, and by when.
- Who can read and write the table, by role.
