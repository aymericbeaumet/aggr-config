---
title: 'From PDFs to Insights: Structured Outputs from PDFs with Gemini 2.0'
link: https://www.philschmid.de/gemini-pdf-to-data
source: philschmid-de
published: 2025-02-07T00:00:00Z
updated: 2025-02-07T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to extract structured data from PDFs with Gemini 2.0 and Pydantic.
content: extracted
html: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.html
preview:
  file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.preview-9ddcd9b0a525.webp
  width: 256
  height: 136
  alt: 'From PDFs to Insights: Structured Outputs from PDFs with Gemini 2.0'
  color: '#cad9d4'
images:
- source: https://www.philschmid.de/static/blog/gemini-pdf-to-data/thumbnail.jpg
  original:
    file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.image-de63d01fed6a.jpg
    width: 1700
    height: 900
  color: '#e5ebfe'
- source: https://storage.googleapis.com/generativeai-downloads/data/pdf_structured_outputs/invoice.png
  original:
    file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.image-634bd444d7e5.png
    width: 760
    height: 1075
  variants:
  - file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.image-c9411805e11d.webp
    width: 320
    height: 453
  - file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.image-600bf6f1ae74.webp
    width: 640
    height: 905
  - file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.image-af3142ba5b72.webp
    width: 760
    height: 1075
  color: '#fcfcfc'
- source: https://storage.googleapis.com/generativeai-downloads/data/pdf_structured_outputs/handwriting_form.jpg
  original:
    file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.image-46387d5c0a26.jpg
    width: 612
    height: 792
  variants:
  - file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.image-6e7088cc6066.webp
    width: 320
    height: 414
  - file: 2025-02-07-from-pdfs-to-insights-structured-outputs-from-pdfs-with.image-f33298e3df3d.webp
    width: 612
    height: 792
  color: '#fafafa'
---

