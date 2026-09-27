---
title: 'Developer Guide: Nano Banana 2 with the Gemini Interactions API'
link: https://www.philschmid.de/nano-banana-2-interactions-api
source: philschmid-de
published: 2026-03-16T00:00:00Z
updated: 2026-03-16T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to use the Gemini Interactions API to build a personalized Japan travel brochure with Nano Banana 2.
content: extracted
html: 2026-03-16-developer-guide-nano-banana-2-with-the-gemini-interactions.html
preview:
  file: 2026-03-16-developer-guide-nano-banana-2-with-the-gemini-interactions.preview-73b42378bfae.webp
  width: 256
  height: 134
  alt: 'Developer Guide: Nano Banana 2 with the Gemini Interactions API'
  color: '#120d0d'
images:
- source: https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/thumbnail.jpg
  original:
    file: 2026-03-16-developer-guide-nano-banana-2-with-the-gemini-interactions.image-bb735d13f91a.jpg
    width: 1500
    height: 785
  color: '#000000'
- source: https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/01_kyoto_poster.jpg
  original:
    file: 2026-03-16-developer-guide-nano-banana-2-with-the-gemini-interactions.image-e94005482fc1.jpg
    width: 384
    height: 688
  color: '#16140c'
- source: https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/02_kyoto_map.jpg
  original:
    file: 2026-03-16-developer-guide-nano-banana-2-with-the-gemini-interactions.image-24a0b7b35997.jpg
    width: 384
    height: 688
  color: '#c8b697'
- source: https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/03_fushimi_inari.jpg
  original:
    file: 2026-03-16-developer-guide-nano-banana-2-with-the-gemini-interactions.image-9eab2dba49e4.jpg
    width: 384
    height: 688
  color: '#27231a'
- source: https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/04_arashiyama_portrait.jpg
  original:
    file: 2026-03-16-developer-guide-nano-banana-2-with-the-gemini-interactions.image-25b09c78d592.jpg
    width: 688
    height: 384
  color: '#373419'
---

