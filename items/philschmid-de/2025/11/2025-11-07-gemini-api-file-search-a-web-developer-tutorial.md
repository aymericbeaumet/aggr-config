---
title: 'Gemini API File Search: A Web Developer Tutorial'
link: https://www.philschmid.de/gemini-file-search-javascript
source: philschmid-de
published: 2025-11-07T00:00:00Z
updated: 2025-11-07T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to use the Gemini API File Search tool with JavaScript/TypeScript to build a Retrieval-Augmented Generation (RAG) system.
content: extracted
html: 2025-11-07-gemini-api-file-search-a-web-developer-tutorial.html
preview:
  file: 2025-11-07-gemini-api-file-search-a-web-developer-tutorial.preview-163236fd4b13.webp
  width: 256
  height: 144
  alt: 'Gemini API File Search: A Web Developer Tutorial'
  color: '#111111'
images:
- source: https://www.philschmid.de/static/blog/gemini-file-search-javascript/thumbnail.jpg
  original:
    file: 2025-11-07-gemini-api-file-search-a-web-developer-tutorial.image-e6c4b58cfc10.jpg
    width: 961
    height: 540
  color: '#000000'
---

The Gemini API File Search tool is a fully managed RAG (Retrieval-Augmented Generation) system built directly into the Gemini API. It automatically manages file storage, chunks your data, creates embeddings, and seamlessly injects the most relevant context into your prompts.

This must be expensive? No, file storage and embedding generation at query time are completely free of charge. You only pay for the initial indexing of your files at a fixed rate of $0.15 (based on the embedding) per 1 million tokens.

This tutorial will walk you through the complete lifecycle of using File Search with the JavaScript/TypeScript, you will learn how to:

1. Create a File Search Store
2. Find a Store by Display Name
3. Upload Multiple Files Concurrently
4. Advanced Upload: Chunking & Metadata
5. Run a Standard Generation Query (RAG)
6. Find a Specific Document
7. Delete a Document
8. Update a Document
9. Cleanup: Delete the File Search Store

