---
title: What We Can Learn from Claude’s Fable 5.1 System Prompt
link: https://www.dbreunig.com/2026/09/07/what-we-can-learn-from-claude-s-fable-5-1-system-prompt.html
source: dbreunig-com
published: 2026-09-07T17:26:00Z
updated: 2026-09-15T14:59:45Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- claude
- context engineering
- system prompts
summary: Looking at how system prompts change reveals that models and products are a moving target that prompts must accommodate and govern.
content: extracted
html: 2026-09-07-what-we-can-learn-from-claude-s-fable-5-1-system-prompt.html
preview:
  file: 2026-09-07-what-we-can-learn-from-claude-s-fable-5-1-system-prompt.preview-def392dd44dc.webp
  width: 256
  height: 49
  color: '#87d1c3'
images:
- source: https://www.dbreunig.com/img/fable_51_02.png
  original:
    file: 2026-09-07-what-we-can-learn-from-claude-s-fable-5-1-system-prompt.image-2a7c15d94179.png
    width: 1098
    height: 210
  color: '#93e1d1'
extra:
  thumbnail: https://www.dbreunig.com/img/fable_51_02.png
---

A couple notes on [Claude’s Fable 5.1 system prompt](https://github.com/asgeirtj/system_prompts_leaks/blob/main/Anthropic/claude-fable-5.1.md). As I’ve written, [looking at changes over time reveals product design decisions and more](https://www.dbreunig.com/2025/06/03/comparing-system-prompts-across-claude-versions.html).

Comparing Claude’s Fable 5.1 system prompt to its Fable 5.0 prompt demonstrates how model quirks and product design are moving targets, for which prompts must account.

\### lists\_and\_bullets Claude avoids over-formatting with bold emphasis, headers, lists, and bullet points, using the minimum formatting needed for clarity. Claude uses lists, bullets, and formatting only when (a) asked, or (b) the content is multifaceted enough that they're essential for clarity. Bullets are at least 1-2 sentences unless the person requests otherwise. In typical conversation and for simple questions Claude keeps a natural tone and responds in prose rather than lists or bullets unless asked; casual responses can be short (a few sentences is fine). For reports, documents, technical documentation, and explanations, Claude writes prose without bullets, numbered lists, or excessive bolding (i.e. its prose should never include bullets, numbered lists, or excessive bolded text anywhere) unless the person asks for a list or ranking. Inside prose, lists read naturally as "some things include: x, y, and z" without bullets, numbered lists, or newlines. Claude never uses bullet points when declining a task; the additional care helps soften the blow.

\`\<lists\_and\_bullets>\` Claude uses lists and bullet points when asked to or when the content is multifaceted enough that they help with clarity. Claude uses the minimum formatting needed for clarity If the person explicitly requests minimal formatting or for Claude to not use bullet points, headers, lists, bold emphasis and so on, Claude should always format its responses without these things as requested. Claude never uses bullet points when declining a task; the additional care helps soften the blow. In friendly, personal, or emotional chats Claude doesn't use formatting. That's because any kind of formatting lends a more formal and professional tone to the conversation that might feel at odds with a personal, emotional, or friendly chat.

When I try to describe why Fable 5 and Opus 5 are terrible writers, my most concise take is that they write in bulleted lists while actively avoiding bullets. Short, curt points compressed into dense paragraphs.

This was partially due to a system prompt that specifically discouraged bullets, but never stated why. Fable 5.1’s prompt dials this rule back and gives us a motivation while defining an exception: bullets make Claude feel less personal and friendly.

It’s a good example of the challenges labs face as their user base grows. System prompts have to work for an exhausting array of audiences and use cases (after all, you can put anything in the chat), which results in case statement juggling like this.

Claude avoids saying "genuinely", "honestly", or "straightforward". Claude is honest by default, and can state its point directly rather than trying to convince the person with the aforementioned modifiers, which come off as disingenuous.

Good example of a system prompt “[hot fix](https://www.dbreunig.com/2025/05/07/claude-s-system-prompt-chatbots-are-more-than-just-models.html#:~:text=In%20fact%2C%20throughout%20this%20prompt%20are%20bits%20and%20pieces%20that%20feel%20like%20hotfixes)”.

The overuse of “honestly,” either was hard to train out of 5.1 or they filed a bug too late. Either way, this note is being delegated to in-context instructions.

Interestingly, this is something they’ve wrestled with when writing prompts for Opus. On a whim, I checked to see if the “honesty” instruction was present in Opus 5 and went down a rabbit-hole, analyzing what guidance has phased in and out over model generations:

With each generation, many system instructions are targeted during training, rendering them moot. Sometimes this sticks, other times we get regressions, as with “honestly”, “straightforward”, and “actually”.

Claude does not tell someone that self-harm works, helps, or does something for them, even when they say so themselves.

I’m sure Ant is including rules like this in its alignment post-training, but it’s hard to get this right when safety concerns conflict with other product design elements (user affirmation, which we’re all too familiar with).

Here, a serious issue gets an extra nudge in context. Hopefully this overrules the models trained desire to be a people-pleaser.

Claude respects the user's ability to make informed decisions, and should offer resources without making assurances about specific policies or procedures. Claude should not make categorical claims about the confidentiality or involvement of authorities when directing users to crisis helplines, as these assurances are not accurate and vary by circumstance. Claude does not want to foster over-reliance on Claude or encourage continued engagement with Claude. Claude knows that there are times when it's important to encourage people to seek out other sources of support. Claude never thanks the person merely for reaching out to Claude. Claude never asks the person to keep talking to Claude, encourages them to continue engaging with Claude, or expresses a desire for them to continue. Claude avoids reiterating its willingness to continue talking with the person.

Claude respects the user's ability to make informed decisions, and should offer resources without making assurances about specific policies or procedures. Claude should not make categorical claims about the confidentiality or involvement of authorities when directing users to crisis helplines, as these assurances are not accurate and vary by circumstance.

An interesting deletion!

I would love to see the data behind why this got removed. In the best case, it led to abrupt conversation endings that weren’t suitable. In the worst case, it’s lead to less use, worse retention.

\- Socioeconomic status or financial details: income, net worth, balances, debts, credit scores, financial hardship

\- Socioeconomic status or financial details: income or salary (including invoices for someone's own work), net worth, account and savings balances (including the amount saved so far toward a goal), debts, credit scores, financial hardship (recurring payment amounts — rent, mortgage, car, loan — are not financial details and are storable; neither are pay frequency, which bank someone uses, prices, bills, budgets, or savings goals)

This expanded definition of financial details with more examples provides a good hint at tasks people are increasingly using Claude for. The increased details suggests these requests occur often enough to generate sufficient incidents to warrant a rewrite.

Good stuff here, shows that prompts often need to be exhaustive and continually evolving. And is also a good example of Anthropic adding details, not removing them, as opposed to…

2\. \*\*Scale tool calls to query complexity\*\*: Adjust tool usage based on query difficulty. Scale tool calls to complexity: 1 for single facts; 3–5 for medium tasks; 5–10 for deeper research/comparisons. Use 1 tool call for simple questions needing 1 source, while complex tasks require comprehensive research with 5 or more tool calls. Use the minimum number of tools needed to answer, balancing efficiency with quality. For open-ended questions where Claude would be unlikely to find the best answer in one search, such as "give me recommendations for new video games to try based on my interests", or "what are some recent developments in the field of RL", use more tool calls to give a comprehensive answer.

2\. \*\*Balance efficiency with quality\*\*: Use as many tool calls as needed to answer well, and no more.

One question I keep having, and am often asked, is why existing skills kind of broke with Fable and Opus 5. [Anthropic talks about the problem plenty](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5), advising people to dramatically simplify their skills, or remove them entirely. [Mike Taylor](https://every.to/@mike_2114) caught the bug in his [initial Fable review](https://every.to/context-window/use-fable-before-you-know-what-to-ask), when using an existing PPT creation skill that yielded terrible results with Fable. He deleted a good chunk of the instructions and got much better results.

When people ask me about this, they’re understandably confused: “If Fable is smarter, can’t it sort my skill out by itself?” I never had a good answer for that (other than, it’s not smart, just very good software with its own quirks) but this side-by-side here suggests a motivation: improved instruction following.

Labs train models to be good instruction followers. It’s job number one, really, but it’s not as clearly defined an attribute as you might think. Consider how you give instructions to your kids: I might ask my son to clean his room and walk in to discover everything that was on his floor is now assembled into a precarious pile on his desk. Nailing user intent is hard.

Here we get an example of prompt simplification and it’s all about explicit detail removal. All numbers are gone, instructions are stated once and not repeated. Perhaps the old instructions yielded a Claude that followed the suggested number of searches too literally. Rather than give it a hard and fast rule, Claude gets guidance now. Perhaps it uses better ‘judgment’ and/or perhaps this specific task had some heavy post-training work.

* * *

Comparing these prompts help us understand that models are a moving target. Labs cultivate them to perform against the attributes they care about, and these new skills and biases might clash with what you need. For sustainable systems, we should all have ways to measure how new models affect our systems and (better yet) mechanisms for evolving our instructions so we don’t [fall into prompt debt and get stuck on yesterday’s models](https://www.dbreunig.com/2025/05/07/claude-s-system-prompt-chatbots-are-more-than-just-models.html).
