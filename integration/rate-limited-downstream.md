---
id: integration.rate-limited-downstream
title: Calling a rate-limited system
domain: integration
owner: Architecture Guild
version: "1.0"
status: approved
checklist_for: [queue-consumer, api]
---

# Calling a rate-limited system

Use this pattern whenever a system you call has a fixed rate limit below your peak
load.

## Rules
- **MUST** buffer, then pace: accept and store first, then deliver from a queue. The
  queue is the buffer; don't add admission control in front of it.
- **MUST** use one shared permit for every sender: a token bucket in a single
  DynamoDB item updated with a conditional write, or one sender with reserved
  concurrency of 1. **MUST NOT** rely on clocks or timed hand-overs between workers.
- **MUST** count retries against the limit, and size the margin with retries included.
- **MUST** classify failures per item: an item that fails its own attempts moves to a
  visible held state, is alerted and listed for redrive, and delivery carries on. It's
  never dropped and never blocks other items.
- **SHOULD** open a circuit breaker only when failures span several distinct items.
- **MUST** send an idempotency key with every write, so a retry or redrive is harmless.

## Design checklist
- The downstream limit, from its owner, in requests per second.
- Requests per item, and the resulting maximum items per second.
- Peak arrival rate, the backlog it creates, and how long it takes to drain.
- How the shared permit works.
- What a held item looks like, who sees it, and how it's redriven.
