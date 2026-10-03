---
title: Amp Now Reads Threads
link: https://ampcode.com/news/read-threads
source: ampcode-com
published: 2025-10-29T00:00:00Z
updated: 2025-10-29T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'You can now reference threads in your messages, and Amp will fetch and extract the relevant context from them. For example: "Implement the plan we devised in https://ampcode.com/threads/T-3f1beb2b-bded-4fda-96cc-1af7192f24b6" "Do what we did in https://ampcode.com/threads/T-f916b832-c070-4853-8ab8-5e7596953bec, but for the oracle tool" "Explain to me what my colleague Lily built in https://ampcode.com/threads/T-330bd49a-2402-453c-bbf4-0c2f2ce7f2b9" "Take the SQL queries from https://ampcode.com/threads/T-95e73a95-f4fe-4f22-8d5c-6297467c97a5 and turn it into a reusable script I can run" "Figure out whether and how Keegan ended up using the function created in https://ampcode.com/threads/T-e7ea6537-b3c6-4833-919c-45aa0af2d52f" To reference your own threads in the CLI and editor extensions, type @ and the title of the thread you want to reference. For other threads to which you have access, such as workspace or public threads, simply paste the thread URL or the ID in your message. Here''s what that looks like in the Amp CLI: Amp pulls in only what''s needed using a new read_thread tool, which first fetches the thread as Markdown and then uses another model to extract the relevant context based on your instructions. We think that threads as first-class entities — shareable, reusable, referenceable — has the potential to unlock new patterns for agentic programming.'
content: extracted
html: 2025-10-29-amp-now-reads-threads.html
preview:
  file: 2025-10-29-amp-now-reads-threads.preview-9aaa0e5efb5b.webp
  width: 256
  height: 134
  color: '#595247'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Amp+Now+Reads+Threads&date=October+29%2C+2025&tagline=Reference+and+let+Amp+read+other+Amp+threads&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fread-threads-screenshot.png&sig=4d7fcb9520d2a6e11b18a2325f455f9610c72595e1e0c30de0fe3cb51a3eedc5
  original:
    file: 2025-10-29-amp-now-reads-threads.image-73f1e31ab7c1.png
    width: 1200
    height: 630
  variants:
  - file: 2025-10-29-amp-now-reads-threads.image-93d8e818947d.webp
    width: 320
    height: 168
  - file: 2025-10-29-amp-now-reads-threads.image-b08ca16e33e5.webp
    width: 640
    height: 336
  - file: 2025-10-29-amp-now-reads-threads.image-f1948b2dda2f.webp
    width: 960
    height: 504
  - file: 2025-10-29-amp-now-reads-threads.image-62361efabc04.webp
    width: 1200
    height: 630
  color: '#221d19'
---

You can now reference threads in your messages, and Amp will fetch and extract the relevant context from them. For example:

- "Implement the plan we devised in [https://ampcode.com/threads/T-3f1beb2b-bded-4fda-96cc-1af7192f24b6](https://ampcode.com/threads/T-3f1beb2b-bded-4fda-96cc-1af7192f24b6)"
- "Do what we did in [https://ampcode.com/threads/T-f916b832-c070-4853-8ab8-5e7596953bec](https://ampcode.com/threads/T-f916b832-c070-4853-8ab8-5e7596953bec), but for the oracle tool"
- "Explain to me what my colleague Lily built in [https://ampcode.com/threads/T-330bd49a-2402-453c-bbf4-0c2f2ce7f2b9](https://ampcode.com/threads/T-330bd49a-2402-453c-bbf4-0c2f2ce7f2b9)"
- "Take the SQL queries from [https://ampcode.com/threads/T-95e73a95-f4fe-4f22-8d5c-6297467c97a5](https://ampcode.com/threads/T-95e73a95-f4fe-4f22-8d5c-6297467c97a5) and turn it into a reusable script I can run"
- "Figure out whether and how Keegan ended up using the function created in [https://ampcode.com/threads/T-e7ea6537-b3c6-4833-919c-45aa0af2d52f](https://ampcode.com/threads/T-e7ea6537-b3c6-4833-919c-45aa0af2d52f)"

To reference your own threads in the CLI and editor extensions, type `@` and the title of the thread you want to reference. For other threads to which you have access, such as workspace or public threads, simply paste the thread URL or the ID in your message.

Here's what that looks like in the Amp CLI:

Amp pulls in only what's needed using a new `read_thread` tool, which first fetches the thread as Markdown and then uses another model to extract the relevant context based on your instructions.

We think that threads as first-class entities — shareable, reusable, referenceable — has the potential to unlock new patterns for agentic programming.
