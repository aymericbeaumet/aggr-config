---
title: 'Drawgent: Coding agent on a live Excalidraw canvas'
link: https://tangled.org/yanndegat.tngl.sh/drawgent
source: hnrss-org-frontpage
published: 2026-09-26T15:56:34Z
updated: 2026-09-26T15:56:34Z
first_seen: 2026-09-27T10:43:41.928516245Z
authors:
- parasitid
summary: 'Article URL: https://tangled.org/yanndegat.tngl.sh/drawgent Comments URL: https://news.ycombinator.com/item?id=49857729 Points: 153 # Comments: 41'
content: extracted
html: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.html
preview:
  file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.preview-7d910c6572cc.webp
  width: 256
  height: 134
  alt: yanndegat.tngl.sh/drawgent
  color: '#eae4df'
images:
- source: https://tangled.org/yanndegat.tngl.sh/drawgent/opengraph
  original:
    file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-1a826d776cfc.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-c57c3a2f920f.webp
    width: 320
    height: 168
  - file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-6a261564d92a.webp
    width: 640
    height: 336
  - file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-9b359b438697.webp
    width: 1200
    height: 630
  color: '#fefefe'
- source: https://tangled.org/yanndegat.tngl.sh/drawgent/raw/main/docs/architecture.png
  original:
    file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-9f38a7f88040.png
    width: 1600
    height: 1326
  variants:
  - file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-b9528077b837.webp
    width: 320
    height: 265
  - file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-e23726c7189f.webp
    width: 640
    height: 530
  - file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-17628af48cc2.webp
    width: 960
    height: 796
  - file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-faf7320fb781.webp
    width: 1280
    height: 1061
  - file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-1ef5c2ff1ede.webp
    width: 1600
    height: 1326
  color: '#fefefe'
- source: https://tangled.org/yanndegat.tngl.sh/drawgent/raw/main/docs/drawgentdemo.gif
  original:
    file: 2026-09-26-drawgent-coding-agent-on-a-live-excalidraw-canvas.image-573bdfb97741.gif
    width: 960
    height: 540
  color: '#1d1b2d'
---

Rust 77.0%

JavaScript 18.0%

CSS 2.1%

Makefile 1.4%

Nix 0.6%

Dockerfile 0.5%

HTML 0.3%

