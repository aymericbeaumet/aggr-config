---
title: 'Cloudflare K2: serverless event streams'
link: https://blog.cloudflare.com/cloudflare-k2-streams/
source: hnrss-org
published: 2026-10-01T14:09:10Z
updated: 2026-10-01T14:09:10Z
first_seen: 2026-10-01T19:33:44.897168732Z
authors:
- elffjs
content: extracted
html: 2026-10-01-cloudflare-k2-serverless-event-streams.html
preview:
  file: 2026-10-01-cloudflare-k2-serverless-event-streams.preview-01f7340fbbb3.webp
  width: 256
  height: 134
  color: '#f6f2f1'
images:
- source: https://blog.cloudflare.com/_emdash/api/media/file/01M3SXWJMRR7ZENPNFNB5QZYPA.jpg
  original:
    file: 2026-10-01-cloudflare-k2-serverless-event-streams.image-5c57dbe9841e.jpg
    width: 1200
    height: 628
  color: '#fefefe'
---

With traditional Remote Procedure Call (RPC) architectures, there exists a core challenge: producers and consumers must align in scale and in time. If your producers send too much data for your consumers to handle or if your consumers or downstream services become unavailable, events are dropped. This problem is compounded with multiple consumers that need to independently process the data. For example, an ecommerce backend may emit events when transactions are completed, which need to be read by an analytics system and a fraud detection service.

We can solve this by *decoupling* our producers and consumers — inserting a service in the middle that absorbs writes while allowing independent readers to consume at their own pace.

Today we are launching Cloudflare K2 in public beta to solve this problem. K2 is a durable event streaming primitive on the Developer Platform. You send events to a K2 stream, which stores them as an ordered log. Consumers can read them in a variety of ways, for example by splitting up reads across a set of consumers, or delivering all messages to all consumers. It's fully serverless, scales to vast quantities of data, and supports long-term retention, so even long periods of consumer downtime do not lose data.

Under the hood, K2 implements a partitioned, durable log on top of R2 object storage, which allows it to scale to huge volumes of storage.

