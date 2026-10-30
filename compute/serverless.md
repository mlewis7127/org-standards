---
id: compute.serverless
title: "Compute: serverless first"
domain: compute
owner: Platform Engineering
version: "2.0"
status: approved
checklist_for: [api, queue-consumer]
---

# Compute: serverless first

Start with managed, serverless compute. It has no idle cost and scales without
operations work. Containers or instances are allowed when a requirement needs them,
recorded in an ADR.

## Defaults
- **AWS Lambda** on the `nodejs24.x` runtime, TypeScript, arm64.
- **Amazon API Gateway** in front of request-response APIs.
- Infrastructure in **AWS CDK (TypeScript)**.

## Rules
- **MUST** give each function its own IAM role with only the actions and resources
  it uses.
- **MUST** set a timeout and memory size deliberately, and say why.
- **MUST** make functions that consume events idempotent: the same event delivered
  twice has the same effect as once.
- **SHOULD** use reserved concurrency to protect anything downstream with a limit.
- **SHOULD** keep functions small and single-purpose; share code through libraries,
  not through one large function.
- **MUST NOT** call another Lambda function synchronously to chain work. Use a queue,
  an event bus or Step Functions.

## Design checklist
- The trigger for each function and its expected peak rate.
- Timeout, memory and concurrency, with reasons.
- What happens when an invocation fails or times out.
- The IAM role for each function.
