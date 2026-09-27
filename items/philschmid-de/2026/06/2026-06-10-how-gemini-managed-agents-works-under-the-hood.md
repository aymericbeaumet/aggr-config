---
title: How Gemini Managed Agents Works under the Hood
link: https://www.philschmid.de/how-managed-agents-work
source: philschmid-de
published: 2026-06-10T00:00:00Z
updated: 2026-06-10T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: A single API call boots a sandbox, loads skills, and lets the model loop until the task is done. Here is what happens at each step.
content: extracted
html: 2026-06-10-how-gemini-managed-agents-works-under-the-hood.html
preview:
  file: 2026-06-10-how-gemini-managed-agents-works-under-the-hood.preview-b183206a7d4d.webp
  width: 256
  height: 134
  alt: How Gemini Managed Agents Works under the Hood
  color: '#111217'
images:
- source: https://www.philschmid.de/static/blog/how-managed-agents-work/thumbnail.jpg
  original:
    file: 2026-06-10-how-gemini-managed-agents-works-under-the-hood.image-9f303a589c53.jpg
    width: 1500
    height: 785
  color: '#0b0c10'
---

You can go from zero to a working agent in five lines. One API call, and the response comes back with a finished PDF, charts, and a summary.

Python

```python
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Analyze the latest lego set data and create a PDF report with charts",
    environment="remote",
)
 
print(interaction.output_text)
```

**What most people miss:** this is not a single model call that returns text. Behind that one call, a sandbox boots, skills get loaded, and the model enters a loop where it reasons, picks tools, executes code, reads the output, and repeats until the task is done.

## The Execution Loop, Step by Step

`interactions.create()` does more than send a prompt. It spins up a full execution environment.

User

Your code

Gemini API

Orchestrator

Gemini 3.5 Flash

Reasoning

Sandbox

Linux VM

1. You send a prompt to the Gemini API
2. The API boots a sandbox, routes your prompt to the model, dispatches tool calls, and collects results
3. Gemini 3.5 Flash plans and picks tools, looping until the task is complete
4. Code runs inside an isolated Linux VM, not on your machine

Prompt

Write a python script that prints hello world and execute it.

### Explore each step

**Request:** Your prompt hits the Interactions API via HTTPS. The API accepts it and starts orchestrating the interaction server-side.

1 / 10

## Inside the Sandbox

Each interaction gets its own isolated Linux container. The agent can install packages, run scripts, read and write files, and browse the web. The sandbox runs Ubuntu with 4 vCPU and 16 GB RAM. Environment compute is **not billed** during preview (you pay only for model tokens).

Environments can persist across interactions. The first call returns an `environment_id` you pass back to keep files and state intact. See the [Environments documentation](https://ai.google.dev/gemini-api/docs/agent-environment) for sources, networking, and lifecycle details.

Python

```python
# Turn 1: sandbox is created
i1 = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Install pandas and create analysis.py",
    environment="remote",
)
 
# Turn 2: same sandbox, files persist
i2 = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Run analysis.py on the Q1 data",
    environment=i1.environment_id,
    previous_interaction_id=i1.id,
)
```

## From Prompt to Production Agent

The execution loop is the same whether you are experimenting or running in production. The difference is how much configuration you want to provide upfront.

1. **Ad-hoc call:** Send a input with `environment="remote"`.

Python

```python
interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Write a Python script that prints hello world",
    environment="remote",
)
```

2. **Add instructions and skills:** Use `system_instruction` or `AGENTS.md` to customize behavior and provide skills via `sources`.

Python

```python
interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Analyze Q1 revenue and create a slide deck.",
    system_instruction="You are a data analyst. Export results as PDF.",
    environment={
        "type": "remote",
        "sources": [
            {"type": "inline", "target": ".agents/AGENTS.md",
             "content": "Always use matplotlib for charts."},
            {"type": "inline", "target": ".agents/skills/slides/SKILL.md",
             "content": "---\nname: slides\n---\n# Slide Maker\nCreate HTML decks."},
        ],
    },
)
```

3. **Save as a managed agent:** Once your setup is stable, persist it with `client.agents.create()`. Each invocation forks a fresh sandbox from the base environment.

Python

```python
agent = client.agents.create(
    id="data-analyst",
    base_agent="antigravity-preview-05-2026",
    system_instruction="You are a data analyst. Export results as PDF.",
    base_environment={
        "type": "remote",
        "sources": [
            {"type": "repository", "source": "https://github.com/my-org/templates",
             "target": "/workspace/templates"},
        ],
    },
)
```

4. **Invoke by ID:** Reference the agent ID on every call. Same loop, no config to repeat.

Python

```python
result = client.interactions.create(
    agent="data-analyst",
    input="Analyze Q1 revenue data and create a slide deck.",
    environment="remote",
)
print(result.output_text)
```

## Where to go next

- [Managed Agents Developer Guide](https://www.philschmid.de/gemini-managed-agents-developer-guide)
- [Try in AI Studio](https://aistudio.google.com/prompts/new_chat?model=antigravity-preview-05-2026)
- [Quickstart](https://ai.google.dev/gemini-api/docs/managed-agents-quickstart)
- [Build Custom Agents](https://ai.google.dev/gemini-api/docs/custom-agents)
- [Environments](https://ai.google.dev/gemini-api/docs/agent-environment)
- [Interactions API](https://ai.google.dev/gemini-api/docs/interactions)
