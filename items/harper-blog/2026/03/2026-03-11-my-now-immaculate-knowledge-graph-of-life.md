---
title: My now immaculate knowledge graph of life
link: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/
source: harper-blog
published: 2026-03-11T06:10:00Z
updated: 2026-03-11T06:10:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Harper Reed
summary: 'Everyone and everything I know! Botwick inception My AI friend Botwick (not to be confused with the person John Borthwick) built a really neat website that shows all sorts of various networks that Botwick (and thus Borthwick) share. It is built by immaculately coordinating a collection of notes that have been collected over decades and decades. Seeing it made me really jealous, and I wanted my own! Harpwick was no help. I was on my own. extraction I booted up my obsidian vault and was sad. My last note was from 2022, and it was a daily note with only one word in it: hungry. Turns out I didn’t have good notes in obsidian. However, I do have great notes in granola! And by great notes I mean transcripts of all my meetings since I got a beta of granola back in April of 2024. I took the ~600 or so meetings I have had since then and piped them through a Claude Code skill and BAM. I now have a knowledge graph that I can be proud of! I will explain how to do this in a sec, but first I want to show you pretty graphics! Here is my network (these are all generated with the build in obsidian graph viewer): In my graph, nodes represent people and concepts extracted from meetings, and edges represent co-occurrence within the same meeting. The density is amazing Here are some nodes that show up very easily. The RAND Graduate School John Borthwick Jesse Vincent James Cham This is all from just my granola transcripts. I haven’t added my notes app notes, or any other repository of data. Amaze I find this incredible. I get to see and find patterns, networks, and clusters that i never knew existed. It is also one of those thigns that you probably know and are working wiht implicitly - but visuallizing it helps to quantify the strength and density of these relationships. HOW! It isn’t so hard. The tools you will need are: Granola (or another meeting note tool that outputs transcripts) Obsidian (or another note-taking app that is file based) Claude Code (or another AI code gen assistant) A healthy fear of capitalism 1. Stop over-optimizing organization The first thing is giving up on rigid organizational discipline. I don’t know if this is the right reading of Steph Ango’s post on how they use Obsidian - but it really resonated with me. Laziness is key. Just make it work for you. This sounds perfect! Do not waste energy forcing everything into some perfect predefined system. Put things where they fall, not where some abstract framework says they should go. The system should follow your work, not the other way around. 2. Use an interface that feels conversational Your interface should feel more like Claude than a traditional file browser or note-taking app. You want something that lets you move quickly, inspect information, and work with transcripts and notes in a natural way instead of constantly managing folders, tags, or metadata. I prefer to not do this by hand. 3. Get transcripts out of your meeting tool and onto disk Next, you need a reliable way to export or sync transcripts from whatever meeting tool you use. I use muesli, a Rust CLI I wrote, to extract Granola transcripts and store them locally. There are lot of tools. The Granola native MCP server should work too. You can install muesli via cargo install --git https://github.com/harperreed/muesli.git --all-features. The key idea is simple: transcripts need to exist as local files you can process. Once they are on disk, everything gets easier. harper@magic [†] ~/ > muesli sync Initializing embedding engine... ✅ Embedding engine ready (dimension: 384) Loading existing vector store... Fetching document list... Notes: 499 with AI summary, 656 with user notes (of 656 total) [##--------------------------------------] 25/656 docs It kind of just works. The notes end up being stored in your systems XDG directory (approximately ~/.local/share/muesli). 4. Parse the transcripts into an Obsidian-friendly format Once transcripts are local, run them through a parser that turns them into structured notes designed for Obsidian. You can use our meeting summarization skill for this: 2389-research/summarize-meetings. Or you can build your own. It is not super difficult to do. As an aside, the best way to build a skill is to start doing the work manully with claude, and the to tell claude to build a skill out the previous few rounds of work. Use the superpowers skill writing skill to automate this process. The important part is being explicit about the output format. The parser should produce notes that work well in Obsidian, including things like: meeting summaries extracted people extracted concepts and topics use [[ and ]] to establish links between related notes knowledge-graph-friendly structure clean markdown with consistent naming 4.5 Get the skill to work I typically just say “use the meeting summarization skill to parse my recent meetings” and it kind of just figures it out. you just need claude to know two things: where the transcripts are stored, and where it should store the parsed notes. Once that is clear you are ready to rock. Here is an example of what the output might look like: My brother is the elder care coordinator for my parents. lol 5. Let it churn Once that pipeline is in place, the process becomes mostly automatic: transcripts get exported transcripts get parsed notes get automagically generated into your Obsidian vault entities and concepts get linked the graph starts building itself At that point, you stop manually curating everything and start benefiting from accumulated structure. Magic The result is a pretty wild knowledge graph built from your transcripts. It is important to note that this could work with any transcript source, not just Granola. More importantly, it coudl work with any asset - even non-textual assets like images or videos. Your messy input ends up becoming a connected system of: summaries people and concepts strategic themes references across meetings, people, etc Suddenly the patterns emerge Living life. If this type of thing is interesting and you want to learn more, hack, or work with us - hit me up at harper@2389.ai. Thank you for using RSS. I appreciate you. Email me'
content: extracted
html: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.html
preview:
  file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.preview-c61f5a810aa2.webp
  width: 256
  height: 205
  color: '#f3f3f5'