[Nano Banana 2](https://blog.google/innovation-and-ai/technology/ai/nano-banana-2/) (`gemini-3.1-flash-image-preview`) is our latest image generation model. It runs at Gemini Flash speed with the capabilities of Nano Banana Pro's world knowledge, precise text rendering, and subject consistency across up to 14 reference images. It comes with a new **Grounding with Google Image Search** feature that lets the model retrieve real images from the web and use them as visual context during generation.

This Developer Guide uses the [Interactions API](https://ai.google.dev/gemini-api/docs/interactions), our next-generation unified interface for Gemini, to build a **personalized Japan travel brochure**. In 4 code snippets, you'll go from a simple text prompt to a search-grounded, personalized image—compositing a real person into a photorealistic scene of Kyoto, informed by live Google Search results.

1. **Text → Image:** Pure creative generation
2. **\+ Google Search (web):** Real-world factual grounding
3. **\+ Google Search (web + images):** Visual accuracy from retrieved images
4. **\+ Reference photo:** Subject-consistent personalization

Bash

```bash
pip install google-genai
export GEMINI_API_KEY="your-api-key"
```

You can create and manage all your Gemini API Keys from [Google AI Studio](https://aistudio.google.com/api-keys).

## 1\. Text to Image

Describe what you want, get an image back. Nano Banana 2 handles photorealism, text rendering, and complex compositions without any tools.

Python

```python
import base64
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    model="gemini-3.1-flash-image-preview",
    input=f"""A cinematic travel poster for Kyoto, Japan in autumn. Cherry-red maple leaves 
frame a traditional wooden torii gate at golden hour. The text 'KYOTO' is rendered in 
elegant serif typography at the bottom. Photorealistic, warm tones, 35mm film look.""",
)
 
images = [output for output in interaction.outputs if output.type == "image"]
with open("01_kyoto_poster.png", "wb") as f:
    f.write(base64.b64decode(images[0].data))
```

![01_kyoto_poster.png](https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/01_kyoto_poster.jpg)

## 2\. Text to Image with Google Search (Web Search)

Adding a `google_search` tool makes the model search the web for factual information before generating.

Python

```python
import base64
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    model="gemini-3.1-flash-image-preview",
    input=f"""Search for the top 5 landmarks to visit in Kyoto, Japan. Then create a beautiful 
illustrated travel map poster showing these landmarks with their real names labeled. Use a 
vintage hand-drawn cartography style with warm earth tones. Title it 'KYOTO WALKING GUIDE'.""",
    tools=[
        {
            "type": "google_search",
            "search_types": ["web_search"]
        }
    ]
)
 
images = [output for output in interaction.outputs if output.type == "image"]
with open("02_kyoto_map.png", "wb") as f:
    f.write(base64.b64decode(images[0].data))
```

![02_kyoto_map.jpg](https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/02_kyoto_map.jpg)

The model now searches for actual Kyoto landmarks and uses their real names and descriptions. It still imagines what they look like, though. For visual accuracy, the model needs to see reference images.

## 3\. Text to Image with Google Search (Web + Image Search)

Nano Banana 2 adds Image Search as a new search type alongside Web Search, controlled via the `search_types` parameter. The model retrieves actual photographs from Google Image Search and uses them as visual references during generation.

With text-only search, the model knows Fushimi Inari has orange torii gates. With Image Search, it sees exactly what they look like.

Python

```python
import base64
from google import genai
 
client = genai.Client()
 
interaction = client.interactions.create(
    model="gemini-3.1-flash-image-preview",
    input=f"""Search for what Fushimi Inari Shrine in Kyoto looks like. Then generate a stunning 
photorealistic travel poster of Fushimi Inari's famous orange torii gate tunnel at sunset with 
a light rain falling. Include the text 'FUSHIMI INARI' in a clean, modern sans-serif font at 
the bottom.""",
    tools=[
        {
            "type": "google_search",
            "search_types": ["web_search", "image_search"]
        }
    ]
)
 
images = [output for output in interaction.outputs if output.type == "image"]
with open("03_fushimi_inari.png", "wb") as f:
    f.write(base64.b64decode(images[0].data))
```

![03_fushimi_inari.jpg](https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/03_fushimi_inari.jpg)

You can also use `image_search` on its own without `web_search`:

Python

```python
tools=[{"type": "google_search", "search_types": ["image_search"]}]
```

The brochure now has factually accurate, visually faithful renderings of real landmarks. Next: adding a real person to the scene.

## 4\. Text + Image to Image with Google Search (Web + Image Search)

We can combine a **reference photo** of a person with text instructions and search grounding. Nano Banana 2 preserves the person's appearance from the reference image in the generated output.

We provide a headshot and ask the model to place the person into the Arashiyama bamboo grove, with Image Search ensuring the scene matches the real location.

Python

```python
import base64
from google import genai
 
client = genai.Client()
 
image_data = base64.b64encode(open("assets/headshot.png", "rb").read()).decode()
 
interaction = client.interactions.create(
    model="gemini-3.1-flash-image-preview",
    input=[
        {
            "type": "text",
            "text": f"""Search for what the Arashiyama bamboo grove in Kyoto looks like. Then 
generate a photo of this person (from the provided image) standing in the Arashiyama bamboo 
grove during golden hour. They are wearing a casual travel outfit and looking up at the 
towering bamboo stalks with a sense of wonder. Photorealistic, cinematic lighting, shallow 
depth of field."""
        },
        {
            "type": "image",
            "data": image_data,
            "mime_type": "image/png"
        }
    ],
    tools=[
        {
            "type": "google_search",
            "search_types": ["web_search", "image_search"]
        }
    ]
)
 
images = [output for output in interaction.outputs if output.type == "image"]
with open("04_arashiyama_portrait.png", "wb") as f:
    f.write(base64.b64decode(images[0].data))
```

![04_arashiyama_portrait.jpg](https://www.philschmid.de/static/blog/nano-banana-2-interactions-api/04_arashiyama_portrait.jpg)

This single API call combines three Nano Banana 2 capabilities:

- **Subject consistency:** Preserves the person's appearance from the reference photo
- **Image Search grounding:** Retrieves real images of the bamboo grove for visual accuracy
- **Web Search grounding:** Pulls factual context about the location

## Tips & Best Practices

- The same prompt produces different images on each run.
- Some prompts may be blocked by safety filters. If your prompt is rejected, try adjusting the description.
- The `search_types` parameter with `image_search` requires the `gemini-3.1-flash-image-preview` model specifically.
- Subject consistency works best with clear, well-lit headshots. The model can maintain resemblance for up to 5 characters and 14 reference objects.

## Other Resources

- [Nano Banana 2 Blog Post](https://blog.google/innovation-and-ai/technology/ai/nano-banana-2/)
- [Image Generation Documentation](https://ai.google.dev/gemini-api/docs/image-generation)
- [Interactions API Documentation](https://ai.google.dev/gemini-api/docs/interactions)
- [Try in AI Studio](https://aistudio.google.com/prompts/new_chat?model=gemini-3.1-flash-image-preview)
- [Pricing](https://ai.google.dev/gemini-api/docs/pricing#gemini-3.1-flash-image-preview)

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
