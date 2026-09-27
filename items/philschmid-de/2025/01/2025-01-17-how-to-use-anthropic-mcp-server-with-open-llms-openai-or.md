---
title: How to use Anthropic MCP Server with open LLMs, OpenAI or Google Gemini
link: https://www.philschmid.de/mcp-example-llama
source: philschmid-de
published: 2025-01-17T00:00:00Z
updated: 2025-01-17T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: How to use Anthropic MCP Server with open LLMs, OpenAI or Google Gemini
content: extracted
html: 2025-01-17-how-to-use-anthropic-mcp-server-with-open-llms-openai-or.html
preview:
  file: 2025-01-17-how-to-use-anthropic-mcp-server-with-open-llms-openai-or.preview-3e423ec36fee.webp
  width: 256
  height: 145
  alt: How to use Anthropic MCP Server with open LLMs, OpenAI or Google Gemini
  color: '#dfdfdb'
images:
- source: https://www.philschmid.de/static/blog/mcp-example-llama/thumbnail.jpg
  original:
    file: 2025-01-17-how-to-use-anthropic-mcp-server-with-open-llms-openai-or.image-808316709864.jpg
    width: 1200
    height: 680
  color: '#f9f9f9'
---

AI agents are evolving from simple chatbots to (semi-)autonomous systems capable of complex reasoning, planning, and interaction with the real world. These agents are not just theoretical concepts; they will be deployed in production across various verticals, tackling increasingly more complex and longer-running tasks. As their capabilities grow, so does the challenge of managing and scaling them efficiently.

Currently, AI agents are designed with all tools, structures, and resources consolidated in a single monolithic application. This could become increasingly challenging as the system grows, leading to maintenance difficulties, hindering innovation, and creating bottlenecks in development processes.

To address these challenges, the Model Context Protocol (MCP), an open standard, was proposed by Anthropic. It tries to decouple components and define clear interfaces. In this blog post, you will learn how to use MCP Servers with any open LLM, OpenAI, or Google Gemini. You will learn how to build a simple CLI Agent that can control and interact with a SQLite database.

## What is the Model Context Protocol (MCP)?

