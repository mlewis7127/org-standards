---
id: storage.object-storage
title: Object storage and uploads
domain: storage
owner: Platform Engineering
version: "1.4"
status: approved
checklist_for: [object-storage]
---

# Object storage and uploads

## Defaults
- **Amazon S3**, private buckets, Block Public Access on at account and bucket level.

## Rules
- **MUST** encrypt at rest with KMS, and deny `aws:SecureTransport = false` and
  `s3:TlsVersion < 1.2` in the bucket policy.
- **MUST** set lifecycle rules that match the data's stated retention.
- **MUST NOT** make a bucket or object public. Readers get short-lived presigned GETs,
  issued after an authorisation check.

## User uploads
- **MUST** upload with a presigned POST to a private staging prefix, with size and
  content-type conditions, a 5-minute expiry and one upload per URL.
- **MUST** scan uploads before anyone else can read them. Copy clean objects to a
  separate prefix and quarantine the rest. Make the scan idempotent on the object
  version.
- **MUST** re-read the parent record before releasing an upload. If the parent can no
  longer change, quarantine the upload instead.
- **SHOULD** strip location metadata from images other people will see.

## Design checklist
- Each bucket, what it holds and its retention.
- The upload path, step by step, and what checks it.
- Who can read what, and how they get a link.
- Storage class and lifecycle, with the cost driver.
