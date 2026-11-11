---
id: mcp-tool-get-supported-formats
url: editor/mcp/tools-reference/get-supported-formats
title: get_supported_formats
weight: 7
description: "The get_supported_formats MCP tool lists the formats GroupDocs.Editor opens and saves per format family, or the formats one document can be saved to."
keywords: get_supported_formats MCP, which formats can AI edit, save docx as pdf epub, editor format families
productName: GroupDocs.Editor MCP Server
generated: true
serverVersion: 26.10.0
toc: True
---

`get_supported_formats` answers *"can it edit this, and what can it save it as?"*. Without a file it returns the catalog per **format family**; with a file, that file's family and save targets. Example prompt: *"What can I save travel-policy.pdf as?"*

**Tool description (as the AI agent sees it):**

> Lists the formats GroupDocs.Editor can open for editing and save to. Without a file it returns the whole catalog per format family; with a file it returns that file's family and the formats it can be saved to. Call this when the user asks which formats are supported, or before save_from_html / replace_text with an unusual target. Families: WordProcessing, Spreadsheet, Presentation, FixedLayout (PDF), Textual (TXT, HTML, XML, Markdown, JSON), Email, Ebook. Returns JSON: without a file { families: [ { family, loadFormats[], saveFormats[] } ] }; with a file { fileName, family, saveFormats[] }. Extensions are lower-case without a dot. On failure isError is set and the text starts with 'Supported-formats lookup failed for'; a file that cannot be found returns the list of available files instead.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | no | Optional: a document to list save targets for — [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |

## Example call

```json
{
  "name": "get_supported_formats",
  "arguments": {
    "file": {
      "filePath": "travel-policy.pdf"
    }
  }
}
```

## Result

JSON — captured from server 26.10.0:

```json
{
  "fileName": "travel-policy.pdf",
  "family": "FixedLayout",
  "saveFormats": [ "docx", "doc", "docm", "dot", "dotx", "dotm", "rtf", "odt",
                   "ott", "pdf", "xps", "txt", "md", "mhtml", "epub", "mobi",
                   "azw3" ]
}
```

Without `file`, the response lists `families[]`, each with `loadFormats` and `saveFormats` — WordProcessing, Spreadsheet, Presentation, FixedLayout (PDF), Textual, and the e-book and email families. The full matrix: [Supported formats]({{< ref "editor/mcp/supported-formats.md" >}}).

## Example prompts

* *"Which formats can you edit?"*
* *"Can this PDF be saved as Word?"*
* *"What can I turn this presentation into?"*
