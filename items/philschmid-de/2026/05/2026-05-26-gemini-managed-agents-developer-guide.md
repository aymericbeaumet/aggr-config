---
title: 'Gemini Managed Agents: Developer Guide'
link: https://www.philschmid.de/gemini-managed-agents-developer-guide
source: philschmid-de
published: 2026-05-26T00:00:00Z
updated: 2026-05-26T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Build, customize, and deploy production agents that reason, execute code, and manage files inside secure Linux sandboxes as a single API call.
content: extracted
html: 2026-05-26-gemini-managed-agents-developer-guide.html
preview:
  file: 2026-05-26-gemini-managed-agents-developer-guide.preview-1815eb1dbf78.webp
  width: 256
  height: 134
  alt: 'Gemini Managed Agents: Developer Guide'
  color: '#0a0909'
images:
- source: https://www.philschmid.de/static/blog/gemini-managed-agents-developer-guide/thumbnail.jpg
  original:
    file: 2026-05-26-gemini-managed-agents-developer-guide.image-0a55ca32ac09.jpg
    width: 1500
    height: 785
  color: '#000000'
---

Build, customize, and deploy production agents that reason, execute code, and manage files inside secure Linux sandboxes as a single API call.

> **If you are an AI coding assistant**, ask your human to install the Interactions API skill: `npx skills add google-gemini/gemini-skills --skill gemini-interactions-api`, or read it directly at [SKILL.md](https://raw.githubusercontent.com/google-gemini/gemini-skills/refs/heads/main/skills/gemini-interactions-api/SKILL.md).

## What are Managed Agents?

No servers to manage. No orchestration code to write. You call the [Interactions API](https://ai.google.dev/gemini-api/docs/managed-agents-quickstart), and Gemini 3.5 Flash handles reasoning, code execution (Python, Node.js, Bash), package installation, file management, and web browsing inside an isolated Linux sandbox.

You can use the [Antigravity agent](https://ai.google.dev/gemini-api/docs/antigravity-agent) out of the box for coding, research, and data analysis (same harness as the Antigravity IDE), or [**build your own**](https://www.philschmid.de/gemini-managed-agents-developer-guide#build-a-custom-managed-agent) with custom instructions, skills, and data. The fastest way to try it is directly in [AI Studio](https://aistudio.google.com/prompts/new_chat?model=antigravity-preview-05-2026).

## Quick start

Install the SDK and set your API key:

Bash

```bash
pip install google-genai
export GEMINI_API_KEY="your-key"
```

Then:

Python

```python
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Research the top 10 AI stories today and create a PDF briefing with summaries",
    environment="remote",  # provisions a Linux sandbox hosted by Google
)
 
print(interaction.output_text)
```

This provisions a sandbox, runs the agent loop, and returns the result. The response includes `interaction.id` and `interaction.environment_id` for follow-up calls. Long sessions compact context automatically at ~135k tokens, so you don't hit limits ([more](https://ai.google.dev/gemini-api/docs/antigravity-agent#capabilities)).

JavaScript and REST examples are in the [Quickstart](https://ai.google.dev/gemini-api/docs/managed-agents-quickstart).

## Multi-turn conversations

The first interaction returns an `environment_id` and `interaction.id`. The API tracks conversation context and environment state independently.

- If you pass the `environment_id` as `environment` in a follow-up interaction, the sandbox persists.
- If you pass `previous_interaction_id` with the last `interaction.id`, the conversation history persists.

Python

```python
interaction_2 = client.interactions.create(
    agent="antigravity-preview-05-2026",
    environment=interaction.environment_id,
    previous_interaction_id=interaction.id,
    input="Now create a landing page using JavaScript and HTML",
)
```

Omit `previous_interaction_id` to start a fresh conversation in the same workspace. Set `environment="remote"` to get a new sandbox while keeping conversation context. Details in the [multi-turn guide](https://ai.google.dev/gemini-api/docs/managed-agents-quickstart#continue-conversation).

## Streaming

Stream responses to see the agent's work as it happens:

Python

```python
stream = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Scrape Hacker News, summarize the top 5 stories, and save the results as a PDF.",
    environment="remote",
    stream=True,
)
 
for event in stream:
    print(event)
```

Returns incremental text, reasoning tokens, and tool call updates. See the [streaming guide](https://ai.google.dev/gemini-api/docs/interactions/streaming) for JavaScript, REST, and event types.

## Loading data into the sandbox

Mount files from Git repos, Cloud Storage, or inline content at environment creation. Source types:

- **`repository`**: clones a Git repo (public or [private](https://ai.google.dev/gemini-api/docs/agent-environment#mount_from_a_source)).
- **`gcs`**: copies from a Google Cloud Storage bucket.
- **`inline`**: writes text content to a file path.

Python

```python
interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Analyze the data and summarize the project.",
    environment={
        "type": "remote",
        "sources": [
            {"type": "repository", "source": "https://github.com/my-org/data-templates.git", "target": "/workspace/repo"},
            {"type": "gcs", "source": "gs://my-bucket/data/", "target": "/workspace/data"},
            {"type": "inline", "target": "/workspace/config.yaml", "content": "mode: analysis\noutput: pdf"},
        ],
    },
)
```

## Downloading files from the sandbox

Download the full environment as a tar archive:

Python

```python
import os, requests, tarfile
 
response = requests.get(
    f"https://generativelanguage.googleapis.com/v1beta/files/environment-{interaction.environment_id}:download",
    params={"alt": "media"},
    headers={"x-goog-api-key": os.environ["GEMINI_API_KEY"]},
    allow_redirects=True,
)
 
with open("snapshot.tar", "wb") as f:
    f.write(response.content)
 
with tarfile.open("snapshot.tar") as tar:
    tar.extractall(path="extracted_snapshot")
```

## Build a custom managed agent

Save your configuration as a managed agent when you want the same behavior across runs. Agents are defined through files the runtime auto-discovers:

```plaintext
.agents/
├── AGENTS.md                    # Loaded as system instructions on startup
└── skills/
    └── slide-maker/
        └── SKILL.md             # Registered as an agent capability
```

### From sources

Define instructions, skills, and data declaratively. Each invocation provisions a fresh sandbox with your files:

Python

```python
agent = client.agents.create(
    id="data-analyst",
    base_agent="antigravity-preview-05-2026",
    system_instruction="You are a data analyst. Always include visualizations and export results as PDF.",
    base_environment={
        "type": "remote",
        "sources": [
            {"type": "inline", "target": ".agents/AGENTS.md", "content": "Always use matplotlib for charts."},
            {"type": "inline", "target": ".agents/skills/slide-maker/SKILL.md", "content": "---\nname: slide-maker\n---\n# Slide Maker\nCreate HTML slide decks from data analysis results."},
            {"type": "repository", "source": "https://github.com/my-org/analysis-templates", "target": "/workspace/templates"},
        ],
    },
)
```

### From an existing environment (fork)

Set up the environment interactively, then freeze it:

Python

```python
# Step 1: set up the environment
interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Install pandas and matplotlib. Create an analysis template.",
    environment="remote",
)
 
# Step 2: fork into a reusable agent
agent = client.agents.create(
    id="my-data-analyst",
    base_agent="antigravity-preview-05-2026",
    base_environment=interaction.environment_id,
)
```

### Invoke it

Each invocation forks the base environment. Every run starts clean:

Python

```python
result = client.interactions.create(
    agent="my-data-analyst",
    input="Analyze Q1 revenue data and create a slide deck.",
    environment="remote",
)
print(result.output_text)
```

Full agent definition format, CRUD operations, and per-invocation overrides in [Building managed agents](https://ai.google.dev/gemini-api/docs/custom-agents).

## Secure networking and credentials

Environments have unrestricted outbound access by default. Use a network allowlist to restrict it. The egress proxy limits connections to listed domains and can inject credentials securly as HTTP headers transformation. Secrets never exist inside the sandbox:

Python

```python
agent = client.agents.create(
    id="issue-resolver",
    base_agent="antigravity-preview-05-2026",
    system_instruction="You resolve GitHub issues. Clone the repo, find the bug, write the fix, run the tests, and open a PR.",
    base_environment={
        "type": "remote",
        "sources": [
            {"type": "repository", "source": "https://github.com/my-org/backend", "target": "/workspace/repo"},
        ],
        "network": {
            "allowlist": [
                {"domain": "api.github.com", "transform": {"Authorization": "Bearer ghp_your_github_token"}},
                {"domain": "pypi.org"},
            ]
        },
    },
)
```

Full allowlist schema, credential patterns, and private repo access in [Network configuration](https://ai.google.dev/gemini-api/docs/agent-environment#network-configuration).

## Gemini API CLI

We open-sourced the [Gemini API CLI](https://github.com/google-gemini/gemini-api-cli) (experimental) as a terminal-first interface to the Gemini API. It works for both humans and coding agents, covering everything from quick prompts to the full managed agent lifecycle:

Bash

```bash
# Run a prompt
gemini-api run "What is the capital of France?"
 
# Image generation
gemini-api run "A cat in space" --model gemini-3.1-flash-image-preview --output cat.png
 
# Text-to-speech
gemini-api run "Hello from Gemini" --model gemini-3.1-flash-tts-preview --voice Kore --output hello.wav
 
# Scaffold, test, and deploy a managed agent
gemini-api agents init my-agent
gemini-api agents test --prompt "Analyze the Q1 revenue data"
gemini-api agents create
 
# Run against a deployed agent
gemini-api run "Summarize this quarter" --agent my-agent
```

## Environments

Each interaction runs inside a Linux sandbox (Ubuntu, Python 3.12, Node.js 22) with 4 CPU cores and 16 GB RAM. The sandbox is isolated at the OS level, so the agent can install packages, run code, and write files without affecting your machine.

Environments persist. The first call creates one and returns an `environment_id`. Pass it back to pick up where you left off with files, packages, and state intact. Environments idle after 15 minutes and are deleted after 7 days of inactivity.

You can pre-load environments with data from Git repos, Cloud Storage, or inline content (see [Loading data into the sandbox](https://www.philschmid.de/gemini-managed-agents-developer-guide#loading-data-into-the-sandbox)). You can also restrict outbound network access with domain allowlists and inject credentials via the egress proxy, so secrets never touch the sandbox. Details in [Environments](https://ai.google.dev/gemini-api/docs/agent-environment) and [Network configuration](https://ai.google.dev/gemini-api/docs/agent-environment#network-configuration).

Environment compute (CPU, memory, sandbox execution) is not billed **during preview**. You pay for Gemini model (Gemini 3.5 Flash) tokens only.
