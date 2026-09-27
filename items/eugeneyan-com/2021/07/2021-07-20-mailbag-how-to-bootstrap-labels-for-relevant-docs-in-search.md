---
title: 'Mailbag: How to Bootstrap Labels for Relevant Docs in Search'
link: https://eugeneyan.com//writing/mailbag-bootstrap-relevant-docs/
source: eugeneyan-com
published: 2021-07-20T00:00:00Z
updated: 2021-07-20T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- machinelearning
- 📬
summary: Building semantic search; how to calculate recall when relevant documents are unknown.
content: extracted
html: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.html
preview:
  file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.preview-7b9bb42e7493.webp
  width: 256
  height: 134
  color: '#898980'
images:
- source: https://eugeneyan.com/assets/og_image/mailbag.jpg
  original:
    file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-8d6d4b130d08.jpg
    width: 1200
    height: 630
  color: '#b5b8b9'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2021-07-20-mailbag-how-to-bootstrap-labels-for-relevant-docs-in-search.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

A writes:

> Recently I’m trying to build a semantic search system with my own data and I came across your [blog post](https://eugeneyan.com/writing/search-query-matching/). I found quite a few papers using “Recall@K” as an evaluation metric (e.g. Semantic Product Search by Amazon, Embedding-based Retrieval in Facebook Search by Facebook, Embedding-based Product Retrieval in Taobao Search), but it is unclear how they obtain the total number of relevant documents (or items) for their query-document pairs.
>
> While it is totally possible to hire a lot of annotators to figure out which documents are relevant to a search query, I don’t think that is economically feasible at all. Do you have any idea how engineers in industry figure out the total number of relevant documents (or items) for their query-document pairs? Many thanks!

If I had to build a search engine from scratch, I would:

- Start with lexical matching such [BM25](https://en.wikipedia.org/wiki/Okapi_BM25) or what’s available in Elasticsearch or Solr
- Deploy this in production and collect data on what users click on (i.e., labels)
- Then, use these labels for semantic search

I think using human annotators can work, but probably only for defects or edge cases, given how costly it is.

* * *

Have a question for me? Happy to answer concise questions via email on topics I know about. More details in [How I Can Help](https://eugeneyan.com/how-i-can-help/).

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
