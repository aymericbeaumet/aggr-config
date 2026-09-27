---
title: Google Gemma 3 Function Calling Example
link: https://www.philschmid.de/gemma-function-calling
source: philschmid-de
published: 2025-03-14T00:00:00Z
updated: 2025-03-14T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to use function calling with Google DeepMind Gemma 3 27B It
content: extracted
html: 2025-03-14-google-gemma-3-function-calling-example.html
preview:
  file: 2025-03-14-google-gemma-3-function-calling-example.preview-f8d0a04e362f.webp
  width: 256
  height: 144
  alt: Google Gemma 3 Function Calling Example
  color: '#1f1e2b'
images:
- source: https://www.philschmid.de/static/blog/gemma-function-calling/thumbnail.jpg
  original:
    file: 2025-03-14-google-gemma-3-function-calling-example.image-c42c56a95598.jpg
    width: 961
    height: 540
  color: '#010101'
- source: https://www.philschmid.de/static/blog/gemini-function-calling/function-calling.png
  original:
    file: 2025-03-14-google-gemma-3-function-calling-example.image-606ac3d766fe.png
    width: 746
    height: 599
  variants:
  - file: 2025-03-14-google-gemma-3-function-calling-example.image-838452892105.webp
    width: 320
    height: 257
  - file: 2025-03-14-google-gemma-3-function-calling-example.image-ee3297f711f6.webp
    width: 746
    height: 599
  color: '#fdfdfd'
---

March 14, 2025 8 minute read [View Code](https://github.com/philschmid/gemini-samples/blob/main/examples/gemma-function-calling.ipynb)

Google Gemma 3 27B It is an open, multilingual, multimodal Vision-Language model. It handles context windows up to 128k tokens, understands over 140 languages, and offers improved math, reasoning, and chat capabilities, including structured outputs and function calling.

Gemma 3 can be used for agentic workflows and has very strong instruction following capabilities. While there are no dedicated tool/function calling special tokens, you can prompt it to do function calling through careful instruction. Gemma 3 27B is available via [AI Studio](https://aistudio.google.com/prompts/new_chat?model=gemma-3-27b-it) and the Gen AI API. Get your API key from [AI Studio](https://aistudio.google.com/apikey).

Function calling is the capability to connect LLMs to external tools and to interact with your code and APIs in a structured way. Instead of generating text responses, LLMs understand when to call specific functions and provide the necessary parameters to execute real-world actions.

![Function calling](https://www.philschmid.de/static/blog/gemini-function-calling/function-calling.png)

Function Calling follows these steps:

1. Your application sends a prompt to the LLM along with function definitions
2. The LLM analyzes the prompt and decides whether to respond directly or use defined functions
3. If using functions, the LLM generates structured arguments for the function call
4. Your application receives the function call details and executes the actual function
5. The function results are sent back to the LLM
6. The LLM provides a final response incorporating the function results

## Raw Text Example of how function calling can be implement with Gemma 3

Below is an textual example of how to use function calling with Gemma 3 27B It. In this example the first user message includes the general instruction how and when to use function calling and example flow. The prompt with the Gemma 3 template would look like this:

1. Send user message with instructions and function definitons and first user message.

````plaintext
<bos><start_of_turn>user
At each turn, if you decide to invoke any of the function(s), it should be wrapped with ```tool_code```. The python methods described below are imported and available, you can only use defined methods. The generated code should be readable and efficient. The response to a method will be wrapped in ```tool_output``` use it to call more tools or generate a helpful, friendly response. When using a ```tool_call``` think step by step why and how it should be used.
 
The following Python methods are available:
 
\`\`\`python
def convert(amount: float, currency: str, new_currency: str) -> float:
    """Convert the currency with the latest exchange rate
 
    Args:
      amount: The amount of currency to convert
      currency: The currency to convert from
      new_currency: The currency to convert to
    """
\`\`\`
 
 
User: What is $200,000 in EUR?<end_of_turn>
<start_of_turn>model
````

*Note: The \`\`\` should not be escaped when you use it, but my blog cannot render \`\`\` inside a code block. See [code](https://github.com/philschmid/gemini-samples/blob/main/examples/gemma-function-calling.ipynb).*

3. Handle Model response when a tool/function is used.

```plaintext
Okay, I need to convert $200,000 to EUR. I will use the `convert` function for this.
\`\`\`tool_code
convert(amount=200000.0, currency="USD", new_currency="EUR")
\`\`\`
```

4. Execute local function and create tool output string

```plaintext
\`\`\`tool_output
180000.0
\`\`\`
```

5. Send the tool output as new request to the model

````plaintext
<bos><start_of_turn>user
At each turn, if you decide to invoke any of the function(s), it should be wrapped with ```tool_code```. The python methods described below are imported and available, you can only use defined methods. The generated code should be readable and efficient. The response to a method will be wrapped in ```tool_output``` use it to call more tools or generate a helpful, friendly response. When using a ```tool_call``` think step by step why and how it should be used.
 
The following Python methods are available:
 
\`\`\`python
def convert(amount: float, currency: str, new_currency: str) -> float:
    """Convert the currency with the latest exchange rate
 
    Args:
      amount: The amount of currency to convert
      currency: The currency to convert from
      new_currency: The currency to convert to
    """
\`\`\`
 
User: What is $200,000 in EUR?<end_of_turn>
<start_of_turn>model
Okay, I need to convert $200,000 to EUR. I will use the `convert` function for this.
\`\`\`tool_code
convert(amount=200000.0, currency="USD", new_currency="EUR")
\`\`\`<end_of_turn>
<start_of_turn>user
\`\`\`tool_output
180000.0
\`\`\`<end_of_turn>
<start_of_turn>model
````

6. Final Response: `$200,000 is approximately €180,000`

## Function Calling Example with Gemma 3 27B and Python.

Now, let's test this using the GenAI API. If you want to do this locally, e.g. ollama. You can just use the prompts and simulate the function execution. So first install the `google-genai` SDK.

Python

```python
%pip install google-genai
```

Then we create our `client`, define Gemma as model id and create a helper method `extract_tool_call`. This method parses the model responses and checks if there is a \`\`\`tool\_code\`\`\`. If there is one it uses the `eval` method to run it, extract the result and create a \`\`\`tool\_output\`\`\`.

*Note: We use `eval` only for demonstration purposes if you plan to use this in production you should add more security and safety as it will execute model generated code in your environment.*

Python

````python
import os
from google import genai
import re 
# create client
api_key = os.getenv("GEMINI_API_KEY","xxx")
client = genai.Client(api_key=api_key)
 
# speicfy the model id
model_id = "gemma-3-27b-it"
 
# extract the tool call from the response
def extract_tool_call(text):
    import io
    from contextlib import redirect_stdout
 
    pattern = r"```tool_code\s*(.*?)\s*```"
    match = re.search(pattern, text, re.DOTALL)
    if match:
        code = match.group(1).strip()
        # Capture stdout in a string buffer
        f = io.StringIO()
        with redirect_stdout(f):
            result = eval(code)
        output = f.getvalue()
        r = result if output == '' else output
        return f'```tool_output\n{r}\n```'''
    return None
 
````

Next, we define a simple example for a function we want to use. Here is is a `convert` method that simulates the conversion calcuation of currencies. Since we use `eval` the method we want to use for function calling needs to be available in the environment.

We define our first user prompt including our instructions and function signature with a docstring and args and a template string for our user message.

Python

````python
def convert(amount: float, currency: str, new_currency: str) -> float:
  # demo implementation
  return amount * 0.9
 
 
instruction_prompt_with_function_calling = '''At each turn, if you decide to invoke any of the function(s), it should be wrapped with ```tool_code```. The python methods described below are imported and available, you can only use defined methods. The generated code should be readable and efficient. The response to a method will be wrapped in ```tool_output``` use it to call more tools or generate a helpful, friendly response. When using a ```tool_call``` think step by step why and how it should be used.
 
The following Python methods are available:
 
\`\`\`python
def convert(amount: float, currency: str, new_currency: str) -> float:
    """Convert the currency with the latest exchange rate
 
    Args:
      amount: The amount of currency to convert
      currency: The currency to convert from
      new_currency: The currency to convert to
    """
 
def get_exchange_rate(currency: str, new_currency: str) -> float:
    """Get the latest exchange rate for the currency pair
 
    Args:
      currency: The currency to convert from
      new_currency: The currency to convert to
    """
\`\`\`
 
User: \{user_message\}'''
````

*Note: The \`\`\` should not be escaped when you use it, but my blog cannot render \`\`\` inside a code block. See [code](https://github.com/philschmid/gemini-samples/blob/main/examples/gemma-function-calling.ipynb).*

The `genai` SDK supports stateful chat session, which makes it quite easy to test our example as we can easily append the different messages. First, we start with a simple greeting to see what Gemma does.

Python

```python
chat = client.chats.create(model=model_id)
 
response = chat.send_message(instruction_prompt_with_function_calling.format(user_message="hello"))
print(response.text)
# Hello! How can I help you today? Do you want to convert some currency, or get an exchange rate?
```

Nice! it greeted us back and didn't use any function call. Okay now lets ask it to convert some currency.

Python

````python
response = chat.send_message("What is $200,000 in EUR?")
print(response.text)
Okay, I need to convert $200,000 to EUR. I will use the `convert` function for this.
# ```tool_code
# convert(amount=200000.0, currency="USD", new_currency="EUR")
# ```
````

Great! it generated out \`\`\`tool\_code\`\`\`, which we can now use and extract and to cool our method.

Python

````python
call_response = extract_tool_call(response.text)
print(call_response)
# ```tool_output
# 180000.0
# ```
````

After we have the response from our tool call we send a final message to generate a user friendly output.

Python

```python
response = chat.send_message(call_response)
print(response.text)
# $200,000 is equivalent to €180,000. Is there anything else I can help you with?
```

## Conclusion

Function calling enables us to build powerful AI assistants that can access real-time data, perform actions, handle complex interactions, and provide natural language interfaces to APIs and tools, making it an increasingly important capability for practical AI applications that interact with the real world.

This is a simplified example on how you could implement function calling with Gemma 3. I didn't run detailed evaluation or benchmarks. Gemma 3 is an open model with strong reasoning and instruction following capabilities. This allows you to further optimize the prompt used to match your use case and data or further fine-tune on data with similar format to customize its agentic capabilities.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
