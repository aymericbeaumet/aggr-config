---
title: Gemini Interactions API Quick Start
link: https://www.philschmid.de/interactions-api-quickstart
source: philschmid-de
published: 2026-01-22T00:00:00Z
updated: 2026-01-22T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The Interactions API is a unified interface for building with Gemini models and agents. It simplifies the development of agentic applications by handling server-side state management, tool orchestration, and long-running tasks.
content: extracted
html: 2026-01-22-gemini-interactions-api-quick-start.html
images:
- source: https://colab.research.google.com/assets/colab-badge.svg
  original:
    file: 2026-01-22-gemini-interactions-api-quick-start.image-60eb1eb4b7b1.png
    width: 117
    height: 20
  variants:
  - file: 2026-01-22-gemini-interactions-api-quick-start.image-fc39a846ab34.webp
    width: 117
    height: 20
  color: '#0576b7'
---

The [Interactions API](https://ai.google.dev/gemini-api/docs/interactions?ua=chat) is a unified interface for building with Gemini models and agents. It simplifies the development of agentic applications by handling server-side state management, tool orchestration, and long-running tasks.

With a single endpoint, you can:

- Interact with Gemini models for text, image, and audio generation
- Build multi-turn conversations without managing history client-side
- Call custom functions and built-in tools like Google Search
- Run specialized agents like Deep Research for complex tasks

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/google-gemini/cookbook/blob/main/quickstarts/Get_started_interactions_api.ipynb)

## Prerequisites

