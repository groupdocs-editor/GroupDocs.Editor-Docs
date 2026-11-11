---
id: mcp-tool-extract-html
url: editor/mcp/tools-reference/extract-html
title: extract_html
weight: 3
description: "The extract_html MCP tool turns a document — or one worksheet, one slide, or a PDF page range — into editable HTML, so an AI agent can edit it and save it back with save_from_html."
keywords: extract_html MCP tool, convert docx to editable HTML, edit one worksheet AI, edit one slide AI agent, PDF to HTML for editing
productName: GroupDocs.Editor MCP Server
generated: true
serverVersion: 26.10.0
toc: True
---

`extract_html` opens a document **for editing**: it converts the whole document — or **one worksheet** (`worksheet`), **one slide** (`slide`), or a **PDF page range** (`pages`) — into an HTML file in the output folder, keeping fonts, styles, tables and images. The agent edits that file, then calls [`save_from_html`]({{< ref "editor/mcp/tools-reference/save-from-html.md" >}}). Example prompt: *"Open the Q3 worksheet of budget.xlsx for editing."*

**Tool description (as the AI agent sees it):**

> Opens a document for editing: converts it — or one worksheet, one slide, or a PDF page range — into editable HTML and saves the .html file to storage, keeping fonts, styles, tables and images. Edit that file, then call save_from_html to turn it back into DOCX, XLSX, PPTX, PDF or another format. For a simple find-and-replace use replace_text instead. Supports DOCX, DOC, RTF, ODT, XLSX, XLS, ODS, CSV, PPTX, PPT, ODP, PDF, TXT, HTML, XML, Markdown, EPUB, MOBI, EML and more. Do NOT pre-check whether files exist — just pass the filename the user provided; a missing file returns the list of available files. By default the HTML is self-contained (images, CSS and fonts embedded); set embedResources=false to get <name>.html plus a <name>_resources folder. Returns the saved path ('Saved to: …'), which part was extracted (e.g. 'worksheet 2 of 3'), and a plain-text preview of the content. On failure isError is set and the text starts with 'HTML extraction failed for'; a file that cannot be found returns the list of available files instead.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password, for password-protected documents |
| `worksheet` | integer | no | Spreadsheets only: which worksheet to extract, 1-based (default 1) |
| `slide` | integer | no | Presentations only: which slide to extract, 1-based (default 1) |
| `pages` | string | no | PDF only: 1-based page or inclusive range, e.g. '3' or '2-4' (default: all pages) |
| `pagination` | boolean | no | Word, PDF and text: true keeps page boundaries (paginal HTML); false (default) gives one continuous flow |
| `skipImages` | boolean | no | PDF only: true leaves images out and keeps text only |
| `embedResources` | boolean | no | true (default) embeds images, CSS and fonts in one .html file; false writes a <name>_resources folder beside it |
| `showHidden` | boolean | no | Spreadsheets and presentations: allow hidden worksheets and slides |

## Example call

```json
{
  "name": "extract_html",
  "arguments": {
    "file": {
      "filePath": "budget.xlsx"
    },
    "worksheet": 2
  }
}
```

## Result

Plain text — captured from server 26.10.0:

```text
Evaluation mode: outputs start with an evaluation banner, saved workbooks gain
an extra worksheet, and only the first 2 PDF pages, worksheets and slides can
be edited. Set GROUPDOCS_LICENSE_PATH, or the metered keys, to remove these
limits.

Extracted worksheet 2 of 3 of 'budget.xlsx' as editable HTML
Saved to: …/budget_worksheet2.html

Next: edit the saved .html and call save_from_html to write it back as DOCX,
XLSX, PPTX, PDF or another format.

Preview (plain text):
Created with evaluation version of GroupDocs.Editor © Aspose Pty Ltd 2001-2026.
All Rights Reserved. Cost centre Planned (EUR) Owner Hosting 57600 IT Support
14400 IT Travel 9000 Sales Training 6000 HR
```

(Wrapped for width.) The HTML file is named after the part: `<name>_worksheet2.html`, `<name>_slide2.html`, `<name>_pages1-2.html`, or `<name>.html` for a whole document. `embedResources` (default `true`) keeps images, CSS and fonts inside the one file; `pagination: true` keeps page boundaries for Word and PDF.

In evaluation mode the server refuses a worksheet or slide past the second, and says why:

```text
HTML extraction failed for 'budget.xlsx': Evaluation mode edits only the first
2 visible worksheets of a workbook; worksheet 3 is beyond them. Set
GROUPDOCS_LICENSE_PATH, or the metered keys, to edit every worksheet.
```

A PDF extraction holds **at most two pages** per call in evaluation mode: `pages: "1-3"` returned pages 1 and 2 and stopped at the first line of page 3, with no error beyond the evaluation line; `pages: "3"` on its own extracted page 3.

## Example prompts

* *"Open the Q3 worksheet of budget.xlsx for editing."*
* *"Give me slide 2 of the review deck as HTML."*
* *"Turn pages 1-2 of travel-policy.pdf into something I can edit."*

See it used end-to-end: [Edit one worksheet or slide and put it back]({{< ref "editor/mcp/use-cases/edit-one-worksheet-or-slide-and-put-it-back.md" >}}).
