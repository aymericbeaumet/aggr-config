---
title: 'Fakecloud: Local AWS cloud emulator for integration tests'
link: https://fakecloud.dev/
source: hnrss-org
published: 2026-09-26T14:25:16Z
updated: 2026-09-26T14:25:16Z
first_seen: 2026-09-28T06:49:16.850449770Z
authors:
- theanonymousone
content: extracted
html: 2026-09-26-fakecloud-local-aws-cloud-emulator-for-integration-tests.html
---

Local AWS cloud emulator for integration tests. Run your app with normal AWS clients, stay fully local, and use fakecloud SDKs when your tests need deeper visibility.

105 services. 7,508 operations. 248,557/248,557 Smithy variants pass — true 100% conformance.

## Why fakecloud?

fakecloud gives you a local AWS environment that behaves like infrastructure, not a mock. Your app uses the regular AWS SDK, CLI, and IaC tools. Unlike today's LocalStack Community setup, you do not need an account, auth token, or paid plan just to keep core development flows local.

**The SDKs make that workflow nicer, not narrower.** Start fakecloud, run the code you actually ship, inspect emails/messages/invocations after the fact, and force async AWS-style behavior to happen on demand.

## Built for realistic local testing

Real APIs for your app, purpose-built tooling for your tests.

Workflow

### Test what your app really does

Use normal AWS clients against localhost, then verify the effects with fakecloud instead of stitching together polling, fixtures, or private test hooks.

SDKs

### First-party test clients

TypeScript, Python, Go, PHP, Java, and Rust SDKs wrap the `/_fakecloud/*` endpoints for resets, assertions, and manual control of async processors after your app has already used the normal AWS APIs.

Coverage

### 105 AWS services

S3, SQS, SNS, EventBridge, EventBridge Pipes, EventBridge Scheduler, Lambda, EC2, DynamoDB, IAM, STS, Organizations, SSM, Secrets Manager, CloudWatch Logs, CloudWatch (Metrics & Alarms), KMS, CloudFormation, Cloud Control API, SES, Cognito User Pools, Cognito Identity, Kinesis, Firehose, RDS, RDS Data API, Aurora DSQL, Resource Groups, Resource Groups Tagging API, ElastiCache, MemoryDB, EKS, Cloud Map, Step Functions, API Gateway v1 (REST), API Gateway v2 (HTTP), Bedrock, Bedrock Agent, Bedrock Agent Runtime, Bedrock Runtime, ECR, ECS, Elastic Load Balancing v2, CloudFront, Route 53, WAF v2, Application Auto Scaling, Athena, ACM, and Glue.

Tested

### Conformance tested

True 100% conformance across all 3,932 implemented API operations: 248,557/248,557 Smithy-model-generated test variants pass on every commit, backed by end-to-end tests against the official AWS SDKs.

Real behavior

### Cross-service flows are wired up

30+ service-to-service integrations: S3 notifications, SNS fanout, EventBridge targets, DynamoDB Streams, CloudWatch Logs subscriptions, Cognito triggers, Step Functions task integrations, API Gateway → Lambda, and more — all exercise real service interactions.

Simple

### No auth, no lock-in, no paid tier

Run a single binary or Docker image, use any dummy credentials, and keep the whole workflow local without signing in to someone else's platform.

## SDKs for test assertions

Keep using the AWS SDK in your app. Use fakecloud SDKs when your tests need visibility and control on top of the emulator itself.

TypeScript

### `npm install fakecloud`

Inspect SES emails, SNS messages, SQS queues, Lambda invocations, Cognito confirmation codes, and more.

