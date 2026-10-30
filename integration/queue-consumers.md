---
id: integration.queue-consumers
title: Queue consumers
domain: integration
owner: Architecture Guild
version: "1.3"
status: approved
checklist_for: [queue-consumer]
---

# Queue consumers

A queue lets a producer accept work faster than a consumer can finish it. The
consumer is where most failures show up, so it gets its own standard.

## Rules
- **MUST** have a dead-letter queue, and an alarm when anything arrives in it.
- **MUST** alarm on the age of the oldest message, with a threshold tied to the
  stated latency target.
- **MUST** be idempotent: use an idempotency key so a redelivered message is harmless.
- **MUST** set the visibility timeout longer than the consumer's own timeout.
- **SHOULD** report partial batch failures, so one bad message doesn't send the whole
  batch back.
- **MUST** state the rate limit of every system it calls, and enforce it. See
  `integration.rate-limited-downstream`.
- **SHOULD** use FIFO queues only when order matters, and say which order and for
  which key.

## Design checklist
- Expected peak arrival rate and the consumer's maximum rate.
- Visibility timeout, retries and dead-letter policy.
- What a redelivered message does, and why it's safe.
- Every downstream system called, with its limit.
- Who is alerted, and on which alarm.
