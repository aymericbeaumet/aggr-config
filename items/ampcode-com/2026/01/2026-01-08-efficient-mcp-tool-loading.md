---
title: Efficient MCP Tool Loading
link: https://ampcode.com/news/lazy-load-mcp-with-skills
source: ampcode-com
published: 2026-01-08T00:00:00Z
updated: 2026-01-08T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'MCP servers often provide a lot of tools, many of which aren''t used. That costs a lot of tokens, because these tool definitions have to be inserted into the context window whether they''re used by the agent or not. As an example: the chrome-devtools MCP currently provides 26 tools that together take up 17k tokens; that''s 10% of Opus 4.5''s context window and 26 tools isn''t even a lot for many MCP servers. To help with that, Amp now allows you to combine MCP server configurations with Agent Skills, allowing the agent to load an MCP server''s tool definitions only when the skill is invoked. How It Works Create an mcp.json file in the skill definition, next to the SKILL.md file, containing the MCP servers and tools you want the agent to load along with the skill: { "chrome-devtools": { "command": "npx", "args": ["-y", "chrome-devtools-mcp@latest"], "includeTools": [ // Tool names or glob patterns "navigate_page", "take_screenshot", "new_page", "list_pages" ] } } At the start of a thread, all the agent will see in the context window is the skill description. When (and if) it then invokes the skill, Amp will append the tool descriptions matching the includeTools list to the context window, making them available just in time. With this specific configuration, instead of loading all 26 tools that chrome-devtools provides, we instead load only four tools, taking up 1.5k tokens instead of 17k. Take a look at our ui-preview skill, that makes use of the chrome-devtools MCP, for a full example. If you want to learn more about skills in Amp, take a look at the Agent Skills section in the manual. To find out more about the implementation of this feature and how we arrived at it, read this blog post by Nicolay.'
content: extracted
html: 2026-01-08-efficient-mcp-tool-loading.html
preview:
  file: 2026-01-08-efficient-mcp-tool-loading.preview-9833a85ac075.webp
  width: 256
  height: 134
  color: '#5c554b'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Efficient+MCP+Tool+Loading&date=January+8%2C+2026&tagline=Load+MCP+tools+into+your+context+window+only+when+you+use+them&sig=3522ab0d5a04d6eff52a1b6e58b0568707ab816b40a904a43b545e81aeed92fd
  original:
    file: 2026-01-08-efficient-mcp-tool-loading.image-bdee0d239eed.png
    width: 1200
    height: 630
  variants:
  - file: 2026-01-08-efficient-mcp-tool-loading.image-66607261316e.webp
    width: 320
    height: 168
  - file: 2026-01-08-efficient-mcp-tool-loading.image-4ee345d93a77.webp
    width: 640
    height: 336
  - file: 2026-01-08-efficient-mcp-tool-loading.image-ca07afd8d325.webp
    width: 960
    height: 504
  - file: 2026-01-08-efficient-mcp-tool-loading.image-c31f8a594bf0.webp
    width: 1200
    height: 630
  color: '#221b16'
---

MCP servers often provide a lot of tools, many of which aren't used. That costs a lot of tokens, because these tool definitions have to be inserted into the context window whether they're used by the agent or not.

As an example: the [chrome-devtools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp) currently provides 26 tools that together take up 17k tokens; that's 10% of Opus 4.5's context window and 26 tools isn't even a lot for many MCP servers.

To help with that, Amp now allows you to combine MCP server configurations with [Agent Skills](https://ampcode.com/news/agent-skills), allowing the agent to load an MCP server's tool definitions only when the skill is invoked.

## How It Works

Create an `mcp.json` file in the skill definition, next to the `SKILL.md` file, containing the MCP servers and tools you want the agent to load along with the skill:

```json
{
	"chrome-devtools": {
		"command": "npx",
		"args": ["-y", "chrome-devtools-mcp@latest"],
		"includeTools": [
			// Tool names or glob patterns
			"navigate_page",
			"take_screenshot",
			"new_page",
			"list_pages"
		]
	}
}
```

At the start of a thread, all the agent will see in the context window is the skill description. When (and if) it then invokes the skill, Amp will append the tool descriptions matching the `includeTools` list to the context window, making them available just in time.

With this specific configuration, instead of loading all 26 tools that `chrome-devtools` provides, we instead load only four tools, **taking up 1.5k tokens instead of 17k**.

Take a look at our [ui-preview skill](https://github.com/ampcode/amp-contrib/tree/main/.agents/skills/ui-preview), that makes use of the `chrome-devtools` MCP, for a full example.

If you want to learn more about skills in Amp, take a look [at the Agent Skills section in the manual](https://ampcode.com/manual#agent-skills).

To find out more about the implementation of this feature and how we arrived at it, read [this blog post by Nicolay](https://nicolaygerold.com/posts/tool-search-is-dead-long-live-skills).