[13](https://tangled.org/yanndegat.tngl.sh/drawgent/commits/main) [1](https://tangled.org/yanndegat.tngl.sh/drawgent/branches) [0](https://tangled.org/yanndegat.tngl.sh/drawgent/tags)

### Clone this repository

Use permalink

HTTPS

https://tangled.org/yanndegat.tngl.sh/drawgent https://tangled.org/did:plc:3zklierfkckthy6sodbm4af3

SSH

git@tangled.org:yanndegat.tngl.sh/drawgent git@tangled.org:did:plc:3zklierfkckthy6sodbm4af3

For self-hosted knots, clone URLs may differ based on your setup.

[Download tar.gz](https://tangled.org/yanndegat.tngl.sh/drawgent/archive/main.tar.gz) [Download .zip](https://tangled.org/yanndegat.tngl.sh/drawgent/archive/main.zip)

 README.md

## drawgent: your coding agent on a live Excalidraw canvas

drawgent connects **your own** Claude Code, Codex or opencode (your install, login, config and repo) to an Excalidraw whiteboard. Ask for a diagram in the chat panel, or write `AGENT: …` next to the part of a drawing you want changed. The agent looks at the canvas (screenshot + scene), edits it live, checks the result, and marks the note `DONE`.

## How it works

![drawgent architecture: the browser, the drawgent server, your coding agent and its MCP canvas tools](https://tangled.org/yanndegat.tngl.sh/drawgent/raw/main/docs/architecture.png)

## Quick start

Prerequisite: one of `claude`, `codex` or `opencode` installed and logged in. The Claude and Codex bridges also need Node.js ≥ 18 (npm).

```
drawgent setup claude        # once per agent: claude | codex | opencode
cd ~/my-repo
drawgent up                  # new agent session in this repo + canvas in your browser
drawgent up --attach         # or: pick one of your running sessions and connect the canvas to it
```

### Demo

![drawgent demo: the agent drawing on a live Excalidraw canvas](https://tangled.org/yanndegat.tngl.sh/drawgent/raw/main/docs/drawgentdemo.gif)

### `drawgent setup <agent>`

Checks everything once, fails with the exact fix when something is missing, and writes `~/.config/drawgent/config.toml`:

1. **The agent CLI.** Your `claude` / `codex` / `opencode` on PATH.

2. **Login.** `claude auth status`, `codex login status` or `opencode auth list`.

3. **ACP bridge.**

   - opencode speaks ACP itself (`opencode acp`).
   - Claude Code and Codex use the official ACP adapters. They are installed once into `~/.cache/drawgent/adapters` (~60 MB), *without* their bundled agent binaries, and pointed at **your** CLI (`CLAUDE_CODE_EXECUTABLE`, `CODEX_PATH`).
   - Setup then verifies the ACP handshake.
4. **Canvas tools for attached sessions.** Only Codex needs a change: `codex mcp add drawgent -- drawgent mcp`. Claude and opencode get the tools at attach time.

5. **Headless Chrome for the renderer.** Setup uses your Chrome/Chromium if you have one. Otherwise it proposes:

   - downloading **Chrome Headless Shell** (Chrome for Testing, ~120 MB, no sudo) into `~/.cache/drawgent/chrome`, and telling you exactly which system libraries are missing, if any;
   - installing Chromium with your package manager (`apt`, `snap`, `dnf`, `pacman`, `zypper`, `apk`, `brew`, or `nix` without sudo).

   Non-interactive: `--chrome download | system | /path/to/chrome`.

`drawgent up` refuses to start until setup succeeded for that agent, or if something setup recorded disappeared.

### `drawgent up`

Runs in the current directory (the workspace):

- starts the editor + API on `127.0.0.1:7300` (next free port if taken);
- starts a new session of your agent over ACP, working in the workspace;
- opens your browser. On a headless box it prints the `ssh -L` command instead.

The live scene and its sync state are kept in `.drawgent/scene.json`, which is git-ignored automatically.

### Keeping the diagram in the repo: `--diagram`

```
drawgent up --diagram docs/architecture.excalidraw   # remembered for this workspace
```

- **What's written:** a standard `.excalidraw` file (it opens on excalidraw.com) that follows the canvas. It holds only the live drawing (no deleted elements, no laser traces), pretty-printed and without per-edit sync fields, so it diffs cleanly and is rewritten only when the drawing changes. Commit it like any other source file.
- **Changes from git:** if the file changes outside drawgent (`git pull`, checkout, a hand edit), it wins. The canvas updates live, deletions included, and a file already on disk at start is applied too. A file that doesn't parse (e.g. merge-conflict markers) is reported in the chat panel and ignored until fixed.
- **Picking the file:** a `drawgent.excalidraw` at the workspace root is used automatically, so a fresh clone needs no flag.
- **Rooms:** `--diagram` can't be combined with `--room`, where excalidraw.com holds the drawing.

### `drawgent up --attach [id]`

Connects the canvas to a session you already run. Without an id it lists the sessions it finds (current directory first) and lets you pick one:

| agent       | discovery                                                                                           | how the canvas reaches it                                                                                                                                                                                               |
| ----------- | --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Claude Code | `claude agents --json` (interactive and background sessions)                                        | **fork**: a new session carrying the full conversation, driven by drawgent over ACP. Your terminal session is left untouched                                                                                            |
| opencode    | opencode servers listening locally: start the TUI with `opencode --port 4096` (or `opencode serve`) | **live**: messages go into your running session (you see them in your TUI); drawgent adds its MCP tools to that server at runtime; replies, tool calls and prompts you type in the TUI are mirrored into the chat panel |
| Codex       | sessions in `~/.codex/sessions`                                                                     | **live**: `codex queue --thread <id>`; replies are mirrored from the session's rollout file. The session needs the drawgent MCP (registered by setup, loaded when Codex starts)                                         |

## Using the canvas

- **Chat panel** (right side): send a request, watch replies and tool calls stream in, approve permission prompts, press **Stop** to cancel a turn.
- **On the canvas**: write a text starting with `AGENT:` next to or inside a shape, or draw an arrow from the note to a shape. It fires about 2.5 s after you stop typing, with its position, what it points at and what is nearby. The agent resolves it into a green `DONE: …` note. Edit it back to `AGENT:` to send it again.
- **Laser zones**: pick Excalidraw's laser (`K`) and circle or scribble over part of the diagram. The trace stays on the canvas as a red outline, and the chat panel opens ready for your instruction, with a chip listing what is under the zone (`🔴 Laser zone · API, Redis ×`). Your next message goes to the agent together with the zone (bounds and covered elements), and the trace disappears when the agent is done. Strokes within 4 s of each other form one zone; × discards it.
- Prompts are queued and run one at a time. A queued note that was already handled is skipped.

```
drawgent up --room 'https://excalidraw.com/#room=<id>,<key>'
```

- drawgent joins the room as a collaborator (`🤖 Agent`, with a cursor that follows its edits). Humans can stay on excalidraw.com: their `AGENT:` notes reach your agent, and its edits appear there live.
- Traffic is end-to-end encrypted with the room key. Empty rooms are loaded from and saved to excalidraw's Firestore storage.
- The local editor mirrors the room and still has the chat panel.

## Other commands

- `drawgent mcp`: stdio MCP server with the canvas tools. Agents launch it; it finds the running `drawgent up` by itself.
- `drawgent serve`: low-level server with explicit agents (`--agent claude,codex,opencode` as set up, or `name=command` for any ACP agent); for scripts and containers.
- Options for `up` / `serve`: `--port`, `--room`, `--token` (API bearer + `?token=` in the URL), `--permissions canvas|ask|all` (default `canvas`: drawing tools auto-approved, anything else asked in the chat panel), `--data`, `--settle-ms`.

## Optional: Docker

`docker compose up --build` runs a canvas server (drawgent + Chromium, **no agents**), e.g. to host a shared canvas or a room bridge on a server. Agents are never bundled: they always run with your own setup.

## Build from source

```
npm ci && npm run build      # editor + renderer pages -> dist/ (embedded into the binary)
cargo install --path .       # or: cargo build --release
```

Release builds (`nix develop -c make …` provides the toolchain, or rustup + cargo-zigbuild):

| command                            | output                                                                                   |
| ---------------------------------- | ---------------------------------------------------------------------------------------- |
| `make musl` / `make dist`          | static Linux x86\_64 binary / tarball (`MUSL_TARGET=aarch64-unknown-linux-musl` for ARM) |
| `make darwin` / `make dist-darwin` | macOS universal binary (Intel + Apple Silicon, macOS ≥ 13) / tarball                     |

- **How the macOS build is made:** it's cross-built from Linux with zig and no Apple SDK (drawgent links only `libSystem`, `libiconv` and `libcharset`).
- **Signing:** the arm64 slice is ad-hoc signed by the linker. It is not notarized, so after downloading run `xattr -d com.apple.quarantine drawgent` once.
- **Status:** not yet tested on a real Mac.
- **Small machines:** use `JOBS=1`. One dependency (`chromiumoxide_cdp`) needs a lot of memory to compile.

## Agent tools (MCP)

`get_scene`, `get_screenshot` (vision; zoom with `element_ids`), `add_elements` (Excalidraw skeletons; arrows bind by id and are routed edge-to-edge), `add_mermaid` (auto-layout), `update_elements` (labels follow shapes, bound arrows re-route), `delete_elements`, `clear_canvas`, `list_instructions`, `resolve_instruction`, `set_status`.

## API

`GET /api/health` · `GET /api/scene` · `GET /api/screenshot?ids=&padding=&max=` · `POST|PATCH|DELETE /api/elements` · `POST /api/mermaid` · `POST /api/clear` · `GET /api/instructions` · `POST /api/instructions/{id}/resolve` · `POST /api/status` · `POST /api/chat {agent?, text}` · `WS /ws` (browser sync + chat events)

## Tests

```
cargo test                          # fractional indices, room crypto/framing
node scripts/smoke.mjs [url]        # chat turn + AGENT: note against a running drawgent
node scripts/e2e-browser.mjs [url]  # real browser: chat panel + note typed on the canvas
node scripts/room-e2e.mjs           # fresh excalidraw.com room ↔ drawgent, both directions
node scripts/laser-e2e.mjs          # laser zone → chat → agent edits only that zone (needs setup claude)
node scripts/diagram-e2e.mjs        # --diagram file: clean output, git pull / hand edits flow back
```

## Layout

`src/` (Rust):

- `main.rs`: CLI.
- `setup.rs`, `config.rs`, `chrome.rs`: setup, config, renderer install.
- `attach.rs`: session discovery and picker.
- `agents.rs` + `acp.rs`: ACP driver (new / fork sessions).
- `live.rs`: live opencode / Codex drivers.
- `hub.rs`: routing, notes, chat log.
- `scene.rs`: store and edit operations.
- `renderer.rs`: Chrome over CDP.
- `mcp.rs`: MCP server.
- `room.rs`: excalidraw.com client.
- `fractional.rs`, `geometry.rs`, `el.rs`: helpers.

`web/`: editor (`main.jsx`, `chat.jsx`, `laser.js`) and renderer page (`render.jsx`).

## Limits

- The renderer needs Chrome (a native renderer is planned).
- Claude "attach" is a fork, because Claude Code has no public way to inject into a running terminal session.
- Codex live attach is implemented, but was not yet tested against a logged-in Codex.
- One scene (and one diagram file) per workspace. Images/files are not synced.
