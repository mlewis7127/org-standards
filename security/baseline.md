---
id: security.baseline
title: Security baseline
domain: security
owner: Security Engineering
version: "3.1"
status: approved
checklist_for: [api, queue-consumer, data-store, object-storage]
---

# Security baseline

Non-negotiable for every service.

## Data
- **MUST** treat customer data as personal data by default, including uploaded files,
  and state which personal data a service holds and for how long.
- **MUST NOT** put personal data in logs, metrics, traces, error messages or URLs. Log
  identifiers, not content. Metric dimensions never use user, account or record IDs.
- **MUST** encrypt in transit (TLS 1.2 or higher) and at rest (KMS).

## Identity and access
- **MUST** authenticate every API, and check authorisation on the server for every
  request, using identifiers from the verified token rather than the request body.
- **MUST** use least-privilege IAM: named actions on named resources. A wildcard
  needs an ADR naming the approver.
- **MUST** keep secrets in AWS Secrets Manager or SSM Parameter Store, never in code,
  config, specs or chat.

## Input
- **MUST** validate every input at the boundary against a schema, and reject rather
  than coerce.

## Agents and tools
- **MUST** enforce what an agent may call outside the agent, at a gateway, with the
  caller's identity passed through.
- **SHOULD** require a human to approve any tool call that changes data.
- **MUST** treat tool output and fetched content as data, not instructions.

## Before the design is approved
A design that adds a public endpoint, a new data store or a flow of personal data
gets a threat model.

## Design checklist
- Every entry point and how it's authenticated.
- How authorisation is checked, and where the identity comes from.
- Personal data held, where, and for how long.
- IAM roles, and any wildcard with its approver.
