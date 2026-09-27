---
title: Model Context Protocol (MCP) an overview
link: https://www.philschmid.de/mcp-introduction
source: philschmid-de
published: 2025-04-03T00:00:00Z
updated: 2025-04-03T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Overview of the Model Context Protocol (MCP) how it works, what are MCP servers and clients, and how to use it.
content: extracted
html: 2025-04-03-model-context-protocol-mcp-an-overview.html
preview:
  file: 2025-04-03-model-context-protocol-mcp-an-overview.preview-6502e9692355.webp
  width: 256
  height: 144
  alt: Model Context Protocol (MCP) an overview
  color: '#bbbcc0'
images:
- source: https://www.philschmid.de/static/blog/mcp-introduction/thumbnail.jpg
  original:
    file: 2025-04-03-model-context-protocol-mcp-an-overview.image-7c3164d69d3f.jpg
    width: 961
    height: 540
  color: '#f9f9f9'
- source: https://www.philschmid.de/static/blog/mcp-introduction/api.png
  original:
    file: 2025-04-03-model-context-protocol-mcp-an-overview.image-50bbc82bf3ee.png
    width: 3700
    height: 1910
  variants:
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-092cba208e1b.webp
    width: 320
    height: 165
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-a91267954fc4.webp
    width: 640
    height: 330
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-431000f5d5c3.webp
    width: 960
    height: 496
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-2cd249313351.webp
    width: 1280
    height: 661
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-4f118f0c17bc.webp
    width: 1600
    height: 826
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-18d33cd00ba3.webp
    width: 3700
    height: 1910
  color: '#f9f9f9'
- source: https://www.philschmid.de/static/blog/mcp-introduction/overview.png
  original:
    file: 2025-04-03-model-context-protocol-mcp-an-overview.image-6f04cfd7ce5a.png
    width: 4000
    height: 2750
  variants:
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-e3d8fa48cd9d.webp
    width: 320
    height: 220
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-ee9db7828cbd.webp
    width: 640
    height: 440
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-4ef938100354.webp
    width: 960
    height: 660
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-7e53385cb4a5.webp
    width: 1280
    height: 880
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-031fb112b220.webp
    width: 1600
    height: 1100
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-2949c899e0b4.webp
    width: 4000
    height: 2750
  color: '#f9f9f9'
- source: https://www.philschmid.de/static/blog/mcp-introduction/architecture.png
  original:
    file: 2025-04-03-model-context-protocol-mcp-an-overview.image-30beb517dd02.png
    width: 4000
    height: 2055
  variants:
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-6d81bbaea41b.webp
    width: 320
    height: 164
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-815feb6e85ef.webp
    width: 640
    height: 329
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-08006e665d58.webp
    width: 960
    height: 493
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-febd439aaa31.webp
    width: 1280
    height: 658
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-bf524421739f.webp
    width: 1600
    height: 822
  - file: 2025-04-03-model-context-protocol-mcp-an-overview.image-c4a73f56016f.webp
    width: 4000
    height: 2055
  color: '#fafafa'
- source: https://i.ytimg.com/vi/HyzlYwjoXOQ/hqdefault.jpg
  original:
    file: 2025-04-03-model-context-protocol-mcp-an-overview.image-53170a49c0fd.jpg
    width: 480
    height: 360
  color: '#1d1d23'
- source: https://i.ytimg.com/vi/kQmXtrmQ5Zg/hqdefault.jpg
  original:
    file: 2025-04-03-model-context-protocol-mcp-an-overview.image-2b304ed567d4.jpg
    width: 480
    height: 360
  color: '#020202'
---

