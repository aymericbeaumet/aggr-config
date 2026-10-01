---
title: RIP, vector database
link: https://turbopuffer.com/blog/rip-vector-database
source: hnrss-org
published: 2026-10-01T16:01:56Z
updated: 2026-10-01T16:01:56Z
first_seen: 2026-10-01T19:33:44.897168732Z
authors:
- razin
content: extracted
html: 2026-10-01-rip-vector-database.html
preview:
  file: 2026-10-01-rip-vector-database.preview-7adc2ca088e8.webp
  width: 256
  height: 134
  color: '#192033'
images:
- source: https://turbopuffer.com/og/turbopuffer.png
  original:
    file: 2026-10-01-rip-vector-database.image-8d8fc3bf4095.png
    width: 1200
    height: 630
  variants:
  - file: 2026-10-01-rip-vector-database.image-9703fbbac45e.webp
    width: 320
    height: 168
  - file: 2026-10-01-rip-vector-database.image-423aa39b278a.webp
    width: 640
    height: 336
  - file: 2026-10-01-rip-vector-database.image-0d9f6ab4d8e2.webp
    width: 1200
    height: 630
  color: '#0f172a'
---

September 30, 2026 • Dan Harrison (Engineer)

We are changing turbopuffer's storage architecture to take search to the next level. turbopuffer v3 changes how documents and indexes are laid out, written, compacted, and queried in turbopuffer. It will allow us to make search faster in every respect — including text, regex, and vector search — but it also lays the foundation to move many more SQL queries to turbopuffer and make them fast.

