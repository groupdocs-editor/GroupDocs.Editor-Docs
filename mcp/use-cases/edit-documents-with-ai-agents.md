---
id: mcp-uc-edit-documents-with-ai-agents
url: editor/mcp/use-cases/edit-documents-with-ai-agents
title: How to edit documents with AI agents using MCP
linkTitle: Edit with AI agents
weight: 1
description: "Make targeted edits to Word, Excel, PowerPoint and PDF documents with an AI agent over MCP, locally: find and replace that keeps the formatting, a count of what changed, the original untouched."
keywords: edit documents with AI agent, MCP document editing, Claude edit Word document locally, AI agent change text keep formatting, targeted document edits
productName: GroupDocs.Editor MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to edit documents with AI agents using MCP"
        description: "Make targeted edits to Word, Excel, PowerPoint and PDF documents with an AI agent over MCP, locally: find and replace that keeps the formatting, a count of what changed, the original untouched."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Editor MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "In contract.docx, replace ACME Corp with Globex Ltd and keep the formatting."
---

Run the GroupDocs.Editor MCP server locally and your agent can change the wording of a Word, Excel, PowerPoint or PDF document **without rewriting it** — the formatting stays, the response counts the changes, and the original file is kept:

```bash
dnx GroupDocs.Editor.Mcp --yes
```

> In contract.docx, replace ACME Corp with Globex Ltd and keep the formatting.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "editor/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* The .NET 10 SDK (for `dnx`) or Docker, and the server registered in your AI client — [quick start]({{< ref "editor/mcp/getting-started/_index.md" >}}).
* `GROUPDOCS_MCP_STORAGE_PATH` pointing at the folder that holds the document.

## The pattern

```text
user            agent                          Editor MCP server
 | "Replace ACME Corp with Globex Ltd in contract.docx"           |
 |-------------->|                                                |
 |               |-- get_document_info ------------------------->|
 |               |   {"file":{"filePath":"contract.docx"}}        |
 |               |<- {"family":"WordProcessing","pageCount":1} --|
 |               |-- replace_text ------------------------------>|
 |               |   {"find":"ACME Corp","replace":"Globex Ltd", |
 |               |    "wholeWord":true}                           |
 |               |<- "Replaced 6 occurrence(s) ...               |
 |               |    Saved to: .../contract_edited.docx" -------|
 |<- "6 places changed, formatting kept; the original is untouched"
    (a find that matches nothing: "No file was written")
```

1. **Inspect** — [`get_document_info`]({{< ref "editor/mcp/tools-reference/get-document-info.md" >}}) says what the file is: a Word document, a workbook with three worksheets, a three-page PDF.
2. **Edit** — [`replace_text`]({{< ref "editor/mcp/tools-reference/replace-text.md" >}}) for wording; [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) and [`save_from_html`]({{< ref "editor/mcp/tools-reference/save-from-html.md" >}}) when the edit is bigger than a find and replace; [`fill_form_fields`]({{< ref "editor/mcp/tools-reference/fill-form-fields.md" >}}) for forms.
3. **Check the count** — the response says how many places changed, and *"No file was written"* when nothing matched.

## What the agent sees

Captured from server 26.10.0 against a synthetic services agreement. The server prints the full path inside your storage folder; it is shortened to `…/` here, and the evaluation line every response starts with is trimmed unless shown.

```text
-> get_document_info {"file":{"filePath":"contract.docx"}}
<- { "fileName": "contract.docx", "family": "WordProcessing",
     "pageCount": 1, "isEncrypted": false, "canEdit": true,
     "canSaveTo": [ "docx", "doc", "rtf", "odt", "pdf", "md", ... ] }

-> replace_text {"file":{"filePath":"contract.docx"},
                 "find":"ACME Corp","replace":"Globex Ltd","wholeWord":true}
<- Replaced 6 occurrence(s) of 'ACME Corp' in 'contract.docx'
   Saved to: …/contract_edited.docx

-> replace_text {"file":{"filePath":"contract.docx"},
                 "find":"Initech","replace":"Globex Ltd"}
<- Text replacement found no matches for 'Initech' in 'contract.docx'. No
   file was written.
```