This week [Google DeepMind released Gemini 2.0](https://developers.googleblog.com/en/gemini-2-family-expands/), including Gemini 2.0 Flash (General Available), Gemini 2.0 Flash-Lite (New cost-efficient) and Gemini 2.0 Pro (Experimental). All models support up to at least 1 million input tokens with support for text, images and audio and function calling/structured outputs.

This opens up a cool use cases especially with PDFs. Converting PDF into structured or machine-readable text has been a major headache. What if we could transform PDFs from documents into structured data? This is where Gemini 2.0 comes into play.

In this tutorial, you will learn how to extract structured information, like invoice numbers, dates, directly from your PDF documents using Gemini 2.0:

1. [Set up Environment and create inference Client](https://www.philschmid.de/gemini-pdf-to-data#1-set-up-environment-and-create-inference-client)
2. [Work with PDFs and other files](https://www.philschmid.de/gemini-pdf-to-data#2-work-with-pdfs-and-other-files)
3. [Structured outputs with Gemini 2.0 and Pydantic](https://www.philschmid.de/gemini-pdf-to-data#3-structured-outputs-with-gemini-20-and-pydantic)
4. [Extract Structured data from PDFs using Gemini 2.0](https://www.philschmid.de/gemini-pdf-to-data#4-extract-structured-data-from-pdfs-using-gemini-20)

## 1\. Set up Environment and create inference Client

The first task is to install the `google-genai` [Python SDK](https://googleapis.github.io/python-genai/) and obtain an API key. If you don”t have a can get one from Google AI Studio: [Get a Gemini API key](https://aistudio.google.com/app/apikey).

Bash

```bash
%pip install "google-genai>=1"
```

Once you have the SDK and API key, you can create a client and define the model you are going to use the new Gemini 2.0 Flash model, which is available via [free tier](https://ai.google.dev/pricing#2_0flash) with 1,500 request per day (at 2025-02-06).

Python

```python
from google import genai
# Create a client
api_key = "XXXXX"
client = genai.Client(api_key=api_key)
 
# Define the model you are going to use
model_id =  "gemini-2.0-flash" # or "gemini-2.0-flash-lite-preview-02-05"  , "gemini-2.0-pro-exp-02-05"
```

*Note: If you want to use Vertex AI see [here](https://googleapis.github.io/python-genai/#create-a-client) how to create your client*

## 2\. Work with PDFs and other files

Gemini models are able to process [images and videos](https://ai.google.dev/gemini-api/docs/vision?lang=python#image-input), which can used with base64 strings or using the `files`api. After uploading the file you can include the file uri in the call directly. The Python API includes a [upload](https://googleapis.github.io/python-genai/#upload) and [delete](https://googleapis.github.io/python-genai/#delete) method.

For this example you have 2 PDFs samples, one basic invoice and on form with and written values.

Bash

```bash
!wget -q -O https://storage.googleapis.com/generativeai-downloads/data/pdf_structured_outputs/handwriting_form.pdf
!wget -q -O https://storage.googleapis.com/generativeai-downloads/data/pdf_structured_outputs/invoice.pdf
```

You can now upload the files using our client with the `upload` method. Let's try this for one of the files.

Python

```python
invoice_pdf = client.files.upload(file="invoice.pdf", config={'display_name': 'invoice'})
```

*Note: The File API lets you store up to 20 GB of files per project, with a per-file maximum size of 2 GB. Files are stored for 48 hours. They can be accessed in that period with your API key, but they cannot be downloaded. File uploads are available at no cost.*

After a file is uploaded you can check to how many tokens it got converted. This not only help us understand the context you are working with it also helps to keep track of the cost.

Python

```python
file_size = client.models.count_tokens(model=model_id,contents=invoice_pdf)
print(f'File: {invoice_pdf.display_name} equals to {file_size.total_tokens} tokens')
# File: invoice equals to 821 tokens
```

## 3\. Structured outputs with Gemini 2.0 and Pydantic

Structured Outputs is a feature that ensures Gemini always generate responses that adhere to a predefined format, such as JSON Schema. This means you have more control over the output and how to integrate it into our application as it is guaranteed to return a valid JSON object with the schema you define.

Gemini 2.0 currenlty supports 3 dfferent types of how to define a JSON schemas:

- A single python type, as you would use in a [typing annotation](https://docs.python.org/3/library/typing.html).
- A Pydantic [BaseModel](https://docs.pydantic.dev/latest/concepts/models/)
- A dict equivalent of [genai.types.Schema](https://googleapis.github.io/python-genai/genai.html#genai.types.Schema) / [Pydantic BaseModel](https://docs.pydantic.dev/latest/concepts/models/)

Lets look at quick text-based example.

Python

```python
from pydantic import BaseModel, Field
 
# Define a Pydantic model
# Use the Field class to add a description and default value to provide more context to the model
class Topic(BaseModel):
    name: str = Field(description="The name of the topic")
 
class Person(BaseModel):
    first_name: str = Field(description="The first name of the person")
    last_name: str = Field(description="The last name of the person")
    age: int = Field(description="The age of the person, if not provided please return 0")
    work_topics: list[Topic] = Field(description="The fields of interest of the person, if not provided please return an empty list")
 
 
# Define the prompt
prompt = "Philipp Schmid is a Senior AI Developer Relations Engineer at Google DeepMind working on Gemini, Gemma with the mission to help every developer to build and benefit from AI in a responsible way.  "
 
# Generate a response using the Person model
response = client.models.generate_content(model=model_id, contents=prompt, config={'response_mime_type': 'application/json', 'response_schema': Person})
 
# print the response as a json string
print(response.text)
 
# sdk automatically converts the response to the pydantic model
philipp: Person = response.parsed
 
# access an attribute of the json response
print(f"First name is {philipp.first_name}")
```

Now, let's combine the File API and structured output to extract information from our PDFs. You can create a simple method that accepts a local file path and a pydantic model and return the structured data for us. The method will:

1. Upload the file to the File API
2. Generate a structured response using the Gemini API
3. Convert the response to the pydantic model and return it

Python

```python
def extract_structured_data(file_path: str, model: BaseModel):
    # Upload the file to the File API
    file = client.files.upload(file=file_path, config={'display_name': file_path.split('/')[-1].split('.')[0]})
    # Generate a structured response using the Gemini API
    prompt = f"Extract the structured data from the following PDF file"
    response = client.models.generate_content(model=model_id, contents=[prompt, file], config={'response_mime_type': 'application/json', 'response_schema': model})
    # Convert the response to the pydantic model and return it
    return response.parsed
```

In our Example every PDF is a different to each other. So you want to define unique Pydantic models for each PDF to show the performance of the Gemini 2.0. If you have very similar PDFs and want to extract the same information you can use the same model for all of them.

- `Invoice.pdf` : Extract the invoice number, date and all list items with description, quantity and gross worth and the total gross worth
- `handwriting_form.pdf` : Extract the form number, plan start date and the plan liabilities beginning of the year and end of the year

*Note: Using Pydantic features you can add more context to the model to make it more accurate as well as some validation to the data. Adding a comprehensive description can significantly improve the performance of the model. Libraries like [instructor](https://python.useinstructor.com/) added automatic retries based on validation errors, which can be a great help, but come at the cost of additional requests.*

### Invoice.pdf

![Invoice.pdf](https://storage.googleapis.com/generativeai-downloads/data/pdf_structured_outputs/invoice.png)

Python

```python
from pydantic import BaseModel, Field
 
class Item(BaseModel):
    description: str = Field(description="The description of the item")
    quantity: float = Field(description="The Qty of the item")
    gross_worth: float = Field(description="The gross worth of the item")
 
class Invoice(BaseModel):
    """Extract the invoice number, date and all list items with description, quantity and gross worth and the total gross worth."""
    invoice_number: str = Field(description="The invoice number e.g. 1234567890")
    date: str = Field(description="The date of the invoice e.g. 2024-01-01")
    items: list[Item] = Field(description="The list of items with description, quantity and gross worth")
    total_gross_worth: float = Field(description="The total gross worth of the invoice")
 
 
result = extract_structured_data("invoice.pdf", Invoice)
print(type(result))
print(f"Extracted Invoice: {result.invoice_number} on {result.date} with total gross worth {result.total_gross_worth}")
for item in result.items:
    print(f"Item: {item.description} with quantity {item.quantity} and gross worth {item.gross_worth}")
```

Fantastic! The model did a great job extracting the information from the invoice.

### handwriting\_form.pdf

![handwriting_form.pdf](https://storage.googleapis.com/generativeai-downloads/data/pdf_structured_outputs/handwriting_form.jpg)

Python

```python
class Form(BaseModel):
    """Extract the form number, fiscal start date, fiscal end date, and the plan liabilities beginning of the year and end of the year."""
    form_number: str = Field(description="The Form Number")
    start_date: str = Field(description="Effective Date")
    beginning_of_year: float = Field(description="The plan liabilities beginning of the year")
    end_of_year: float = Field(description="The plan liabilities end of the year")
 
result = extract_structured_data("handwriting_form.pdf", Form)
 
print(f'Extracted Form Number: {result.form_number} with start date {result.start_date}. \nPlan liabilities beginning of the year {result.beginning_of_year} and end of the year {result.end_of_year}')
# Extracted Form Number: CA530082 with start date 02/05/2022. 
# Plan liabilities beginning of the year 40000.0 and end of the year 55000.0
```

## Best Practices and Limitations

When working with Gemini 2.0 for PDF processing, keep these considerations in mind:

1. File Size Management: While the File API supports large files, it's good practice to optimize your PDFs before upload.
2. Token Limits: Check token counts when processing large documents to ensure you stay within model limits and your budget.
3. Structured Output Design: Design your Pydantic models carefully to capture all necessary information while maintaining clarity, adding descriptions and examples can improve the performance of the model.
4. Error Handling: Implement robust error handling for file uploads and processing states, inclduing retries and handeling error messages from the model.

## Conclusion

Gemini 2.0's multimodal capabilities, combined with structured output help you to process and extract information from PDFs and other files. This can eliminate complex and painfull manual or semi-automated data extraction processes. Whether you're building an invoice processing system, document analysis tool, or any other document-centric application you should try out Gemini 2.0 as it is free to test and then only $0.1 per 1M input tokens.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
