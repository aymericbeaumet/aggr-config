---
title: 'Obsidian-Copilot: An Assistant for Writing & Reflecting'
link: https://eugeneyan.com//writing/obsidian-copilot/
source: eugeneyan-com
published: 2023-06-11T00:00:00Z
updated: 2023-06-11T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- llm
- 🛠
summary: Writing drafts via retrieval-augmented generation. Also reflecting on the week's journal entries.
content: extracted
html: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.html
preview:
  file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.preview-c79cadf6209b.webp
  width: 256
  height: 134
  color: '#353535'
images:
- source: https://eugeneyan.com/assets/og_image/copilot-og.jpg
  original:
    file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-49a31cc3df83.jpg
    width: 1200
    height: 630
  color: '#353535'
- source: https://eugeneyan.com/assets/copilot.webp
  original:
    file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-300096a0f6ef.webp
    width: 1200
    height: 392
  color: '#f6f0e2'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://i.ytimg.com/vi/QRJW5jT5VRA/hqdefault.jpg
  original:
    file: 2023-06-11-obsidian-copilot-an-assistant-for-writing-reflecting.image-d269f319753f.jpg
    width: 480
    height: 360
  color: '#f5f0dd'
---

What would a copilot for writing and thinking look like? To try answering this question, I built a prototype: Obsidian-Copilot. Given a section header, it helps draft a few paragraphs via [retrieval-augmented generation](https://docs.aws.amazon.com/sagemaker/latest/dg/jumpstart-foundation-models-customize-rag.html). Also, if you write a daily journal, it can help you reflect on the past week and plan for the week ahead.

![Obsidian Copilot: Helping to write drafts and reflect on the week](https://eugeneyan.com/assets/copilot.webp "Obsidian Copilot: Helping to write drafts and reflect on the week")

Obsidian Copilot: Helping to write drafts and reflect on the week

Here’s a short 2-minute demo. The code is available at [obsidian-copilot](https://github.com/eugeneyan/obsidian-copilot).

[YouTube video player](https://www.youtube.com/watch?v=QRJW5jT5VRA)

## How does it work?

**We start by parsing documents into chunks.** A sensible default is to [chunk documents by token length](https://github.com/hwchase17/langchain/blob/master/langchain/text_splitter.py#L58), typically 1,500 to 3,000 tokens per chunk. However, I found that this [didn’t work very well](https://eugeneyan.com/writing/llm-experiments/#documents-may-be-inadequately-chunked). A better approach might be to chunk by paragraphs (e.g., split on `\n\n`).

Given that my notes are mostly in bullet form, I [chunk by top-level bullets](https://github.com/eugeneyan/obsidian-copilot/blob/main/src/prep/build_vault_dict.py#L73): Each chunk is made up of a single top-level bullet and its sub-bullets. There are usually 5 to 10 sub-bullets per top-level bullet making each chunk similar in length to a paragraph.

```python
chunks = defaultdict()
current_chunk = []
chunk_idx = 0
current_header = None

for line in lines:

    if '##' in line:  # Chunk header = Section header
        current_header = line
    
    if line.startswith('- '):  # Top-level bullet
        if current_chunk:  # If chunks accumulated, add it to chunks
            if len(current_chunk) >= min_chunk_lines:
                chunks[chunk_idx] = current_chunk
                chunk_idx += 1
            current_chunk = []  # Reset current chunk
            if current_header:
                current_chunk.append(current_header)
    
    current_chunk.append(line)
```

Next, we build an OpenSearch index and a semantic index on these chunks. In a previous experiment, I found that [embedding-based retrieval alone might be insufficient](https://eugeneyan.com/writing/llm-experiments/#embedding-based-retrieval-alone-might-be-insufficient) and thus added classical search (i.e., [BM25](https://en.wikipedia.org/wiki/Okapi_BM25) via OpenSearch) in this prototype.

**For OpenSearch, we start by configuring filters and fields.** We include [filters](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/analysis-tokenfilters.html) such as [stripping HTML](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/analysis-htmlstrip-charfilter.html), [removing possessives](https://lucene.apache.org/core/9_6_0/analysis/common/org/apache/lucene/analysis/en/EnglishPossessiveFilter.html) (i.e., the trailing ‘s in words), removing stopwords, and basic stemming. These filters are applied on both documents (during indexing) and queries. We also specify the fields we want to index and their respective types. Types matter because filters are applied on text fields (e.g., title, chunk) but not on keyword fields (e.g., path, document type). We don’t apply preprocessing on file paths to keep them as they are.

```python
'mappings': {
	'properties': {
		'title': {'type': 'text', 'analyzer': 'english_custom'},
		'type': {'type': 'keyword'},
		'path': {'type': 'keyword'},
		'chunk_header': {'type': 'text', 'analyzer': 'english_custom'},
		'chunk': {'type': 'text', 'analyzer': 'english_custom'},
	}
}
```

When querying, we apply [boosts](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/mapping-boost.html) to make some fields count more towards the relevance score. In this prototype, I arbitrarily [boosted](https://github.com/eugeneyan/obsidian-copilot/blob/main/src/prep/build_opensearch_index.py#L163) titles by 5x and chunk headers (i.e., top-level bullets) by 2x. Retrieval can be improved by tweaking these boosts as well as [other features](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/query-dsl-rank-feature-query.html).

**For semantic search, we start by picking an embedding model.** I referred to the [Massive Text Embedding Benchmark Leaderboard](https://huggingface.co/spaces/mteb/leaderboard), sorted it on descending order of retrieval score, and picked a model that had a good balance of embedding dimension and score.

This led me to [e5-small-v2](https://huggingface.co/intfloat/e5-small-v2). Currently, it’s ranked a respectable 7th, right below text-embedding-ada-002. What’s impressive is its embedding size of 384 which is far smaller than what most models have (768 - 1,536). And while it supports a maximum sequence length of only 512, this is sufficient given my shorter chunks. (More details in the paper [Text Embeddings by Weakly-Supervised Contrastive Pre-training](https://arxiv.org/abs/2212.03533).) After embedding these documents, we store them in a `numpy` array.

During query time, we tokenize and embed the query, do a dot product with the document embedding array, and take the top *n* results (in this case, 10).

```python
def query_semantic(query, tokenizer, model, doc_embeddings_array, n_results=10):
    query_tokenized = tokenizer(f'query: {query}', max_length=512, padding=False, truncation=True, return_tensors='pt')
    outputs = model(**query_tokenized)
    query_embedding = average_pool(outputs.last_hidden_state, query_tokenized['attention_mask'])
    query_embedding = F.normalize(query_embedding, p=2, dim=1).detach().numpy()

    cos_sims = np.dot(doc_embeddings_array, query_embedding.T)
    cos_sims = cos_sims.flatten()

    top_indices = np.argsort(cos_sims)[-n_results:][::-1]

    return top_indices
```

If you’re thinking of using the e5 models, remember to add the necessary prefixes during preprocessing. For documents, you’ll have to prefix them with “`passage:‎`” and for queries, you’ll have to prefix them with “`query:‎`”

**The retrieval service is a FastAPI app.** Given an input query, it [performs both BM25 and semantic search](https://github.com/eugeneyan/obsidian-copilot/blob/main/src/app.py#L144), deduplicates the results, and returns the documents’ text and associated title. The latter is used to [link source documents](https://www.youtube.com/watch?v=QRJW5jT5VRA&t=72s) when generating the draft.

To start the OpenSearch node and semantic search + FastAPI server, we use a [simple-docker compose file](https://github.com/eugeneyan/obsidian-copilot/blob/main/docker-compose.yml). They each run in their own containers, bridged by a common network. For convenience, we also define common commands in a [Makefile](https://github.com/eugeneyan/obsidian-copilot/blob/main/Makefile).

**Finally, we integrate with Obsidian via a TypeScript plugin.** The [obsidian-plugin-sample](https://github.com/obsidianmd/obsidian-sample-plugin) made it easy to get started and I added functions to display retrieved documents in a new tab, query APIs, and stream the output. (I’m new to TypeScript so feedback appreciated!)

## What else can we apply this to?

While this prototype uses local notes and journal entries, it’s not a stretch to imagine the copilot retrieving from other documents (online). For example, team documents such as product requirements and technical design docs, internal wikis, and even code. I’d guess that’s what Microsoft, Atlassian, and Notion are working on right now.

It also extends beyond personal productivity. Within my field of recommendations and search, researchers and practitioners are excited about [layering LLM-based generation](https://arxiv.org/abs/2306.02887) on top of existing systems and products to improve the customer experience. (I expect we’ll see some of this in production by the end of the year.)

## Ideas for improvement

One idea is to try LLMs with larger context sizes that allow us to feed in entire documents instead of chunks. (This may help with retrieval recall but puts more onus on the LLM to identify the relevant context for generation.) Currently, I’m using gpt-3.5-turbo which is a good balance of speed and cost. Nonetheless, I’m excited to try claude-1.3-100k and provide entire documents as context.

Another idea is to augment retrieval with web or internal search when necessary. For example, when documents and notes go stale (e.g., based on last updated timestamp), we can look up the web or internal documents for more recent information.

Here’s the [GitHub repo](https://github.com/eugeneyan/obsidian-copilot) if you’re keen to try. Start by cloning the repo and updating the path to your obsidian-vault and huggingface hub cache. The latter saves us from downloading the tokenizer and model each time you start the containers.

```bash
git clone https://github.com/eugeneyan/obsidian-copilot.git

# Open Makefile and update the following paths
export OBSIDIAN_PATH = /Users/eugene/obsidian-vault/
export TRANSFORMER_CACHE = /Users/eugene/.cache/huggingface/hub
```

Then, build the image and indices before starting the retrieval app.

```bash
# Build the docker image
make build

# Start the opensearch container and wait for it to start. 
# You should see something like this: [c6587bf83572] Node 'c6587bf83572' initialized
make opensearch

# In ANOTHER terminal, build your artifacts (this can take a while)
make build-artifacts

# Start the app. You should see this: Uvicorn running on http://0.0.0.0:8000
make run
```

Finally, install the copilot plugin, enable it in community plugin settings, and update the API key. You’ll have to restart your Obsidian app if you had it open before installation.

```bash
make install-plugin
```

If you tried it, I would love to hear how it went, especially where it didn’t work well and how it can be improved. Or if you’ve been working with retrieval-augmented generation, I’d love to hear about your experience so far!

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Jun 2023). Obsidian-Copilot: An Assistant for Writing & Reflecting. eugeneyan.com. https://eugeneyan.com/writing/obsidian-copilot/.

or

```
@article{yan2023copilot,
  title   = {Obsidian-Copilot: An Assistant for Writing & Reflecting},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Jun},
  url     = {https://eugeneyan.com/writing/obsidian-copilot/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
