---
title: OpenAI Codex CLI, how does it work?
link: https://www.philschmid.de/openai-codex-cli
source: philschmid-de
published: 2025-04-17T00:00:00Z
updated: 2025-04-17T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: I used Gemini 2.5 Pro to better understand the OpenAI Codex CLI, a tool that allows you to interact with an AI model directly in your terminal to perform coding tasks.
content: extracted
html: 2025-04-17-openai-codex-cli-how-does-it-work.html
preview:
  file: 2025-04-17-openai-codex-cli-how-does-it-work.preview-129ead9492b2.webp
  width: 256
  height: 136
  alt: OpenAI Codex CLI, how does it work?
  color: '#101010'
images:
- source: https://www.philschmid.de/static/blog/openai-codex-cli/thumbnail.jpg
  original:
    file: 2025-04-17-openai-codex-cli-how-does-it-work.image-88810821e820.jpg
    width: 1700
    height: 900
  color: '#070707'
---

OpenAI Codex is a open source CLI released with OpenAI o3/o4-mini to be a "chat-driven development" tool. It allows developers to use AI models via API directly in their terminal to perform coding tasks. Unlike a simple chatbot, it can read files, write files (via patches), execute shell commands (often sandboxed), and iterate based on the results and user feedback.

*Note: This overview was generated with Gemini 2.5 Pro and updated collaboratively iterated on with Gemini 2.5 Pro and myself.*

## Core Components & Workflow

### User Interface (UI)

