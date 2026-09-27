---
title: Getting started with the Gemini Interactions API
link: https://www.philschmid.de/interactions-api-developer-guide
source: philschmid-de
published: 2026-06-23T00:00:00Z
updated: 2026-06-23T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: A developer guide to the Gemini Interactions API, text generation, streaming, multi-turn chat, multimodal inputs, structured output, tool use, managed agents, and background execution from a single endpoint.
content: extracted
html: 2026-06-23-getting-started-with-the-gemini-interactions-api.html
preview:
  file: 2026-06-23-getting-started-with-the-gemini-interactions-api.preview-4580d6b3c9a1.webp
  width: 256
  height: 134
  alt: Getting started with the Gemini Interactions API
  color: '#100e0b'
images:
- source: https://www.philschmid.de/static/blog/interactions-api-developer-guide/thumbnail.jpg
  original:
    file: 2026-06-23-getting-started-with-the-gemini-interactions-api.image-64326274570e.jpg
    width: 1500
    height: 785
  color: '#010100'
---

The [Interactions API](https://ai.google.dev/gemini-api/docs/interactions) is Google's primary interface for Gemini models and agents. A single endpoint covers text generation, streaming, multi-turn chat, multimodal inputs, image generation, structured output, tool use, function calling, managed agents, and background execution.

This guide uses JavaScript. For Python and REST examples, see the [Interactions API quickstart](https://ai.google.dev/gemini-api/docs/interactions/quickstart).

**Using a coding agent?** Install the skill so your agent stays current with Interactions API patterns:

Shell

```shell
npx skills add google-gemini/gemini-skills --skill gemini-interactions-api
```

## Setup

Create a free API key at [Google AI Studio](https://aistudio.google.com/apikey), then set it as an environment variable:

Shell

```shell
export GEMINI_API_KEY="YOUR_API_KEY"
```

Install the SDK:

Shell

```shell
npm install @google/genai
```

Send your first request.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const interaction = await ai.interactions.create({
  model: "gemini-3.5-flash",
  input: "Explain how AI works in a few words",
});
console.log(interaction.output_text);
```

`interaction.output_text` gives you the final text directly. See the [text generation guide](https://ai.google.dev/gemini-api/docs/interactions/text-generation) for system instructions and generation config.

## Streaming

Add `stream: true` and iterate over events. Each `step.delta` with `type === "text"` is a chunk you can display immediately.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const stream = await ai.interactions.create({
  model: "gemini-3.5-flash",
  input: "Explain how AI works",
  stream: true,
});
 
for await (const event of stream) {
  if (event.event_type === "step.delta") {
    if (event.delta.type === "text") {
      process.stdout.write(event.delta.text);
    }
  }
}
```

See the [streaming guide](https://ai.google.dev/gemini-api/docs/interactions/streaming) for event types and delta handling.

## Multi-turn conversations

Chain interactions by passing `previous_interaction_id`. The server manages history for you.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const interaction1 = await ai.interactions.create({
  model: "gemini-3.5-flash",
  input: "I have 2 dogs in my house.",
});
console.log("Response 1:", interaction1.output_text);
 
const interaction2 = await ai.interactions.create({
  model: "gemini-3.5-flash",
  input: "How many paws are in my house?",
  previous_interaction_id: interaction1.id,
});
console.log("Response 2:", interaction2.output_text);
```

For client-side history management, set `store: false`. See the [multi-turn guide](https://ai.google.dev/gemini-api/docs/interactions/text-generation#multi-turn-conversations).

## Multimodal understanding

Gemini natively understands images, audio, video, and documents. Upload a file and pass it alongside text.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const uploadedFile = await ai.files.upload({ file: "photo.jpg" });
 
const interaction = await ai.interactions.create({
  model: "gemini-3.5-flash",
  input: [
    { type: "text", text: "What is in this image?" },
    {
      type: "image",
      uri: uploadedFile.uri,
      mime_type: uploadedFile.mimeType,
    },
  ],
});
console.log(interaction.output_text);
```

Audio, video, and documents use the same structure. See the guides for [audio](https://ai.google.dev/gemini-api/docs/interactions/audio), [video](https://ai.google.dev/gemini-api/docs/interactions/video-understanding), and [document processing](https://ai.google.dev/gemini-api/docs/interactions/document-processing).

## Image generation

Generate images with Nano Banana 2 using the `gemini-3.1-flash-image` model.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
import fs from "node:fs";
 
const ai = new GoogleGenAI({});
 
const interaction = await ai.interactions.create({
  model: "gemini-3.1-flash-image",
  input: "Generate an image of a futuristic city skyline at sunset",
});
 
fs.writeFileSync(
  "generated_image.png",
  Buffer.from(interaction.output_image.data, "base64")
);
```

[Speech generation](https://ai.google.dev/gemini-api/docs/interactions/speech-generation) (multi-speaker TTS) and [music generation](https://ai.google.dev/gemini-api/docs/interactions/music-generation) (Lyria 3) work the same way. See the [image generation guide](https://ai.google.dev/gemini-api/docs/interactions/image-generation) for editing, aspect ratios, and style references.

## Structured output

Get JSON that matches a schema you define. Works with [Zod](https://zod.dev/).

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
import * as z from "zod";
 
const ai = new GoogleGenAI({});
 
const recipeJsonSchema = {
  type: "object",
  properties: {
    recipe_name: { type: "string", description: "Name of the recipe." },
    ingredients: {
      type: "array", items: { type: "string" }, description: "List of ingredients."
    },
    prep_time_minutes: { type: "integer", description: "Prep time in minutes." }
  },
  required: ["recipe_name", "ingredients"]
};
 
const recipeSchema = z.fromJSONSchema(recipeJsonSchema);
 
const interaction = await ai.interactions.create({
  model: "gemini-3.5-flash",
  input: "Give me a recipe for banana bread",
  response_format: {
    type: "text",
    mime_type: "application/json",
    schema: recipeJsonSchema
  },
});
 
const recipe = recipeSchema.parse(JSON.parse(interaction.output_text));
console.log(recipe);
```

See the [structured output guide](https://ai.google.dev/gemini-api/docs/interactions/structured-output) for recursive schemas and enums.

## Tools (Google Search)

Ground responses in real-time data by passing `tools: [{ type: "google_search" }]`.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const interaction = await ai.interactions.create({
  model: "gemini-3.5-flash",
  input: "Who won the euro 2024?",
  tools: [{ type: "google_search" }],
});
console.log(interaction.output_text);
```

Other built-in tools: [Code Execution](https://ai.google.dev/gemini-api/docs/interactions/code-execution), [URL Context](https://ai.google.dev/gemini-api/docs/interactions/url-context), [File Search](https://ai.google.dev/gemini-api/docs/interactions/file-search), [Google Maps](https://ai.google.dev/gemini-api/docs/interactions/maps-grounding), [Computer Use](https://ai.google.dev/gemini-api/docs/interactions/computer-use). Mix multiple tools in one request. See the [tool combination guide](https://ai.google.dev/gemini-api/docs/interactions/tool-combination).

## Function calling

Declare functions, let the model decide when to call them, execute locally, and return results.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const weatherTool = {
  type: "function",
  name: "get_current_temperature",
  description: "Gets the current temperature for a given location.",
  parameters: {
    type: "object",
    properties: {
      location: {
        type: "string",
        description: "The city name, e.g. San Francisco",
      },
    },
    required: ["location"],
  },
};
 
const availableFunctions = {
  get_current_temperature: ({ location }) => ({
    location, temperature: "22", unit: "celsius"
  }),
};
 
let input = "What is the temperature in London?";
let previousId = null;
let interaction;
 
while (true) {
  interaction = await ai.interactions.create({
    model: "gemini-3.5-flash",
    input,
    tools: [weatherTool],
    previous_interaction_id: previousId,
  });
 
  const functionResults = [];
  for (const step of interaction.steps) {
    if (step.type === "function_call") {
      const result = availableFunctions[step.name](step.arguments);
      console.log(`Called ${step.name}(${JSON.stringify(step.arguments)}) →`, result);
      functionResults.push({
        type: "function_result",
        name: step.name,
        call_id: step.id,
        result: [{ type: "text", text: JSON.stringify(result) }],
      });
    }
  }
 
  if (functionResults.length === 0) break;
 
  input = functionResults;
  previousId = interaction.id;
}
 
console.log(interaction.output_text);
```

The model returns `status: "requires_action"` with `function_call` steps. You execute locally and submit `function_result` steps back. See the [function calling guide](https://ai.google.dev/gemini-api/docs/interactions/function-calling) for parallel calls and function choice modes.

## Managed agents

Run an agent in a remote sandbox with code execution, web browsing, and file management. Pass `agent` instead of `model` and set `environment: "remote"`.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const interaction = await ai.interactions.create({
  agent: "antigravity-preview-05-2026",
  input: "Write a script that generates the first 20 Fibonacci numbers and saves them to fibonacci.txt.",
  environment: "remote",
});
console.log(interaction.output_text);
```

Define [custom agents](https://ai.google.dev/gemini-api/docs/custom-agents) with your own instructions, skills, and data sources. See the [Managed Agents quickstart](https://ai.google.dev/gemini-api/docs/managed-agents-quickstart).

## Background execution

Set `background: true` for long-running tasks. The call returns immediately and you poll for results.

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const interaction = await ai.interactions.create({
  model: "gemini-3.5-flash",
  input: "Write a detailed analysis of AI in healthcare.",
  background: true,
});
console.log(`Task started: ${interaction.id} (status: ${interaction.status})`);
 
const poll = setInterval(async () => {
  const result = await ai.interactions.get(interaction.id);
  if (result.status === "completed") {
    console.log(result.output_text);
    clearInterval(poll);
  } else if (result.status === "failed") {
    console.error("Failed:", result.error);
    clearInterval(poll);
  }
}, 5000);
```

See the [background execution guide](https://ai.google.dev/gemini-api/docs/interactions/interactions-overview#background-execution).

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
