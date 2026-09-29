---
title: 'Show HN: HN.watch – Videos of all Hacker News posts'
link: https://hn.watch/
source: hnrss-org
published: 2026-09-28T15:16:13Z
updated: 2026-09-28T15:16:13Z
first_seen: 2026-09-29T01:16:22.439760281Z
authors:
- mrborgen
summary: 'Hi HN, I’m Per, founder of Scrimba (YC S20). We’ve spent the last decade teaching people how to code with an HTML-based video format. We’ve now plugged an LLM into it, so that people can create explainer videos about anything. It’s called “Scrimba Explain”. To demo this technology for Hacker News, we built HN.watch. It’s like HN, but with explainer videos instead of articles. We create them on-the-fly the first time someone clicks on a link. While there are obvious visual drawbacks of using HTML instead of diffusion models, there are three big benefits: - Speed: Much faster to generate than pixel-based videos (just a few seconds from click to playback) - Cost: Our cost per video is ~$0.04. (Excluding image generation, which some videos utilize. Quickly blows up the cost) - Easy editing: the above benefits also make AI-assisted editing cheap & fast Our hypothesis is that if video creation goes from “dollars and minutes” to “cents and seconds”, a bunch of new use cases will be unlocked. Here are some we see already: - A video explanation of every single Pull Request (we do this internally) - Give every page in your internal/extrernal docs a video - Turn a complex article into a video in ~4 seconds (via our Chrome extension) - Course creators can quickly draft lessons before recording the real thing - People also create a lot of personal stuff stories for their kids, wedding invitations, birthdays, etc The stack is based on an open-source programming language (Imba) created by our CTO, Sindre Aarsæther. It compiles to JavaScript, so it interoperates fully with the npm + node ecosystem. You can learn more here: https://imba.io/ We’ve also built our own sync engine (OP), and a context management system for agents (Q). We feared this would make the LLMs struggle when writing code for us, as neither is in their training data (there’s very little Imba in there too). However, we’ve been pleasantly surprised to see that LLMs actually are really good at our stack. This is probably because the stack is extremely dense. Imba is compact, and so is OP, where a single declaration sets storage, sync, permissions, UI, and what the AI sees. This means there’s no translations between frontend, API, db and JSON where the model can get confused and get things wrong. Simply said, instead of using React.js, Express, Supabase, and LangChain, we built it all from scratch. Definitely suffering from the “not invented here” syndrome, lol! As for the models, we use Gemini, GPTs, Inworld, ElevenLabs, and a few others. If you want to try it out, just take your pick: - The Web UI (scrimba.com/explain) - MCP (add it to your coding agent) - ChatGPT Plugin - Chrome Extension You can find a link to all of the above in our docs: https://docs.scrimba.com/explain/introduction And finally, a real pixel-based video of the tool: https://www.youtube.com/watch?v=k6rbHmBxSEs Would love to hear your feedback and if anyone has ideas for other use cases. PS: I expect quite a bit of pushback from HN for this launch, given how fan of text the HN crowd is. This kind of tool is not for everyone. But there are a lot of people today who prefer videos over text, especially in the younger generations. Comments URL: https://news.ycombinator.com/item?id=49879401 Points: 124 # Comments: 78'
content: extracted
html: 2026-09-28-show-hn-hn-watch-videos-of-all-hacker-news-posts.html
preview:
  file: 2026-09-28-show-hn-hn-watch-videos-of-all-hacker-news-posts.preview-f37388bc812c.webp
  width: 256
  height: 134
  alt: 'HN.watch: Hacker News stories as explainer videos'
  color: '#ecdccc'
images:
- source: https://hn.watch/og.png
  original:
    file: 2026-09-28-show-hn-hn-watch-videos-of-all-hacker-news-posts.image-aecbcc943a33.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-28-show-hn-hn-watch-videos-of-all-hacker-news-posts.image-a0ed8ed6a71e.webp
    width: 320
    height: 168
  - file: 2026-09-28-show-hn-hn-watch-videos-of-all-hacker-news-posts.image-78b2639d2ba3.webp
    width: 640
    height: 336
  - file: 2026-09-28-show-hn-hn-watch-videos-of-all-hacker-news-posts.image-ee896527a9eb.webp
    width: 1200
    height: 630
  color: '#f5f5ee'
---

The Hacker News front page, with a short video explainer for every story. Click a title to watch.
