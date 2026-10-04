---
title: Distributed databases with Peter Mattis
link: https://newsletter.pragmaticengineer.com/p/distributed-databases-with-peter
source: newsletter-pragmaticengineer-com
published: 2026-09-30T16:30:05Z
updated: 2026-09-30T16:30:05Z
first_seen: 2026-09-30T20:48:45.272500234Z
authors:
- Gergely Orosz
summary: Cockroach Labs co-founder Peter Mattis discusses building reliable distributed systems and how AI helps him write more code without sacrificing quality.
content: extracted
html: 2026-09-30-distributed-databases-with-peter-mattis.html
preview:
  file: 2026-09-30-distributed-databases-with-peter-mattis.preview-c7880c524835.webp
  width: 256
  height: 144
  color: '#bbb6b5'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/0GzwuYGvKA4
  original:
    file: 2026-09-30-distributed-databases-with-peter-mattis.image-d779b4747fd7.jpg
    width: 728
    height: 410
  color: '#f0f0f0'
- source: https://substackcdn.com/image/fetch/$s_!Qhi0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1228ff6a-514b-4643-9360-02cda14dc5ac_800x70.png
  original:
    file: 2026-09-30-distributed-databases-with-peter-mattis.image-e0041e8ec5a7.jpg
    width: 800
    height: 70
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!jw61!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F906855ba-aa27-4c43-9e76-ee966ca9cb9e_548x288.png
  original:
    file: 2026-09-30-distributed-databases-with-peter-mattis.image-e18f80b9b9ec.jpg
    width: 548
    height: 288
  color: '#fefefe'
- source: https://substackcdn.com/image/fetch/$s_!7W2s!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc1ead9b0-c1ba-4418-8712-e1cd39f7bef6_960x267.png
  original:
    file: 2026-09-30-distributed-databases-with-peter-mattis.image-e9a1fc225830.png
    width: 960
    height: 267
  color: '#3771c7'
- source: https://substackcdn.com/image/fetch/$s_!0D2M!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F46bf5715-c7d5-487a-b90a-1979fb5c10a8_1395x700.png
  original:
    file: 2026-09-30-distributed-databases-with-peter-mattis.image-01728cd6296b.png
    width: 1395
    height: 700
  variants:
  - file: 2026-09-30-distributed-databases-with-peter-mattis.image-d1a0ef337a8d.webp
    width: 320
    height: 161
  - file: 2026-09-30-distributed-databases-with-peter-mattis.image-abaefed4022b.webp
    width: 640
    height: 321
  - file: 2026-09-30-distributed-databases-with-peter-mattis.image-4e0d998e10bd.webp
    width: 960
    height: 482
  - file: 2026-09-30-distributed-databases-with-peter-mattis.image-41887e0d839a.webp
    width: 1395
    height: 700
  color: '#242424'
- source: https://i.ytimg.com/vi/0GzwuYGvKA4/hqdefault.jpg
  original:
    file: 2026-09-30-distributed-databases-with-peter-mattis.image-bdc1c7131aa7.jpg
    width: 480
    height: 360
  color: '#f1f1f1'
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/217889431/bba96d28746310eb65c6f215045feb9f.mp3
  duration_seconds: 6110
---