Before we start, make sure you have an [API key from Google AI Studio](https://aistudio.google.com/api-keys) and have installed the latest SDK:

Bash

```bash
npm install @google/genai
```

Initialize your client in your JavaScript environment:

JavaScript

```js
import { GoogleGenAI } from '@google/genai';
import fs from 'fs';
import path from 'path';
 
const ai = new GoogleGenAI({});
```

### 1) Create a File Search Store

A **File Search Store** is a persistent container for your document chunks and embeddings. It's distinct from raw file storage and can hold gigabytes of data.

JavaScript

```js
const fileStoreName = 'my-example-store';
 
const createStoreOp = await ai.fileSearchStores.create({
  config: { displayName: fileStoreName }
});
 
console.log(`Store created with Name: ${createStoreOp.name}`);
```

### 2) Find a Store by Display Name

Often the creation of a store and the use are in a different application session. Since the API assigns a unique ID (`fileSearchStores/xyz...`), you need to look it up by the human-readable `displayName`.

JavaScript

```js
let fileStore = null;
// List stores with a page size limit
const pager = await ai.fileSearchStores.list({ config: { pageSize: 10 } });
let page = pager.page;
 
// Iterate through pages until we find a match
searchLoop: while (true) {
  for (const store of page) {
    if (store.displayName === fileStoreName) {
      fileStore = store;
      break searchLoop;
    }
  }
  if (!pager.hasNextPage()) break;
  page = await pager.nextPage();
}
 
if (!fileStore) {
  throw new Error(`Store with display name '${fileStoreName}' not found.`);
}
console.log(`Found store: ${fileStore.name}`);
```

### 3) Upload Multiple Files Concurrently

Speed matters. When ingesting a folder of documents, don't process them sequentially. The API supports concurrent operations, so we can use `Promise.all` to upload and process multiple files at once.

We'll use the helper method `uploadToFileSearchStore`, which handles uploading the raw file and initiating the indexing process in one step. We then monitor `operation.done` to ensure processing completes before moving on.

JavaScript

```js
const docsDir = "docs"; // Ensure you have a 'docs' folder with some text files
const files = fs.readdirSync(docsDir).map(file => path.join(docsDir, file));
 
await Promise.all(files.map(async (filePath) => {
  // 1. Initiate upload and indexing
  let operation = await ai.fileSearchStores.uploadToFileSearchStore({
    file: filePath,
    fileSearchStoreName: fileStore.name,
    config: {
      displayName: path.basename(filePath),
    }
  });
 
  // 2. Poll until the document is fully processed
  while (!operation.done) {
    await new Promise(resolve => setTimeout(resolve, 1000)); // wait 1s
    operation = await ai.operations.get({ operation });
  }
  console.log(`Processing complete for: ${path.basename(filePath)}`);
  return operation;
}));
```

### 4) Upload with Custom Chunking Strategies

By default, Gemini handles chunking intelligently. However, for specific use cases you might want tighter control over how your data is split.

You can define a `chunkingConfig` during upload to specify parameters like `maxTokensPerChunk` and `maxOverlapTokens` and `customMetadata` to attach key-value pairs to the document.

JavaScript

```js
const specialDocPath = 'special-docs/technical-manual.txt';
 
let advancedUploadOp = await ai.fileSearchStores.uploadToFileSearchStore({
  file: specialDocPath,
  fileSearchStoreName: fileStore.name,
  config: {
    displayName: 'technical-manual.txt',
    customMetadata: [
      { key: "doc_type", stringValue: "manual" },
    ],
    chunkingConfig: {
      whiteSpaceConfig: {
        maxTokensPerChunk: 500, // Smaller chunks for more precise retrieval
        maxOverlapTokens: 50    // Ensure context isn't lost between chunks
      }
    }
  }
});
 
// Wait for the file to process
while (!advancedUploadOp.done) {
  await new Promise(resolve => setTimeout(resolve, 1000));
  advancedUploadOp = await ai.operations.get({ operation: advancedUploadOp });
}
console.log("Advanced file processed.");
```

### 5) Run a Generation Query using File Search (RAG)

We don't need to manually retrieve chunks. We just tell the Gemini model to use the `fileSearch` tool and point it to our store name. Gemini understands it needs more information, searches the store, and grounds its response automatically.

JavaScript

```js
const response = await ai.models.generateContent({
  model: "gemini-2.5-flash",
  contents: "What is Gemini and what is the File API?",
  config: {
    tools: [{
      fileSearch: {
        fileSearchStoreNames: [fileStore.name]
      }
    }]
  }
});
 
console.log("Model response:", response.text);
// Optionally check response.candidates[0].groundingMetadata for citations!
```

Because we tagged our technical manual in Step 4, we can now force Gemini to *only* look at documents matching that tag by using a metadataFilter.

JavaScript

```js
const responseFiltered = await ai.models.generateContent({
  model: "gemini-2.5-flash",
  contents: "How do I reset the device according to the manual?",
  config: {
    tools: [{
      fileSearch: {
        fileSearchStoreNames: [fileStore.name],
        metadataFilter: 'doc_type="manual"'
      }
    }]
  }
});
 
console.log("Filtered response:", responseFiltered.text);
```

### 6) Find a Specific Document within a Store

You'll often need to manage individual documents within your store. You can find a specific document by its display name.

JavaScript

```js
const docToFind = 'doc1.txt';
let targetDoc = null;
 
let documentPager = await ai.fileSearchStores.documents.list({
  parent: fileStore.name,
});
 
// Iterate through the store's document list
searchDocsLoop: while (true) {
  for (const document of documentPager.page) {
    if (document.displayName === docToFind) {
      targetDoc = document;
      break searchDocsLoop;
    }
  }
  if (!documentPager.hasNextPage()) break;
  documentPager = await documentPager.nextPage();
}
 
if (!targetDoc) throw new Error(`Document '${docToFind}' not found.`);
```

### 7) Delete a Document

Currently, the standard flow to update a document in File Search is to delete the old version and upload a new one.

JavaScript

```js
await ai.fileSearchStores.documents.delete({
    name: targetDoc.name,
    config: { force: true } // Required to permanently delete from the store
});
```

### 8) Update a Document

File Search documents are immutable once indexed. To "update" a document, you must find it, delete it, and upload the new version. In this step, we will automate this entire loop to update `doc1.txt` with new information.

JavaScript

```js
const docToUpdate = 'doc1.txt'; // assuming it has new content
const localDocPath = path.join(docsDir, docToUpdate);
 
// 1. Find the existing document ID in the store based on its display name
let documentPager = await ai.fileSearchStores.documents.list({ parent: fileStore.name });
let foundDoc = null;
findLoop: while (true) {
  for (const doc of documentPager.page) {
    if (doc.displayName === docToUpdate) {
      foundDoc = doc;
      break findLoop;
    }
  }
  if (!documentPager.hasNextPage()) break;
  documentPager = await documentPager.nextPage();
}
 
// 2. If we found it, delete it
if (foundDoc) {
   await ai.fileSearchStores.documents.delete({
     name: foundDoc.name,
     config: { force: true } // 'force' is required to delete indexed docs
   });
}
 
// 3. Upload the new version
let updateOp = await ai.fileSearchStores.uploadToFileSearchStore({
  file: localDocPath,
  fileSearchStoreName: fileStore.name,
  config: { displayName: docToUpdate }
});
while (!updateOp.done) {
   await new Promise(resolve => setTimeout(resolve, 1000));
   updateOp = await ai.operations.get({ operation: updateOp });
}
 
console.log("Revision uploaded and indexed successfully.");
```

### 9) Cleanup: Delete the File Search Store

You are currently limited to 10 File Search Stores per project, so it's important to clean up resources when you are finished with development.

JavaScript

```js
await ai.fileSearchStores.delete({
    name: fileStore.name,
    config: { force: true }
});
```

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
