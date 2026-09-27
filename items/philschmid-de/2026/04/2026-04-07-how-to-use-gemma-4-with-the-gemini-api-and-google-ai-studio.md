---
title: How to use Gemma 4 with the Gemini API and Google AI Studio
link: https://www.philschmid.de/gemma-4-gemini-api
source: philschmid-de
published: 2026-04-07T00:00:00Z
updated: 2026-04-07T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to use Gemma 4 with the Gemini API and Google AI Studio.
content: extracted
html: 2026-04-07-how-to-use-gemma-4-with-the-gemini-api-and-google-ai-studio.html
preview:
  file: 2026-04-07-how-to-use-gemma-4-with-the-gemini-api-and-google-ai-studio.preview-183e40056de5.webp
  width: 256
  height: 134
  alt: How to use Gemma 4 with the Gemini API and Google AI Studio
  color: '#080809'
images:
- source: https://www.philschmid.de/static/blog/gemma-4-gemini-api/thumbnail.jpg
  original:
    file: 2026-04-07-how-to-use-gemma-4-with-the-gemini-api-and-google-ai-studio.image-ed9e822427b9.jpg
    width: 1500
    height: 785
  color: '#000000'
---

Google's Gemma 4 family of open models is now available through the [Gemini API](https://ai.google.dev/gemma/docs/core/gemma_on_gemini_api) and [Google AI Studio](https://aistudio.google.com/prompts/new_chat?model=gemma-4-31b-it). Built from the same research behind Gemini 3, these models bring advanced reasoning, native function calling, multimodal understanding, and 256K context windows to an open, Apache 2.0-licensed package you can run anywhere.

Two models are available through the Gemini API today:

- `gemma-4-26b-a4b-it`
- `gemma-4-31b-it`

## What makes Gemma 4 different

Gemma 4 models handle function calling, structured JSON output, and system instructions at the model level rather than through prompt engineering. The 31B dense model currently ranks as the #3 open model on the Arena AI text leaderboard, with the 26B MoE model at #6, competing with models 20x their size.

Key capabilities:

- 256K context window on both models
- Native function calling and structured output
- Multimodal text, images, and video
- 140+ languages trained natively
- Apache 2.0 license full commercial use, no restrictions

## Getting started with AI Studio

The fastest way to try Gemma 4 is [Google AI Studio](https://aistudio.google.com/prompts/new_chat?model=gemma-4-31b-it). Select `gemma-4-26b-a4b-it` or `gemma-4-31b-it` from the model picker, type a prompt, and start chatting. You can test system instructions, adjust temperature, and experiment with multimodal inputs all through the browser. No API key or code required.

Or click **Get Code** to export Python, JavaScript, or cURL snippets from any conversation.

## Using Gemma 4 with the Gemini API

Install the Python SDK:

Bash

```bash
pip install google-genai
```

Set your API key as an environment variable. You can create one at [aistudio.google.com/apikey](https://aistudio.google.com/apikey).

Bash

```bash
export GEMINI_API_KEY="your-api-key"
```

### Text generation

Generate text with Gemma 4:

Python

```python
from google import genai
 
client = genai.Client()
 
response = client.models.generate_content(
    model="gemma-4-26b-a4b-it",
    contents="Compare ramen and udon in 3 bullet points: broth, noodle texture, and best season to eat."
)
print(response.text)
```

Pass a system instruction to set the model's behavior:

Python

```python
from google import genai
from google.genai import types
 
client = genai.Client()
 
response = client.models.generate_content(
    model="gemma-4-31b-it",
    config=types.GenerateContentConfig(
        system_instruction="You are a wise Kyoto tea master. Speak calmly and poetically, using nature metaphors. Keep answers under 3 sentences."
    ),
    contents="What is the purpose of the tea ceremony?"
)
print(response.text)
```

### Multi-turn conversations

The SDK provides a chat interface that tracks conversation history automatically:

Python

```python
from google import genai
 
client = genai.Client()
chat = client.chats.create(model="gemma-4-26b-a4b-it")
 
response = chat.send_message("What are the three most famous castles in Japan?")
print(response.text)
 
response = chat.send_message("Which one should I visit in spring for cherry blossoms?")
print(response.text)
```

### Image understanding

Pass an image alongside your text prompt:

Python

```python
from google import genai
from google.genai import types
 
client = genai.Client()
 
with open("path/to/image.png", "rb") as f:
    image_bytes = f.read()
 
response = client.models.generate_content(
    model="gemma-4-26b-a4b-it",
    contents=[
        types.Part.from_bytes(data=image_bytes, mime_type="image/png"),
        "Describe this image in 2-3 sentences as if writing a caption for a Japanese travel magazine."
    ]
)
print(response.text)
```

### Function calling

Define tools as function declarations. The model decides when to call them:

Python

```python
from google import genai
from google.genai import types
 
# Define the function declaration
get_weather = {
    "name": "get_weather",
    "description": "Get current weather for a given location.",
    "parameters": {
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "City and state, e.g. 'San Francisco, CA'",
            },
        },
        "required": ["location"],
    },
}
 
client = genai.Client()
tools = types.Tool(function_declarations=[get_weather])
config = types.GenerateContentConfig(tools=[tools])
 
response = client.models.generate_content(
    model="gemma-4-26b-a4b-it",
    contents="Should I bring an umbrella to Kyoto today?",
    config=config,
)
 
# The model returns a function call instead of text
if response.candidates[0].content.parts[0].function_call:
    fc = response.candidates[0].content.parts[0].function_call
    print(f"Function: {fc.name}")
    print(f"Arguments: {fc.args}")
```

### Google Search

Ground Gemma 4 responses in real-time web data with Google Search:

Python

```python
from google import genai
from google.genai import types
 
client = genai.Client()
 
response = client.models.generate_content(
    model="gemma-4-26b-a4b-it",
    contents="What are the dates for cherry blossom season in Tokyo this year?",
    config=types.GenerateContentConfig(
        tools=[{"google_search":{}}]
    ),
)
 
print(response.text)
 
# Access grounding metadata for citations
for chunk in response.candidates[0].grounding_metadata.grounding_chunks:
    print(f"Source: {chunk.web.title} — {chunk.web.uri}")
```

## Where to go next

- [Google AI Studio](https://aistudio.google.com) — Try Gemma 4 in the browser
- [Gemini API documentation](https://ai.google.dev/gemini-api/docs)
- [Gemma Documentation](https://ai.google.dev/gemma/docs)
- [Gemma Cookbooks](https://github.com/google-gemma/cookbook)

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