**Listen and watch now on [YouTube](https://youtu.be/0GzwuYGvKA4), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

[![](https://substackcdn.com/image/fetch/$s_!Qhi0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1228ff6a-514b-4643-9360-02cda14dc5ac_800x70.png)](https://substackcdn.com/image/fetch/$s_!Qhi0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1228ff6a-514b-4643-9360-02cda14dc5ac_800x70.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – The turbopuffer engineering team is doing something really cool: they are completely redesigning their storage architecture from first principles to make search faster, cheaper, and more reliable at scale. And they’re documenting all of it! Follow along their rewrite at [turbopuffer.com/v3](http://turbopuffer.com/v3)

• **[Linear](https://linear.app/pragmatic)** – most of us work with agents in a “single-player” setting. Linear’s take is agent work should be teamwork: your teammates can follow the session of the agent, check out the PR it produces, and join the review. Works with Codex, Cursor, Linear’s own agent, or custom agents. [Check it out](https://linear.app/pragmatic)

**• [WorkOS](https://workos.com/)** – how do you deal with agents and permissions? WorkOS built [Airlock](http://workos.com/airlock), the authorization layer for AI agents. It evaluates every request against the agent’s intent and your rules and either allows it, denies it, or routes it to a human for approval. Works with Claude Code, Codex and MCP gateways. [Give it a spin](http://workos.com/airlock).

How is it that a software veteran who regularly shipped ~100K of database-grade code to production each year, pre-AI, feels like he’s even more productive today, with no drop in quality? [Peter Mattis](https://x.com/petermattis) is co-founder and CTO of [Cockroach Labs](https://www.cockroachlabs.com), and an original creator of GIMP. He also worked on Gmail and distributed storage at Google.

In this episode, Peter reflects on his journey from open source to Google to founding a database company, and we explore how to keep systems fast, reliable, and correct at scale, from Gmail’s early storage challenges to the tradeoffs in building distributed databases.

Peter tells us how AI has brought him back to writing code after his work shifted toward management, and why he believes AI can improve quality and multiply the impact of domain experts. We also consider the future of code review, and Peter has some advice about how to level up our engineering skills.

[www.youtube.com](https://www.youtube.com/watch?v=0GzwuYGvKA4)

**1\. Peter initially turned down Sergey Brin’s offer to join Google because of the commute**. The very first version of the famous Google logo was made in GIMP, the free, open-source raster graphics and image editing software created by Peter and his college roommate Spencer Kimball.

[![](https://substackcdn.com/image/fetch/$s_!jw61!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F906855ba-aa27-4c43-9e76-ee966ca9cb9e_548x288.png)](https://substackcdn.com/image/fetch/$s_!jw61!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F906855ba-aa27-4c43-9e76-ee966ca9cb9e_548x288.png)

*The first version of the Google logo, created by GIMP*

In 2001, Sergey reached out to Peter, invited him to an interview and then made an offer. But Peter said “no” because he lived in San Francisco and did not fancy the hour-long commute to Mountain View. Instead, he joined another startup, but that didn’t go anywhere. When Google reached out again, he took the chance.

**2\. Peter almost did not ship GIMP after learning about an even more ambitious photo editor**. Peter and Spencer worked hard on GIMP, but a few weeks before launching it, they saw an announcement about another program that promised to do everything GIMP did – and then some! Peter and Spencer felt discouraged but shipped anyway – and the rest is history. They never heard about the other ambitious project again. “There’s always going to be someone else working on your idea,” says Peter. “You can’t get dissuaded if they pre-announce it. To founders: assume that dozens of people have the same idea you have, but most won’t ship it! Know that it’s a competition; you’ve got to enjoy that aspect of it – don’t be afraid of it.”

**3\. B-trees were used in the first version of Gmail**. Gmail launched on 1 April 2004, offering 1GB of email storage for free, an offer seen as so ridiculously generous at the time that most people assumed it was an April Fool’s joke! B-trees played a role in Gmail’s storage layer, thanks to storing email threads. Under the hood, each incoming message was matched to a thread using the search index. Threads and their unread counts were tracked by B-trees.

**4\. Colossus reduced Google’s file storage overhead by 33%, while increasing redundancy**. Before Colossus, Google File System (GFS) stored three full copies of data. For Colossus, which became GFS’s successor, Peter and the team pioneered [Reed–Solomon erasure coding](https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction) in a distributed file system. Data was stored twice, but redundancy increased!

**5\. The fastest way to send a packet around the globe is through space!** Peter keeps “speed of light numbers” in his head – similar to what turbopuffer founder Simon Eskildsen does with [”napkin math”](https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits) calculations. For example, a network round trip within a zone went from milliseconds when Colossus was built to about 100 microseconds today (10x faster!). But sometimes the bottleneck is the speed of light, and as light speed is faster in a vacuum than anywhere else, that means the fastest global route is straight up to Starlink, across by laser, and then back down. Find out more in ‘ [Delay is not an option: low latency routing in space](https://discovery.ucl.ac.uk/id/eprint/10062262/7/Handley_hotnets.pdf) ’.

**6\. Peter twice “beat” standard library data structures in performance terms.** At Google, one of his colleagues noticed that std::map showed up in memory profiles. Peter looked closer and figured out that the std::map implementation is a [red-black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree), meaning every node has two pointers. Peter built a B-tree with nearly the same semantics which was both faster, due to spatial locality, and smaller because it used fewer pointers. They used this data structure inside Google. Peter also did something similar with Go’s map: it was performant, but he built a [Swiss Table](https://abseil.io/about/design/swisstables) implementation that was faster. That implementation later made it into the Go library, with the Go team helping to finish it!

**7\. If you squint hard, everything in distributed databases and storage systems starts looking like a B-tree**. These B-trees are a recurring theme in this podcast episode: the backend of Gmail, the std::map replacement, CockroachDB’s range index, etc. There’s even a paper on this phenomenon, [The Ubiquitous B-Tree](https://dl.acm.org/doi/10.1145/356770.356776).

[![](https://substackcdn.com/image/fetch/$s_!7W2s!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc1ead9b0-c1ba-4418-8712-e1cd39f7bef6_960x267.png)](https://substackcdn.com/image/fetch/$s_!7W2s!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc1ead9b0-c1ba-4418-8712-e1cd39f7bef6_960x267.png)

*A B-tree: a very useful data structure. Source: [Wikipedia](https://en.wikipedia.org/wiki/B-tree)*

**8.** **For consensus in a distributed database, at least three replicas are needed**. With only one, recovery after it crashes isn’t possible. With primary and secondary replicas, neither one will know if the other received the last write following a crash. So, CockroachDB uses three replicas by default – and up to five for some system tables – and customers can add more, although this will result in higher latency.

**9\. CockroachDB’s design was inspired by Google’s Colossus data storage system and Spanner**. Colossus was append-only, meaning you could only add to files and not mutate them. Google’s distributed database, Spanner, was built on top of Colossus, and so Spanner’s design decisions came from Colossus’ append-only nature. But if you cannot update files and only append to them, it’s not possible to use B-trees as the database’s design structure since B-trees need the files to be updated. An ideal data structure for append-only filesystems is the [Log-structured merge tree](https://en.wikipedia.org/wiki/Log-structured_merge-tree) (LSM tree), where writes are appended to a log file for durability:

[![](https://substackcdn.com/image/fetch/$s_!0D2M!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F46bf5715-c7d5-487a-b90a-1979fb5c10a8_1395x700.png)](https://substackcdn.com/image/fetch/$s_!0D2M!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F46bf5715-c7d5-487a-b90a-1979fb5c10a8_1395x700.png)

*A log-structured merge tree. Source: [Wikipedia](https://en.wikipedia.org/wiki/Log-structured_merge-tree)*

Google popularized LSMs with [LevelDB](https://github.com/google/leveldb), and [RocksDB](https://rocksdb.org/) later forked LevelDB. RocksDB was the underlying storage engine that CockroachDB used in its early years. In 2019, Peter wrote and open sourced [Pebble](https://github.com/cockroachdb/pebble), which is now CockroachDB’s storage engine.

**10\. Peter, the former C++ readability reviewer at Google, predicts we will stop reviewing code**: At Google, every code change needs to be signed off for “readability” by a language readability reviewer, and Peter was a C++ readability reviewer for years. But these days, he’s reviewing less code and foresees this continuing. As he puts it: “What I’m finding is the agents are getting better; you’re having to give less and less scrutiny. I don’t know if it’s going to be this year or next, \[but\] we’re materially going to stop looking at the code in the same way we don’t look at assembly anymore.”

**11\. Non-engineers at Cockroach Labs built ~1,000 internal apps (!!) in a couple of months.** Earlier this year, Cockroach Labs launched an internal platform for non-devs to build apps (think of it as an “internal Lovable”). Folks jumped at it; HR professionals built new tools they’d only dreamt of, while the CFO created the dashboards they’d always longed for, and many others too. Peter and the engineering team were surprised at this uptake, but it chimes with how OpenAI saw nearly all its non-engineers move almost their entire token spend from ChatGPT to Codex within just 4 months.

**12\. Does “flow” still exist with AI**? I asked Peter this, and he believes it does, but it’s different from the “coding flow” state: “It definitely feels a bit different. It’s maybe a bit less intense, but you’re managing more things cognitively. I’m thinking of ideas that normally would’ve taken me a week to experiment with. And I think of multiple of these experiments and then fire them off all simultaneously. \[It feels as\] if I was a college professor with a whole swarm of research assistants, and they’re all off doing things and it’s coming back *really* rapidly.”

• [Inside Google’s Engineering Culture](https://newsletter.pragmaticengineer.com/p/google)

• [Resiliency in distributed systems](https://newsletter.pragmaticengineer.com/p/resiliency-in-distributed-systems)

• [How to debug large, distributed systems: Antithesis](https://newsletter.pragmaticengineer.com/p/antithesis)

• [Pushing software engineering limits with “napkin math”](https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits)

• [Designing Data-intensive Applications with Martin Kleppmann](https://newsletter.pragmaticengineer.com/p/designing-data-intensive-applications)

• [Formal methods with Hillel Wayne](https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne)

[00:00](https://www.youtube.com/watch?v=0GzwuYGvKA4) Intro

[02:42](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=162s) Peter’s path into tech

[04:00](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=240s) Building GIMP

[09:30](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=570s) Working on Gmail at Google

[14:51](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=891s) Google’s infra: google3, build files, Bazel, and Colossus

[21:30](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=1290s) Distributed storage bottlenecks

[23:59](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=1439s) Latency, throughput, and availability

[30:04](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=1804s) Contributing to libraries

[41:52](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=2512s) Google Spanner

[46:10](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=2770s) CockroachDB

[52:00](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=3120s) Manual vs. automatic sharding

[55:28](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=3328s) Consistency models and strong consistency

[1:00:03](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=3603s) Raft consensus

[1:06:15](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=3975s) How AI brought Peter back to coding

[1:19:12](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=4752s) Peter’s tools and agentic workflows

[1:23:08](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=4988s) How AI can improve quality

[1:26:39](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=5199s) Code reviews: are they done?

[1:29:17](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=5357s&pp=0gcJCWMAwfN6Pr3D) 100x engineers

[1:35:33](https://www.youtube.com/watch?v=0GzwuYGvKA4&t=5733s) Peter’s advice for leveling up your engineering skills

**Where to find Peter Mattis:**

• X: [https://x.com/petermattis](https://x.com/petermattis)

• LinkedIn: [https://www.linkedin.com/in/peter-mattis-46549144](https://www.linkedin.com/in/peter-mattis-46549144)

**Mentions during the episode:**

• Cockroach Labs: [https://www.cockroachlabs.com](https://www.cockroachlabs.com)

• GIMP: [https://www.gimp.org](https://www.gimp.org)

• Usenet: [https://en.wikipedia.org/wiki/Usenet](https://en.wikipedia.org/wiki/Usenet)

• Larry Page: [https://en.wikipedia.org/wiki/Larry_Page](https://en.wikipedia.org/wiki/Larry_Page)

• Sergey Brin: [https://en.wikipedia.org/wiki/Sergey_Brin](https://en.wikipedia.org/wiki/Sergey_Brin)

• Paul Buchheit on X: [https://x.com/paultoo](https://x.com/paultoo)

• Y Combinator: [https://www.ycombinator.com](https://www.ycombinator.com)

• Delay is Not an Option: Low Latency Routing in Space: [https://discovery.ucl.ac.uk/id/eprint/10062262/7/Handley_hotnets.pdf](https://discovery.ucl.ac.uk/id/eprint/10062262/7/Handley_hotnets.pdf)

• Bazel: [https://opensource.google/projects/bazel](https://opensource.google/projects/bazel)

• Colossus under the hood: a peek into Google’s scalable storage system: [https://cloud.google.com/blog/products/storage-data-transfer/a-peek-behind-colossus-googles-file-system](https://cloud.google.com/blog/products/storage-data-transfer/a-peek-behind-colossus-googles-file-system)

• Pushing software engineering limits with “napkin math”: [https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits](https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits)

• turbopuffer: [https://turbopuffer.com](https://turbopuffer.com)

• Trimodal Nature of Tech Compensation in the US, UK and India: [https://newsletter.pragmaticengineer.com/p/trimodal](https://newsletter.pragmaticengineer.com/p/trimodal)

• Quicksort: [https://en.wikipedia.org/wiki/Quicksort](https://en.wikipedia.org/wiki/Quicksort)

• B-tree: [https://en.wikipedia.org/wiki/B-tree](https://en.wikipedia.org/wiki/B-tree)

• Go: [https://go.dev](https://go.dev)

• Google Goggles: [https://en.wikipedia.org/wiki/Google_Goggles](https://en.wikipedia.org/wiki/Google_Goggles)

• Spencer Kimball on LinkedIn: [linkedin.com/in/spencerwkimball](http://linkedin.com/in/spencerwkimball)

• Ben Darnell: [https://en.wikipedia.org/wiki/Ben_Darnell](https://en.wikipedia.org/wiki/Ben_Darnell)

• The Ubiquitous B-Tree: [https://dl.acm.org/doi/pdf/10.1145/356770.356776](https://dl.acm.org/doi/pdf/10.1145/356770.356776)

• Paxos: [https://martinfowler.com/articles/patterns-of-distributed-systems/paxos.html](https://martinfowler.com/articles/patterns-of-distributed-systems/paxos.html)

• Raft is so fetch: The Raft Consensus Algorithm explained through “Mean Girls”: [https://www.cockroachlabs.com/blog/raft-is-so-fetch/](https://www.cockroachlabs.com/blog/raft-is-so-fetch/)

• DoorDash: [https://www.doordash.com](https://www.doordash.com)

• RocksDB: [https://rocksdb.org](https://rubberduckdebugging.com)

• Rubber duck debugging: [https://rubberduckdebugging.com](https://rubberduckdebugging.com)

• Claude Code: [https://claude.com/product/claude-code](https://claude.com/product/claude-code)

• Codex: [https://chatgpt.com/codex](https://chatgpt.com/codex)

• Building Claude Code with Boris Cherny: [https://newsletter.pragmaticengineer.com/p/building-claude-code-with-boris-cherny](https://newsletter.pragmaticengineer.com/p/building-claude-code-with-boris-cherny)

• Programming TypeScript: [https://www.oreilly.com/library/view/programming-typescript/9781492037644](https://www.oreilly.com/library/view/programming-typescript/9781492037644)

• Building Codex with Tibo Sottiaux: [https://newsletter.pragmaticengineer.com/p/building-codex-with-tibo-sottiaux](https://newsletter.pragmaticengineer.com/p/building-codex-with-tibo-sottiaux)

• Terence Tao Digests the Jacobian Conjecture Counterexample: How Claude Fable 5 Broke an 87-Year-Old Math Problem: [https://www.developersdigest.tech/blog/jacobian-conjecture-counterexample-fable](https://www.developersdigest.tech/blog/jacobian-conjecture-counterexample-fable)

• Jeff Dean on LinkedIn: [https://www.linkedin.com/in/jeff-dean-8b212555](https://www.linkedin.com/in/jeff-dean-8b212555)

• Sanjay Ghemawat on LinkedIn: [https://www.linkedin.com/in/sanjay-ghemawat-763485428](https://www.linkedin.com/in/sanjay-ghemawat-763485428)

—

Production and marketing by [Pen Name](https://penname.co/).
