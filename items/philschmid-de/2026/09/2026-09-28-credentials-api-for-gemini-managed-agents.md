---
title: Credentials API for Gemini Managed Agents
link: https://www.philschmid.de/managed-agents-credentials
source: philschmid-de
published: 2026-09-28T00:00:00Z
updated: 2026-09-28T00:00:00Z
first_seen: 2026-09-28T15:21:52.698000543Z
summary: Store secrets once with the Credentials API, reference them by ID, and let the egress proxy inject them on the wire so they never enter the agent sandbox.
content: extracted
html: 2026-09-28-credentials-api-for-gemini-managed-agents.html
preview:
  file: 2026-09-28-credentials-api-for-gemini-managed-agents.preview-67b3c0c77ff0.webp
  width: 256
  height: 107
  alt: Credentials API for Gemini Managed Agents
  color: '#f3f3f1'
images:
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/credentials_flow.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-e996af90e2df.png
    width: 4320
    height: 1798
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-95eb24c66aaf.webp
    width: 320
    height: 133
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-e7e4a548943a.webp
    width: 640
    height: 266
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-1c41c5101c41.webp
    width: 960
    height: 400
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-0c4c65329e9e.webp
    width: 1280
    height: 533
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-9e27c4dfedb4.webp
    width: 1600
    height: 666
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-5b6585a3b5e0.webp
    width: 4320
    height: 1798
  color: '#f6f6f4'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/credentials_flow_dark.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-379d9e4aeb8c.png
    width: 4320
    height: 1798
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-d69c013e2a38.webp
    width: 320
    height: 133
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-50862d343293.webp
    width: 640
    height: 266
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-cf11180e979b.webp
    width: 960
    height: 400
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-4c1b25197538.webp
    width: 1280
    height: 533
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-abbb59d6e87a.webp
    width: 1600
    height: 666
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-a046eee8f151.webp
    width: 4320
    height: 1798
  color: '#121317'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/step1_store_once.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-9a0c1935caf4.png
    width: 2960
    height: 1278
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-a16f019a1377.webp
    width: 320
    height: 138
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-a08e064c36c5.webp
    width: 640
    height: 276
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-93709db1d27f.webp
    width: 960
    height: 414
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-805a1d23cd98.webp
    width: 1280
    height: 553
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-f0e955fb498d.webp
    width: 1600
    height: 691
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-15c70a51db52.webp
    width: 2960
    height: 1278
  color: '#f8f8f6'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/step1_store_once_dark.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-3ef199e8b283.png
    width: 2960
    height: 1278
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-dd77f1479efb.webp
    width: 320
    height: 138
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-89835b8d2deb.webp
    width: 640
    height: 276
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-c803d40e80d4.webp
    width: 960
    height: 414
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-ecbd42f595dc.webp
    width: 1280
    height: 553
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-d8f14e2721b8.webp
    width: 1600
    height: 691
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-a6822096fac0.webp
    width: 2960
    height: 1278
  color: '#131418'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/step2_reference_by_id.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-a92d0aa8ccde.png
    width: 4080
    height: 1202
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-968cc4be034b.webp
    width: 320
    height: 94
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-cbc9dd1b28ee.webp
    width: 640
    height: 189
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-3395f8188bde.webp
    width: 960
    height: 283
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-7d881b76a650.webp
    width: 1280
    height: 377
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-8693e335b763.webp
    width: 1600
    height: 471
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-4613a768fd63.webp
    width: 4080
    height: 1202
  color: '#f6f6f4'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/step2_reference_by_id_dark.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-20564bf9d610.png
    width: 4080
    height: 1202
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-9afbb8d4a367.webp
    width: 320
    height: 94
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-bfecd5d39b0e.webp
    width: 640
    height: 189
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-0eb9c12df5c9.webp
    width: 960
    height: 283
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-db61e452d15a.webp
    width: 1280
    height: 377
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-6e6cb91da691.webp
    width: 1600
    height: 471
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-f0a03bc54739.webp
    width: 4080
    height: 1202
  color: '#121317'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/step3_injected_on_wire.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-61b6dd454f60.png
    width: 3200
    height: 1238
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-1c59606f4abd.webp
    width: 320
    height: 124
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-e76243f307ec.webp
    width: 640
    height: 248
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-5dc4a8d46c7d.webp
    width: 960
    height: 371
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-404910237018.webp
    width: 1280
    height: 495
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-aa424153b277.webp
    width: 1600
    height: 619
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-112b3a23fb71.webp
    width: 3200
    height: 1238
  color: '#f8f8f7'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/step3_injected_on_wire_dark.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-c3211e3e4b32.png
    width: 3200
    height: 1238
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-57a5c61c1af0.webp
    width: 320
    height: 124
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-82f335915311.webp
    width: 640
    height: 248
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-f85c5508498b.webp
    width: 960
    height: 371
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-0eb5378aa0c0.webp
    width: 1280
    height: 495
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-32546ddf20cf.webp
    width: 1600
    height: 619
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-7fcfe893fbd1.webp
    width: 3200
    height: 1238
  color: '#141518'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/credentials_mcp_allowlist_architecture.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-ba38347c0938.png
    width: 4320
    height: 1798
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-bf2fdc5df7a3.webp
    width: 320
    height: 133
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-d4f8319ece10.webp
    width: 640
    height: 266
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-dbfd32034afa.webp
    width: 960
    height: 400
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-55571edc7618.webp
    width: 1280
    height: 533
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-60909e516189.webp
    width: 1600
    height: 666
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-d002a29f601d.webp
    width: 4320
    height: 1798
  color: '#f6f6f4'
