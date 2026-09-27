---
title: 'MCP is Not the Problem, It''s your Server: Best Practices for Building MCP Servers'
link: https://www.philschmid.de/mcp-best-practices
source: philschmid-de
published: 2026-01-21T00:00:00Z
updated: 2026-01-21T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The Model Context Protocol (MCP) has exploded roughly 1 year ago, everyone rushed to build MCP servers. The hype was real. Yet, most MCP servers disappoint. Most developers blame the protocol. The protocol feels like it's dying on social media.
content: extracted
html: 2026-01-21-mcp-is-not-the-problem-it-s-your-server-best-practices-for.html
preview:
  file: 2026-01-21-mcp-is-not-the-problem-it-s-your-server-best-practices-for.preview-46022805eca3.webp
  width: 256
  height: 134
  alt: 'MCP is Not the Problem, It''s your Server: Best Practices for Building MCP Servers'
  color: '#0c0c0c'
images:
- source: https://www.philschmid.de/static/blog/mcp-best-practices/thumbnail.jpg
  original:
    file: 2026-01-21-mcp-is-not-the-problem-it-s-your-server-best-practices-for.image-093d5bc165c3.jpg
    width: 1500
    height: 785
  color: '#010101'
---

The Model Context Protocol (MCP) has exploded roughly 1 year ago, everyone rushed to build MCP servers. The hype was real. Yet, most MCP servers disappoint. Most developers blame the protocol. The protocol feels like it's dying on social media.

But enterprise adoption tells a different story. Companies are deploying MCP servers. The integrations are live. But the results? Disappointing. Why?

Developers treat MCP like a REST API wrapper. MCP is a User Interface for Agents. Different users, different design principles.

**The protocol works fine. The servers don't.**

This post breaks down why MCP servers fail, six best practices for building ones that work, and how Skills and MCP complement each other.

## What is MCP?

MCP (Model Context Protocol) provides a universal connection between LLMs and external tools, data sources, and services. Before MCP, every integration required custom connectors. MCP standardizes three primitives:

- **Tools**: Functions the agent can call (`search_documents`, `create_issue`)
- **Resources**: Data the agent can read (files, database records)
- **Prompts**: Pre-built workflows the user or agent can invoke

The idea behind MCP is to build your MCP server once, and use with any agent.

## What MCP is NOT

MCP servers are not thin wrappers around your existing API. A Good REST API is not a good MCP server. We assume just because LLMs are "smart", they can use APIs as a human would. Thats wrong.

REST API design principles advocate for composability, discoverability, flexibility, and stability. Small endpoints developers combine. Self-documenting schemas.

These principles work for human developers. They don't work for AI agents. MCP is a User Interface for AI agents. Different users, different design principles.

| REST API Principles | Developers                      | Agents                                |
| ------------------- | ------------------------------- | ------------------------------------- |
| **Discovery**       | Cheap (read docs once)          | Expensive (schema in every request)   |
| **Composability**   | Mix and match small endpoints   | Multi-step tool calls, slow iteration |
| **Flexibility**     | Many options > more flexibility | Complexity leads to hallucination     |

Additionally, MCP servers are not data dump services. Throwing large scale raw data at an agent bloats its context window.

**A Practical Example: Order Tracking**

You're building an agent that tracks orders.

As a human developer, you read the API docs once, write a script calling `GET /users`, `GET /orders`, `GET /shipments` in sequence, debug it, deploy it.

A bad MCP server exposes those three endpoints as tools. The agent must load all three tool description, make 3 round-trips and stores all intermediate results in conversation history.

A good MCP server exposes one tool, `track_order(email)`. The tool calls all three internally and returns "Order #12345 shipped via FedEx, arriving Thursday." Same outcome, one call, outcome oriented.

MCP is a User Interface for a non-human user. Same product thinking, different user.

## How to Build a Good MCP Server (Best Practices)

To fix your server, you need to curate the experience. Here are the six best practices for building a good MCP server.

### 1\. Outcomes, Not Operations

**Trap:** Converting REST endpoints 1:1 into MCP tools.

**Fix:** Design tools around what the user/agent wants to achieve.

Don't expose three separate tools for order status:

- `get_user_by_email()`
- `list_orders(user_id)`
- `get_order_status(order_id)`

This forces the agent to orchestrate three round-trips. Instead of three atomic tools, give the agent one high-level tool: `track_latest_order(email)`. Do the orchestration in your code, not in the LLM's context window.

### 2\. Flatten Your Arguments

**Trap:** Using complex nested dictionaries or configuration objects as arguments.

**Fix:** Top-level primitives and constrained types.

| ❌ Bad                                      | ✅ Good                                                                                                                   |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| `def search_orders(filters: dict) -> list` | `def search_orders(email: str, status: Literal["pending", "shipped", "delivered"] = "pending", limit: int = 10) -> list` |
| Agent guesses the structure                | Clear, typed, constrained                                                                                                |
| Hallucinates keys, misses required fields  | `Literal` constrains choices, defaults reduce decisions                                                                  |

### 3\. Instructions are Context

