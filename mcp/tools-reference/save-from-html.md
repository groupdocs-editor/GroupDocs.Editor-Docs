---
id: mcp-tool-save-from-html
url: editor/mcp/tools-reference/save-from-html
title: save_from_html
weight: 4
description: "The save_from_html MCP tool saves edited HTML back as DOCX, XLSX, PPTX, PDF and 20+ formats, or splices one edited worksheet or slide back into the original document."
keywords: save_from_html MCP tool, HTML to docx, splice worksheet back into workbook, replace one slide AI agent, save edited HTML as PDF
productName: GroupDocs.Editor MCP Server
generated: true
serverVersion: 26.10.0
toc: True
---

`save_from_html` writes edited HTML back as a **real document**: DOCX, DOC, RTF, ODT, XLSX, XLS, ODS, CSV, PPTX, PPT, ODP, PDF, XPS, TXT, Markdown, MHTML, EPUB, MOBI, AZW3, EML or MSG. With `sourceFile` plus `worksheet` or `slide`, it **splices** the edited part into a copy of the original and keeps every other part. Example prompt: *"Put the edited Q3 worksheet back into budget.xlsx."*

**Tool description (as the AI agent sees it):**

> Saves edited HTML back as a real document: DOCX, DOC, RTF, ODT, XLSX, XLS, ODS, CSV, PPTX, PPT, ODP, PDF, XPS, TXT, Markdown (md), MHTML, EPUB, MOBI, AZW3, EML or MSG. Use it after extract_html when the user has edited the HTML, or to turn any HTML file in storage into a document. To put ONE edited worksheet or slide back into the original workbook or presentation (keeping every other part), pass the original as sourceFile together with the same 1-based worksheet or slide number (required for workbooks; a presentation sourceFile without slide gives a new one-slide presentation). PPTX and email targets always need sourceFile of the same kind. A resource folder named <name>_resources next to the .html is picked up automatically. Returns the saved path ('Saved to: …'). The output name is the HTML's name with the new extension unless outputName is given; an existing file is never overwritten (a ' (1)' suffix is added). On failure isError is set and the text starts with 'Save from HTML failed for'; a file that cannot be found returns the list of available files instead.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `htmlFile` | object | yes | The .html file to save, e.g. the file extract_html wrote — [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `format` | string | yes | Target format extension: docx, xlsx, pptx, pdf, txt, md, epub, odt, rtf, csv, xps, mhtml, … |
| `outputName` | string | no | Output file name (the extension is set from 'format'). Default: the HTML's name |
| `password` | string | no | Password-protect the output (Word, spreadsheet, presentation and PDF targets) |
| `sourceFile` | object | no | The original document to splice the edited part back into (needed for PPTX and email targets) — [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `worksheet` | integer | no | With sourceFile: which worksheet the HTML replaces, 1-based |
| `slide` | integer | no | With sourceFile: which slide the HTML replaces, 1-based |
| `sourcePassword` | string | no | Password of sourceFile, if it is protected |

## Example call

```json
{
  "name": "save_from_html",
  "arguments": {
    "htmlFile": {
      "filePath": "budget_worksheet2.html"
    },
    "format": "xlsx",
    "sourceFile": {
      "filePath": "budget.xlsx"
    },
    "worksheet": 2,
    "outputName": "budget-q3-revised.xlsx"
  }
}
```

## Result

Plain text — captured from server 26.10.0 (evaluation line trimmed):

```text
Saved 'budget_worksheet2.html' into a copy of 'budget.xlsx', replacing
worksheet 2 of 3 (XLSX)
Saved to: …/budget-q3-revised.xlsx
```

(Wrapped for width.) In that capture the edit changed two cells of the Q3 worksheet; the `Summary` and `Q4` worksheets came back untouched. Unlicensed, the workbook also gained an `Evaluation Warning` worksheet and the banner line as the first row of the edited worksheet.

| Target | Needs `sourceFile` |
|---|---|
| DOCX, XLSX, PDF, TXT, Markdown, EPUB and the other document formats | no — HTML alone is enough |
| **PPTX** (and PPT, ODP), **EML**, **MSG** | **yes** — the original carries what HTML cannot |
| one worksheet or slide back into the original | yes, with `worksheet` / `slide` |

Without it, a presentation target fails with a reason:

```text
Save from HTML failed for 'review_slide2.html' (-> pptx): 'pptx' output cannot be
created from HTML alone; pass the original document as 'sourceFile'.
```

`password` protects the output (Word, workbook, presentation, PDF); `sourcePassword` opens a protected `sourceFile`. On failure `isError` is set.

## Example prompts

* *"Put the edited Q3 worksheet back into budget.xlsx."*
* *"Save my edited HTML as a Word document."*
* *"Replace slide 2 of the deck with the version I just edited."*

See it used end-to-end: [Edit one worksheet or slide and put it back]({{< ref "editor/mcp/use-cases/edit-one-worksheet-or-slide-and-put-it-back.md" >}}).
