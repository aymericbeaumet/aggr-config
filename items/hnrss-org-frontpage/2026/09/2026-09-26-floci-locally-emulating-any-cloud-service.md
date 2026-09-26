---
title: 'Floci: Locally emulating any cloud service'
link: https://floci.io/
source: hnrss-org-frontpage
published: 2026-09-26T08:31:02Z
updated: 2026-09-26T08:31:02Z
first_seen: 2026-09-26T18:31:35.369872690Z
authors:
- theanonymousone
summary: 'Article URL: https://floci.io Comments URL: https://news.ycombinator.com/item?id=49854416 Points: 117 # Comments: 25'
content: extracted
html: 2026-09-26-floci-locally-emulating-any-cloud-service.html
preview:
  file: 2026-09-26-floci-locally-emulating-any-cloud-service.preview-75bbd6967cd4.webp
  width: 256
  height: 134
  color: '#e7e9f0'
images:
- source: https://floci.io/og/home.png
  original:
    file: 2026-09-26-floci-locally-emulating-any-cloud-service.image-3047f286e1c6.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-26-floci-locally-emulating-any-cloud-service.image-9900f3d8b771.webp
    width: 320
    height: 168
  - file: 2026-09-26-floci-locally-emulating-any-cloud-service.image-1cc425ea7202.webp
    width: 640
    height: 336
  - file: 2026-09-26-floci-locally-emulating-any-cloud-service.image-dae90986e39f.webp
    width: 1200
    height: 630
  color: '#f7f9fb'
---

Any cloud.\
Locally.
---

Run AWS, Azure, GCP, and OCI locally in milliseconds: the fast, credential-free loop your team and its AI agents need to ship faster.

Cloud Emulators

## Pick your cloud. Start instantly.

Each emulator is a standalone MIT-licensed binary. No auth tokens, no feature gates, no cloud account needed.

AWS

floci

Drop-in replacement for LocalStack. Same port 4566, 119 services, native binary. Switch with zero code changes.

4566

port

119

services

24 ms

startup

S3 SQS Lambda DynamoDB RDS EKS +113 more

Azure

floci-az

Covers Blob, Queue, Table, Functions, App Config, Key Vault, Event Hubs and Service Bus. Native speed, MIT license, no Azure account needed.

4577

port

28

services

13 MiB

idle memory

Blob Queue Table Functions Key Vault Event Hubs Service Bus +21 more

GCP

floci-gcp

Covers GCS, Pub/Sub, Firestore, Cloud Run, Cloud SQL, GKE and more. All 25 services on one port, native binary. MIT license, no GCP account needed.

4588

port

25

services

MIT

license

GCS Pub/Sub Firestore Datastore Secret Manager IAM +19 more

OCI

floci-oci

Covers Object Storage, Identity, Queue, Streaming, KMS, Vault and Functions. All 8 services on one port. MIT license, no Oracle Cloud account needed.

4599

port

8

services

MIT

license

Object Storage Queue Streaming KMS Vault Functions +2 more

Built for AI-assisted development

## Give your AI agents a cloud they can't break.

Coding agents write cloud code faster than ever, but they can't safely run it against a real account. Point them at Floci instead: a local cloud to build, run, and verify against. Instant, free, and credential-free.

0

### No credentials to leak.

Agents connect with throwaway keys. No real cloud secrets in the agent's context or sandbox. Nothing to exfiltrate, nothing to bill.

24ms

### Inner-loop fast.

24 ms cold start, 13 MiB idle. Agents spin up, test, and tear down inside the edit loop instead of waiting on round-trips to a remote account.

real

### Real signal, not mock theater.

Lambda, RDS, Redis, and Kafka run for real, so code an agent verifies locally behaves the same in production. No mock-shaped false positives.

✗

### Zero blast radius.

Let agents iterate aggressively. Worst case, they reset a local container, not your staging environment or your cloud bill.

 export AWS\_ENDPOINT\_URL=http://localhost:4566\
 pytest tests/

Works with every SDK, CLI, Terraform/OpenTofu module, and test runner your agents already use, with no special integration.

Developer Tools

## One toolchain. All clouds.

Manage and explore every Floci emulator from a unified CLI or visual dashboard.

CLI

floci-cli

A unified command-line interface to start, stop, and inspect all Floci emulators. One install covers AWS, Azure, GCP, and OCI from a single tool.

 curl -fsSL https://floci.io/install.sh | sh

 floci start

 floci doctor

UI

floci-ui

A visual dashboard for all Floci cloud emulators. Browse resources, inspect data, and manage services across AWS, Azure, GCP, and OCI from one place.

BROWSE ALL SERVICES

S3 Browser DynamoDB SQS Queues Lambda Logs Blob Storage Azure Queue \+ more

Why Floci

## Built for developers who ship.

No gatekeeping. No pricing tiers. No surprises. A local cloud that starts in milliseconds and works everywhere.

$0

### MIT Licensed. Forever free.

Fork it, embed it, extend it. No "community edition" sunset, no enterprise feature flags. Every service is available to every developer, always.

✗

### No auth token. Ever.

Pull the Docker image and go. No sign-ups, no API keys, no telemetry. LocalStack started requiring an auth token in March 2026. Floci never will.

24ms

### Native binary speed.

Compiled with GraalVM Mandrel. Starts in 24ms, idles at 13 MiB, fast enough for inner-loop iteration. Your CI, your laptop, and your AI agents will all thank you.

real

### Real engines, not mocks.

Lambda runs in real Docker containers. RDS uses real PostgreSQL/MySQL. ElastiCache runs real Redis. 100% protocol fidelity, no surprises in production.

Use Cases

## Where teams put Floci to work.

From the inner loop to CI to the classroom — anywhere a real cloud account is too slow, too risky, or too expensive.

CI

### Ephemeral test environments.

Spin up the full stack inside every pipeline job. 24 ms startup adds nothing to the build, each job gets an isolated cloud, and teardown is free.

λ

### Local-first development.

Build against S3, Pub/Sub, or Service Bus on your laptop — offline if you want. No shared dev account to trip over, no credentials to rotate.

tf

### Infrastructure-as-code dry runs.

Apply Terraform, OpenTofu, or CloudFormation against localhost first. Catch typos, drift, and bad refactors before they ever touch a real account.

101

### Workshops & onboarding.

Teach cloud services with zero billing risk. Every student runs the whole stack on their own machine — no accounts to provision, nothing to clean up.

Quick Start

## One install. Every service, locally.

No account, no token. Pick your platform and go.

 curl -fsSL https://floci.io/install.sh | sh

 floci start && eval $(floci env)

 aws s3 mb s3://my-bucket

 echo "Why pay for S3 when floci is free? 🎉" > hello-floci.txt\
 aws s3 cp hello-floci.txt s3://my-bucket/

 aws s3 cp s3://my-bucket/hello-floci.txt hello-back.txt\
 cat hello-back.txt

119 AWS services ready on :4566. S3 · SQS · DynamoDB · Lambda · RDS · +114 more