**Trap:** Empty docstrings, generic error messages.

**Fix:** Every piece of text is part of the agent's context.

Docstrings are instructions. Specify:

- When to use the tool ("Use when the user asks about order status")
- How to format arguments ("Email must be lowercase")
- What to expect back ("Returns order ID and current status")

Error Messages are context too! If a tool call fails, don't throw a Python exception. Return a helpful string: *"User not found. Please try searching by email address instead."* The agent sees the error as an observation and uses your instruction to self-correct in the next turn.

### 4\. Curate Ruthlessly

**Trap:** Exposing everything your API can do. Returning everything the API returns.

**Fix:** Design for discovery, not exhaustive exposure.

Agents operate under tight context constraints. Every tool description, every response payload, every error message, it all competes in the context window.

- 5–15 tools per server.
- One server, one job.
- Delete unused tools.
- Split by persona. (Admin/user)

Build for discovery. The agent should find the right tool quickly and get an actionable response.

### 5\. Name Tools for Discovery

**Trap:** Generic names like `create_issue` or `send_message`.

**Fix:** Service-prefixed, action-oriented names.

Your MCP server runs alongside others. If GitHub and Jira both have `create_issue`, the agent guesses. Pattern: `{service}_{action}_{resource}`. Examples: `slack_send_message`, `linear_list_issues`, `sentry_get_error_details`.

Note: Some MCP clients prefix tools with the server name automatically.

### 6\. Paginate Large Results

**Trap:** Returning hundreds of records.

**Fix:** Paginate with metadata.

- Respect a `limit` parameter (default 20–50)
- Return `has_more`, `next_offset`, `total_count`
- Never load all results into memory

## Practical Example: Gmail MCP Server

You're building an MCP server for Gmail. Here's how most developers approach it versus the agent-first design.

### Before

Python

```python
# Reading an email requires 2 tools + understanding nested types
def messages_list(query: str, max_results: int) -> {"messages": [{"id": str, "threadId": str}], "nextPageToken": str}: ...
def messages_get(message_id: str, format: str) -> {"id": str, "snippet": str, "payload": {"headers": list, "body": {"data": str}}}: ...
 
# Sending an email requires base64-encoding a MIME message
def messages_send(message: {"raw": str}) -> {"id": str, "threadId": str}: ...  # raw = base64url RFC 2822
 
# Creating a draft has nested message object
def drafts_create(draft: {"message": {"raw": str}}) -> {"id": str, "message": {"id": str}}: ...
```

**Problems:** Agent must construct `{"raw": base64(...)}` and parse nested `payload.body.data`. Generic names.

### After

Python

```python
# Reading: 2 flat tools with curated returns
def gmail_search(query: str, limit: int = 10) -> [{"id": str, "subject": str, "sender": str, "date": str, "snippet": str}]: ...
def gmail_read(message_id: str) -> {"subject": str, "sender": str, "body": str, "attachments": [str]}: ...
 
# Writing: Simple examples, not including cc, bcc.
def gmail_send(to: List[str], subject: str, body: str, reply_to_id: str = None) -> {"success": bool, "message_id": str}: ...
```

## Skills vs. MCP: Complementary, Not Competitive

Skills package instructions, metadata, and resources that the agent uses when relevant. They're filesystem-based with progressive disclosure. Each skill has a name, description, which is part of the Model context. The Model/Agent can then decide to use the skill or not based on the given context.

| Level            | When Loaded    | Token Cost   | Content                            |
| ---------------- | -------------- | ------------ | ---------------------------------- |
| **Metadata**     | Startup        | \~100 tokens | `name` and `description` from YAML |
| **Instructions** | When triggered | <5k tokens   | SKILL.md body                      |
| **Resources**    | As needed      | Unlimited    | Scripts via bash                   |

Skills don't add tool definitions directly, but can include scripts that run locally to provide capabilities similar to MCP servers. The difference is that skills leverage generic execution tools (like `bash`) rather than defining new tool schemas. Leading to potentially more steps and discovery needed. MCP provides a structured interfaces. The model gets parameter validation, typed responses.

Neither is better than the other. It depends on the use case and your preference. In company contexts, MCP servers shine when teams expose their own services. Skills complement this by teaching agents when and how to combine those tools for specific workflows. Use both.

## Conclusion

When building an MCP Server you are not building infrastructure, you are building an interface for AI agents.

1. **Outcomes over operations:** Design for agent goals
2. **Flatten arguments:** Primitives and enums
3. **Instructions are context:** Docstrings and errors matter
4. **Curate ruthlessly:** Fewer tools, tighter responses
5. **Name for discovery:** Service-prefixed names
6. **Paginate results:** Metadata for large lists

MCP is a User Interface for AI agents. Build it like one.

*Other resources: [Block's MCP Playbook](https://block.xyz/inside/designing-high-quality-mcp-servers), [GitHub's Security Guide](https://github.blog/ai-and-ml/generative-ai/how-to-build-secure-and-scalable-remote-mcp-servers/), [FastMCP AI Engineering Summit](https://www.youtube.com/watch?v=96G7FLab8xc).*

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