[View on npm](https://www.npmjs.com/package/fakecloud)

Python

### `pip install fakecloud`

Async and sync clients for pytest or app-level integration tests, with the same introspection and simulation coverage.

[View on PyPI](https://pypi.org/project/fakecloud/)

Go

### `go get github.com/faiscadev/fakecloud/sdks/go`

Context-friendly helpers for resets, event history, SES inbound simulation, and the rest of the fakecloud test surface.

[View on pkg.go.dev](https://pkg.go.dev/github.com/faiscadev/fakecloud/sdks/go)

Rust

### `cargo add fakecloud-sdk`

Native Rust client for tests that need resets, state inspection, and direct access to the fakecloud introspection surface.

[View on crates.io](https://crates.io/crates/fakecloud-sdk)

## Quick Start

Start fakecloud, point your AWS client at localhost, then assert what happened.

$ curl -fsSL https://fakecloud.dev/install.sh | bash\
$ fakecloud

$ aws --endpoint-url http://localhost:4566 sqs create-queue \\\
     --queue-name my-queue

$ npm install fakecloud

## How it compares

If you want a fully local workflow with real AWS APIs and no account requirement, fakecloud is built for that path. The SDKs are an extra testing advantage, not a replacement for that positioning.

|                     | fakecloud                                                                                        | LocalStack Community           |
| ------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------ |
| License             | AGPL-3.0                                                                                         | Proprietary                    |
| Auth required       | No                                                                                               | Yes (account + token)          |
| Commercial use      | Free                                                                                             | Paid plans only                |
| Docker required     | No (standalone binary)                                                                           | Yes                            |
| Startup time        | \~300ms                                                                                          | \~3s                           |
| Idle memory         | \~10 MiB                                                                                         | \~150 MiB                      |
| Install size        | \~19 MB binary                                                                                   | \~1 GB Docker image            |
| AWS services        | 46 at true 100% conformance                                                                      | 30+ (paid-only past Community) |
| Test assertion SDKs | TypeScript, Python, Go, PHP, Java, Rust                                                          | Python, Java                   |
| Cognito User Pools  | 122 operations                                                                                   | Paid only                      |
| SES v2              | 110 operations                                                                                   | Paid only                      |
| SES inbound email   | Real receipt rule actions                                                                        | Stored but never executed      |
| RDS                 | 163 operations, PostgreSQL/MySQL/MariaDB via Docker, PostgreSQL `aws_lambda` extension           | Paid only                      |
| ElastiCache         | 75 operations, Redis and Valkey via Docker                                                       | Paid only                      |
| MemoryDB            | 45 operations, full control plane                                                                | Paid only                      |
| EKS                 | Cluster control plane (batch 1)                                                                  | Paid only                      |
| API Gateway v2      | 103 operations, HTTP APIs + developer portals + JWT/Lambda authorizers                           | Paid only                      |
| Bedrock             | 214 operations across 4 APIs (Bedrock + Bedrock Runtime + Bedrock Agent + Bedrock Agent Runtime) | Not available                  |

## Supported services

Every implemented API is covered by conformance and behavioral testing, with the service mix tuned for local development and integration tests.

S3

REST protocol · XML

SQS

Query protocol · XML

SNS

Query protocol · XML

EventBridge

JSON protocol

EventBridge Scheduler

JSON protocol

SSM

JSON protocol

DynamoDB

JSON protocol

IAM

Query protocol

STS

Query protocol

Lambda

REST protocol · JSON

Secrets Manager

JSON protocol

CloudWatch Logs

JSON protocol

KMS

JSON protocol

CloudFormation

Query protocol

SES

REST + Query protocol

Cognito User Pools

JSON protocol

Kinesis

JSON protocol

RDS

Query protocol

ElastiCache

Query protocol

MemoryDB

JSON 1.1 protocol

EKS

REST-JSON protocol

Step Functions

JSON protocol

API Gateway v1 (REST)

REST protocol · JSON

API Gateway v2

REST protocol · JSON

Bedrock

REST protocol · JSON

Bedrock Runtime

REST protocol · JSON

Bedrock Agent

REST protocol · JSON

Bedrock Agent Runtime

REST protocol · JSON

ECR

REST protocol · JSON

ECS

JSON protocol

Elastic Load Balancing v2

Query protocol

CloudFront

REST protocol · XML

Route 53

REST protocol · XML

ACM

JSON protocol

ACM PCA

JSON protocol

Application Auto Scaling

JSON protocol

WAF v2

JSON protocol

Athena

JSON protocol

Glue

JSON protocol

Cognito Identity

JSON protocol

CloudWatch (Metrics & Alarms)

Query protocol

Firehose

JSON protocol

EC2

Query protocol · XML

Organizations

JSON protocol

Cloud Control API

JSON protocol

EventBridge Pipes

REST protocol · JSON

RDS Data API

REST protocol · JSON

Aurora DSQL

REST protocol · JSON

Resource Groups

REST protocol · JSON

Resource Groups Tagging API

JSON protocol