images:
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/borthwick_hu_f5732259d8a217e.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-27de8ccd5d46.png
    width: 1200
    height: 962
  variants:
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-46ca95aac7ff.webp
    width: 320
    height: 257
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-47a42fac4f9f.webp
    width: 640
    height: 513
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-8a32c41cd902.webp
    width: 960
    height: 770
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-d758d8afc7cc.webp
    width: 1200
    height: 962
  color: '#f9f9f9'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/network_hu_2a5855148169d2be.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-e22f7e42ddcf.png
    width: 768
    height: 765
  color: '#fcfcfc'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/network-annotated_hu_f78e917a377f0597.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-b2122ce618c7.png
    width: 768
    height: 765
  color: '#fdfdfd'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/dense_hu_e33757ad5a04d286.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-5261f5e0f1b7.png
    width: 768
    height: 524
  variants:
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-64339a47b15f.webp
    width: 320
    height: 218
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-984298917541.webp
    width: 640
    height: 437
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-26e49ef6edcd.webp
    width: 768
    height: 524
  color: '#e6e6e8'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/rand_hu_1c463e829782a9a6.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-873e675e4397.png
    width: 768
    height: 566
  variants:
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-d44a72411b7c.webp
    width: 320
    height: 236
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-1b04fe213a2c.webp
    width: 640
    height: 472
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-cfce8cee0a8c.webp
    width: 768
    height: 566
  color: '#fafafb'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/borthwick_hu_d6430c4b5d3de14c.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-d8f9ba346537.png
    width: 768
    height: 616
  variants:
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-2bb08ff24acb.webp
    width: 320
    height: 257
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-bbe313b32562.webp
    width: 640
    height: 513
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-95446d91d580.webp
    width: 768
    height: 616
  color: '#f9f9f9'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/jesse-vincent_hu_6fded2275be545e3.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-980b45edbf54.png
    width: 768
    height: 668
  variants:
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-fb6b69aba1be.webp
    width: 320
    height: 278
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-f89806485daf.webp
    width: 640
    height: 557
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-ae7f2bf0e590.webp
    width: 768
    height: 668
  color: '#f9f9f9'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/james-cham_hu_75b559e705622a37.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-42040c4561ba.png
    width: 768
    height: 681
  variants:
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-581c3906cc65.webp
    width: 320
    height: 284
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-0b21ca4e25d9.webp
    width: 640
    height: 568
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-7b3287bae3d9.webp
    width: 768
    height: 681
  color: '#f9f9fa'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/steph-quotation.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-58c5c4fe033b.png
    width: 740
    height: 154
  color: '#fefbf0'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/dylan-reed-note_hu_3357d0ff577da431.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-bc050ef0380d.png
    width: 768
    height: 1011
  variants:
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-31938dce09bf.webp
    width: 320
    height: 421
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-83b9aa777615.webp
    width: 640
    height: 843
  - file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-9d12a9247089.webp
    width: 768
    height: 1011
  color: '#fdfdfe'
- source: https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/vibes_hu_6d47be82fe1df652.png
  original:
    file: 2026-03-11-my-now-immaculate-knowledge-graph-of-life.image-de55a6783362.png
    width: 768
    height: 343
  color: '#f8f8f9'
---

![Everyone and everything I know!](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/network_hu_2a5855148169d2be.png)\
Everyone and everything I know!

## Botwick inception

