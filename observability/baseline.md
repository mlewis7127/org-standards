---
id: observability.baseline
title: Observability baseline
domain: observability
owner: Platform Engineering
version: "1.2"
status: approved
checklist_for: [api, queue-consumer]
---

# Observability baseline

## Rules
- **MUST** put a correlation ID on every request, generated on the server and carried
  through every hop and log line.
- **MUST** write structured JSON logs, with no personal data (see `security.baseline`).
- **MUST** define each service's key signals (throughput, latency, errors, saturation,
  and the health of each dependency) with an alert threshold and a target time to alert.
- **MUST** link every page-worthy alert to a runbook.
- **SHOULD** keep logs only as long as the stated retention, and no longer.

## Design checklist
- The key signals, and the threshold for each alert.
- Who is alerted, and how fast.
- The runbook for each page-worthy alert.
- Log retention.