Get a [Gemini API key](https://aistudio.google.com/apikey) and install the Google GenAI SDK:

Bash

```bash
pip install google-genai
```

Set your API key:

Bash

```bash
export GEMINI_API_KEY="your-api-key"
```

## Create an interaction

At its simplest, the Interactions API works like a standard chat completion. You provide a model and an input string. By default, interactions are stored (`store=True`), allowing you to reference them later.

Python

```python
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    model="gemini-3-flash-preview",
    system_instruction="You are a helpful assistant.",
    input="Explain quantum entanglement in one sentence."
)
 
print(interaction.outputs[-1].text)
# Output: Quantum entanglement is a phenomenon where particles become linked...
```

## Stateful Multi-turn Conversations

One of the most powerful features of this API is **server-side state management**. You do not need to append messages to a list and send the full history back to the server every time.

Use `previous_interaction_id` to continue a conversation.

Note: Only conversation history is preserved. Parameters like `tools` or `generation_config` are interaction-scoped and must be re-declared if needed.

Python

```python
from google import genai
 
client = genai.Client()
 
turn_1 = client.interactions.create(
    model="gemini-3-flash-preview",
    input="My name is Alice and I am a software engineer."
)
print(f"Turn 1 ID: {turn_1.id}")
 
turn_2 = client.interactions.create(
    model="gemini-3-flash-preview",
    input="What is my job?",
    previous_interaction_id=turn_1.id
)
 
print(turn_2.outputs[-1].text)
# Output: You are a software engineer.
```

For client-managed history, see [Stateless conversations](https://www.philschmid.de/gemini-api/docs/interactions#stateless-conversation).

### Forking Conversations

Because state is managed by ID, you can "fork" a conversation by referencing an older interaction ID with a different prompt.

Python

```python
# Branch off from Turn 1 with a different topic
turn_2_fork = client.interactions.create(
    model="gemini-3-flash-preview",
    input="What is my name?",
    previous_interaction_id=turn_1.id
)
 
print(turn_2_fork.outputs[-1].text)
# Output: Your name is Alice.
```

## Multimodal Interactions

Gemini models natively understand and generate multiple content types. You can pass text, images, audio, or PDF documents in a single interaction. This example uses a remote image URL.

### Multimodal understanding

Python

```python
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    model="gemini-3-flash-preview",
    input=[
        {"type": "text", "text": "Generate a recipe for the shown scones."},
        {
            "type": "image", 
            "uri": "https://storage.googleapis.com/generativeai-downloads/images/scones.jpg"
        }
    ],
)
 
print(interaction.outputs[-1].text)
```

For audio, video, and document (PDF) understanding, see [Multimodal understanding](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#understanding).

### Multimodal Generation

Python

```python
import base64
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    model="gemini-3-pro-image-preview",
    input="Generate an image of a futuristic city at sunset."
)
 
for output in interaction.outputs:
    if output.type == "image":
        with open("city.png", "wb") as f:
            f.write(base64.b64decode(output.data))
```

For audio generation see [Multimodal generations](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#generation).

## Tool use

Tools extend the model's capabilities by letting it call external functions or services. The API includes ready-to-use tools like Google Search, and lets you define custom tools as JSON schemas. The model decides when to call them based on the conversation:

### Built-in tools

Python

```python
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    model="gemini-3-flash-preview",
    input="Who won the 2024 Nobel Prize in Physics?",
    tools=[{"type": "google_search"}]
)
 
text_output = next((o for o in interaction.outputs if o.type == "text"), None)
if text_output:
    print(text_output.text)
```

Other built-in tools include [Code Execution](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#tools-and-function-calling) and [Computer Use](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#tools-and-function-calling).

### Function calling

Python

```python
from google import genai
 
client = genai.Client()
 
# Define a tool
weather_tool = {
    "type": "function",
    "name": "get_weather",
    "description": "Get current weather for a location",
    "parameters": {
        "type": "object",
        "properties": {
            "location": {"type": "string", "description": "City name"}
        },
        "required": ["location"]
    }
}
 
# Send request with tool
interaction = client.interactions.create(
    model="gemini-3-flash-preview",
    input="What's the weather in Tokyo?",
    tools=[weather_tool]
)
 
# Handle tool call
for output in interaction.outputs:
    if output.type == "function_call":
        # Execute your function (mocked here)
        # result = get_weather(output.arguments
        result = {"temperature": "22°C", "condition": "sunny"}
 
        # Return result to model
        interaction = client.interactions.create(
            model="gemini-3-flash-preview",
            previous_interaction_id=interaction.id,
            input={
                "type": "function_result",
                "name": output.name,
                "call_id": output.id,
                "result": result
            }
        )
        print(interaction.outputs[-1].text)
```

For code execution, URL context, and MCP servers, see [Agentic capabilities](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#tools-and-function-calling).

## Agents & Long-Running Tasks

Beyond models, the Interactions API provides access to specialized agents. Deep Research executes multi-step research tasks, synthesizing information from multiple sources into comprehensive reports.

Agents run asynchronously with `background=True`. Poll the interaction status to retrieve results:

Python

```python
import time
from google import genai
 
client = genai.Client()
 
agent_interaction = client.interactions.create(
    agent="deep-research-pro-preview-12-2025", # Note: use 'agent', not 'model'
    input="Research the history of the Google TPUs with a focus on 2025 specs.",
    background=True
)
 
 
# Poll for completion
while True:
    status_check = client.interactions.get(agent_interaction.id)
    print(f"Status: {status_check.status}")
    
    if status_check.status == "completed":
        print("\n--- Final Report ---\n")
        print(status_check.outputs[-1].text)
        break
    elif status_check.status in ["failed", "cancelled"]:
        print("Agent failed.")
        break
        
    time.sleep(10)
```

For more details, see [Deep Research](https://ai.google.dev/gemini-api/docs/deep-research).

## Next Steps

The Interactions API supports many more complex workflows. Check the [API Reference](https://ai.google.dev/api/interactions-api) for details on:

- **[Structured Outputs](https://ai.google.dev/gemini-api/docs/structured-output):** Force the model to return valid JSON matching a specific schema.
- **[Streaming](https://ai.google.dev/gemini-api/docs/interactions#streaming):** Stream token responses for real-time applications.
- **[Thinking Models](https://ai.google.dev/api/interactions-api#thinking):** Configure `thinking_level` for Gemini 2.5 and 3.0 models to handle complex reasoning.
- **[Remote MCP](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#remote-mcp-model-context-protocol):** Connect Gemini to your own private MCP servers.
- **[Combining tools and structured outputs](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#combining-tools-and-structured-output):** Combine tools and structured outputs to create more complex workflows.
- **[File Uploads](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#working-with-files):** Upload files to the model for processing.
- **[Data Model](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#data-model):** high level overview of the main inputs and outputs of the API.

## Feedback

**This API is in Beta, and we want your feedback!** We're actively listening to developers to shape the future of this API. What features would help your agent workflows? What pain points are you experiencing? Please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
