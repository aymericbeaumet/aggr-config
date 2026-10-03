---
title: Read Bigger Threads
link: https://ampcode.com/news/read-bigger-threads
source: ampcode-com
published: 2026-07-02T00:00:00Z
updated: 2026-07-02T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Threads outgrew read_thread, so we rewrote it. read_thread is the tool that lets Amp pull context out of other Amp threads when you mention them. Before the rewrite, it would fetch the whole thread and extract the relevant parts in a single call to another LLM. That used to work when threads were shorter and contained a single context window. Then we added compaction and now a single thread can run for weeks. Our longest thread has been compacted over 68 times — without compaction, it would be over 21 million tokens long. A 21-million-token thread doesn''t fit into a single context window, so asking another LLM to extract relevant parts doesn''t work anymore. And even threads with 1 million tokens that fit gave bad answers: one giant prompt over-weights whatever the thread ended with or started with and ignores the information in the middle. read_thread is now a subagent tuned to extract information from long threads. The subagent takes a thread and a question, searches the thread, reads the messages, and checks whether later work revised or reverted what it found. Our first version of the read_thread subagent answered from the first plausible hit. In a long thread, the first hit is often an attempt that was later revised or reverted. We switched the model to GLM 5.2 from Gemini 3.5 Flash and tuned its prompt to optimize for correctness over speed: "Do not stop at the first relevant hit; check for newer messages that revise, supersede, revert, or contradict it." "Tool calls record attempted actions, not outcomes." It checks whether an edit actually succeeded before believing it. "Use compactions for orientation, but inspect original messages when exact requirements, wording, code, commands, chronology, edits, or verification matter." It also works on the thread you''re in. When the agent needs something from three weeks ago — a decision, an error, the original plan — it goes back and looks instead of trusting the compaction. Nothing changes on your end. Either tell Amp what you''re looking for and let it find the thread, or give it a thread explicitly: paste a URL, or @-mention it. And when you open a new thread, hit Enter twice to reference the thread you just left. Mention a thread and ask a question, just like before, except it now works with big threads too: "Implement the plan from https://ampcode.com/threads/T-…" "What did we implement for the visibility submenu in https://ampcode.com/threads/T-…? Which files changed, and how does it work now?" "Summarize the requirements, implementation decisions, and known caveats from https://ampcode.com/threads/T-… before you review this." "Find the thread where we debugged the executor connection timeout and summarize the fix." "Hey, I know it''s a lot, I know I should''ve stopped way earlier, but can you look at this thread in which we went for 271 rounds and extract that bootstrap script? https://ampcode.com/threads/T-…"'
content: extracted
html: 2026-07-02-read-bigger-threads.html
preview:
  file: 2026-07-02-read-bigger-threads.preview-79d36cd52bb9.webp
  width: 256
  height: 134
  color: '#645d52'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Read+Bigger+Threads&date=July+2%2C+2026&tagline=Amp+can+now+read+threads+of+any+size+and+answer+questions+about+them+%E2%80%94+even+the+one+where+you+went+271+rounds.&sig=adf8e7b45e3f940276331509dfddd62e1ad6238fa528ab8ae2d115eb185e033d
  original:
    file: 2026-07-02-read-bigger-threads.image-bdbc8ce8b5df.png
    width: 1200
    height: 630
  variants:
  - file: 2026-07-02-read-bigger-threads.image-89e45d17f86d.webp
    width: 320
    height: 168
  - file: 2026-07-02-read-bigger-threads.image-3c865c416a2e.webp
    width: 640
    height: 336
  - file: 2026-07-02-read-bigger-threads.image-83dd5ee2ad35.webp
    width: 960
    height: 504
  - file: 2026-07-02-read-bigger-threads.image-598536661a9e.webp
    width: 1200
    height: 630
  color: '#221b16'
---

Threads outgrew `read_thread`, so we rewrote it.

`read_thread` is [the tool that lets Amp pull context out of other Amp threads](https://ampcode.com/news/read-threads) when you mention them. Before the rewrite, it would fetch the whole thread and extract the relevant parts in a single call to another LLM.

That used to work when threads were shorter and contained a single context window. Then we added [compaction](https://ampcode.com/news/neo) and now a single thread can run for weeks. Our longest thread has been compacted over 68 times — without compaction, it would be over 21 million tokens long.

A 21-million-token thread doesn't fit into a single context window, so asking another LLM to extract relevant parts doesn't work anymore. And even threads with 1 million tokens that fit gave bad answers: one giant prompt over-weights whatever the thread ended with or started with and ignores the information in the middle.

`read_thread` is now a subagent tuned to extract information from long threads. The subagent takes a thread and a question, searches the thread, reads the messages, and checks whether later work revised or reverted what it found.

Our first version of the `read_thread` subagent answered from the first plausible hit. In a long thread, the first hit is often an attempt that was later revised or reverted. We switched the model to GLM 5.2 from Gemini 3.5 Flash and tuned its prompt to optimize for correctness over speed:

- "Do not stop at the first relevant hit; check for newer messages that revise, supersede, revert, or contradict it."
- "Tool calls record attempted actions, not outcomes." It checks whether an edit actually succeeded before believing it.
- "Use compactions for orientation, but inspect original messages when exact requirements, wording, code, commands, chronology, edits, or verification matter."

It also works on the thread you're in. When the agent needs something from three weeks ago — a decision, an error, the original plan — it goes back and looks instead of trusting the compaction.

Nothing changes on your end. Either tell Amp what you're looking for and let it find the thread, or give it a thread explicitly: paste a URL, or @-mention it. And when you open a new thread, hit Enter twice to reference the thread you just left.

Mention a thread and ask a question, just like before, except it now works with big threads too:

- "Implement the plan from [https://ampcode.com/threads/T-…](https://ampcode.com/threads/T-%E2%80%A6)"
- "What did we implement for the visibility submenu in [https://ampcode.com/threads/T-…](https://ampcode.com/threads/T-%E2%80%A6)? Which files changed, and how does it work now?"
- "Summarize the requirements, implementation decisions, and known caveats from [https://ampcode.com/threads/T-…](https://ampcode.com/threads/T-%E2%80%A6) before you review this."
- "Find the thread where we debugged the executor connection timeout and summarize the fix."
- "Hey, I know it's a lot, I know I should've stopped way earlier, but can you look at this thread in which we went for 271 rounds and extract that bootstrap script? [https://ampcode.com/threads/T-…](https://ampcode.com/threads/T-%E2%80%A6)"