- The interactive terminal UI is built using `ink` and `react`, offering a richer experience than plain text. Key components reside in [`src/components/`](https://github.com/openai/codex/tree/main/codex-cli/src/components/), particularly within [`src/components/chat/`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/).
- The application entry point is [`src/cli.tsx`](https://github.com/openai/codex/tree/main/codex-cli/src/cli.tsx) (using `meow` for argument parsing), which sets up the main [`TerminalChat`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-chat.tsx) component via [`src/app.tsx`](https://github.com/openai/codex/tree/main/codex-cli/src/app.tsx).
- [`TerminalChat`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-chat.tsx) manages the overall display including history, input prompts, loading states, and overlays.
- User input is handled by [`TerminalChatInput`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-chat-input.tsx) (or [`TerminalChatNewInput`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-chat-new-input.tsx)), supporting command history and slash commands.
- The conversation history is displayed by [`TerminalMessageHistory`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-message-history.tsx), using components like [`TerminalChatResponseItem`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-chat-response-item.tsx) to render different message types.

### Agent Loop

- The core logic resides in [`src/utils/agent/agent-loop.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/agent-loop.ts).
- The `AgentLoop` class manages the interaction cycle with the OpenAI API.
- It takes the user's input, combines it with conversation history and instructions, and sends it to the model.
- It uses the `openai` Node.js library (v4+) and specifically calls `openai.responses.create`, indicating use of the `/responses` endpoint which supports streaming and tool use.

### Model Interaction

- The `AgentLoop` sends the context (history, instructions, user input) to the specified model (default `o4-mini`, configurable via `--model` or config file).
- It requests a *streaming* response.
- It handles different response item types (`message`, `function_call`, `function_call_output`, `reasoning`).
- [`src/utils/model-utils.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/model-utils.ts) handles fetching available models and checking compatibility.

### Tools & Execution

- The primary "tool" defined is `shell` (or `container.exec`), allowing the model to request shell command execution. See the `tools` array in [`src/utils/agent/agent-loop.ts`](https://github.com/openai/codex/blob/b0ccca555685b1534a0028cb7bfdcad8fe2e477a/codex-cli/src/utils/agent/agent-loop.ts#L512).

#### Command Execution

- When the model emits a `function_call` for `shell`, the `AgentLoop` invokes `handleExecCommand` ([`src/utils/agent/handle-exec-command.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/handle-exec-command.ts)).
- This function checks the approval policy (`suggest`, `auto-edit`, `full-auto`).
- [`src/approvals.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/approvals.ts) (`canAutoApprove`) determines if the command is known-safe or needs user confirmation based on the policy.
- If confirmation is needed, the UI ([`TerminalChatCommandReview`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-chat-command-review.tsx)) prompts the user.
- If approved (or auto-approved), the command is executed via [`src/utils/agent/exec.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/exec.ts).

#### Sandboxing

- The execution logic in `handleExecCommand` decides *how* to run the command based on the approval policy and safety assessment.
- `full-auto` mode implies sandboxing.
- [`src/utils/agent/sandbox/`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/sandbox/) contains the sandboxing implementations:
  - [`macos-seatbelt.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/sandbox/macos-seatbelt.ts): Uses macOS's `sandbox-exec` to restrict file system access and block network calls (`READ_ONLY_SEATBELT_POLICY`). Writable paths are whitelisted.
  - [`raw-exec.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/sandbox/raw-exec.ts): Executes commands directly without sandboxing (used when sandboxing isn't needed or available).
  - Linux: The [`README.md`](https://github.com/openai/codex/blob/main/codex-cli/README.md), [`Dockerfile`](https://github.com/openai/codex/blob/main/codex-cli/Dockerfile), and [`scripts/`](https://github.com/openai/codex/tree/main/codex-cli/scripts/) indicate a Docker-based approach. The CLI runs inside a minimal container where [`scripts/init_firewall.sh`](https://github.com/openai/codex/tree/main/codex-cli/scripts/init_firewall.sh) uses `iptables`/`ipset` to restrict network access only to the OpenAI API. The user's project directory is mounted into the container.

#### File Patching (`apply_patch`)

- The model is instructed (via the system prompt in [`src/utils/agent/agent-loop.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/agent-loop.ts)) to use a specific format like `{"cmd":["apply_patch","*** Begin Patch..."]}` when it wants to edit files.
- `handleExecCommand` detects this pattern.
- Instead of running `apply_patch` as a shell command, it uses `execApplyPatch` ([`src/utils/agent/exec.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/exec.ts)), which calls `process_patch` from [`src/utils/agent/apply-patch.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/apply-patch.ts).
- [`src/utils/agent/apply-patch.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/apply-patch.ts) parses the patch format and uses Node.js `fs` calls to modify files directly on the host system (or within the container on Linux).
- [`parse-apply-patch.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/parse-apply-patch.ts) (likely used by the UI) helps render diffs for user review.

### Prompts & Context Awareness

- **System Prompt:** A long, detailed system prompt is hardcoded as `prefix` within [`src/utils/agent/agent-loop.ts`](https://github.com/openai/codex/blob/b0ccca555685b1534a0028cb7bfdcad8fe2e477a/codex-cli/src/utils/agent/agent-loop.ts#L1021). This tells the model about its role as the Codex CLI, its capabilities (shell, patching), constraints (sandboxing), and coding guidelines.
- **User Instructions:** Instructions are gathered from both global (`~/.codex/instructions.md`) and project-specific (`codex.md` or similar, discovered via logic in [`src/utils/config.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/config.ts)) files. These combined instructions are prepended to the conversation history sent to the model.
- **Conversation History:** The `items` array (containing `ResponseItem` objects like user messages, assistant messages, tool calls, tool outputs) is passed back to the model on each turn, providing conversational context. [`src/utils/approximate-tokens-used.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/approximate-tokens-used.ts) estimates context window usage.
- **File Context (Standard Mode):** The agent doesn't automatically read project files. It gains file context only when the model explicitly requests to read a file (e.g., via `cat`) or when file content appears in the output of a previous command (e.g., `git diff`).
- **File Context (Experimental `--full-context` Mode):** This mode utilizes a distinct flow (see [`src/cli_singlepass.tsx`](https://github.com/openai/codex/tree/main/codex-cli/src/cli_singlepass.tsx), [`src/utils/singlepass/`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/singlepass/)). It involves:
  - Walking the directory, reading, and caching files via [`src/utils/singlepass/context_files.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/singlepass/context_files.ts).
  - Formatting the prompt, directory structure, and file contents into a single large request using [`src/utils/singlepass/context.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/singlepass/context.ts).
  - Expecting the model to return all file changes (creations, updates, deletes, moves) in a specific Zod schema defined in [`src/utils/singlepass/file_ops.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/singlepass/file_ops.ts).
- **Configuration:** Stores default model, approval mode settings, etc. Managed by [`src/utils/config.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/config.ts), loads from `~/.codex/config.yaml` (or `.yml`/`.json`) (not in repo).

## Step-by-Step Manual Walkthrough (Simulating the CLI)

Let's imagine the user runs: `codex "Refactor utils.ts to use arrow functions"` in a directory `/home/user/myproject`.

1.  **Initialization ([`cli.tsx`](https://github.com/openai/codex/tree/main/codex-cli/src/cli.tsx), [`app.tsx`](https://github.com/openai/codex/tree/main/codex-cli/src/app.tsx)):**

    - Parse arguments: Prompt is "Refactor...", model is default (`o4-mini`), approval mode is default (`suggest`).
    - Load config (`loadConfig` in [`src/utils/config.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/config.ts)): Read `~/.codex/config.yaml` and `~/.codex/instructions.md`.
    - Discover and load project docs (`loadProjectDoc` in [`src/utils/config.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/config.ts)): Find `/home/user/myproject/codex.md` and read its content.
    - Combine instructions: Merge user instructions and project docs.
    - Check Git status (`checkInGit` in [`src/utils/check-in-git.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/check-in-git.ts)): Confirm `/home/user/myproject` is a Git repo.
    - Render the main UI ([`TerminalChat`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-chat.tsx)).
2.  **First API Call (`AgentLoop.run` in [`src/utils/agent/agent-loop.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/agent-loop.ts)):**

    - Create initial input: `[{ role: "user", content: [{ type: "input_text", text: "Refactor..." }] }]`.
    - Construct API request payload: Include system prompt (from `prefix`), combined instructions, and the user input message. Set `model: "o4-mini"`, `stream: true`, `tools: [...]`. No `previous_response_id`.
    - Send request: Call `openai.responses.create(...)` (using the `openai` library). UI shows "Thinking...".
3.  **Model Response (Stream):**

    - Assume the model decides it needs to read the file first.
    - Stream event 1: `response.output_item.done` with `item: { type: "function_call", name: "shell", arguments: '{"cmd": ["cat", "utils.ts"]}', call_id: "call_1" }`
    - Stream event 2: `response.completed` with `output: [...]` containing the same function call, `id: "resp_1"`.
    - Agent receives the function call. `onLastResponseId` is called with `"resp_1"`.
4.  **Tool Call Handling (`handleExecCommand` in [`src/utils/agent/handle-exec-command.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/handle-exec-command.ts)):**

    - Parse arguments: `cmd = ["cat", "utils.ts"]`.
    - Check approval: `canAutoApprove(["cat", "utils.ts"], "suggest", ["/home/user/myproject"])` (in [`src/approvals.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/approvals.ts)) -> returns `{ type: "auto-approve", reason: "View file contents", group: "Reading files", runInSandbox: false }`.
    - Execute command (`execCommand` in [`src/utils/agent/handle-exec-command.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/handle-exec-command.ts)): Run `cat utils.ts` directly (no sandbox needed for safe commands). *Note: Assuming `utils.ts` exists at the root for this example; in reality, the model might need to specify a path like `src/utils.ts`.*
    - Simulate result: `stdout = "/* content of utils.ts */", stderr = "", exitCode = 0`.
5.  **Second API Call (`AgentLoop.run` continues):**

    - Format tool result: Create a `function_call_output` item like `{ type: "function_call_output", call_id: "call_1", output: '{"output": "/* content ... */", "metadata": {"exit_code": 0, ...}}' }`.
    - Construct API request payload: Include system prompt, combined instructions, the *entire history so far* (user message, assistant function call request, function call output), set `previous_response_id: "resp_1"`.
    - Send request. UI shows "Thinking...".
6.  **Model Response (Stream):**

    - Assume model generates the refactored code and decides to apply it.
    - Stream event 1: `response.output_item.done` with `item: { type: "function_call", name: "shell", arguments: '{"cmd": ["apply_patch", "*** Begin Patch\n*** Update File: utils.ts\n@@ ... -old +new ...\n*** End Patch"]}', call_id: "call_2" }`.
    - Stream event 2: `response.completed` with `output: [...]` containing the patch function call, `id: "resp_2"`.
    - Agent receives the patch function call. `onLastResponseId` is called with `"resp_2"`.
7.  **Tool Call Handling (Patch):**

    - Parse arguments: Identify `apply_patch` and extract the patch text.
    - Check approval: `canAutoApprove(["apply_patch", "..."], "suggest", ["/home/user/myproject"])`. Since policy is `suggest`, this returns `{ type: "ask-user", applyPatch: { patch: "..." } }`.
    - Request confirmation (`requestConfirmation`): The UI ([`TerminalChatCommandReview`](https://github.com/openai/codex/tree/main/codex-cli/src/components/chat/terminal-chat-command-review.tsx)) displays the patch diff and asks "Allow command? \[y/N/e/a\]".
    - User reviews and presses 'y'. `submitConfirmation` is called with `{ decision: ReviewDecision.YES }`.
    - Execute patch (`execApplyPatch` in [`src/utils/agent/exec.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/exec.ts) -> `process_patch` in [`src/utils/agent/apply-patch.ts`](https://github.com/openai/codex/tree/main/codex-cli/src/utils/agent/apply-patch.ts)): Reads `utils.ts`, applies the diff logic, and writes the modified content back using Node.js `fs.writeFileSync`.
    - Simulate result: `stdout = "Done!", stderr = "", exitCode = 0`.
8.  **Third API Call:**

    - Format tool result: Create `function_call_output` item for the patch, `{ call_id: "call_2", output: '{"output": "Done!", ...}' }`.
    - Construct API request: Include history + patch result, `previous_response_id: "resp_2"`.
    - Send request.
9.  **Model Response (Final):**

    - Assume model confirms the refactoring is done.
    - Stream event 1: `response.output_item.done` with `item: { type: "message", role: "assistant", content: [{ type: "output_text", text: "OK, I've refactored utils.ts to use arrow functions." }] }`.
    - Stream event 2: `response.completed`, `id: "resp_3"`.
    - Agent receives the message. `onLastResponseId` called with `"resp_3"`.
    - No more tool calls. The loop finishes for this turn. UI stops showing "Thinking...".
10. **User Interaction:**

    - The user sees the final message and the updated prompt, ready for the next command. The file `utils.ts` on their disk has been modified.