The [Model Context Protocol](https://modelcontextprotocol.io/introduction) (MCP) is an open standard [introduced by Anthropic](https://www.anthropic.com/news/model-context-protocol) with the goal to standardize how AI applications (chatbots, IDE assistants, or custom agents) connect with external tools, data sources, and systems.

![api](https://www.philschmid.de/static/blog/mcp-introduction/api.png)

Think of it like USB for AI integrations. Before standards like USB, connecting peripherals required a mess of different ports and custom drivers. Similarly, integrating AI applications with external tools and systems is/was an "M×N problem".\
 If you have M different AI applications (Chat, RAG 1, custom agents, etc.) and N different tools/systems (GitHub, Slack, Asana, databases, etc.), you might need to build M×N different integrations. This leads to duplicated effort across teams, inconsistent implementations.

MCP aims to simplify this by providing a common API and transforming this into an "M+N problem". Tool creators build N MCP servers (one for each system), while application developers build M MCP clients (one for each AI application). MCP defines a client-server architecture where:

- **Hosts:** Applications the user interacts with (e.g., Claude Desktop, an IDE like Cursor, a custom agent).
- **Clients:** Live within the Host application and manage the connection to one specific MCP server. Maintain a 1:1 to connection.
- **Servers:** External programs that expose Tools, Resources and Prompts via standard API to the AI model via the client.

The current components of MCP servers include:

1. **Tools (Model-controlled):** These are functions (tools) that LLMs can call to perform specific actions, e.g. weather API, basically function calling
2. **Resources (Application-controlled):** These are data sources that LLMs can access, similar to GET endpoints in a REST API. Resources provide data without performing significant computation, no side effects. Part of the context/request
3. **Prompts (User-controlled):** These are pre-defined templates to use tools or resources in the most optimal way. Selected before running inference

![overview](https://www.philschmid.de/static/blog/mcp-introduction/overview.png)

## How does MCP work?

MCP operates on the client-server model described earlier. Here’s a simplified flow:

![architecture](https://www.philschmid.de/static/blog/mcp-introduction/architecture.png)

1. **Initialization:** When a Host application starts it creates N MCP Clients, which exchange information about capabilities and protocol versions via a handshake.
2. **Discovery:** Clients requests what capabilities (Tools, Resources, Prompts) the server offers. The Server responds with a list and descriptions.
3. **Context Provision:** The Host application can now make resources and prompts available to the user or parses the tools into a LLM compatible format, e.g. JSON Function calling
4. **Invocation:** If the LLM determines it needs to use a Tool (e.g., based on the user's request like "What are the open issues in the 'X' repo?"), the Host directs the Client to send an invocation request to the appropriate Server.
5. **Execution:** The Server receives the request (e.g., fetch\_github\_issues with repo 'X'), executes the underlying logic (calls the GitHub API), and gets the result.
6. **Response:** The Server sends the result back to the Client.
7. **Completion:** The Client relays the result to the Host, which incorporates it into the LLM's context, allowing the LLM to generate a final response for the user based on the fresh, external information.

### MCP servers

MCP Servers are the bridge/API between the MCP world and the specific functionality of an external system (an API, a database, local files, etc.). They are essentially wrappers that expose these external capabilities according to the MCP specification.

Servers can be built in various languages (Python, TypeScript, Java, Rust, etc.) as long as they can communicate over the supported transports. Servers communicate with clients primarily via two methods:

- **stdio (Standard Input/Output):** Used when Client and Server run on the same machines. This is simple and effective for local integrations (e.g., accessing local files or running a local script).
- **HTTP via SSE (Server-Sent Events):** The Client connects to the Server via HTTP. After an initial setup, the Server can push messages (events) to the Client over a persistent connection using the SSE standard.

Example of how to build an MCP server with Python and [FastMCP](https://github.com/jlowin/fastmcp/tree/main):

Python

```py
from fastmcp import FastMCP
 
# Create an MCP server
mcp = FastMCP("Demo")
 
# Add a tool, will be converted into JSON spec for function calling
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b
 
# Add a data resource, e.g. displayed on new chats
@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """Get a personalized greeting"""
    return f"Hello, {name}!"
 
# Specific prompt templates for better use
@mcp.prompt()
def review_code(code: str) -> str:
    return f"Please review this code:\n\n{code}"
```

List of pre-build and community build MCP servers:

- [https://github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers)
- [https://github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
- [https://mcp.composio.dev/](https://mcp.composio.dev/)

### MCP Clients

MCP Clients are part of Host applications (the IDE, chatbot, etc.) that manage the communication with a specific MCP Server.

- **Role:** Handle connection management, capability discovery, request forwarding, and response handling according to the MCP spec.
- **Examples of Hosts/Clients:**
  - UI Apps: Claude Desktop, Microsoft Copilot Studio, LibreChat, Claude Code
  - IDEs: Cursor, Windsurf, Continue, Zed, Cline
  - Custom Agents (Python/TypeScript):
    - Firebase Genkit
    - LangGraph
    - OpenAI agents sdk
    - ….

Example of how to build an MCP client with Python and mcp.

Python

```py
from mcp import ClientSession, StdioServerParameters, types
from mcp.client.stdio import stdio_client
 
# Commands for running/connecting to MCP Server
server_params = StdioServerParameters(
    command="python",  # Executable
    args=["example_server.py"],  # Optional command line arguments
    env=None,  # Optional environment variables
)
 
async with stdio_client(server_params) as (read, write):
    async with ClientSession(
        read, write, sampling_callback=handle_sampling_message
    ) as session:
        # Initialize the connection
        await session.initialize()
 
        # List available prompts
        prompts = await session.list_prompts()
 
        # Get a prompt
        prompt = await session.get_prompt(
            "example-prompt", arguments={"arg1": "value"}
        )
 
        # List available resources
        resources = await session.list_resources()
 
        # List available tools
        tools = await session.list_tools()
 
        # Read a resource
        content, mime_type = await session.read_resource("file://some/path")
 
        # Call a tool
        result = await session.call_tool("tool-name", arguments={"arg1": "value"})
 
```

## Why is there so much hype? Did MCP win?

While Anthropic announced MCP in late 2024, its momentum significantly accelerated in early 2025. This isn't just random hype; several factors converged:

- **"AI-Native"** while older standards like OpenAPI, GraphQL, or SOAP exist for API interaction, MCP was designed specifically for the needs of modern AI agents. MCP refines patterns seen in agent development:
  - Tools (Model-controlled): Actions the AI decides to take.
  - Resources (Application-controlled): Context provided to the AI.
  - Prompts (User-controlled): Specific user-invoked interactions.
- **"Open Standard" with a Big Backer:** Any "open standard” should have a spec, and [MCP has a VERY good spec](https://spec.modelcontextprotocol.io/specification/2024-11-05/). The spec alone defeats a lot of contenders, who do not provide such detailed specs.
- **Built on Proven Foundations:** Instead of re-inventing everything from scratch, Anthropic adapted from Language Server Protocol (LSP), e.g. [JSON-RPC 2.0](https://www.jsonrpc.org/)
- **Strong Initial Ecosystem & Dogfooding:** MCP didn't launch as just a spec. Anthropic "dogfooded" it extensively and released it with a comprehensive initial set:
  - Client: Claude Desktop.
  - Servers: Numerous reference implementations (filesystem, git, Slack, etc.).
  - Tooling: MCP Inspector for testing, great documentation
  - SDKs: Python and TypeScript libraries, now Java, Kotlin C#
- **Network Effects:** The open nature fostered a community. Tools like Cursor and Windsurf integrated MCP. Companies like Composio provided pre-built servers for hundreds of integrations. OpenAI announced support for MCP. Developers built thousands of community MCP servers (GitHub, Slack, databases, Docker, etc.).

## Practical Example with Gemini and Python uSDK

Python

```py
from typing import List
from google import genai
from google.genai import types
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client
import os
 
client = genai.Client(api_key=os.getenv("GEMINI_API_KEY"))
model = "gemini-2.0-flash"
 
# Create server parameters for stdio connection
server_params = StdioServerParameters(
    command="npx",  # Executable
    args=[
        "-y",
        "@openbnb/mcp-server-airbnb",
    ],  # Optional command line arguments
    env=None,  # Optional environment variables
)
 
async def agent_loop(prompt: str, client: genai.Client, session: ClientSession):
    contents = [types.Content(role="user", parts=[types.Part(text=prompt)])]
    # Initialize the connection
    await session.initialize()
    
    # --- 1. Get Tools from Session and convert to Gemini Tool objects ---
    mcp_tools = await session.list_tools()
    tools = types.Tool(function_declarations=[
        {
            "name": tool.name,
            "description": tool.description,
            "parameters": tool.inputSchema,
        }
        for tool in mcp_tools.tools
    ])
    
    # --- 2. Initial Request with user prompt and function declarations ---
    response = await client.aio.models.generate_content(
        model=model,  # Or your preferred model supporting function calling
        contents=contents,
        config=types.GenerateContentConfig(
            temperature=0,
            tools=[tools],
        ),  # Example other config
    )
    
    # --- 3. Append initial response to contents ---
    contents.append(response.candidates[0].content)
 
    # --- 4. Tool Calling Loop ---            
    turn_count = 0
    max_tool_turns = 5
    while response.function_calls and turn_count < max_tool_turns:
        turn_count += 1
        tool_response_parts: List[types.Part] = []
 
        # --- 4.1 Process all function calls in order and return in this turn ---
        for fc_part in response.function_calls:
            tool_name = fc_part.name
            args = fc_part.args or {}  # Ensure args is a dict
            print(f"Attempting to call MCP tool: '{tool_name}' with args: {args}")
 
            tool_response: dict
            try:
                # Call the session's tool executor
                tool_result = await session.call_tool(tool_name, args)
                print(f"MCP tool '{tool_name}' executed successfully.")
                if tool_result.isError:
                    tool_response = {"error": tool_result.content[0].text}
                else:
                    tool_response = {"result": tool_result.content[0].text}
            except Exception as e:
                tool_response = {"error":  f"Tool execution failed: {type(e).__name__}: {e}"}
            
            # Prepare FunctionResponse Part
            tool_response_parts.append(
                types.Part.from_function_response(
                    name=tool_name, response=tool_response
                )
            )
 
        # --- 4.2 Add the tool response(s) to history ---
        contents.append(types.Content(role="user", parts=tool_response_parts))
        print(f"Added {len(tool_response_parts)} tool response parts to history.")
 
        # --- 4.3 Make the next call to the model with updated history ---
        print("Making subsequent API call with tool responses...")
        response = await client.aio.models.generate_content(
            model=model,
            contents=contents,  # Send updated history
            config=types.GenerateContentConfig(
                temperature=1.0,
                tools=[tools],
            ),  # Keep sending same config
        )
        contents.append(response.candidates[0].content)
 
    if turn_count >= max_tool_turns and response.function_calls:
        print(f"Maximum tool turns ({max_tool_turns}) reached. Exiting loop.")
 
    print("MCP tool calling loop finished. Returning final response.")
    # --- 5. Return Final Response ---
    return response
        
async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(
            read,
            write,
        ) as session:
            # Test prompt
            prompt = "I want to book an apartment in Paris for 2 nights. 03/28 - 03/30"
            print(f"Running agent loop with prompt: {prompt}")
            # Run agent loop
            res = await agent_loop(prompt, client, session)
            return res
res = await run()
print(res.text)
```

## What about Security, Updates, Authentication?

MCP is a living protocol. The specification is actively maintained on GitHub, last update from 03/26 improves security, scalability, and usability.

- **Authentication & Security (OAuth 2.1):** The protocol now mandates the OAuth 2.1 framework for authenticating remote HTTP servers
- **Improved Transport & Efficiency:** The previous HTTP+SSE transport will be replaced with a more flexible Streamable HTTP transport and support for JSON-RPC batching.
- **Richer Context & Control:** New tool annotations provide more metadata about a tool's behavior (e.g., read-only vs. destructive)

## Acknowledgements

This overview was compiled with the help of deep and manual research, drawing inspiration and information from several excellent resources, including:

- [What Is MCP, and Why Is Everyone – Suddenly!– Talking About It?](https://huggingface.co/blog/Kseniase/mcp)
- [What is MCP](https://python.useinstructor.com/blog/2025/03/27/understanding-model-context-protocol-mcp/#conclusion)
- [I gave Claude root access to my server... Model Context Protocol explained](https://www.youtube.com/watch?v=HyzlYwjoXOQ)
- [Why MCP Won](https://www.latent.space/p/why-mcp-won)
- [Building Agents with Model Context Protocol - Full Workshop with Mahesh Murag of Anthropic](https://www.youtube.com/watch?v=kQmXtrmQ5Zg)

* * *

If you have any questions, feedback, or ideas, please dm me on [X](https://x.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/). I am excited to hear about how you are experimenting and pushing the boundaries of AI agents.
