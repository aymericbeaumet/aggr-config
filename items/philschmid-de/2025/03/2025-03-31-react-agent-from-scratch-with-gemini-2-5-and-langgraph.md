---
title: ReAct agent from scratch with Gemini 2.5 and LangGraph
link: https://www.philschmid.de/langgraph-gemini-2-5-react-agent
source: philschmid-de
published: 2025-03-31T00:00:00Z
updated: 2025-03-31T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Build a ReAct agent from scratch with Gemini 2.5 and LangGraph.
content: extracted
html: 2025-03-31-react-agent-from-scratch-with-gemini-2-5-and-langgraph.html
preview:
  file: 2025-03-31-react-agent-from-scratch-with-gemini-2-5-and-langgraph.preview-b6c9d39d6b1c.webp
  width: 256
  height: 144
  alt: ReAct agent from scratch with Gemini 2.5 and LangGraph
  color: '#181e2c'
images:
- source: https://www.philschmid.de/static/blog/langgraph-gemini-2-5-react-agent/thumbnail.jpg
  original:
    file: 2025-03-31-react-agent-from-scratch-with-gemini-2-5-and-langgraph.image-5a9b5118de75.jpg
    width: 961
    height: 540
  color: '#040813'
- source: https://www.philschmid.de/static/blog/langgraph-gemini-2-5-react-agent/react.png
  original:
    file: 2025-03-31-react-agent-from-scratch-with-gemini-2-5-and-langgraph.image-da4facb995fa.png
    width: 723
    height: 760
  variants:
  - file: 2025-03-31-react-agent-from-scratch-with-gemini-2-5-and-langgraph.image-6aee414bb38d.webp
    width: 320
    height: 336
  - file: 2025-03-31-react-agent-from-scratch-with-gemini-2-5-and-langgraph.image-eccf29173b3e.webp
    width: 640
    height: 673
  - file: 2025-03-31-react-agent-from-scratch-with-gemini-2-5-and-langgraph.image-7eddf68aaa45.webp
    width: 723
    height: 760
  color: '#fefefe'
---

AI applications are evolving from simple chatbots to (semi-)autonomous systems capable of complex reasoning, planning, and interaction with the real world. We call these system agents.

> An AI Agent is a system that uses LLMs to decide the control flow of an application.

Agents are not just theoretical concepts; they are and will be deployed in production across various verticals, tackling increasingly more complex and longer-running tasks. In this blog post, we'll explore how to create a ReAct agent using Google's Gemini 2.5 Pro or Gemini 2.0 Flash and LangGraph from scratch.

## What are ReAct Agents?

ReACT (Reasoning and Acting) Agents are AI systems that combine reasoning capabilities of LLMs with action execution, enabling them to iteratively think through problems, use tools, and act based on observations to achieve the user goals.

