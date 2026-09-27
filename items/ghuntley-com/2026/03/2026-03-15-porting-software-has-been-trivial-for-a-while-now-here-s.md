---
title: porting software has been trivial for a while now. here’s how you do it.
link: https://ghuntley.com/porting/
source: ghuntley-com
published: 2026-03-15T10:02:33Z
updated: 2026-03-15T10:02:33Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Geoffrey Huntley
labels:
- ai
summary: 'This one is short and sweet. if you want to port a codebase from one language to another here’s the approach: Run a ralph loop which compresses all tests into /specs/.md which looks similar to “study every file in tests/* using separate subagents and document in'
content: extracted
html: 2026-03-15-porting-software-has-been-trivial-for-a-while-now-here-s.html
preview:
  file: 2026-03-15-porting-software-has-been-trivial-for-a-while-now-here-s.preview-538bbaca7347.webp
  width: 256
  height: 256
  color: '#deb094'
images:
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/03/IMG_1831.jpeg
  original:
    file: 2026-03-15-porting-software-has-been-trivial-for-a-while-now-here-s.image-6e0cd54fa01d.jpg
    width: 1024
    height: 1024
  color: '#fefef6'
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/size/w2000/2026/03/IMG_1831.jpeg
  original:
    file: 2026-03-15-porting-software-has-been-trivial-for-a-while-now-here-s.image-5c40b80246bf.jpg
    width: 1024
    height: 1024
  color: '#fefef6'
---

By [Geoffrey Huntley](https://ghuntley.com/author/ghuntley/) in [AI](https://ghuntley.com/tag/ai/) — 15 Mar 2026

![porting software has been trivial for a while now. here’s how you do it.](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/size/w2000/2026/03/IMG_1831.jpeg)

This one is short and sweet. if you want to port a codebase from one language to another here’s the approach:

1. Run a ralph loop which compresses all tests into /specs/*.md which looks similar to “study every file in tests/*\* using separate subagents and document in /specs/\*.md and link the implementation as citations in the specification“
2. Then do a separate Ralph loop for all product functionality - ensuring there’s citations to the specification. “study every file in src/\* using seperate subagents per file and link the implementation as citations in the specification“
3. Once you have that - within the same repo run a Ralph loop to create a TODO file and then execute a classic ralph - doing just one thing and the most important thing per loop. Remind the agent that it can study the specifications and follow the citations to reference source code.
4. For best outcomes you wanna configure your target language to have strict compilation

The key theory here is usage of citations in the specifications which tease the file\_read tool to study the original implementation during stage 3. Reducing stage 1 and stage 2 to specs is the precursor which transforms a code base into high level PRDs without coupling the implementation from the source language.

### p.p.s don't miss out on free lifetime access

Join thousands getting premium insights delivered weekly. **Subscribe now and you'll never pay** - even when I start charging new subscribers. This free-for-life offer won't last forever.
