---
title: Combine Built-in Tools and Function Calling in the Gemini Interactions API
link: https://www.philschmid.de/tool-combo
source: philschmid-de
published: 2026-03-24T00:00:00Z
updated: 2026-03-24T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to combine built-in tools and function calling in the Gemini Interactions API.
content: extracted
html: 2026-03-24-combine-built-in-tools-and-function-calling-in-the-gemini.html
preview:
  file: 2026-03-24-combine-built-in-tools-and-function-calling-in-the-gemini.preview-ba2a08578b73.webp
  width: 256
  height: 134
  alt: Combine Built-in Tools and Function Calling in the Gemini Interactions API
  color: '#0b0a0c'
images:
- source: https://www.philschmid.de/static/blog/tool-combo/thumbnail.jpg
  original:
    file: 2026-03-24-combine-built-in-tools-and-function-calling-in-the-gemini.image-3659ac5b7d6a.jpg
    width: 1500
    height: 785
  color: '#000000'
---

Most useful agents need several tools at once. A DevOps agent that spots a CVE also needs to file a ticket for it. Until recently, you had orchestrate those handoffs yourself or only use custom tools without built-in tools.

[Tool combination in the Gemini API](https://ai.google.dev/gemini-api/docs/interactions?ua=chat#combining_built-in_tools_and_function_calling) now lets you combine [built-in tools](https://ai.google.dev/gemini-api/docs/tools) with custom function declarations in a single request. Gemini decides which tools to call, in what order, and circulates context between them automatically.

Below are two examples of tool combination.

## Example 1: Tool Combination with Function Calling

Combine Google Search, URL Context, and a custom function in one request. The model searches the web, reads a page, and calls your function without you specifying the order.

Get a [Gemini API key](https://aistudio.google.com/apikey) and install the SDK:

```plaintext
pip install google-genai
```

Define a custom function and pass it alongside built-in tools:

Python

```python
from google import genai
 
client = genai.Client()
 
file_incident = {
    "type": "function",
    "name": "file_incident",
    "description": "Files a security incident in the internal tracking system.",
    "parameters": {
        "type": "object",
        "properties": {
            "cve_id": {"type": "string", "description": "CVE identifier"},
            "severity": {"type": "string", "description": "Critical, High, Medium, or Low"},
            "summary": {"type": "string", "description": "Brief description of the vulnerability"},
        },
        "required": ["cve_id", "severity", "summary"],
    },
}
 
interaction = client.interactions.create(
    model="gemini-3-flash-preview",
    input="Search for the latest critical CVE affecting react, read the full advisory page, and file an incident for it.",
    tools=[
        {"type": "google_search"},
        {"type": "url_context"},
        {"type": "function", "name": "file_incident", "parameters": ...},
    ],
)
 
for output in interaction.outputs:
    print(f"{output.type}...")
    if output.type == "function_call":
        print(f"Function: {output.name}")
        print(f"Arguments: {output.arguments}")
 
# google_search_call...
# google_search_result...
# google_search_call...
# google_search_result...
# thought...
# function_call...
# Function: file_incident
# Arguments: {'summary': 'Unauthenticated remote code execution (RCE) vulnerability in React Server Components (RSC) affecting React 19.0, 19.1.x, and 19.2.0. The vulnerability (React2Shell) stems from insecure deserialization in the RSC "Flight" protocol, allowing an attacker to execute arbitrary code via a crafted HTTP request. Patches are available in React versions 19.0.1, 19.1.2, and 19.2.1.', 'severity': 'Critical', 'cve_id': 'CVE-2025-55182'}
```

## Example 2: Cross-Turn Context Circulation

Context circulation preserves built-in tool results across turns. Pass `previous_interaction_id` and follow-up questions can reason over earlier results without re-executing tools. The model can still make new tool calls if it needs fresh data or thinks it needs to.

Python

```python
from google import genai
 
client = genai.Client()
 
# Turn 1: URL Context reads Philipp's about page
turn1 = client.interactions.create(
    model="gemini-3-flash-preview",
    input="Who is Philipp Schmid? https://www.philschmid.de/philipp-schmid",
    tools=[{"type": "url_context"}],
)
print(turn1.outputs[-1].text)
 
# Turn 2: Follow-up uses context from turn 1
turn2 = client.interactions.create(
    model="gemini-3-flash-preview",
    input="What is his twitter handle?",
    tools=[{"type": "url_context"}],
    previous_interaction_id=turn1.id,
)
print(turn2.outputs[-1].text)
```

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