The ReAct pattern was first introduced in the [“ReAct: Synergizing Reasoning and Acting in Language Models”](https://arxiv.org/abs/2210.03629) in 2023. It was inspired by how humans plan and solve complex tasks, rather than implementing predefined workflows. ReAct agents rely on the LLM's reasoning capabilities to dynamically adjust their actions based on new information or the results of previous steps.

ReACT Agents have gained traction due to their ability to handle complex tasks by breaking them into manageable reasoning steps and leveraging external tools.

![react.png](https://www.philschmid.de/static/blog/langgraph-gemini-2-5-react-agent/react.png)

The ReAct agent:

1. Takes a user **query** as input
2. Reasons about the query and decides on an action
3. Executes the chosen action using available tools
4. Observes the result of the action
5. Repeats steps 2-4 until it can provide a final answer

### The First ReAct Agents

These first-generation ReAct agents used a simple but effective prompting technique to generate a chain of "Thought, Action, Observation" steps:

- "Thought" component plans the next action or decide it knows the final answer
- "Action" interacts with external resources (like search engines or calculators).
- "Observation" incorporates the results from the action into the reasoning process.

Here's a pseudo-code example demonstrating the flow of an early ReAct agent.

Xml

```xml
User: Who is Olivia Wilde's boyfriend? What is his current age raised to the 0.23 power?
 
Thought: I need to find out who Olivia Wilde's boyfriend is and then calculate his age raised to the 0.23 power.
Action: [search("Olivia Wilde boyfriend")]
Observation: Olivia Wilde started dating Harry Styles after ending her years-long engagement to Jason Sudeikis — see their relationship timeline.
 
Thought: I need to find out Harry Styles' age.
Action: [search("Harry Styles age")]
Observation: 29 years
 
Thought: I need to calculate 29 raised to the 0.23 power.
Action: [calculator(29^0.23)]
Observation: Answer: 2.169459462491557
 
Thought: I now know the final answer.
Final Answer: Harry Styles, Olivia Wilde's boyfriend, is 29 years old and his age raised to the 0.23 power is 2.169459462491557.
```

### Current ReAct Agents

Since the introduction of ReAct Agent the capabilities of LLMs has evolved. One of the most important improvements we made is function calling. Function calling allows us to to connect LLMs to external tools in a structured way, which is more reliable than parsing raw text and reduces the likelihood of errors and hallucinations.

Here's a pseudo-code example demonstrating the flow of an ReAct agent using function calling

Xml

```xml
User: Who is Olivia Wilde's boyfriend? What is his current age raised to the 0.23 power?
 
Assistant: FunctionCall(name="search", args={"query": "Olivia Wilde boyfriend"})
User: FunctionResponse(result="Olivia Wilde started dating Harry Styles after ending her years-long engagement to Jason Sudeikis — see their relationship timeline.")
 
Assistant: FunctionCall(name="search", args={"query": "Harry Styles age"})
User: FunctionResponse(result="29 years")
 
Assistant: FunctionCall(name="calculator", args={"expression": "29^0.23"})
User: FunctionResponse(result="2.169459462491557")
 
Assistant: Harry Styles, Olivia Wilde's boyfriend, is 29 years old. His age raised to the 0.23 power is 2.169459462491557.
```

### Traditional ReAct Agents vs. Current ReAct Agents (Function Calling)

| **Aspect**      | **Traditional ReAct Agents**                 | **Current ReAct Agents (Function Calling)**             |
| --------------- | -------------------------------------------- | ------------------------------------------------------- |
| Actions         | Text-based description, parsed by system     | Direct function calls in structured format              |
| Efficiency      | Lower, due to parsing errors                 | Higher, with reduced parsing overhead                   |
| Reliability     | More prone to errors and hallucinations      | More reliable and accurate tool execution               |
| LLM Requirement | Works with any LLM                           | Requires LLMs supporting function calling               |
| Implementation  | Primarily through careful prompt engineering | Often facilitated by SDKs and frameworks like LangGraph |

## How to create a ReAct agent from scratch with LangGraph

We know have base understanding on how ReAct agents work. Now, let's build our own from scratch. We are going to use LangGraph and Gemini 2.5 Pro. LangGraph is framework for building controllable agents. LangGraph comes already with prebuilt ReAct agent [create\_react\_agent](https://langchain-ai.github.io/langgraph/reference/prebuilt/#langgraph.prebuilt.chat_agent_executor.create_react_agent), but sometimes you might want more control and customization. Additionally, It is good to understand the underlying concepts and how to build your own ReAct agent from scratch.

LangGraph models agent as graphs. You define the behavior of agents using three key components:

- `State`: Shared data structure that represents the current snapshot of your application. It can be any Python type, but is typically a TypedDict or Pydantic BaseModel, which is shared across all nodes.
- `Nodes`: Encodes logic of your agents. They receive the current State as input, perform some computation or side-effect, and return an updated State, e.g. LLM calls, tool calls, etc.
- `Edges`: Determine which Node to execute next based on the current State. They can be conditional branches or fixed transitions.

First, we installed required packages and set our API keys. If you don't have an API Key yet you can get one for free in the [Google AI Studio](https://aistudio.google.com/app/apikey).

Python

```python
%pip install langgraph langchain-google-genai geopy requests
```

Python

```python
import os 
 
# Read your API key from the environment variable or set it manually
api_key = os.getenv("GEMINI_API_KEY","xxx")
```

We are going to create the most basic ReAct agents, which uses mocked 1 tool to retrieve the weather for a given location. For this we need to store the conversation history as a list of messages in our graph state. We are going to use the [add\_messages](https://langchain-ai.github.io/langgraph/reference/graphs/#langgraph.graph.message.add_messages) helper function to add messages to the state. The `add_messages` function is a [reducer](https://langchain-ai.github.io/langgraph/concepts/low_level/#reducers) that merges two lists of messages, updating existing messages by ID and ensures the state is "append-only", unless the new message has the same ID as an existing message. For demonstration purposes we also store the number of steps in the state.

Note: Since having a list of messages in the state is so common, there exists a prebuilt state called `MessagesState` which makes it easy to use messages.

Python

```python
from typing import Annotated,Sequence, TypedDict
 
from langchain_core.messages import BaseMessage 
from langgraph.graph.message import add_messages # helper function to add messages to the state
 
 
class AgentState(TypedDict):
    """The state of the agent."""
    messages: Annotated[Sequence[BaseMessage], add_messages]
    number_of_steps: int
```

Next, we define our weather tool.

Python

```python
from langchain_core.tools import tool
from geopy.geocoders import Nominatim
from pydantic import BaseModel, Field
import requests
 
geolocator = Nominatim(user_agent="weather-app") 
 
class SearchInput(BaseModel):
    location:str = Field(description="The city and state, e.g., San Francisco")
    date:str = Field(description="the forecasting date for when to get the weather format (yyyy-mm-dd)")
 
@tool("get_weather_forecast", args_schema=SearchInput, return_direct=True)
def get_weather_forecast(location: str, date: str):
    """Retrieves the weather using Open-Meteo API for a given location (city) and a date (yyyy-mm-dd). Returns a list dictionary with the time and temperature for each hour."""
    location = geolocator.geocode(location)
    if location:
        try:
            response = requests.get(f"https://api.open-meteo.com/v1/forecast?latitude={location.latitude}&longitude={location.longitude}&hourly=temperature_2m&start_date={date}&end_date={date}")
            data = response.json()
            return {time: temp for time, temp in zip(data["hourly"]["time"], data["hourly"]["temperature_2m"])}
        except Exception as e:
            return {"error": str(e)}
    else:
        return {"error": "Location not found"}
 
tools = [get_weather_forecast]
```

Next, we initalize our model and bind the tools to the model.

Python

```python
from langchain_google_genai import ChatGoogleGenerativeAI
  
# Create LLM class 
llm = ChatGoogleGenerativeAI(
    model= "gemini-2.5-pro-exp-03-25", # replace with "gemini-2.0-flash"
    temperature=1.0,
    max_tokens=None,
    timeout=None,
    max_retries=2,
    google_api_key=api_key,
)
 
# Bind tools to the model
model = llm.bind_tools([get_weather_forecast])
 
# Test the model with tools
model.invoke("What is the weather in Berlin on 12th of March 2025?")
```

The last step before we can run our agent is to define our nodes and edges. In our examples we have two nodes and 1 edge.

- `call_tool` node that executes our tool method. LangGraph has a prebuilt node for this called [ToolNode](https://langchain-ai.github.io/langgraph/how-tos/tool-calling/).
- `call_model` node that uses the `model_with_tools` to call the model.
- `should_continue` edge that decides whether to call the tool or the model.

The number of nodes and edges is not fixed. You can add as many nodes and edges as you want to your graph. For example, you could add a node for adding structured output or self-verification/reflection node to check the model output before calling the tool or the model.

Python

```python
import json
from langchain_core.messages import ToolMessage, SystemMessage
from langchain_core.runnables import RunnableConfig
 
tools_by_name = {tool.name: tool for tool in tools}
 
# this is similar to customizing the create_react_agent with 'prompt' parameter, but is more flexible
# system_prompt = SystemMessage(
#     "You are a helpful assistant that use tools to access and retrieve information from a weather API. Today is 2025-03-04. Help the user with their questions. Use the history to answer the question."
# )
 
# Define our tool node
def call_tool(state: AgentState):
    outputs = []
    # Iterate over the tool calls in the last message
    for tool_call in state["messages"][-1].tool_calls:
        # Get the tool by name
        tool_result = tools_by_name[tool_call["name"]].invoke(tool_call["args"])
        outputs.append(
            ToolMessage(
                content=tool_result,
                name=tool_call["name"],
                tool_call_id=tool_call["id"],
            )
        )
    return {"messages": outputs}
 
def call_model(
    state: AgentState,
    config: RunnableConfig,
):
    # Invoke the model with the system prompt and the messages
    response = model.invoke(state["messages"], config)
    # We return a list, because this will get added to the existing messages state using the add_messages reducer
    return {"messages": [response]}
 
 
# Define the conditional edge that determines whether to continue or not
def should_continue(state: AgentState):
    messages = state["messages"]
    # If the last message is not a tool call, then we finish
    if not messages[-1].tool_calls:
        return "end"
    # default to continue
    return "continue"
```

Awesome, now we have all the components to build our agent. Let's put them together.

Python

```python
from langgraph.graph import StateGraph, END
 
# Define a new graph with our state
workflow = StateGraph(AgentState)
 
# 1. Add our nodes 
workflow.add_node("llm", call_model)
workflow.add_node("tools",  call_tool)
# 2. Set the entrypoint as `agent`, this is the first node called
workflow.set_entry_point("llm")
# 3. Add a conditional edge after the `llm` node is called.
workflow.add_conditional_edges(
    # Edge is used after the `llm` node is called.
    "llm",
    # The function that will determine which node is called next.
    should_continue,
    # Mapping for where to go next, keys are strings from the function return, and the values are other nodes.
    # END is a special node marking that the graph is finish.
    {
        # If `tools`, then we call the tool node.
        "continue": "tools",
        # Otherwise we finish.
        "end": END,
    },
)
# 4. Add a normal edge after `tools` is called, `llm` node is called next.
workflow.add_edge("tools", "llm")
 
# Now we can compile and visualize our graph
graph = workflow.compile()
```

We can visualize our graph using the `draw_mermaid_png` method.

Python

```python
 
from IPython.display import Image, display
 
display(Image(graph.get_graph().draw_mermaid_png()))
```

![png](https://www.philschmid.de/static/blog/langgraph-gemini-2-5-react-agent/langgraph-react-agent_16_0.png)

Awesome, now lets our agent.

Python

```python
# Create our initial message dictionary
inputs = {"messages": [("user", "How is the weather in Berlin on 12th of March 2025?")]}
 
# call our graph with streaming to see the steps
 
for state in graph.stream(inputs, stream_mode="values"):
    last_message = state["messages"][-1]
    last_message.pretty_print()
```

Result:

```plaintext
================================[1m Human Message [0m=================================
 
How is the weather in Berlin on 12th of March 2025?
==================================[1m Ai Message [0m==================================
Tool Calls:
    get_weather_forecast (162a0f9f-352f-4b06-b6be-e071d6eee35b)
    Call ID: 162a0f9f-352f-4b06-b6be-e071d6eee35b
    Args:
    date: 2025-03-12
    location: Berlin
=================================[1m Tool Message [0m=================================
Name: get_weather_forecast
 
{'2025-03-12T00:00': 4.5, '2025-03-12T01:00': 4.5, '2025-03-12T02:00': 4.8, '2025-03-12T03:00': 5.2, '2025-03-12T04:00': 5.2, '2025-03-12T05:00': 5.1, '2025-03-12T06:00': 5.0, '2025-03-12T07:00': 5.1, '2025-03-12T08:00': 5.6, '2025-03-12T09:00': 6.2, '2025-03-12T10:00': 7.0, '2025-03-12T11:00': 7.3, '2025-03-12T12:00': 7.5, '2025-03-12T13:00': 7.8, '2025-03-12T14:00': 8.2, '2025-03-12T15:00': 8.1, '2025-03-12T16:00': 7.9, '2025-03-12T17:00': 7.6, '2025-03-12T18:00': 7.1, '2025-03-12T19:00': 6.9, '2025-03-12T20:00': 5.5, '2025-03-12T21:00': 6.0, '2025-03-12T22:00': 5.0, '2025-03-12T23:00': 4.7}
==================================[1m Ai Message [0m==================================
 
OK. Here is the weather forecast for Berlin on March 12th, 2025:
 
The temperature will range from 4.5°C in the early morning to a high of 8.2°C in the afternoon. Temperatures will start around 4.5°C at midnight, rise through the morning to reach the peak in the early afternoon (around 2 PM), and then cool down again in the evening, dropping to around 4.7°C by 11 PM.
```

We can now continue with our conversation and for example ask for the weather in another city or let it compare it.

Python

```python
state["messages"].append(("user", "Would it be in Munich warmer?"))
 
 
for state in graph.stream(state, stream_mode="values"):
    last_message = state["messages"][-1]
    last_message.pretty_print()
```

Result:

```plaintext
================================[1m Human Message [0m=================================
    
Would it be in Munich warmer?
==================================[1m Ai Message [0m==================================
Tool Calls:
    get_weather_forecast (1736dcae-c173-4b90-8c07-3751b6fd848d)
    Call ID: 1736dcae-c173-4b90-8c07-3751b6fd848d
    Args:
    date: 2025-03-12
    location: Munich
=================================[1m Tool Message [0m=================================
Name: get_weather_forecast
 
{'2025-03-12T00:00': 5.3, '2025-03-12T01:00': 5.0, '2025-03-12T02:00': 4.7, '2025-03-12T03:00': 3.8, '2025-03-12T04:00': 3.5, '2025-03-12T05:00': 3.4, '2025-03-12T06:00': 3.5, '2025-03-12T07:00': 4.5, '2025-03-12T08:00': 4.9, '2025-03-12T09:00': 5.7, '2025-03-12T10:00': 6.6, '2025-03-12T11:00': 8.0, '2025-03-12T12:00': 9.2, '2025-03-12T13:00': 10.1, '2025-03-12T14:00': 10.6, '2025-03-12T15:00': 9.9, '2025-03-12T16:00': 9.6, '2025-03-12T17:00': 9.3, '2025-03-12T18:00': 8.9, '2025-03-12T19:00': 8.2, '2025-03-12T20:00': 7.5, '2025-03-12T21:00': 6.9, '2025-03-12T22:00': 6.3, '2025-03-12T23:00': 5.6}
==================================[1m Ai Message [0m==================================
 
Yes, it looks like Munich will be warmer than Berlin on March 12th, 2025, especially during the afternoon.
 
*   **Munich:** The temperature is expected to reach a high of about 10.6°C around 2 PM. The lowest temperature will be around 3.4°C in the early morning.
*   **Berlin:** The high is predicted to be 8.2°C, also around 2 PM, with a low of 4.5°C.
 
So, while Munich might start a bit colder in the very early morning, its daytime high will be noticeably warmer than Berlin's.
```

## Conclusion

Building ReAct agents with LangGraph and Gemini 2.5 Pro or Gemini 2.0 Flash enables Developers to build production-ready agents with the flexibility to customize the control flow and the reliability to execute complex tasks.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
