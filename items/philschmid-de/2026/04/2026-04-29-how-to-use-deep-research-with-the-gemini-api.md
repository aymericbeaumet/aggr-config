---
title: How to use Deep Research with the Gemini API
link: https://www.philschmid.de/deep-research-update
source: philschmid-de
published: 2026-04-29T00:00:00Z
updated: 2026-04-29T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The Gemini Deep Research Agent autonomously plans, searches, and synthesizes long-horizon research tasks into detailed, cited reports.
content: extracted
html: 2026-04-29-how-to-use-deep-research-with-the-gemini-api.html
preview:
  file: 2026-04-29-how-to-use-deep-research-with-the-gemini-api.preview-7ab38fdf656d.webp
  width: 256
  height: 134
  alt: How to use Deep Research with the Gemini API
  color: '#08090b'
images:
- source: https://www.philschmid.de/static/blog/deep-research-update/thumbnail.jpg
  original:
    file: 2026-04-29-how-to-use-deep-research-with-the-gemini-api.image-57e8c8619b35.jpg
    width: 1500
    height: 785
  color: '#000000'
---

The Gemini Deep Research Agent autonomously plans, searches, and synthesizes long-horizon research tasks into detailed, cited reports.

Deep Research handles long-running tasks by executing in the background. It is exclusively available through the **Interactions API** (not `generate_content`).

Two new versions are available:

- `deep-research-preview-04-2026`: Designed for speed & efficiency, ideal to be streamed back to a client UI
- `deep-research-max-preview-04-2026`: Maximum comprehensiveness for automated context gathering & synthesis

## What's new

- **Collaborative planning:** Review and refine the research plan before execution
- **Native charts & infographics:** Agent-generated charts, graphs, and infographics
- **Remote MCP server:** Connect external tools via the Model Context Protocol
- **Extended tooling:** Google Search, URL Context, Code Execution, MCP, and File Search
- **Multimodal research grounding:** Pass images, PDFs, and audio as research context

## Setup

Install the Python SDK:

Shell

```shell
pip install google-genai
```