turbopuffer launched as a [serverless vector database (v1)](https://x.com/Sirupsen/status/1709602855160869275), highly specialized to the task of serving extremely cheap and reasonably fast vector searches. Object storage as the source of truth gave the economics, and tiered NVMe SSD/memory caches gave the performance. The value of these particular tradeoffs was validated by our earliest customers, including Cursor and Notion.

turbopuffer evolved to have very strong text and regex search (v2), and is being used for many non-search use cases, like [Linear's syncing engine](https://linear.app/now/rebuilding-delta-sync-read-path). The query engine has evolved along the way to support all of these query plans, but the storage architecture has remained largely unchanged: the ANN vector index was and still is the primary index around which all other indexes and query plans revolve. This design has constrained several query plans, like `GROUP BY` and aggregations.

We've pushed the vector-primary architecture as far as we can, and it's time to move on. We're in the process of moving to a new primary index, and making ANN "just another" secondary index. We thought it might be fun to open up the doors and let you follow along.

For this first update, we'll set the stage with why we're doing this in the first place. Walk with me on a short journey from tpuf v1 to today.

## v1: an ID and a vector

In the first version of turbopuffer, documents consisted of nothing but an ID and a vector. The prevailing wisdom at the time was graph-based vector indexes, but a hierarchical clustering index plays better with object storage. We started with [SPANN](https://arxiv.org/abs/2111.08566), and eventually migrated to [SPFresh](https://arxiv.org/abs/2410.14452) to support incremental indexing. Vectors are clustered into groups, whose centroids are clustered in turn, repeated to form a tree with a single root.

```

                      ┌───────────────────┐
                      │   root centroid   │
                      └───────────────────┘
                     ╱          │          ╲
                    ╱           │           ╲
┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐
│   leaf centroid   │ │   leaf centroid   │ │   leaf centroid   │
└───────────────────┘ └───────────────────┘ └───────────────────┘
      ╱       ╲             ╱       ╲             ╱       ╲
     ╱         ╲           ╱         ╲           ╱         ╲
┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐
│ vector │ │ vector │ │ vector │ │ vector │ │ vector │ │ vector │
└────────┘ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘

```

```
      ┌───────────────┐
      │ root centroid │
      └───────────────┘
        ╱     │     ╲
       ╱      │      ╲
┌────────┐┌────────┐┌────────┐
│  leaf  ││  leaf  ││  leaf  │
│centroid││centroid││centroid│
└────────┘└────────┘└────────┘
   ╱  ╲      ╱  ╲      ╱  ╲
┌───┐┌───┐┌───┐┌───┐┌───┐┌───┐
│vec││vec││vec││vec││vec││vec│
└───┘└───┘└───┘└───┘└───┘└───┘
```

We implemented this on top of a storage layer presenting as a key-value map, with sorted and unique keys. Each cluster is given a `ClusterId`, and vectors within each cluster are given a dense `LocalId`.

```rust
// leaf vectors
K::Vector(C0L0) = vec![0.45, 0.32, ...]
K::Id(C0L0) = 7
K::Vector(C0L1) = vec![-0.28, 0.96, ...]
K::Id(C0L1) = 13

// cluster centroid for C0 is itself clustered at the next level of the tree
K::Vector(C1L4) = vec![0.64, -0.48, ...]
K::Id(C1L4) = C0
```

As you can see above, everything is keyed by `ClusterId` and `LocalId` (e.g. `C0L1`), which together we call the **ANN address**. This is what we mean when we say the ANN index is the primary index.

## v2: attribute filtering and full-text search

Two new query plans marked the informal transition from turbopuffer v1 → v2: attribute filtering and full-text search.

### Attribute filtering

Naturally, customers wanted to be able to add attribute values and filter vector searches on them. To [make filtering fast and high-recall](https://turbopuffer.com/blog/native-filtering), we modeled these as an inverted index that maps an attribute value to the ANN address of the documents that contain it.

```rust
K::AttrIndex("family", "Alcidae") -> vec![C0L3, C1L2, C1L3, ...]
K::AttrIndex("genus", "Fratercula") -> vec![C0L3, C1L2, C1L9, ...]
```

For projections (`include_attributes`), we also stored the document attributes alongside the ID and the vector.

```rust
K::Vector(C0L0) = vec![0.45, 0.32, ...]
K::Id(C0L0) = 7
K::Attr(C0L0, "family") = "Alcidae"
K::Attr(C0L0, "genus") = "Fratercula"
```

### Full-text search

BM25 full-text search was another obvious and much-demanded query plan. Similar to attribute search, full-text search works by first finding the documents that have the query term present (commonly called "postings"). For an FTS index, we also include the `(term count, document length)` metadata necessary for BM25 scoring:

```rust
K::FTS("description", "Atlantic") -> vec![(C0L0, 2, 37), (C9L4, 1, 42), ...]
K::Attr(C0L0, "description") -> "A sharply dressed black-and-white seabird with a \
huge, multicolored bill, the Atlantic Puffin is often \
called the clown of the sea. It breeds in burrows on \
islands in the North Atlantic, and winters at sea."
```

Over time, we've shipped several other index structures and query engines: [aggregations](https://turbopuffer.com/docs/query#aggregations), [regex search](https://turbopuffer.com/docs/query#param-Regex), [fuzzy matching](https://turbopuffer.com/docs/fts#fuzzy-matching), [sparse vector search](https://turbopuffer.com/docs/query#sparse-vector-search), and [attribute ordering](https://turbopuffer.com/docs/query#ordering-by-attributes) — all built around the same vector-primary storage layout.

## The problem with a vector primary index

The ANN primary index has largely remained intact until today for one simple reason: it works really, really well for ANN search on object storage. On top of this architecture, we've pushed vector search to [single indexes of 100B+ vectors serving 200 ms p99 reads at 1k+ QPS](https://turbopuffer.com/blog/ann-v3). Any significant change here risks introducing regressions in ANN performance.

However, this layout holds us back from being state-of-the-art for the non-vector query shapes we support, in three main ways: storage amplification, write amplification, and limited vectorization.

### Storage amplification

As described above, turbopuffer currently puts the full contents of each document under its ANN address. When there is only one vector, the non-vector data is stored alongside the vector only once.

However, for multi-vector representations of a document, such as document nesting or late interaction, this means we have to duplicate the contents for each vector. This is the reason for some of our more unfortunate [limits](https://turbopuffer.com/docs/limits#limit-max-vector-columns-per-namespace).

### Write amplification

Any time a document is inserted, updated, or deleted, SPFresh may rebalance the vectors to ensure they remain well clustered (otherwise recall may suffer). Because everything in a document is stored keyed by the ANN address of the document's vector, this rebalancing cascades to moving the full document contents, as well as any inverted (attribute and FTS) indexes that reference it. Updating just one vector can move hundreds of attributes and their indexes.

This write amplification is large enough that our efforts to tune indexing throughput have started to hit diminishing returns.

### Limited vectorization

Modern query engines are [vectorized](https://turbopuffer.com/blog/fts-v2-maxscore): they run tight loops over blocks of values, which amortizes fixed per-block costs, compresses better, keeps the CPU pipeline full, and unlocks SIMD. DuckDB, for example, works in batches of 2,048 rows, ClickHouse up to ~65k, Lucene's posting blocks are 256 docs, and our ANN index works best with clusters of around 100–200 documents. Every query plan has an optimal block size, but today they are all constrained by the ANN primary index. A plan that wants blocks of thousands of documents to keep the CPU saturated is still stuck at 100–200.

We've already documented how much this matters in turbopuffer. Our first version of full-text search partitioned posting lists along ANN cluster boundaries, and the median block held just ~1.5 postings. FTS v2 [reworked postings](https://turbopuffer.com/blog/fts-v2-postings) into fixed blocks of ~256, and the index got 10x smaller and queries got up to 20x faster. Posting lists could do that because they're stored separately and point at documents, so their layout doesn't have to follow the clusters. Aggregations and other scans read the documents themselves, and those are stored one block per cluster. As long as the ANN address is the primary key, their block size is constrained to the cluster size, even if they'd prefer something larger.

## RIP, primary vector index

The solution to these problems is simple: don't key on the ANN address. That is precisely the change turbopuffer v3 makes. As you can imagine, it is not a trivial change.

v3 is a new foundation that will unlock significant performance improvement on all query plans, and we hit a major milestone earlier this month: 100% of CI passes on turbopuffer v3. We started by focusing on correctness. Now we will make it correct *and* fast. Watching benchmark numbers go down is great fun, so we wanted to get you in at day zero of perf grinding. We will share [the benchmarks](https://turbopuffer.com/v3) in public over the coming weeks, as we work toward (and beyond) performance parity before rolling out v3 to production.

## turbopuffer

turbopuffer is a fast search engine that hosts 1T+ documents, handles 10M+ writes/s, and serves 25k+ queries/s. We are ready for far more. We hope you'll trust us with your queries.

[Get started](https://turbopuffer.com/join)