The contract mentions *ACME Corp* seven times. Six changed — in the parties paragraph (where the name is bold), the services clause, both rows of the fee table and the signature line, each keeping its formatting. The seventh, in the notices clause, is written with *ACME* in italics and *Corp* in plain text: two formatting runs, which `replace_text` does not match as one phrase. That is why the count matters.

## Expected result

`contract_edited.docx` next to the original, with the six occurrences changed and every style, table and heading as it was. In evaluation mode its first line is the evaluation banner.

## Which tool for which edit

| The edit | Tool |
|---|---|
| a word, a name, a number, a version string | [`replace_text`]({{< ref "editor/mcp/tools-reference/replace-text.md" >}}) — `wholeWord`, `matchCase`, `regex` |
| a paragraph rewritten, a phrase across formatting, a table row | [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) → edit → [`save_from_html`]({{< ref "editor/mcp/tools-reference/save-from-html.md" >}}) |
| one worksheet or slide | the same pair, with `worksheet` / `slide` and `sourceFile` |
| a Word form | [`get_form_fields`]({{< ref "editor/mcp/tools-reference/get-form-fields.md" >}}) → [`fill_form_fields`]({{< ref "editor/mcp/tools-reference/fill-form-fields.md" >}}) |

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| The count is lower than the occurrences you see | the missed ones are split across formatting runs | ask the agent to search the edited copy; edit the rest through `extract_html` |
| *"Text replacement found no matches … No file was written."* | the text differs — case (`matchCase` is `true` by default), spacing, a line break | `matchCase: false`, or a `regex` |
| *"File 'contract-final.docx' not found in storage."* with a file list | the name is wrong, or the file is outside the storage folder | pass a name from the list |

## FAQ

**Is the original changed?** No. The edited copy is `<name>_edited.<ext>` (or the `outputName` you ask for) in the output folder.

**Does it work on Excel and PowerPoint?** Yes, one worksheet or slide per call — `"worksheet": 2`, `"slide": 1`. In evaluation mode, parts 1–2 only.

**Does it keep tracked changes or comments?** Existing ones stay in the document; new edits are not tracked. Compare the two files to show a reviewer the changes.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
To change text in documents in ./documents, use the groupdocs-editor MCP server:
replace_text, never a re-typed copy. Report the occurrence count per file and
say when a file had no match. Do not overwrite originals.
```

## Honest limits

* **Evaluation mode:** every edited document starts with an evaluation banner, a saved workbook gains an `Evaluation Warning` worksheet, and only worksheets and slides 1–2 can be edited (a PDF extraction holds two pages). [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}).
* **One formatting run:** `replace_text` does not see a phrase whose words are formatted differently; use the HTML round-trip for those.
* **No tracked changes:** the edited copy carries the new text, not a redline.
* **PDF editing is a conversion:** text and structure come across; exact page layout does not.

## Related

* [Find and replace across a folder]({{< ref "editor/mcp/use-cases/find-and-replace-across-a-folder.md" >}})
* [Fill Word forms from data]({{< ref "editor/mcp/use-cases/fill-word-forms-from-data.md" >}})
* [Edit one worksheet or slide]({{< ref "editor/mcp/use-cases/edit-one-worksheet-or-slide-and-put-it-back.md" >}})
* [Turn a PDF into editable Word]({{< ref "editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document.md" >}})
* [On-premise deployment]({{< ref "editor/mcp/use-cases/on-premise-document-editing.md" >}})
* [Tools reference]({{< ref "editor/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "editor/mcp/troubleshooting-faq.md" >}})
* Questions: [Editor forum](https://forum.groupdocs.com/c/editor/20)

## Read more

* [Why an AI agent should not rewrite your document to change three words](https://blog.groupdocs.com/editor/ai-agent-should-not-rewrite-document-to-edit-it-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

