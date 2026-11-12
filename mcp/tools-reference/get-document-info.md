---
id: mcp-tool-get-document-info
url: editor/mcp/tools-reference/get-document-info
title: get_document_info
weight: 1
description: "The get_document_info MCP tool inspects a document before editing: format family, page, worksheet or slide count, protection, and the formats it can be saved to."
keywords: get_document_info MCP tool, inspect document before editing, how many worksheets slides AI, document format family MCP
productName: GroupDocs.Editor MCP Server
generated: true
serverVersion: 26.10.0
toc: True
---

`get_document_info` tells the agent what it is about to edit: the **format family** (WordProcessing, Spreadsheet, Presentation, FixedLayout for PDF, Textual, Ebook, Email), the number of pages — **worksheets** for a workbook, **slides** for a presentation — whether it is password-protected, and which formats it can be saved to. Example prompt: *"How many worksheets does budget.xlsx have?"*

**Tool description (as the AI agent sees it):**

> Inspects a document before editing it: its format family, format, number of pages (worksheets for spreadsheets, slides for presentations), size, whether it is password-protected, and which formats it can be saved to. Supports DOCX, DOC, RTF, ODT, XLSX, XLS, ODS, CSV, PPTX, PPT, ODP, PDF, TXT, HTML, XML, Markdown, EPUB, MOBI, EML, MSG and more. Call this first when the user asks how many pages, worksheets or slides a document has, or whether it can be edited. Do NOT pre-check whether files exist — just pass the filename the user provided; a missing file returns the list of available files. Returns JSON: { fileName, family, format: { name, extension, mime }, pageCount, size, isEncrypted, infoType, canEdit, canSaveTo[] }. family is one of WordProcessing, Spreadsheet, Presentation, FixedLayout, Textual, Email, Ebook. On failure isError is set and the text starts with 'Document-info lookup failed for'; a file that cannot be found returns the list of available files instead.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes | The document — see [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password, for password-protected documents |

## Example call

```json
{
  "name": "get_document_info",
  "arguments": {
    "file": {
      "filePath": "budget.xlsx"
    }
  }
}
```

## Result

JSON — captured from server 26.10.0:

```json
{
  "fileName": "budget.xlsx",
  "family": "Spreadsheet",
  "format": {
    "name": "Office Open XML Workbook Macro-Free (XLSX)",
    "extension": "xlsx",
    "mime": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
  },
  "pageCount": 3,
  "size": 6100,
  "isEncrypted": false,
  "infoType": "SpreadsheetDocumentInfo",
  "canEdit": true,
  "canSaveTo": [ "xlsx", "xls", "xlsm", "xlsb", "xltx", "xltm", "xlt",
                 "ods", "csv", "tsv", "pdf", "xps", "txt", "mhtml" ]
}
```

| Field | Meaning |
|---|---|
| `family` | decides which parameters apply: `worksheet` for Spreadsheet, `slide` for Presentation, `pages` for FixedLayout (PDF) |
| `pageCount` | pages — or worksheets, or slides |
| `isEncrypted` | `true` means the tools need `password` |
| `canSaveTo` | valid `format` values for [`replace_text`]({{< ref "editor/mcp/tools-reference/replace-text.md" >}}) and [`save_from_html`]({{< ref "editor/mcp/tools-reference/save-from-html.md" >}}) |

On failure the text starts with `Document-info lookup failed for` and `isError` is set.

## Example prompts

* *"How many worksheets does budget.xlsx have?"*
* *"Is this PDF password-protected?"*
* *"What can I save this presentation as?"*
