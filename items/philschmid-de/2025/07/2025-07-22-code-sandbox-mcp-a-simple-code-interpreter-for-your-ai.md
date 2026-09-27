---
title: 'Code Sandbox MCP: A Simple Code Interpreter for Your AI Agents'
link: https://www.philschmid.de/code-sandbox-mcp
source: philschmid-de
published: 2025-07-22T00:00:00Z
updated: 2025-07-22T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Code Sandbox MCP is a simple, self-hosted code interpreter for your AI agents. It allows you to execute code snippets in containerized environments.
content: extracted
html: 2025-07-22-code-sandbox-mcp-a-simple-code-interpreter-for-your-ai.html
preview:
  file: 2025-07-22-code-sandbox-mcp-a-simple-code-interpreter-for-your-ai.preview-a690f9f1f2dd.webp
  width: 256
  height: 136
  alt: 'Code Sandbox MCP: A Simple Code Interpreter for Your AI Agents'
  color: '#2a2a2a'
images:
- source: https://www.philschmid.de/static/blog/code-sandbox-mcp/thumbnail.jpg
  original:
    file: 2025-07-22-code-sandbox-mcp-a-simple-code-interpreter-for-your-ai.image-9ea9918c51e8.jpg
    width: 1700
    height: 900
  color: '#272727'
- source: https://www.philschmid.de/static/blog/code-sandbox-mcp/code-sandbox-mcp.png
  original:
    file: 2025-07-22-code-sandbox-mcp-a-simple-code-interpreter-for-your-ai.image-da7a0f0e604f.png
    width: 1560
    height: 728
  color: '#272727'
---

July 22, 2025 4 minute read [View Code](https://github.com/philschmid/code-sandbox-mcp)

Code agents are transforming software development. But how do we safely let them execute code? Code sandboxes have evolved from basic security tools into essential development infrastructure.

Today, I'm launching **[Code Sandbox MCP](https://github.com/philschmid/code-sandbox-mcp)**, a lightweight, STDIO-based Model Context Protocol (MCP) Server, allowing AI assistants and LLM applications to safely execute code snippets using containerized environments. It is uses the [llm-sandbox](https://github.com/vndee/llm-sandbox) package for the containerization and execution of the code snippets.

It exposes `run_python_code` and `run_javascript_code` tools, giving your AI agent the ability to execute code on your own infrastructure.

## How it works

1. Starts a container session (podman, docker, etc.) and ensures the session is open.
2. Writes the code to a temporary file on the host.
3. Copies this temporary file into the container at the configured workdir.
4. Executes the language-specific commands to run the code, e.g. python `python3 -u code.py` or javascript `node -u code.js`
5. Captures the output and error streams from the container.
6. Returns the output and error streams to the client.

![Code Sandbox MCP](https://www.philschmid.de/static/blog/code-sandbox-mcp/code-sandbox-mcp.png)

This entire workflow happens on your machine, locally or on your own server, keeping your data and code private.

## Get Started with the Gemini SDK

Integrating Code Sandbox MCP with your Python application using the Gemini SDK is incredibly straightforward. First, install the package:

Bash

```bash
pip install git+https://github.com/philschmid/code-sandbox-mcp.git
```

Then, you can use `fastmcp` to connect your Gemini client to the local code execution tool:

Python

```python
from fastmcp import Client
from google import genai
import asyncio
 
# Configure the MCP client to use the local server
mcp_client = Client(
    {
        "local_server": {
            "transport": "stdio",
            "command": "code-sandbox-mcp",
        }
    }
)
gemini_client = genai.Client()
 
async def main():
    async with mcp_client:
        response = await gemini_client.aio.models.generate_content(
            model="gemini-1.5-flash",
            contents="Use Python to ping google.com and return the response.",
            config=genai.types.GenerateContentConfig(
                tools=[mcp_client.session],  # Pass the FastMCP client session
            ),
        )
        # The model's response will include the output from the code execution
        print(response.text)
 
if __name__ == "__main__":
    asyncio.run(main())
 
```

## Empower the Gemini CLI

You can also supercharge the Gemini CLI with code execution capabilities. Simply add the server configuration to your `~/.gemini/settings.json` file:

JSON

```json
{
  "mcpServers": {
    "code-sandbox": {
      "command": "code-sandbox-mcp"
    }
  }
}
 
```

Now, when you use the Gemini CLI, it can automatically discover and use the `run_python_code` tool to answer your questions!

## Security

By leveraging `llm-sandbox`, Code Sandbox MCP benefits from multiple layers of security:

1. **Container Isolation**: All code runs inside a sandboxed container (Docker, Podman, etc.), completely isolated from the host system's filesystem and processes.
2. **Resource Limits**: The underlying sandbox can be configured with strict memory, CPU, and execution time limits to prevent resource exhaustion.
3. **Network Controls**: You can define network policies to restrict or completely block outbound network access from the container.
4. **Pre-execution Analysis**: The underlying `llm-sandbox` framework supports security policies that can analyze code for dangerous patterns (e.g., `os.system`, file system access) before it even runs.

## Why Build This?

I built Code Sandbox MCP primarily for my own use. When working with the Gemini CLI or other coding agents, I constantly struggled with testing code snippets in an isolated environment. Especially when you hand over more responsbility to the agent.

While managed solutions like [Daytona](https://www.daytona.io/) and [E2B](https://e2b.dev/) are excellent for scaling or building customer facing agents, they didn't fit my specific needs. In my use cases, I wanted to give agents a way to execute code in an environment I can control and don't have to pay for.

I mean:

- **Having the libraries I need installed**: Unlike integrated code execution tools that come with limited packages, I can customize my environment with any dependencies.
- **Access to secrets and credentials**: I can safely provide API keys or other sensitive information.
- **Local code access**: Agents can work with my actual codebase, files, and local resources. I export files.
- **No unnecessary costs**: There's simply no need to pay for a cloud solution when executing small code snippets for personal development work.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