- source: https://www.philschmid.de/static/blog/managed-agents-credentials/credentials_mcp_allowlist_architecture_dark.png
  original:
    file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-fb36bbaa454d.png
    width: 4320
    height: 1798
  variants:
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-d91c7c9fcc05.webp
    width: 320
    height: 133
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-5e6e0e51dd19.webp
    width: 640
    height: 266
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-cc674e06219a.webp
    width: 960
    height: 400
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-93c8600d0aee.webp
    width: 1280
    height: 533
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-e4ac562c365f.webp
    width: 1600
    height: 666
  - file: 2026-09-28-credentials-api-for-gemini-managed-agents.image-073bd31c60b4.webp
    width: 4320
    height: 1798
  color: '#121317'
---

We recently shipped the [Credentials API](https://aistudio.google.com/docs/agent-credentials) for [Managed Agents](https://aistudio.google.com/docs/agents) ([`antigravity-preview-09-2026`](https://aistudio.google.com/docs/antigravity-agent)). It lets your agent authenticate with external services like GitHub, Notion, or the Gemini API without putting raw secrets inside the [Linux sandbox](https://aistudio.google.com/docs/agent-environment).

![Credentials API end-to-end flow](https://www.philschmid.de/static/blog/managed-agents-credentials/credentials_flow.png)![](https://www.philschmid.de/static/blog/managed-agents-credentials/credentials_flow_dark.png)

If you pass an API key as a normal environment variable, any dependency or script inside the sandbox can read `os.environ` and leak it. With the Credentials API, the secret stays on the server and is injected on the wire by the egress proxy.

## How it works

### 1\. Store the secret once

Register a credential with [`credentials.create()`](https://aistudio.google.com/docs/agent-credentials#create-a-credential). Secrets are write-only and encrypted at rest; API responses only return metadata (`id`, `type`, `status`, timestamps).

![Step 1: Store the secret once](https://www.philschmid.de/static/blog/managed-agents-credentials/step1_store_once.png)![](https://www.philschmid.de/static/blog/managed-agents-credentials/step1_store_once_dark.png)

There are [3 credential types](https://aistudio.google.com/docs/agent-credentials#credential-types):

- **[`bearer_token`](https://aistudio.google.com/docs/agent-credentials#bearer-token)**: Static tokens and API keys injected as HTTP headers (`Authorization: Bearer <token>` by default, or custom `header_name` and `prefix`).
- **[`oauth2`](https://aistudio.google.com/docs/agent-credentials#oauth2)**: Refresh tokens (`client_id`, `client_secret`, `refresh_token`, `token_url`). Gemini exchanges and refreshes access tokens automatically before expiry.
- **[`environment_variable`](https://aistudio.google.com/docs/agent-credentials#environment-variable)**: For CLIs and SDKs that read secrets from the process environment. The container receives a placeholder (`__GEMINI_CRED_<id>__`), and the egress proxy swaps in the real value only for requests to `trusted_domains`.

### 2\. Reference it by ID

Pass the credential `id` when defining a reusable agent with [`agents.create()`](https://aistudio.google.com/docs/custom-agents#create-a-managed-agent) (or per run in [`interactions.create()`](https://aistudio.google.com/docs/interactions)) on a remote [`mcp_server`](https://aistudio.google.com/docs/agent-credentials#use-credentials-with-mcp-servers) tool, a domain in [`network.allowlist`](https://aistudio.google.com/docs/agent-credentials#use-credentials-in-the-network-allowlist), or a variable in [`base_environment.env`](https://aistudio.google.com/docs/agent-credentials#use-credentials-as-environment-variables).

![Step 2: Reference it by ID](https://www.philschmid.de/static/blog/managed-agents-credentials/step2_reference_by_id.png)![](https://www.philschmid.de/static/blog/managed-agents-credentials/step2_reference_by_id_dark.png)

### 3\. Injected on the wire

When the agent calls an [MCP tool](https://aistudio.google.com/docs/antigravity-agent#mcp-servers) or makes an outbound HTTP request from the [sandbox](https://aistudio.google.com/docs/agent-environment#network-configuration), the egress proxy checks the target domain and injects the real credential on the wire.

![Step 3: Injected on the wire](https://www.philschmid.de/static/blog/managed-agents-credentials/step3_injected_on_wire.png)![](https://www.philschmid.de/static/blog/managed-agents-credentials/step3_injected_on_wire_dark.png)

## Example Reviewing and smoke-testing GitHub PRs

Here is a practical examples on a reusable [custom agent](https://aistudio.google.com/docs/custom-agents) that manages [`google-gemini/cookbook`](https://github.com/google-gemini/cookbook) via the Github MCP server and can make securte Gemini API calls. The `GEMINI_API_KEY` is bound as a credential, untrusted code in the sandbox can call `generativelanguage.googleapis.com` without ever seeing the real API key.

![Architecture diagram: GitHub MCP bearer_token plus sandbox GEMINI_API_KEY environment_variable](https://www.philschmid.de/static/blog/managed-agents-credentials/credentials_mcp_allowlist_architecture.png)![](https://www.philschmid.de/static/blog/managed-agents-credentials/credentials_mcp_allowlist_architecture_dark.png)

### Create a read-only GitHub token

1. Open [GitHub Fine-grained Personal Access Tokens](https://github.com/settings/personal-access-tokens) and click **Generate new token**.
2. Scope **Repository access** to your target repositories and set **Pull requests** and **Contents** permissions to *Read-only*.
3. Export `GITHUB_PAT` and [`GEMINI_API_KEY`](https://aistudio.google.com/docs/interactions/api-key) locally.

### Run the agent

Install the SDK (`bun add @google/genai`) and run:

TypeScript

```typescript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({});
 
// 1. Store credentials once
const github = await ai.credentials.create({
  id: "github-mcp",
  type: "bearer_token",
  token: process.env.GITHUB_PAT!,
});
 
const geminiKey = await ai.credentials.create({
  id: "gemini-key",
  type: "environment_variable",
  value: process.env.GEMINI_API_KEY!,
  injection_location: ["header", "query"],
  trusted_domains: ["generativelanguage.googleapis.com"],
});
 
// 2. Define a reusable agent wired to both credentials
const agent = await ai.agents.create({
  id: "cookbook-pr-reviewer",
  base_agent: "antigravity-preview-09-2026",
  system_instruction:
    "You are a maintainer for google-gemini/cookbook. Use the github tools to inspect pull requests, and use the sandbox to smoke-test Gemini API snippets and draft release notes.",
  tools: [
    { type: "code_execution" },
    {
      type: "mcp_server",
      name: "github",
      url: "https://api.githubcopilot.com/mcp/",
      credential: github.id,
    },
  ],
  base_environment: {
    type: "remote",
    env: {
      GEMINI_API_KEY: { credential: geminiKey.id },
    },
  },
});
 
// 3. Run the agent
const interaction = await ai.interactions.create(
  {
    agent: agent.id,
    environment: "remote",
    input:
      "Draft a release note for the 3 most recently merged PRs in google-gemini/cookbook, and run a live smoke test against gemini-3.8-flash in the sandbox to verify the code snippet works.",
  },
  { timeout: 300000 },
);
 
console.log(interaction.output_text);
```

Output:

Markdown

```markdown
# Release Notes: `google-gemini/cookbook`
 
### 1. [PR #1347](https://github.com/google-gemini/cookbook/pull/1347) - Zero-Cloud Local Hybrid RAG Recipe with SQLite FTS5 & Gemini
- **Author:** @Cagrik34 | **Merged At:** 2026-09-28 10:49:52 UTC
- **Summary:** Combines SQLite FTS5 (`unicode61`) BM25 search with `text-embedding-004` dense vectors and Reciprocal Rank Fusion (RRF, k=60) in `examples/Zero_Cloud_Hybrid_RAG_SQLite_FTS5_Gemini.ipynb`.
 
### 2. [PR #1383](https://github.com/google-gemini/cookbook/pull/1383) - Fix `musicGenerationConfig` Schema in Lyria RealTime Notebook
- **Author:** @kkorpal | **Merged At:** 2026-09-28 10:29:00 UTC
- **Summary:** Updates `music_generation_config` to `musicGenerationConfig` and flattens the payload in `quickstarts/Get_started_LyriaRealTime.ipynb`.
 
### 3. [PR #1384](https://github.com/google-gemini/cookbook/pull/1384) - Fix HTTP 400 BadRequest via File API in Prompting Notebook
- **Author:** @kkorpal | **Merged At:** 2026-09-25 12:29:48 UTC
- **Summary:** Switches multimodal image upload in `quickstarts/Prompting.ipynb` to `client.files.upload`.
```

What happens during the run:

1. **GitHub MCP (`github-mcp`)**: Connects to `api.githubcopilot.com/mcp/` and calls `github:list_pull_requests` and `github:pull_request_read` to inspect the latest merged PRs (`#1347`, `#1383`, `#1384`).
2. **Sandbox execution (`gemini-key`)**: Inside the container, `$GEMINI_API_KEY` is `__GEMINI_CRED_gemini-key__`. When the smoke test calls `generativelanguage.googleapis.com`, the egress proxy swaps the placeholder for the real key on the wire.
3. **Domain restriction**: Any request carrying `__GEMINI_CRED_gemini-key__` to a domain outside `trusted_domains` is blocked (`403`).

### Rotate or delete credentials

[Update a credential](https://aistudio.google.com/docs/agent-credentials#rotate-a-credential) by ID to rotate its secret without changing your agent definition, or [delete it](https://aistudio.google.com/docs/agent-credentials#delete-a-credential) when it is no longer needed:

TypeScript

```typescript
await ai.credentials.update("github-mcp", {
  type: "bearer_token",
  token: process.env.GITHUB_PAT_NEW!,
});
 
await ai.credentials.delete("github-mcp");
await ai.credentials.delete("gemini-key");
```

See the [Credentials API docs](https://aistudio.google.com/docs/agent-credentials), [Custom Agents guide](https://aistudio.google.com/docs/custom-agents), and [Managed Agents Quickstart](https://aistudio.google.com/docs/managed-agents-quickstart) for full details.

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
