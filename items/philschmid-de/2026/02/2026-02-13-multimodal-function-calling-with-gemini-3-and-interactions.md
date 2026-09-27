---
title: Multimodal Function Calling with Gemini 3 and Interactions API
link: https://www.philschmid.de/interactions-multimodal-fc
source: philschmid-de
published: 2026-02-13T00:00:00Z
updated: 2026-02-13T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Multimodal function calling allows tools to return images the model can process natively, similar to how you pass images in prompts. Instead of describing what's in a file, your tool returns the actual image and Gemini 3 processes it natively.
content: extracted
html: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.html
preview:
  file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.preview-eb321a318b24.webp
  width: 256
  height: 134
  alt: Multimodal Function Calling with Gemini 3 and Interactions API
  color: '#0e0e0e'
images:
- source: https://www.philschmid.de/static/blog/interactions-multimodal-fc/thumbnail.jpg
  original:
    file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.image-4f8485bacd75.jpg
    width: 1500
    height: 785
  color: '#010101'
- source: https://www.philschmid.de/static/blog/interactions-multimodal-fc/chart.png
  original:
    file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.image-30c65224d8fb.png
    width: 5715
    height: 1965
  variants:
  - file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.image-1d16f04096ad.webp
    width: 320
    height: 110
  - file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.image-c4eccb1c8430.webp
    width: 640
    height: 220
  - file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.image-b97aab24f918.webp
    width: 960
    height: 330
  - file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.image-1a8d38a3fbba.webp
    width: 1280
    height: 440
  - file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.image-622baee1092a.webp
    width: 1600
    height: 550
  - file: 2026-02-13-multimodal-function-calling-with-gemini-3-and-interactions.image-e7d424e66ccf.webp
    width: 5715
    height: 1965
  color: '#fdfdfd'
---

Building AI agents that can truly "see" requires more than just passing images in prompts. Coding agents need to read screenshots of UIs they're building. Computer use agents need to capture and analyze browser screenshots. Research agents need to process charts and diagrams from documents.

Multimodal function calling allows tools to return images the model can process natively, similar to how you pass images in prompts. Instead of describing what's in a file, your tool returns the actual image and Gemini 3 processes it natively.

This guide shows you how Multimodal Function Calling works using the [Interactions API](https://ai.google.dev/gemini-api/docs/interactions). The Interactions API is a unified interface for interacting with Gemini models and agents. It simplifies state management and tool orchestration, making it easy to build multi-turn agentic workflows.

![chart](https://www.philschmid.de/static/blog/interactions-multimodal-fc/chart.png)

The key difference from standard function calling is in the `function_result` returns the actual image data. Gemini 3 processes this content natively—describing images, analyzing documents, or using visual information to make decisions.

JSON

```json
{
  "type": "function_result",
  "call_id": "abc123",
  "name": "read_image",
  "result": [
    {"type": "text", "text": "Additional context..."},
    {"type": "image", "data": "<base64>", "mime_type": "image/png"}
  ]
}
```

## Example: Reading images from Fileystem

Let's build a tool that reads images from disk and returns them to the model for description. Get a [Gemini API key](https://aistudio.google.com/apikey) and install the Google GenAI SDK:

Bash

```bash
pip install google-genai
```

Set your API key:

Bash

```bash
export GEMINI_API_KEY="your-api-key"
```

### 1\. Define the tool

Python

```python
read_image = {
    "type": "function",
    "name": "read_image",
    "description": "Reads an image file from disk and returns its contents.",
    "parameters": {
        "type": "object",
        "properties": {
            "path": {
                "type": "string", 
                "description": "The path to the image file to read"
            }
        },
        "required": ["path"]
    }
}
```

### 2\. Implement the tool execution

Python

```python
import base64
from pathlib import Path
 
def execute_read_image(path: str) -> list[dict]:
    """Read image from disk and return as function result content."""
    image_path = Path(path)
    
    if not image_path.exists():
        return [{"type": "text", "text": f"Error: Image not found at {path}"}]
    
    mime_types = {
        ".jpg": "image/jpeg",
        ".jpeg": "image/jpeg",
        ".png": "image/png",
        ".webp": "image/webp",
    }
    mime_type = mime_types.get(image_path.suffix.lower(), "image/png")
    
    image_data = base64.b64encode(image_path.read_bytes()).decode()
    return [{"type": "image", "data": image_data, "mime_type": mime_type}]
```

### 3\. Create the agentic loop

Python

```python
from google import genai
 
client = genai.Client()
 
image_path = "path/to/your/image.png"
prompt=f"Use the read_image tool to read '{image_path}' and describe what you see."
 
# Step 1: Send the initial request with our tool
interaction = client.interactions.create(
    model="gemini-3-flash-preview",
    input=prompt,
    tools=[read_image],
)
 
# Step 2: Handle the function call
for output in interaction.outputs:
    if output.type == "function_call" and output.name == "read_image":
      
        image_result = execute_read_image(output.arguments.get("path"))
        
        # Step 3: Send the image back to the model
        interaction = client.interactions.create(
            model="gemini-3-flash-preview",
            input=[{
                "type": "function_result",
                "call_id": output.id,
                "name": output.name,
                "result": image_result,  # Contains the image!
            }],
            tools=[read_image],
            previous_interaction_id=interaction.id,
        )
        
        # Step 4: Get the model's description
        for out in interaction.outputs:
            if out.type == "text":
                print(out.text)
```

## Concluding how it works

The interaction flow has four steps:

1. **User request** → You send a prompt asking the model to use the tool
2. **Function call** → Model responds with a `function_call` specifying which tool to use and arguments
3. **Tool execution** → You execute the tool and return the result (including the image)
4. **Model response** → Model processes the image and generates a text description

Python

```python
# The function_call output from the model looks like:
{
    "type": "function_call",
    "id": "call_abc123",
    "name": "read_image",
    "arguments": {"path": "photo.jpg"}
}
 
# Your function_result response looks like:
{
    "type": "function_result",
    "call_id": "call_abc123",
    "name": "read_image",
    "result": [
        {"type": "image", "data": "iVBORw0KGgo...", "mime_type": "image/png"}
    ]
}
```

## Conclusion

Multimodal function calling unlocks multimodal agentic use cases. By returning images directly in your `function_result`, you give Gemini 3 the ability to see what your tools see.

This pattern extends beyond file reading. You can build tools that capture screenshots, fetch images from APIs, render charts, or process scanned documents. Combined with the Interactions API's stateful sessions, you have everything you need to build sophisticated visual agents.

**Next steps:**

- Try the [Interactions API quickstart](https://ai.google.dev/gemini-api/docs/interactions) notebook
- Explore [Computer Use](https://ai.google.dev/gemini-api/docs/computer-use) for browser automation with visual feedback
- Build tools that return PDFs or multiple images for complex document workflows

* * *

**This API is in Beta, and we want your feedback!** We're actively listening to developers to shape the future of this API. What features would help your agent workflows? What pain points are you experiencing? Please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
