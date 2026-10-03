---
title: Global Plugins and Skills
link: https://ampcode.com/news/global-plugins-and-skills
source: ampcode-com
published: 2026-08-11T00:00:00Z
updated: 2026-08-11T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'With so much work happening in orbs there needed to be a new place to store Amp plugins and skills. So we added global plugins and skills. They''re Amp-hosted, built for agents, and work everywhere Amp runs. You can now tell Amp to: "Create a personal plugin that runs our formatter on every file the agent edits." "Import the browser-testing skill from this repo into my personal skills so I can use it on all my projects." "Does anyone on my team share a skill for writing release notes?" "Check if any of my imported plugins are out of date." If you''re a workspace admin: "Copy Thorsten''s plain-writing plugin into our workspace plugins." You can find them in your User Settings and Workspace Settings: Personal vs. Workspace Personal plugins and skills are good place to experiment and try things out. You can have your agent build something and reload the plugins live within the same thread. Workspace plugins and skills are pushable by workspace admins, and are loaded by default for everyone, so we recommend only publishing there once you''ve tested out something yourself. And if you want to make big changes to an existing workspace plugin, import it into your personal one, give it a test, and then publish it back up. Remix and Share Personal plugin and skills are much more than a replacement for ~/.config—they can be shared with your workspace too, letting people discover, import and remix them. If there''s upstream updates you''d like to pull into your version, or just update your version, you can run amp skill update <name>, amp plugins update <name>, or just ask Amp: "Update the browser-testing skill to the latest version." "Check if any of my imported skills are out of date."'
content: extracted
html: 2026-08-11-global-plugins-and-skills.html
preview:
  file: 2026-08-11-global-plugins-and-skills.preview-dd2675921aeb.webp
  width: 256
  height: 134
  color: '#80786e'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Global+Plugins+and+Skills&date=August+11%2C+2026&tagline=Use+plugins+and+skills+everywhere.+Share+and+remix+them+with+your+workspace.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fglobal-plugins-personal.png&sig=70c47db8f306688ed9373ccbd45e857bdf4ca9a9ccf75477d3e526c5937a30ae
  original:
    file: 2026-08-11-global-plugins-and-skills.image-b92c4dcf2793.png
    width: 1200
    height: 630
  variants:
  - file: 2026-08-11-global-plugins-and-skills.image-4f54bf88fa56.webp
    width: 320
    height: 168
  - file: 2026-08-11-global-plugins-and-skills.image-03b4ff7afdbc.webp
    width: 640
    height: 336
  - file: 2026-08-11-global-plugins-and-skills.image-e98754b88838.webp
    width: 960
    height: 504
  - file: 2026-08-11-global-plugins-and-skills.image-9d4cc47d3c40.webp
    width: 1200
    height: 630
  color: '#625c58'
- source: https://static.ampcode.com/news/global-plugins-personal.png
  original:
    file: 2026-08-11-global-plugins-and-skills.image-60701a906505.png
    width: 2400
    height: 1774
  color: '#f9f9f8'
- source: https://static.ampcode.com/news/global-skills-workspace.png
  original:
    file: 2026-08-11-global-plugins-and-skills.image-b5a3cf324bf9.png
    width: 2400
    height: 1774
  color: '#f9faf8'
- source: https://static.ampcode.com/news/global-skills-share.png
  original:
    file: 2026-08-11-global-plugins-and-skills.image-9b3826a6d5c7.png
    width: 2336
    height: 1670
  color: '#f9f9f7'
- source: https://static.ampcode.com/news/global-plugin-page.png
  original:
    file: 2026-08-11-global-plugins-and-skills.image-0d13d7c191c3.png
    width: 2336
    height: 1670
  color: '#fafaf9'
---

With so much work happening in [orbs](https://ampcode.com/manual/orbs) there needed to be a new place to store Amp plugins and skills. So we added **global plugins and skills**. They're Amp-hosted, built for agents, and work everywhere Amp runs.

You can now tell Amp to:

- "Create a personal plugin that runs our formatter on every file the agent edits."
- "Import the browser-testing skill from this repo into my personal skills so I can use it on all my projects."
- "Does anyone on my team share a skill for writing release notes?"
- "Check if any of my imported plugins are out of date."
- If you're a workspace admin: "Copy Thorsten's plain-writing plugin into our workspace plugins."

You can find them in your **User Settings** and **Workspace Settings**:

![Personal plugins settings showing plugins with their source](https://static.ampcode.com/news/global-plugins-personal.png)

![Workspace skills settings showing shared skills](https://static.ampcode.com/news/global-skills-workspace.png)

## Personal vs. Workspace

Personal plugins and skills are good place to experiment and try things out. You can have your agent build something and reload the plugins live within the same thread.

Workspace plugins and skills are pushable by workspace admins, and are loaded by default for everyone, so we recommend only publishing there once you've tested out something yourself. And if you want to make big changes to an existing workspace plugin, import it into your personal one, give it a test, and then publish it back up.

## Remix and Share

Personal plugin and skills are much more than a replacement for `~/.config`—they can be shared with your workspace too, letting people discover, import and remix them.

![Share dialog with Private and Workspace visibility](https://static.ampcode.com/news/global-skills-share.png)

![A shared plugin's page showing its source files and where it was imported from](https://static.ampcode.com/news/global-plugin-page.png)

If there's upstream updates you'd like to pull into your version, or just update your version, you can run `amp skill update <name>`, `amp plugins update <name>`, or just ask Amp:

- "Update the browser-testing skill to the latest version."
- "Check if any of my imported skills are out of date."