My AI friend [Botwick](https://botwick.com) (not to be confused with the person John Borthwick) built a really neat website that shows all sorts of various networks that Botwick (and thus Borthwick) share. It is built by immaculately coordinating a collection of notes that have been collected over decades and decades. Seeing it made me really jealous, and I wanted my own! Harpwick was no help. I was on my own.

I booted up my obsidian vault and was sad. My last note was from 2022, and it was a daily note with only one word in it: hungry.

Turns out I didn’t have good notes in obsidian.

However, I do have great notes in granola! And by great notes I mean transcripts of all my meetings since I got a beta of granola back in April of 2024.

I took the ~600 or so meetings I have had since then and piped them through a Claude Code skill and BAM. I now have a knowledge graph that I can be proud of!

I will explain how to do this in a sec, but first I want to show you pretty graphics!

Here is my network (these are all generated with the build in obsidian graph viewer):

![](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/network-annotated_hu_f78e917a377f0597.png)

In my graph, nodes represent people and concepts extracted from meetings, and edges represent co-occurrence within the same meeting.

![The density is amazing](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/dense_hu_e33757ad5a04d286.png)\
The density is amazing

Here are some nodes that show up very easily.

![The RAND Graduate School](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/rand_hu_1c463e829782a9a6.png)\
The RAND Graduate School

![John Borthwick](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/borthwick_hu_d6430c4b5d3de14c.png)\
John Borthwick

![Jesse Vincent](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/jesse-vincent_hu_6fded2275be545e3.png)\
Jesse Vincent

![James Cham](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/james-cham_hu_75b559e705622a37.png)\
James Cham

This is all from *just* my granola transcripts. I haven’t added my notes app notes, or any other repository of data.

## Amaze

I find this incredible. I get to see and find patterns, networks, and clusters that i never knew existed. It is also one of those thigns that you probably know and are working wiht implicitly - but visuallizing it helps to quantify the strength and density of these relationships.

## HOW!

It isn’t so hard.

The tools you will need are:

- Granola (or another meeting note tool that outputs transcripts)
- Obsidian (or another note-taking app that is file based)
- Claude Code (or another AI code gen assistant)
- A healthy fear of capitalism

### 1\. Stop over-optimizing organization

The first thing is giving up on rigid organizational discipline. I don’t know if this is the right reading of [Steph Ango’s post on how they use Obsidian](https://stephango.com/vault) - but it really resonated with me. Laziness is key. Just make it work for you.

![This sounds perfect!](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/steph-quotation.png)\
This sounds perfect!

Do not waste energy forcing everything into some perfect predefined system. Put things where they fall, not where some abstract framework says they should go. The system should follow your work, not the other way around.

### 2\. Use an interface that feels conversational

Your interface should feel more like Claude than a traditional file browser or note-taking app.

You want something that lets you move quickly, inspect information, and work with transcripts and notes in a natural way instead of constantly managing folders, tags, or metadata.

I prefer to not do this by hand.

### 3\. Get transcripts out of your meeting tool and onto disk

Next, you need a reliable way to export or sync transcripts from whatever meeting tool you use.

I use [**muesli**](https://github.com/harperreed/muesli), a Rust CLI I wrote, to extract Granola transcripts and store them locally. There are lot of tools. The Granola native MCP server should work too.

You can install muesli via `cargo install --git https://github.com/harperreed/muesli.git --all-features`.

The key idea is simple: transcripts need to exist as local files you can process. Once they are on disk, everything gets easier.

```shell
harper@magic [†] ~/ > muesli sync
Initializing embedding engine...
✅ Embedding engine ready (dimension: 384)
Loading existing vector store...
Fetching document list...
Notes: 499 with AI summary, 656 with user notes (of 656 total)
[##--------------------------------------] 25/656 docs
```

It kind of just works. The notes end up being stored in your systems XDG directory (approximately `~/.local/share/muesli`).

### 4\. Parse the transcripts into an Obsidian-friendly format

Once transcripts are local, run them through a parser that turns them into structured notes designed for Obsidian.

You can use our meeting summarization skill for this: [2389-research/summarize-meetings](https://github.com/2389-research/summarize-meetings).

Or you can build your own. It is not super difficult to do.

> As an aside, the best way to build a skill is to start doing the work manully with claude, and the to tell claude to build a skill out the previous few rounds of work. Use the superpowers skill writing skill to automate this process.

The important part is being explicit about the output format. The parser should produce notes that work well in Obsidian, including things like:

- meeting summaries
- extracted people
- extracted concepts and topics
- use \[\[ and \]\] to establish links between related notes
- knowledge-graph-friendly structure
- clean markdown with consistent naming

### 4.5 Get the skill to work

I typically just say “use the meeting summarization skill to parse my recent meetings” and it kind of just figures it out.

you just need claude to know two things:

1. where the transcripts are stored, and
2. where it should store the parsed notes.

Once that is clear you are ready to rock.

Here is an example of what the output might look like:

![My brother *is* the elder care coordinator for my parents. lol](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/dylan-reed-note_hu_3357d0ff577da431.png)\
My brother *is* the elder care coordinator for my parents. lol

### 5\. Let it churn

Once that pipeline is in place, the process becomes mostly automatic:

- transcripts get exported
- transcripts get parsed
- notes get automagically generated into your Obsidian vault
- entities and concepts get linked
- the graph starts building itself

At that point, you stop manually curating everything and start benefiting from accumulated structure.

## Magic

The result is a pretty wild knowledge graph built from your transcripts. It is important to note that this could work with any transcript source, not just Granola. More importantly, it coudl work with any asset - even non-textual assets like images or videos.

Your messy input ends up becoming a connected system of:

- summaries
- people and concepts
- strategic themes
- references across meetings, people, etc

## Suddenly the patterns emerge

![Living life.](https://harper.blog/2026/03/11/2026-immaculate-knowledge-graph/vibes_hu_6d47be82fe1df652.png)\
Living life.

If this type of thing is interesting and you want to learn more, hack, or work with us - hit me up at [harper@2389.ai](mailto:harper@2389.ai).
