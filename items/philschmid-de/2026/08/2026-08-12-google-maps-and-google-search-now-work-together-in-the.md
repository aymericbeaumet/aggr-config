---
title: Google Maps and Google Search now work together in the Gemini API
link: https://www.philschmid.de/gemini-search-maps
source: philschmid-de
published: 2026-08-12T00:00:00Z
updated: 2026-08-12T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Use Google Maps and Google Search in the same Gemini API call with Gemini 3.6 Flash, then add custom functions or MCP servers for actions like booking a table.
content: extracted
html: 2026-08-12-google-maps-and-google-search-now-work-together-in-the.html
preview:
  file: 2026-08-12-google-maps-and-google-search-now-work-together-in-the.preview-a1b558be3071.webp
  width: 256
  height: 134
  alt: Google Maps and Google Search now work together in the Gemini API
  color: '#5f5f61'
images:
- source: https://www.philschmid.de/static/blog/gemini-search-maps/thumbnail.jpg
  original:
    file: 2026-08-12-google-maps-and-google-search-now-work-together-in-the.image-f8b9d55b4de5.jpg
    width: 1500
    height: 785
  color: '#0b0c10'
- source: https://www.philschmid.de/static/blog/gemini-search-maps/agent_loop_tools_diagram.jpg
  original:
    file: 2026-08-12-google-maps-and-google-search-now-work-together-in-the.image-314bb817dcdf.jpg
    width: 1376
    height: 768
  color: '#f4f3ed'
- source: https://www.philschmid.de/static/blog/gemini-search-maps/screen_1_empty_input.jpg
  original:
    file: 2026-08-12-google-maps-and-google-search-now-work-together-in-the.image-d0754f4088b9.jpg
    width: 1440
    height: 900
  color: '#ebebeb'
- source: https://www.philschmid.de/static/blog/gemini-search-maps/screen_2_results_and_map.jpg
  original:
    file: 2026-08-12-google-maps-and-google-search-now-work-together-in-the.image-491290891a0f.jpg
    width: 1440
    height: 900
  color: '#ececec'
- source: https://www.philschmid.de/static/blog/gemini-search-maps/screen_4_confirmed.jpg
  original:
    file: 2026-08-12-google-maps-and-google-search-now-work-together-in-the.image-b9f8991875a8.jpg
    width: 1440
    height: 900
  color: '#ececec'
---

We just shipped something I've wanted for a while. You can now use the [Google Maps](https://ai.google.dev/gemini-api/docs/interactions/maps-grounding) and [Google Search](https://ai.google.dev/gemini-api/docs/interactions/google-search) tools in the exact same call with Gemini 3.5 Flash and 3.6 Flash. You can also add custom functions or MCP servers into the same request via [Tool Combination](https://ai.google.dev/gemini-api/docs/interactions/tool-combination).

## How the tools fit together

Building location apps usually means juggling web context and physical data yourself. You'd write separate LLM calls, parse messy JSON, and glue external APIs together.

Now Gemini handles the whole loop in one interaction:

- **[Google Search](https://ai.google.dev/gemini-api/docs/interactions/google-search)**: finds live web info (tonight's concerts, recent food blogs, pop-up events).
- **[Google Maps](https://ai.google.dev/gemini-api/docs/interactions/maps-grounding)**: pulls physical details (exact coordinates, current opening hours, ratings, place IDs).
- **Custom tools & MCP**: runs your app logic (reserving a table, adding to calendar, saving to a database).

![Agent Loop with Built-in and Client Tools](https://www.philschmid.de/static/blog/gemini-search-maps/agent_loop_tools_diagram.jpg)

When Gemini finds a venue on the web, it queries Maps for the exact place details, then hands structured parameters to your [function call](https://ai.google.dev/gemini-api/docs/interactions/function-calling). Zero roundtrips on your side.

## The JavaScript snippet

Here is how you set it up with the [Interactions API](https://ai.google.dev/gemini-api/docs/interactions) using `@google/genai`:

JavaScript

```javascript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
const bookTableTool = {
  type: "function",
  name: "book_table",
  description: "Reserves a table at a chosen venue.",
  parameters: {
    type: "object",
    properties: {
      placeName: { type: "string" },
      time: { type: "string" },
      partySize: { type: "number" },
      seatingPreference: { type: "string" }
    },
    required: ["placeName", "time"]
  }
};
 
const interaction = await ai.interactions.create({
  model: "gemini-3.6-flash",
  input: "Find a quiet coffee spot in SoHo with a garden patio open this morning, and book a table for 2 at 10am.",
  tools: [
    { type: "google_search" },
    { type: "google_maps" },
    bookTableTool
  ]
});
```

## Running it in an app

I put together a quick app to test the flow end-to-end.

You start with a prompt:

![Prompt Input State](https://www.philschmid.de/static/blog/gemini-search-maps/screen_1_empty_input.jpg)

In one turn, Gemini searches the web for patio spots in SoHo, verifies the venues on Google Maps, drops pins on the map, and prepares the reservation action:

![Results with Map Pins and Proposed Action](https://www.philschmid.de/static/blog/gemini-search-maps/screen_2_results_and_map.jpg)

When the user clicks confirm, your app calls your real reservation service, gets a reference ID, and hands that result back to Gemini in turn 2 using `previous_interaction_id`:

JavaScript

```javascript
// 1. Execute the actual booking in your backend
const bookingResult = await reservationService.book({
  venue: functionCall.arguments.placeName,
  time: functionCall.arguments.time,
  party: functionCall.arguments.partySize
});
 
// 2. Send the result back to Gemini to finish the interaction
const finalInteraction = await ai.interactions.create({
  model: "gemini-3.6-flash",
  previous_interaction_id: interaction.id,
  input: [{
    type: "function_result",
    call_id: functionCall.id,
    name: "book_table",
    result: [{
      type: "text",
      text: JSON.stringify({
        status: "confirmed",
        reference: bookingResult.id
      })
    }]
  }]
});
```

The server keeps the whole grounding context and conversation state alive without you having to re-send token histories:

![Reservation Confirmed](https://www.philschmid.de/static/blog/gemini-search-maps/screen_4_confirmed.jpg)

## Using OpenTable via MCP

Instead of writing custom booking functions by hand, you can plug in an existing OpenTable MCP server directly:

JavaScript

```javascript
const interaction = await ai.interactions.create({
  model: "gemini-3.6-flash",
  input: "Find a romantic pasta bar in the West Village with an open table at 7:30pm and book it for 2.",
  tools: [
    { type: "google_search" },
    { type: "google_maps" },
    {
      type: "mcp_server",
      name: "opentable",
      url: "https://mcp.opentable.com/sse"
    }
  ]
});
```

Gemini finds the restaurant through Search, checks reviews and coordinates on Maps, and handles the live reservation through OpenTable's MCP server in the background.

## Why this is fun

Having Search, Maps, and MCP together cuts out a lot of boilerplate. You don't have to extract keywords, call Places APIs, and re-prompt to figure out the next step.

Check out the docs for [Tool Combination](https://ai.google.dev/gemini-api/docs/interactions/tool-combination), [Maps Grounding](https://ai.google.dev/gemini-api/docs/interactions/maps-grounding), and the [Interactions API Quickstart](https://ai.google.dev/gemini-api/docs/interactions/quickstart) to try it out.