The [Model Context Protocol](https://modelcontextprotocol.io/introduction) (MCP) is an open standard that enables developers to build secure agents and complex workflows on top of LLMs. MCP allows AI Agents to connect seamlessly with various external data sources and tools by standardizing how applications provide context to LLMs. MCP implements a Client-Server Architecture where the AI Agents initiate clients and communicate with servers. The current components of MCP servers include:

1. **Tools**: These are functions that LLMs can call to perform specific actions, e.g. weather API
2. **Resources**: These are data sources that LLMs can access, similar to GET endpoints in a REST API. Resources provide data without performing significant computation.
3. **Prompts**: These are pre-defined templates to use tools or resources in the most optimal way.

MCP allows AI Agents to interact with multiple resources through a unified protocol. Tools and resources can be independently updated, tested, extended, and reused across different platforms without the need to copy code.

However, there are still limitations and missing features, including remote MCP Servers or how to handle authentication correctly. The good news is that this is being worked on!

*Note: If you want to see a Google Gemini example, check out the [repository](https://github.com/philschmid/mcp-openai-gemini-llama-example/blob/master/sqlite_gemini_mcp_agent.py).*

## How to use Anthropic MCP Server with open LLMs, OpenAI, or Google Gemini

We are going to build a simple CLI Agent that can control an SQLite database through an MCP Server. We will use the official [SQLite MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/sqlite) using Docker. The architecture is quite simple:

1. First, we need to create our LLM client, e.g. `OpenAI` or `GenerativeModel`
2. Initiate an MCP client and connect to our SQLite MCP Server
3. Load existing tools, resources, and prompts provided by the MCP Server
4. Convert tools into LLM-compatible function calling tools (JSON Schema) with callable to our MCP Server
5. Create a custom System Message based on available MCP features.
6. Start interactive loop waiting for user inputs.

In our example, we are going to use [Meta Llama 3.3 70B instruct](https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct) hosted on Hugging Face Inference API with the `openai` SDK.

Note: Before we can start make sure you have `docker` setup and are logged into your Hugging Face account:

Bash

```bash
huggingface-cli login --token YOUR_TOKEN
```

Before we can start coding we need to install our required libraries. We are going to use the `openai` SDK to interact with the Hugging Face Inference API and we need the official python `mcp` SDK.

Bash

```bash
pip install huggingface_hub openai "mcp==1.1.2"
```

**Note: Different from other blog posts, you will find the full code below as a file, including detailed code comments on the different sections.**

Python

```python
import json
from huggingface_hub import get_token
from openai import AsyncOpenAI
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client
from typing import Any, List
import asyncio
 
MODEL_ID = "meta-llama/Llama-3.3-70B-Instruct"
 
# System prompt that guides the LLM's behavior and capabilities
# This helps the model understand its role and available tools
SYSTEM_PROMPT = """You are a helpful assistant capable of accessing external functions and engaging in casual chat. Use the responses from these function calls to provide accurate and informative answers. The answers should be natural and hide the fact that you are using tools to access real-time information. Guide the user about available tools and their capabilities. Always utilize tools to access real-time information when required. Engage in a friendly manner to enhance the chat experience.
 
# Tools
 
{tools}
 
# Notes 
 
- Ensure responses are based on the latest information available from function calls.
- Maintain an engaging, supportive, and friendly tone throughout the dialogue.
- Always highlight the potential of available tools to assist users comprehensively."""
 
 
# Initialize OpenAI client with HuggingFace inference API
# This allows us to use Llama models through HuggingFace's API
client = AsyncOpenAI(
    base_url="https://api-inference.huggingface.co/v1/",
    api_key=get_token(),
)
 
 
class MCPClient:
    """
    A client class for interacting with the MCP (Model Control Protocol) server.
    This class manages the connection and communication with the SQLite database through MCP.
    """
 
    def __init__(self, server_params: StdioServerParameters):
        """Initialize the MCP client with server parameters"""
        self.server_params = server_params
        self.session = None
        self._client = None
 
    async def __aenter__(self):
        """Async context manager entry"""
        await self.connect()
        return self
 
    async def __aexit__(self, exc_type, exc_val, exc_tb):
        """Async context manager exit"""
        if self.session:
            await self.session.__aexit__(exc_type, exc_val, exc_tb)
        if self._client:
            await self._client.__aexit__(exc_type, exc_val, exc_tb)
 
    async def connect(self):
        """Establishes connection to MCP server"""
        self._client = stdio_client(self.server_params)
        self.read, self.write = await self._client.__aenter__()
        session = ClientSession(self.read, self.write)
        self.session = await session.__aenter__()
        await self.session.initialize()
 
    async def get_available_tools(self) -> List[Any]:
        """
        Retrieve a list of available tools from the MCP server.
        """
        if not self.session:
            raise RuntimeError("Not connected to MCP server")
 
        tools = await self.session.list_tools()
        _, tools_list = tools
        _, tools_list = tools_list
        return tools_list
 
    def call_tool(self, tool_name: str) -> Any:
        """
        Create a callable function for a specific tool.
        This allows us to execute database operations through the MCP server.
 
        Args:
            tool_name: The name of the tool to create a callable for
 
        Returns:
            A callable async function that executes the specified tool
        """
        if not self.session:
            raise RuntimeError("Not connected to MCP server")
 
        async def callable(*args, **kwargs):
            response = await self.session.call_tool(tool_name, arguments=kwargs)
            return response.content[0].text
 
        return callable
 
 
async def agent_loop(query: str, tools: dict, messages: List[dict] = None):
    """
    Main interaction loop that processes user queries using the LLM and available tools.
 
    This function:
    1. Sends the user query to the LLM with context about available tools
    2. Processes the LLM's response, including any tool calls
    3. Returns the final response to the user
 
    Args:
        query: User's input question or command
        tools: Dictionary of available database tools and their schemas
        messages: List of messages to pass to the LLM, defaults to None
    """
    messages = (
        [
            {
                "role": "system",
                "content": SYSTEM_PROMPT.format(
                    tools="\n- ".join(
                        [
                            f"{t['name']}: {t['schema']['function']['description']}"
                            for t in tools.values()
                        ]
                    )
                ),  # Creates System prompt based on available MCP server tools
            },
        ]
        if messages is None
        else messages  # reuse existing messages if provided
    )
    # add user query to the messages list
    messages.append({"role": "user", "content": query})
 
    # Query LLM with the system prompt, user query, and available tools
    first_response = await client.chat.completions.create(
        model=MODEL_ID,
        messages=messages,
        tools=([t["schema"] for t in tools.values()] if len(tools) > 0 else None),
        max_tokens=4096,
        temperature=0,
    )
    # detect how the LLM call was completed:
    # tool_calls: if the LLM used a tool
    # stop: If the LLM generated a general response, e.g. "Hello, how can I help you today?"
    stop_reason = (
        "tool_calls"
        if first_response.choices[0].message.tool_calls is not None
        else first_response.choices[0].finish_reason
    )
 
    if stop_reason == "tool_calls":
        # Extract tool use details from response
        for tool_call in first_response.choices[0].message.tool_calls:
            arguments = (
                json.loads(tool_call.function.arguments)
                if isinstance(tool_call.function.arguments, str)
                else tool_call.function.arguments
            )
            # Call the tool with the arguments using our callable initialized in the tools dict
            tool_result = await tools[tool_call.function.name]["callable"](**arguments)
            # Add the tool result to the messages list
            messages.append(
                {
                    "role": "tool",
                    "tool_call_id": tool_call.id,
                    "name": tool_call.function.name,
                    "content": json.dumps(tool_result),
                }
            )
 
        # Query LLM with the user query and the tool results
        new_response = await client.chat.completions.create(
            model=MODEL_ID,
            messages=messages,
        )
 
    elif stop_reason == "stop":
        # If the LLM stopped on its own, use the first response
        new_response = first_response
 
    else:
        raise ValueError(f"Unknown stop reason: {stop_reason}")
 
    # Add the LLM response to the messages list
    messages.append(
        {"role": "assistant", "content": new_response.choices[0].message.content}
    )
 
    # Return the LLM response and messages
    return new_response.choices[0].message.content, messages
 
 
async def main():
    """
    Main function that sets up the MCP server, initializes tools, and runs the interactive loop.
    The server is run in a Docker container to ensure isolation and consistency.
    """
    # Configure Docker-based MCP server for SQLite
    server_params = StdioServerParameters(
        command="docker",
        args=[
            "run",
            "--rm",  # Remove container after exit
            "-i",  # Interactive mode
            "-v",  # Mount volume
            "mcp-test:/mcp",  # Map local volume to container path
            "mcp/sqlite",  # Use SQLite MCP image
            "--db-path",
            "/mcp/test.db",  # Database file path inside container
        ],
        env=None,
    )
 
    # Start MCP client and create interactive session
    async with MCPClient(server_params) as mcp_client:
        # Get available database tools and prepare them for the LLM
        mcp_tools = await mcp_client.get_available_tools()
        # Convert MCP tools into a format the LLM can understand and use
        tools = {
            tool.name: {
                "name": tool.name,
                "callable": mcp_client.call_tool(
                    tool.name
                ),  # returns a callable function for the rpc call
                "schema": {
                    "type": "function",
                    "function": {
                        "name": tool.name,
                        "description": tool.description,
                        "parameters": tool.inputSchema,
                    },
                },
            }
            for tool in mcp_tools
            if tool.name
            != "list_tables"  # Excludes list_tables tool as it has an incorrect schema
        }
 
        # Start interactive prompt loop for user queries
        messages = None
        while True:
            try:
                # Get user input and check for exit commands
                user_input = input("\nEnter your prompt (or 'quit' to exit): ")
                if user_input.lower() in ["quit", "exit", "q"]:
                    break
 
                # Process the prompt and run agent loop
                response, messages = await agent_loop(user_input, tools, messages)
                print("\nResponse:", response)
                # print("\nMessages:", messages)
            except KeyboardInterrupt:
                print("\nExiting...")
                break
            except Exception as e:
                print(f"\nError occurred: {e}")
 
 
if __name__ == "__main__":
    asyncio.run(main())
```

You can run the agent and start chatting with

Python

```python
python sqlite_llama_mcp_agent.py
```

Below is an example conversation of what you can do and how it response.

```plaintext
Enter your prompt (or 'quit' to exit): HEllo 
Response: Hello! How can 1 assist you today?
 
Enter your prompt (or 'quit' to exit): Who are you?
Response: I am a helpful assistant capable of accessing external functions and engaging in casual chat. I can use the available tools to provide accurate and infor mative answers. The available tools include read_query,
write_query, create_table, describe_table, and append_insight. I can guide you about these tools and their capabilities, and I will utilize them to access real-time information when required. 
 
Enter your prompt (or 'quit' to exit): Cool! Create a new table for fake products. Each product should have a name and a price. Generate 10 fake products based on video games.
Response: The table has been created successfully with the following schema:
- name (TEXT): The name of the product.
- price (REAL): The price of the product.
 
Here are 10 fake products based on video games:
1. "The Last of Us" T-shirt - $19.99
2. Minecraft Creeper Plush Toy - $14.99
3. Grand Theft Auto V Poster - $9,99
4. The Legend of Zelda: Breath of the Wild Strategy Guide - $24.99
5. Call of Duty: Modern Warfare Gaming Keyboard - $69.99
6. World of Warcraft Subscription Card (3 months) - $39.99
7. Assassin's Creed Odyssey Action Figure - $29.99
8. Fortnite Monopoly Board Game - $29.99    
9. Resident Evil 2 Remake Collector's Edition - $99.99
10. Pokemon Sword and Shield Nintendo Switch Bundle - $399.99
 
Let me know if you'd like to add more products or perform any other operations on the table!
```

Feel free to change the code, include debugging statements to better understand the outputs from each step.

- [Google Gemini Example](https://github.com/philschmid/mcp-openai-gemini-llama-example/blob/master/sqlite_gemini_mcp_agent.py)

## Conclusion

This simple example illustrates how you can get started with building an AI Agent using MCP Servers alongside open LLMs, OpenAI, or Google Gemini. It demonstrates the core principles of how an agent might connect to a server, interact with tools, and respond to user commands. It is not a framework or a reliable implementation—its purpose is to help you understand the building blocks of a distributed AI Agent architecture using MCP Server.

I’m working on a small toolkit to make implementing AI Agents via MCP easier, so stay tuned for more updates.

If you have any questions, feedback, or ideas, please dm me on [X](https://x.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/). I am excited to hear about how you are experimenting and pushing the boundaries of AI agents.
