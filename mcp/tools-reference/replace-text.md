---
id: mcp-tool-replace-text
url: editor/mcp/tools-reference/replace-text
title: replace_text
weight: 2
description: "The replace_text MCP tool finds and replaces text in Word, Excel, PowerPoint, PDF and text documents, keeps the formatting, reports how many places changed, and saves an edited copy."
keywords: replace_text MCP tool, find and replace docx keep formatting, AI agent change text in document, regex replace Word Excel PowerPoint
productName: GroupDocs.Editor MCP Server
generated: true
serverVersion: 26.10.0
toc: True
---

`replace_text` changes wording **in place**: it finds the text, replaces it, keeps the fonts, tables, images and layout, and saves an edited copy — reporting how many occurrences changed. `wholeWord`, `matchCase` (default `true`) and `regex` (with `$1` group references) control the match; `worksheet` or `slide` picks the part of a workbook or presentation; `format` converts while editing. Example prompt: *"Replace ACME Corp with Globex Ltd in contract.docx."*

**Tool description (as the AI agent sees it):**

> Finds and replaces text in a document and saves an edited copy, keeping the document's formatting, tables, images and layout. Use it whenever the user asks to change, rename, update or correct wording in a Word, Excel, PowerPoint, PDF or text document. Supports DOCX, DOC, RTF, ODT, XLSX, XLS, ODS, CSV, PPTX, PPT, ODP, PDF, TXT, Markdown, EPUB and more. Spreadsheets and presentations are edited one worksheet or slide at a time (1-based, default 1) and every other part is kept. Do NOT pre-check whether files exist — just pass the filename the user provided; a missing file returns the list of available files. Matching is case-sensitive by default; set matchCase=false, wholeWord=true or regex=true as needed. A phrase split across differently formatted runs is not matched — use extract_html and save_from_html for those edits. Returns 'Replaced N occurrence(s) …' and the saved path ('Saved to: …') of '<name>_edited.<ext>'. When nothing matches it returns 'Text replacement found no matches …' and writes no file. On failure isError is set and the text starts with 'Text replacement failed for'; a file that cannot be found returns the list of available files instead.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `find` | string | yes | Text to find (or a .NET regular expression when regex=true) |
| `replace` | string | yes | Replacement text; with regex=true it may use $1-style group references |
| `matchCase` | boolean | no | Case-sensitive matching (default true) |
| `wholeWord` | boolean | no | Match whole words only (default false) |
| `regex` | boolean | no | Treat 'find' as a regular expression (default false) |
| `worksheet` | integer | no | Spreadsheets only: worksheet to edit, 1-based (default 1) |
| `slide` | integer | no | Presentations only: slide to edit, 1-based (default 1) |
| `format` | string | no | Output format extension (default: same as the input), e.g. 'pdf' to also convert |
| `outputName` | string | no | Output file name (the extension follows the output format). Default: '<name>_edited.<ext>' |
| `password` | string | no | Password, for password-protected documents. The edited copy keeps the same password when its format can carry one (Word, workbook, presentation, PDF) |

## Example call

```json
{
  "name": "replace_text",
  "arguments": {
    "file": {
      "filePath": "contract.docx"
    },
    "find": "ACME Corp",
    "replace": "Globex Ltd",
    "wholeWord": true
  }
}
```

## Result

Plain text — captured from server 26.10.0 (the server prints the full path inside your storage folder):

```text
Evaluation mode: outputs start with an evaluation banner, saved workbooks gain
an extra worksheet, and only the first 2 PDF pages, worksheets and slides can
be edited. Set GROUPDOCS_LICENSE_PATH, or the metered keys, to remove these
limits.

Replaced 6 occurrence(s) of 'ACME Corp' in 'contract.docx'
Saved to: …/contract_edited.docx
```

(The first paragraph is one line in the response, wrapped here for width; it appears only in evaluation mode.)

**Nothing matched, nothing written** — a find that matches nothing is an answer, not a failure:

```text
Text replacement found no matches for 'Initech' in 'contract.docx'. No file was
written.
```

**One part of a workbook or presentation per call** — the response names it:

```text
Replaced 1 occurrence(s) of 'ACME Corp' in worksheet 1 of 3 of 'budget.xlsx'
Replaced 1 occurrence(s) of 'ACME Corp' in slide 1 of 3 of 'review.pptx'
```

* The copy is saved as `<name>_edited.<ext>` (or `outputName`) in the output folder's **root**, even when the input sits in a sub-folder. With `format: "pdf"` the edited copy is a PDF.
* Matching works **within one formatting run**: a phrase half in italics is not found. In the capture above, the document held 7 occurrences; the 7th was split across two runs and stayed unchanged. Use [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) for such phrases.

On failure the text starts with `Text replacement failed for` and `isError` is set — for example, in evaluation mode:

```text
Text replacement failed for 'review.pptx' (find='ACME Corp'): Evaluation mode
edits only the first 2 slides of a presentation; slide 3 is beyond them. Set
GROUPDOCS_LICENSE_PATH, or the metered keys, to edit every part.
```

## Example prompts

* *"Replace ACME Corp with Globex Ltd in contract.docx and keep the formatting."*
* *"Change every v2.3 to v2.4 in the NDA — whole versions only."*
* *"Rename the supplier on the first worksheet of budget.xlsx."*
* *"Fix the typo 'recieve' in all of these documents."*

See it used end-to-end: [Find and replace across a folder]({{< ref "editor/mcp/use-cases/find-and-replace-across-a-folder.md" >}}).
