---
title: Agents, Anywhere
link: https://ampcode.com/news/agents-anywhere
source: ampcode-com
published: 2026-07-08T00:00:00Z
updated: 2026-07-08T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'You can now start new agents remotely from ampcode.com anywhere you can run amp: That means, in addition to running agents in orbs, you can now run agents on any machine you want: your laptop, your server, your cloud dev box, your Raspberry Pi. Your lawn mower even, if it has a shell. Enable it by using the command amp: enable remote creation of threads or with the setting: // ~/.config/amp/settings.json { "amp.remoteThreadCreation.enabled": true } Once enabled, every Amp client you start will accept and run new threads in its working directory. Runner Mode You can also use the new runner mode with: amp --no-tui That starts Amp in a headless mode in which it only waits to start and run new threads: You can start multiple runners on the same machine, as long as they''re started in different directories. Each runner is uniquely identified by host and working directory. Directories don''t have to be version controlled. They can be anything, even home directories. You can start agents anywhere now. Walkthrough Here''s Thorsten with a walkthrough:'
content: extracted
html: 2026-07-08-agents-anywhere.html
preview:
  file: 2026-07-08-agents-anywhere.preview-a933a19d0005.webp
  width: 256
  height: 134
  color: '#686551'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Agents%2C+Anywhere&date=July+8%2C+2026&tagline=Remotely+start+agents+anywhere+you+can+run+%27amp%27&backgroundImage=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fstart-agents-anywhere-2-upper.jpg&sig=f4e4d27888cc8538c324cf3cfab6b66ec7961398aa353023ff7406257bfdf1a3
  original:
    file: 2026-07-08-agents-anywhere.image-84b7bc7c6511.png
    width: 1200
    height: 630
  variants:
  - file: 2026-07-08-agents-anywhere.image-de25313bc882.webp
    width: 320
    height: 168
  - file: 2026-07-08-agents-anywhere.image-30a5a5ab39fd.webp
    width: 640
    height: 336
  - file: 2026-07-08-agents-anywhere.image-cd82ddb5a8ce.webp
    width: 960
    height: 504
  - file: 2026-07-08-agents-anywhere.image-7faa27885e45.webp
    width: 1200
    height: 630
  color: '#fef2d7'
---

You can now start new agents remotely from [ampcode.com](https://ampcode.com) anywhere you can run `amp`:

That means, in addition to running [agents in orbs](https://ampcode.com/news/agents-in-orbs), you can now run agents on any machine you want: your laptop, your server, your cloud dev box, your Raspberry Pi. Your lawn mower even, if it has a shell.

Enable it by using the command `amp: enable remote creation of threads` or with the setting:

```json
// ~/.config/amp/settings.json

{
	"amp.remoteThreadCreation.enabled": true
}
```

Once enabled, every Amp client you start will accept and run new threads in its working directory.

## Runner Mode

You can also use the new *runner* mode with:

```
amp --no-tui
```

That starts Amp in a headless mode in which it only waits to start and run new threads:

You can start multiple runners on the same machine, as long as they're started in different directories. Each runner is uniquely identified by host and working directory. Directories don't have to be version controlled. They can be anything, even home directories.

You can start agents *anywhere* now.

## Walkthrough

Here's Thorsten with a walkthrough:
