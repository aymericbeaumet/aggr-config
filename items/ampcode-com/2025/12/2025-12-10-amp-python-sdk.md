---
title: Amp Python SDK
link: https://ampcode.com/news/python-sdk
source: ampcode-com
published: 2025-12-10T00:00:00Z
updated: 2025-12-10T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'For all of you who swear by tabs and clean syntax, the Amp Python SDK is now live. You can run Amp programmatically from your Python code, just like you already do in TypeScript. Here, for example, is how you instruct Amp to migrate React components with custom toolbox tools to validate changes: import asyncio import os from amp_sdk import execute, AmpOptions prompt = """ Goal: Migrate all React components from React 17 to React 18. 1. Find all React component files (.tsx, .jsx) 2. For each component: - Update deprecated lifecycle methods - Replace ReactDOM.render with createRoot 3. Track any components that fail migration with the reason 4. Run the typecheck_test_tool after each change 5. Output a summary: migrated count, failed list with reasons """ async def main(): # Use the toolbox directory to share tools with Amp toolbox_dir = os.path.join(os.getcwd(), "toolbox") async for message in execute( prompt, AmpOptions( cwd=os.getcwd(), toolbox=toolbox_dir, visibility="workspace", dangerously_allow_all=True, ), ): if message.type == "result": if message.is_error: print(f"Error: {message.error}") else: print(f"Summary: {message.result}") if __name__ == "__main__": asyncio.run(main()) To get started, install the pip package and the Amp CLI: # Install the Amp SDK with pip $ pip install amp-sdk # Install the Amp CLI globally $ npm install -g @ampcode/cli Now you can build anything with Amp in any Python runtime environment. To get more ideas and familiar with the SDK, take a look at the examples in the manual.'
content: extracted
html: 2025-12-10-amp-python-sdk.html
preview:
  file: 2025-12-10-amp-python-sdk.preview-b07e97749fde.webp
  width: 256
  height: 134
  color: '#5f584e'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Amp+Python+SDK&date=December+10%2C+2025&tagline=Amp+Python+SDK+is+now+available&sig=842a7f2a74dea163e8e0c29387beda1092e6d2706559f62d238fa673aa69067f
  original:
    file: 2025-12-10-amp-python-sdk.image-27d3a4f73e46.png
    width: 1200
    height: 630
  variants:
  - file: 2025-12-10-amp-python-sdk.image-08b3ef4087eb.webp
    width: 320
    height: 168
  - file: 2025-12-10-amp-python-sdk.image-6876268baa16.webp
    width: 640
    height: 336
  - file: 2025-12-10-amp-python-sdk.image-972e9c555e83.webp
    width: 960
    height: 504
  - file: 2025-12-10-amp-python-sdk.image-f2d0605949a0.webp
    width: 1200
    height: 630
  color: '#221b16'
---

For all of you who swear by tabs and clean syntax, the Amp Python SDK is now live.

You can run Amp programmatically from your Python code, just like you already do in TypeScript.

Here, for example, is how you instruct Amp to migrate React components with [custom toolbox tools](https://ampcode.com/news/toolboxes) to validate changes:

```python
import asyncio
import os
from amp_sdk import execute, AmpOptions

prompt = """
	  Goal: Migrate all React components from React 17 to React 18.

    1. Find all React component files (.tsx, .jsx)
    2. For each component:
       - Update deprecated lifecycle methods
       - Replace ReactDOM.render with createRoot
    3. Track any components that fail migration with the reason
    4. Run the typecheck_test_tool after each change
    5. Output a summary: migrated count, failed list with reasons
"""

async def main():

    # Use the toolbox directory to share tools with Amp
    toolbox_dir = os.path.join(os.getcwd(), "toolbox")

    async for message in execute(
        prompt,
        AmpOptions(
            cwd=os.getcwd(),
            toolbox=toolbox_dir,
            visibility="workspace",
            dangerously_allow_all=True,
        ),
    ):

        if message.type == "result":
            if message.is_error:
                print(f"Error: {message.error}")
            else:
                print(f"Summary: {message.result}")

if __name__ == "__main__":
    asyncio.run(main())
```

To get started, install the pip package and the Amp CLI:

```bash
# Install the Amp SDK with pip
$ pip install amp-sdk

# Install the Amp CLI globally
$ npm install -g @ampcode/cli
```

Now you can build anything with Amp in any Python runtime environment. To get more ideas and familiar with the SDK, take a look at the examples in the [manual](https://ampcode.com/manual/sdk#advanced-usage).
