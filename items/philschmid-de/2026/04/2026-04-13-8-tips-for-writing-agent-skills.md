---
title: 8 Tips for Writing Agent Skills
link: https://www.philschmid.de/agent-skills-tips
source: philschmid-de
published: 2026-04-13T00:00:00Z
updated: 2026-04-13T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 8 Tips for Writing Agent Skills. Know What a Skill Is, Nail the Description, Write Instructions, Keep It Lean, Set the Right Level of Freedom, Don't Skip Negative Cases, Test It Before You Ship It, Know When to Retire a Skill.
content: extracted
html: 2026-04-13-8-tips-for-writing-agent-skills.html
preview:
  file: 2026-04-13-8-tips-for-writing-agent-skills.preview-b9c0bb8e32c0.webp
  width: 256
  height: 134
  alt: 8 Tips for Writing Agent Skills
  color: '#0c0c0d'
images:
- source: https://www.philschmid.de/static/blog/agent-skills-tips/thumbnail.jpg
  original:
    file: 2026-04-13-8-tips-for-writing-agent-skills.image-c316d838af9b.jpg
    width: 1500
    height: 785
  color: '#010101'
---

Skills have become one of the most used extension points in agents. They’re flexible, easy to make, and simple to distribute.

But this flexibility also makes it hard to know what good and what works. What type of skills are worth making? What's the secret to writing a good skill? When do you share them with others?

I have been using skills extensively with many of them in active use. Here are some tips I've learned along the way.

## 1\. Know What a Skill Is

A skill is a folder with a **`SKILL.md`** file and, optionally, some helper files:

```plaintext
my-skill/
├── SKILL.md          ← The only required file
├── scripts/          ← Reusable code the agent can run
├── references/       ← Docs the agent reads when needed
└── assets/           ← Templates, images, or files used in output
```

A Skill consists out of 3 layers:

- **`name` and `description` Frontmatter** Goes into every prompt and tells the agent *when* to use the skill.
- **Body of `SKILL.md`** Markdown instructions (below frontmatter), that tells the agent *how* to do the task.
- **Assets** (optional), `scripts/`, `references/`, and `assets/` folders.

Skills always fall into two categories:

- **Capability skills** help the agent do something the base model can't do consistently (e.g., PDF form filling) without it. These may become unnecessary as models improve, evals will tell you when.
- **Preference skills** encode your specific workflow (e.g., your team's code review steps). These are durable but need to stay in sync with your actual process.

## 2\. Nail the Description

The `description` in your `SKILL.md` is the trigger mechanism. If it's vague, the agent won't know when to activate the skill. If it's too broad, the skill fires on every request. Be specific about what the skill does AND when to use it. Include both the "what" and the "when" in the description. The body of the skill only loads *after* the skill triggers.

| ❌ Too vague              | ✅ Specific and actionable                                                                                                |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| *"Helps with documents"* | *"Create, edit, and analyze .docx files, use for tracked changes, comments, formatting, or text extraction"*             |
| *"API helper"*           | *"Use when writing code that calls the Gemini API for text generation, multi-turn chat, image generation, or streaming"* |

I have seen 50% improvements just by improving the description.

## 3\. Write Instructions, Not Essays

Agents are smart. Your job is to tell it what it **doesn't already know**. Research shows longer, more comprehensive with too much context actually hurts performance.

- **Use directives:** *"Always use `interactions.create()`"*, not *"The Interactions API is the recommended approach."* The first is an instruction, the second is trivia the agent won't act on.
- **Lead with examples:** A 5-line code snippet beats a 5-paragraph explanation.
- **Explain the why:** When a rule matters, say why. *"Use model X, model Y is deprecated and will return errors"* helps the agent generalize beyond specific test cases, not just memorize.
- **Don't overfit:** Avoid "fiddly" changes that only pass your three test prompts. Write skills that work across millions of invocations.

## 4\. Keep It Lean

Don't dump everything into one file. The agent loads information in layers:

1. **Always loaded:** Frontmatter of `SKILL.md`, `name` + `description`
2. **Loaded when skill triggers:** the `SKILL.md` body (keep under 500 lines)
3. **Loaded on demand:** reference files, scripts, assets

If your skill covers multiple topics (e.g. AWS vs. GCP deployment), split them into separate reference files. The agent only reads the one it needs. This saves context for the actual task.

**Tip:** If a reference file is longer than 500 lines, add a table of contents with "line hints" at the top so the agent can quickly find what it needs.

## 5\. Set the Right Level of Freedom

A common mistake in creating skills is turning the skill into a step-by-step worklow: *"Step 1: Read the file. Step 2: Parse the JSON. Step 3: Extract the fields…"* When you dictate every step, you take away their ability to adapt, recover from errors, or find better approaches. Describe what you want, not the path to get there.

**Tell the agent what to achieve:**

- ❌ *"Step 1: Read the config file. Step 2: Find the database URL. Step 3: Update the port number. Step 4: Write the file back."*
- ✅ *"Update the database port in the config file to the value specified by the user."*

**Provide constraints, not procedures:**

- ❌ *"Step 1: Create a branch. Step 2: Make the change. Step 3: Run tests. Step 4: Open a PR."*
- ✅ *"Always run tests before opening a PR. Never push directly to main."*

**If exact steps matter, write a script.** If the task is fragile and doing step 3 before step 2 breaks everything, that's not a skill problem, it's a scripting problem.

## 6\. Don't Skip Negative Cases

Think about when the skill should **not** fire. A description like *"Use for any coding task"* will hijack every request.

> *"Use when working with PDF files. Do NOT use for general document editing, spreadsheets, or plain text files."*

Testing both "should trigger" and "shouldn't trigger" cases are essential. Without you'll optimize the skill in one direction.

## 7\. Test It Before You Ship It

Don't ship a skill without evaluating. Each run might behave differently, so a single check isn't enough.

1. **Run it manually** a few times with different prompts. Watch where it breaks. Does it assume a dependency exists? Does it skip steps?
2. **Write down what "success" looks like** measurable. Does the output compile? Does it use the right API? Did it follow the steps? Grade outcomes, not paths.
3. **Try 10–20 test prompts**. Mix prompts the skill should handle, prompts it should ignore, and tricky edge cases. Each prompt should have its own success criteria.
4. **Run multiple trials.** Agent output is nondeterministic. Run 3–5 trials per prompt and look at the distribution instead of a single pass/fail.
5. **Isolate each run.** Use a clean environment for each test. Context bleeding between runs masks real failures.
6. **Fix the description first.** Most problems are in the trigger, not the instructions.

## 8\. Know When to Retire a Skill

Run evals without the skill. If they pass, the model has absorbed the skill's value and the skill is no longer necessary. Retire it. This is especially true for capability skills, as models improve, the gap narrows.

For a practical step-by-step eval workflow, see [Practical Guide to Evaluating and Testing Agent Skills](https://www.philschmid.de/testing-skills).

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
