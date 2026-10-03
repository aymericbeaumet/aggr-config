---
title: Workspace Settings
link: https://ampcode.com/news/cli-workspace-settings
source: ampcode-com
published: 2025-11-04T00:00:00Z
updated: 2025-11-04T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'The Amp CLI now supports workspace-specific settings via .amp/settings.json files in your project directory. This allows you to configure the Amp CLI differently for each project, with settings like permissions, MCP servers, and other preferences automatically picked up when you run amp in that workspace. For example, here''s how you can configure the Amp CLI to use a specific MCP server in a given project: $ cd my-project $ amp mcp add --workspace playwright -- npx -y @playwright/mcp@latest Due to the --workspace flag, the setting was added in the .amp/settings.json file in my-project: $ cat .amp/settings.json { "amp.mcpServers": { "playwright": { "command": "npx", "args": [ "-y", "@playwright/mcp@latest" ] } } } When you then run amp in my-project, the settings in .amp/settings.json will be merged with your global Amp settings. See the manual for details on how settings are resolved.'
content: extracted
html: 2025-11-04-workspace-settings.html
preview:
  file: 2025-11-04-workspace-settings.preview-471ff51162bf.webp
  width: 256
  height: 134
  color: '#5d564c'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Workspace+Settings&date=November+4%2C+2025&tagline=Configure+the+Amp+CLI+per+workspace&sig=c7af7afb8e7e62f034ed76006aa86d8efdbfe9f95625bd341e06e8331b980101
  original:
    file: 2025-11-04-workspace-settings.image-6b93676a7c6f.png
    width: 1200
    height: 630
  variants:
  - file: 2025-11-04-workspace-settings.image-e0e4a5b6c21f.webp
    width: 320
    height: 168
  - file: 2025-11-04-workspace-settings.image-221ca2612779.webp
    width: 640
    height: 336
  - file: 2025-11-04-workspace-settings.image-e1b29d589df2.webp
    width: 960
    height: 504
  - file: 2025-11-04-workspace-settings.image-08b84847e11b.webp
    width: 1200
    height: 630
  color: '#221b16'
---

The Amp CLI now supports workspace-specific settings via `.amp/settings.json` files in your project directory.

This allows you to configure the Amp CLI differently for each project, with settings like permissions, MCP servers, and other preferences automatically picked up when you run `amp` in that workspace.

For example, here's how you can configure the Amp CLI to use a specific MCP server in a given project:

```
$ cd my-project
$ amp mcp add --workspace playwright -- npx -y @playwright/mcp@latest
```

Due to the `--workspace` flag, the setting was added in the `.amp/settings.json` file in `my-project`:

```
$ cat .amp/settings.json
{
  "amp.mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "-y",
        "@playwright/mcp@latest"
      ]
    }
  }
}
```

When you then run `amp` in `my-project`, the settings in `.amp/settings.json` will be merged with your global Amp settings.

See the [manual](https://ampcode.com/manual#configuration) for details on how settings are resolved.