If you’re ready to get started, you can create your first stream in seconds by following [the guide here](https://developers.cloudflare.com/k2/get-started/).

## Streams on the edge

We first built K2 because *we* needed a durable buffer on the edge, initially to serve as the ingestion layer for [Basin Pipelines](https://developers.cloudflare.com/pipelines/). Pipelines is powered by a [stream processing engine](https://www.arroyo.dev) that operates on a pull-based model, which means some other system has to store events before they are read, transformed, and written to R2. And because we commit to never dropping events once they’re accepted into the Pipelines Stream, that storage has to be durable — meaning it can’t lose data — over potentially long periods of time.

This is where most companies would deploy Apache Kafka. However, Pipelines runs on the Cloudflare edge, which spans a huge number of servers across over 335 cities. Our unique architecture means we often cannot run traditional distributed systems software like Kafka, and need to rethink how these systems are built and operated.

For stateful services, in particular, Cloudflare’s global infrastructure presents some challenges: we get relatively small slices of machines, those machines are relatively ephemeral, and networking is often over the public Internet. But our infrastructure also has a few superpowers: it’s close to users wherever they are in the world and has an incredible capacity to scale horizontally.

In designing the durable buffering system that became K2, we decided to rely on the powerful state primitive we already have: R2. Object storage systems like R2 combine extremely durable storage ([11 9s!](https://developers.cloudflare.com/r2/reference/durability/)) with strongly consistent APIs. Offloading replication and consensus to the storage layer allows us to make the application layer (K2 in this case) radically simpler, cheaper, and higher performance. A secondary benefit is that it separates compute and storage, meaning each can be scaled independently. This allows us to store vast quantities of historical data at low cost.

How do we build a log on top of object storage? An immediate issue is that R2 — like other object stores — does not support appends, the standard operation on a log. Instead, we must write complete files, or segments, that are large enough to overcome the cost of writing and reading each one. We do this by first accumulating writes in-memory on an edge service. After waiting a short period for data to arrive, we write all events as a segment file. We achieve ordering and strictly incrementing offsets using R2’s atomic operations without needing a separate coordination service.

While building on R2 has many advantages, there is one downside: higher produce latencies. Writing to object storage is slower than a local disk, and we have to wait for the local batch to accumulate before starting the write. In our initial release of K2, this adds up to about 1 second of produce latency at the 99th percentile of response times.

We will be sharing more details on the design of K2 in an upcoming technical deep dive.

## Streams, Queues, or Pipelines?

Cloudflare has several existing asynchronous delivery primitives, including [Queues](https://developers.cloudflare.com/queues/) and [Basin Pipelines](https://developers.cloudflare.com/basin-pipelines/). When should you reach for K2 instead of these existing products?

There are some superficial similarities between Queues and K2 Streams: both receive events, durably store them, and deliver them to consumers. Queues are designed around tracking individual items of expensive or time consuming work that need to be asynchronously completed. For example, an image processing application may enqueue a user request to be handled by the actual image processing service. They support complex logic on the grain of a particular work item, like retries, delays, and dead-letter queues for failed attempts.

K2, by contrast, is designed for high-scale data movement, long-term retention, and fan-out consumption. Messages are produced and consumed as batches — enabling efficient processing at the expense of message-level retries. This batching also drives higher producer latency than for queues.

Basin Pipelines is a serverless ingestion service. You can send your Pipeline JSON events, which can be transformed and written to R2 or a Basin Catalog. We recommend Pipelines when the end result is writing your events to object storage or Iceberg tables, and K2 when doing custom processing or writing to other destinations.

## Getting started

Using K2 involves first creating a stream. You can have many streams across your account for different use cases or types of events. Streams can be created via [cf](https://blog.cloudflare.com/cloudflare-cf-cli-launch/), Wrangler, the dashboard, or API.

Let's take the example of collecting and processing product analytics. First, we'll create a stream with cf:

```
$ cf k2 streams create --name app_events --http-enabled

{
  "id": "d78b09ee1f50430e9ec92a8af92b0231",
  "name": "app_events",
  "retention_seconds": 604800,
  "endpoint": "https://d78b09ee1f50430e9ec92a8af92b0231.k2.cloudflarestorage.com",
  "http": {
    "enabled": true,
    "authentication": false
  },
  "worker_binding": {
    "enabled": true
  },
  "created_at": "2026-09-28T15:14:39.053Z",
  "modified_at": "2026-09-28T15:14:39.053Z"
}
```

Once we have a stream, we can start producing to it, via an HTTP API or Worker binding. For example, from a Worker:

```
const result = await env.EVENTS.send([
  {
    content: new TextEncoder().encode(
      JSON.stringify({
        event: "page_view",
        path: new URL(request.url).pathname,
        timestamp: Date.now(),
      }),
    ),
    headers: { "content-type": "application/json" },
  },
]);

if (!result.success) {
  console.error(`Produce failed: ${result.error.message}`);
  return new Response("Failed to record event", {
    status: result.error.retryable ? 503 : 500,
  });
}
```

K2 represents data as bytes, so you can use whatever format or encoding makes sense for your application.

Now that we have events in a stream, we can create a subscription. Subscriptions divide up work between consumers, enabling *read parallelism* — scaling out to multiple readers to handle more load than a single server can manage.

We can create a subscription via the HTTP API.

```
$ curl -X POST "https://d78b09ee1f50430e9ec92a8af92b0231.k2.cloudflarestorage.com/subscriptions" \
    -H "Authorization: Bearer ${CLOUDFLARE_API_TOKEN}" \
    -H "Content-Type: application/json" \
    --data '{
      "name": "analytics_processor",
      "start_at": { "type": "earliest" }
    }'
```

```
{
  "result": { "id": "ee13f761783d3823a447a47b572ebf76" },
  "success": true,
  "errors": [],
  "messages": []
}
```

With the subscription created, we can then poll it from each of our consumers:

```
$ curl -X POST \"https://4d8f5394e3e733debdeca9c65c5b7439.k2.cloudflarestorage.com/subscriptions/ee13f761783d3823a447a47b572ebf76/consume" \
    -H "Authorization: Bearer ${CLOUDFLARE_API_TOKEN}" \
    -H "Content-Type: application/json" \
    --data '{
      "worker_id": "analytics-1",
      "max_records": 100
    }' 
```

```
{
  "result": {
    "batch_id": "b7e4c9210a3f468d95c2e1068fdb734a",
    "leased_until_ms": 1790633929437,
    "records": [
      {
        "timestamp_ms": 1790633629168,
        "content": "eyJldmVudCI6InBhZ2VfdmlldyJ9",
        "headers": {
          "content-type": "application/json"
        }
      },
      ...
    ]
  },
  "success": true,
  "errors": [],
  "messages": []
}
```

When a client calls `consume`, they receive a *lease* for that particular batch of events for 5 minutes. The client can do one of three things:

- *ack* the batch, which marks it as processed and ensures it will not be redelivered
- *nack* (negative ack) it, meaning we’ve failed to process it and would like it to be redelivered
- *extend* its lease, in case it needs more time to complete processing

```
$ curl -X POST "https://d78b09ee1f50430e9ec92a8af92b0231.k2.cloudflarestorage.com/subscriptions/ee13f761783d3823a447a47b572ebf76/batches/b7e4c9210a3f468d95c2e1068fdb734a/ack" \
  -H "Authorization: Bearer ${CLOUDFLARE_API_TOKEN}" \
  -H "Content-Type: application/json" \
  --data '{ "worker_id": "analytics-1" }'
```

This is one way to consume from K2: splitting work amongst multiple consumers such that each consumer gets a portion of the data. Another way to read is with a separate subscription for each consumer — the pub/sub pattern — in which case each consumer sees all of the messages. Or you can mix-and-match between these two approaches, having multiple independent consumer pools.

See the [K2 docs](https://developers.cloudflare.com/k2/) for full details on the APIs.

## Pricing and Availability

K2 is available today in public beta for accounts with Workers Paid subscriptions, within these limits:

- Maximum of 10GB of storage used
- 30 MB/s produce per stream

If you need higher limits, please reach out to the team on Discord or fill out the [limit increase form](https://docs.google.com/forms/d/e/1FAIpQLSf1_eGOxXFGsh4O72fqdvDbYBjzpV3Q6Vf5X3fqpoTHObGFIA/viewform).

Usage of K2 will not be billed during the beta period. Once we begin billing, we anticipate this pricing:

|               |                    |
| ------------- | ------------------ |
|               | Pricing            |
| Data Produced | $0.04 / GB         |
| Data Consumed | $0.04 / GB         |
| Data Retained | $0.02 / GB / month |

## What’s next

We have an exciting roadmap for K2 over the coming months, including:

- Higher write parallelism, up to multi-GB/s streams
- Message keys and key-based ordering guarantees
- Push-based worker consumers
- Express tier with lower produce and end-to-end latencies
- Drop-in support for Apache Kafka clients

We’re excited to see what you build on K2! Share your feedback on the [Cloudflare Discord](https://discord.com/invite/cloudflaredev).