Set your API key as an environment variable. You can create one at [aistudio.google.com/apikey](https://aistudio.google.com/apikey).

Shell

```shell
export GEMINI_API_KEY="your-api-key"
```

## Run your first Deep Research task

Start a research task with `background=True` and poll for the result. Deep Research is asynchronous as tasks can take several minutes to complete.

Python

```py
import time
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    input="Research the history of Google TPUs.",
    agent="deep-research-preview-04-2026",
    background=True,
)
 
while True:
    interaction = client.interactions.get(interaction.id)
    if interaction.status == "completed":
        print(interaction.outputs[-1].text)
        break
    elif interaction.status == "failed":
        print(f"Research failed: {interaction.error}")
        break
    time.sleep(10)
```

## Collaborative planning

Set `collaborative_planning=True` to get a research plan back instead of running immediately. Iterate on the plan with `previous_interaction_id`, then set `collaborative_planning=False` to execute.

Step 1: Request a plan

Python

```py
import time
from google import genai
 
client = genai.Client()
 
plan = client.interactions.create(
    agent="deep-research-preview-04-2026",
    input="Research Google TPUs vs competitor hardware.",
    agent_config={"type": "deep-research", "collaborative_planning": True},
    background=True,
)
 
while (result := client.interactions.get(id=plan.id)).status != "completed":
    time.sleep(5)
 
print(result.outputs[-1].text)
```

**Refine the plan:** Use `previous_interaction_id` to continue the conversation. Keep `collaborative_planning=True` to stay in planning mode. Repeat as needed.

Python

```py
refined = client.interactions.create(
    agent="deep-research-preview-04-2026",
    input="Add a section comparing power efficiency.",
    agent_config={"type": "deep-research", "collaborative_planning": True},
    previous_interaction_id=plan.id,
    background=True,
)
 
while (result := client.interactions.get(id=refined.id)).status != "completed":
    time.sleep(5)
 
print(result.outputs[-1].text)
```

**Approve and execute:** Set `collaborative_planning=False` to approve the plan and start the research.

**Important:** You must explicitly set `collaborative_planning=False` on the final turn. Simply sending "go ahead" without flipping the flag will not trigger report generation.

Python

```py
report = client.interactions.create(
    agent="deep-research-preview-04-2026",
    input="Plan looks good!",
    agent_config={"type": "deep-research", "collaborative_planning": False},
    previous_interaction_id=refined.id,
    background=True,
)
 
while (result := client.interactions.get(id=report.id)).status != "completed":
    time.sleep(5)
 
print(result.outputs[-1].text)
```

## Native charts and infographics

Set `visualization="auto"` and ask for visuals in your prompt. The agent generates charts and infographics returned as base64-encoded images.

Python

```py
import base64
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    agent="deep-research-preview-04-2026",
    input="Analyze global semiconductor market trends. Include charts showing market share changes.",
    agent_config={"type": "deep-research", "visualization": "auto"},
    background=True,
)
 
while (result := client.interactions.get(id=interaction.id)).status != "completed":
    time.sleep(5)
 
for output in result.outputs:
    if output.type == "text":
        print(output.text)
    elif output.type == "image" and output.data:
        image_bytes = base64.b64decode(output.data)
        # display(Image(data=image_bytes))  # Jupyter
```

**Tip:** Setting `visualization="auto"` enables the capability, but best results are achieved by explicitly asking for what you want.

## Remote MCP servers

Connect remote MCP servers to give the agent access to external tools. Pass the server `name`, `url`, and optional auth headers.

Python

```py
interaction = client.interactions.create(
    agent="deep-research-preview-04-2026",
    input="Research how recent geopolitical events influenced USD interest rates",
    tools=[
        {
            "type": "mcp_server",
            "name": "Finance Data Provider",
            "url": "https://finance.example.com/mcp",
            "headers": {"Authorization": "Bearer my-token"},
        }
    ],
    background=True,
)
```

MCP servers support no-auth, bearer token, and OAuth. For OAuth, fetch the token with a library like `google-auth` and pass it in `headers`. Use `allowed_tools` to restrict which tools the agent can call from the server.

## Tool configuration

By default the agent uses Google Search, URL Context, and Code Execution. You can customize the tools the agent can use by providing a list of `tools`, similar to models. This allows you to, for example, only search the web (via `google_search` and `url_context`), only search private sources (via `file_search` and custom MCP servers), or search a mix of both.

| Tool           | Type             | Default | Description                                 |
| -------------- | ---------------- | ------- | ------------------------------------------- |
| Google Search  | `google_search`  | ✅       | Search the public web                       |
| URL Context    | `url_context`    | ✅       | Read and summarize web pages                |
| Code Execution | `code_execution` | ✅       | Run code for calculations and data analysis |
| MCP Server     | `mcp_server`     | —       | Connect remote MCP servers                  |
| File Search    | `file_search`    | —       | Search uploaded document corpora            |

Python

```py
# Only web search allowed
interaction = client.interactions.create(
    agent="deep-research-preview-04-2026",
    input="Latest developments in quantum computing.",
    tools=[{"type": "google_search"}],
    background=True,
)
```

**Note:** Passing no `tools` at all defaults to Search, URL Context, and Code Execution enabled.

## Multimodal research grounding

Pass images, PDFs, and documents alongside your text prompt to ground the research.

Python

```py
interaction = client.interactions.create(
    agent="deep-research-preview-04-2026",
    input=[
        {"type": "text", "text": "What has been the impact of this research paper?"},
        {"type": "document", "uri": "https://arxiv.org/pdf/1706.03762", "mime_type": "application/pdf"},
    ],
    background=True,
)
```

## Real-time streaming with visuals and thought summaries

Stream research progress in real time. Enable `thinking_summaries="auto"` to receive the agent's intermediate reasoning alongside text and generated images.

Python

```py
import base64
from google import genai
from IPython.display import Image, display
 
client = genai.Client()
 
interaction_id = None
last_event_id = None
is_complete = False
 
def process_stream(stream):
    global interaction_id, last_event_id, is_complete
    for chunk in stream:
        if chunk.event_type == "interaction.start":
            interaction_id = chunk.interaction.id
        if chunk.event_id:
            last_event_id = chunk.event_id
        if chunk.event_type == "content.delta":
            if chunk.delta.type == "text":
                print(chunk.delta.text, end="", flush=True)
            elif chunk.delta.type == "thought_summary":
                print(f"\n💭 {chunk.delta.content.text}", flush=True)
            elif chunk.delta.type == "image" and chunk.delta.data:
                image_bytes = base64.b64decode(chunk.delta.data)
                display(Image(data=image_bytes))
        elif chunk.event_type in ("interaction.complete", "error"):
            is_complete = True
            if chunk.event_type == "interaction.complete":
                print("\n✅ Research Complete")
 
stream = client.interactions.create(
    input="Research AI chip market trends. Include charts comparing vendors.",
    agent="deep-research-preview-04-2026",
    background=True,
    stream=True,
    agent_config={
        "type": "deep-research",
        "thinking_summaries": "auto",
        "visualization": "auto",
    },
)
process_stream(stream)
 
# Reconnect if the connection drops
while not is_complete and interaction_id:
    status = client.interactions.get(interaction_id)
    if status.status != "in_progress":
        break
    stream = client.interactions.get(
        id=interaction_id, stream=True, last_event_id=last_event_id,
    )
    process_stream(stream)
```

## Where to go next

- [Deep Research documentation](https://ai.google.dev/gemini-api/docs/deep-research)
- [Try Deep Research in Google AI Studio](https://aistudio.google.com)
- [Interactions API Documentation](https://ai.google.dev/gemini-api/docs/interactions)
