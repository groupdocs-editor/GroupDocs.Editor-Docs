---
id: mcp-uc-edit-one-worksheet-or-slide-and-put-it-back
url: editor/mcp/use-cases/edit-one-worksheet-or-slide-and-put-it-back
title: How to edit one worksheet or slide and splice it back into the original
linkTitle: One worksheet or slide
weight: 4
description: "Edit one worksheet of a workbook or one slide of a presentation with an AI agent over MCP, locally: extract it as HTML, change it, and splice it back into a copy of the original with every other part kept."
keywords: edit one worksheet with AI, edit single slide AI agent, splice worksheet back into workbook, edit Excel sheet keep others, HTML round trip document editing
productName: GroupDocs.Editor MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to edit one worksheet or slide and splice it back into the original"
        description: "Edit one worksheet of a workbook or one slide of a presentation with an AI agent over MCP, locally: extract it as HTML, change it, and splice it back into a copy of the original with every other part kept."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Editor MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "In budget.xlsx, on worksheet 2 (Q3), raise Travel to 12500 and rename Training to Training (external). Keep the other worksheets as they are."
---

To change one worksheet of a budget or one slide of a deck, the agent does not need the whole file. Run the GroupDocs.Editor MCP server locally: `extract_html` gives the agent that one part as HTML, the agent edits it, and `save_from_html` splices it back into a copy of the original — every other worksheet or slide untouched:

```bash
dnx GroupDocs.Editor.Mcp --yes
```

with `GROUPDOCS_MCP_STORAGE_PATH` pointing at your documents folder — or the Docker image, `ghcr.io/groupdocs-editor/editor-net-mcp`. Per-client config: [Register in AI clients]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}}).

> In budget.xlsx, on worksheet 2 (Q3), raise Travel to 12500 and rename Training to Training (external). Keep the other worksheets as they are.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "editor/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* A workbook (XLSX, XLS, ODS, …) or presentation (PPTX, PPT, ODP, …) in the storage folder.
* The server registered in your AI client, and an agent that can edit a file — [quick start]({{< ref "editor/mcp/getting-started/_index.md" >}}).

## The round-trip

```text
budget.xlsx  (Summary | Q3 | Q4)
   |
   |-- extract_html {"worksheet":2} --> budget_worksheet2.html
   |        the agent edits the HTML: one value, one label
   |
   '-- save_from_html {"htmlFile":"budget_worksheet2.html",
                       "format":"xlsx",
                       "sourceFile":"budget.xlsx","worksheet":2}
          --> budget-q3-revised.xlsx: Q3 replaced, Summary and Q4 kept
evaluation mode: worksheets and slides 1-2 only; 3+ is refused with a reason
```

The tools address worksheets and slides by **1-based number**, hidden ones counted; `get_document_info` returns the count, not the names. Name the number in the prompt, or let the agent extract a part and check its preview.

## What the agent sees

Captured from server 26.10.0 against a synthetic three-worksheet budget (`Summary`, `Q3`, `Q4`). The server prints the full path inside your storage folder; it is shortened to `…/` here, and the evaluation line every response starts with is trimmed unless shown.

```text
-> extract_html {"file":{"filePath":"budget.xlsx"},"worksheet":2}
<- Extracted worksheet 2 of 3 of 'budget.xlsx' as editable HTML
   Saved to: …/budget_worksheet2.html
   Next: edit the saved .html and call save_from_html to write it back as
   DOCX, XLSX, PPTX, PDF or another format.
   Preview (plain text):
   … Cost centre Planned (EUR) Owner Hosting 57600 IT Support 14400 IT
   Travel 9000 Sales Training 6000 HR

   (the agent changes 9000 to 12500 and Training to Training (external)
    in budget_worksheet2.html)

-> save_from_html {"htmlFile":{"filePath":"budget_worksheet2.html"},
     "format":"xlsx","sourceFile":{"filePath":"budget.xlsx"},
     "worksheet":2,"outputName":"budget-q3-revised.xlsx"}
<- Saved 'budget_worksheet2.html' into a copy of 'budget.xlsx', replacing
   worksheet 2 of 3 (XLSX)
   Saved to: …/budget-q3-revised.xlsx
```

Opening `budget-q3-revised.xlsx`: Q3 reads *Travel 12500* and *Training (external)*; `Summary` and `Q4` are exactly as they were. Unlicensed, the workbook also has an `Evaluation Warning` worksheet and the evaluation banner as Q3's first row.

## Slides work the same way

```text
-> extract_html {"file":{"filePath":"review.pptx"},"slide":2}
<- Extracted slide 2 of 3 of 'review.pptx' as editable HTML
   Saved to: …/review_slide2.html

-> save_from_html {"htmlFile":{"filePath":"review_slide2.html"},
     "format":"pptx","sourceFile":{"filePath":"review.pptx"},"slide":2,
     "outputName":"review-revised.pptx"}
<- Saved 'review_slide2.html' into a copy of 'review.pptx', replacing
   slide 2 of 3 (PPTX)
```

A presentation **needs** `sourceFile` — HTML alone cannot become a PPTX:

```text
<- Save from HTML failed for 'review_slide2.html' (-> pptx): 'pptx' output
   cannot be created from HTML alone; pass the original document as
   'sourceFile'.
```

## Expected result

A copy of the workbook or deck with one part changed and every other part as it was — the same file type, formulas and formatting outside the edited part untouched.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| *"…worksheet 3 is beyond them."* | evaluation mode edits worksheets and slides 1–2 | license the server — [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}) |
| *"'pptx' output cannot be created from HTML alone…"* | no `sourceFile` | pass the original as `sourceFile` with `slide` |
| The edited worksheet starts with an extra row | the evaluation banner (unlicensed) | license the server; delete the row meanwhile |

## FAQ

**Why HTML?** It is the format an agent edits reliably: a table is a table, a cell is a cell, and styles come along. The engine turns it back into the original format.

**Can I save the edited worksheet as a new file instead?** Yes — drop `sourceFile` and pick a `format`: XLSX, CSV, PDF.

**Does the agent see the other worksheets?** No — only the part it extracted. That keeps the context small and the other parts out of reach.

**What about hidden worksheets and slides?** A hidden slide needs `"showHidden": true`. A hidden worksheet is refused whatever `showHidden` says — unhide it in the workbook first.

**Why does a slide's preview say "Click to edit Master title style"?** The plain-text preview can pick up the master's placeholder text before the slide's own words. Check the HTML file, not the preview, to confirm the slide.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
To change one worksheet or slide in ./documents, use groupdocs-editor extract_html
with worksheet/slide, edit only that HTML, then save_from_html with sourceFile and
the same worksheet/slide. Never rebuild a workbook or deck from scratch.
```

## Honest limits

* **Evaluation mode:** every edited document starts with an evaluation banner, a saved workbook gains an `Evaluation Warning` worksheet, only worksheets and slides 1–2 can be edited, and a PDF is edited two pages per call (`replace_text` refuses a longer PDF). [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}).
* **One formatting run:** `replace_text` does not see a phrase whose words are formatted differently — not on a second run either; use the HTML round-trip for those.
* **Word footers:** `replace_text`, and `extract_html` without `pagination`, drop a Word document's footer on server 26.10.0; content controls are flattened. [What is kept]({{< ref "editor/mcp/troubleshooting-faq.md" >}}#what-does-replace_text-keep-in-a-word-document).
* **No tracked changes:** the edited copy carries the new text, not a redline.
* **PDF editing is a conversion:** text and structure come across; exact page layout does not.

## Related

* [Edit documents with AI agents]({{< ref "editor/mcp/use-cases/edit-documents-with-ai-agents.md" >}})
* [Find and replace across a folder]({{< ref "editor/mcp/use-cases/find-and-replace-across-a-folder.md" >}})
* [Fill Word forms from data]({{< ref "editor/mcp/use-cases/fill-word-forms-from-data.md" >}})
* [Turn a PDF into editable Word]({{< ref "editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document.md" >}})
* [On-premise deployment]({{< ref "editor/mcp/use-cases/on-premise-document-editing.md" >}})
* [Tools reference]({{< ref "editor/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "editor/mcp/troubleshooting-faq.md" >}})
* Questions: [Editor forum](https://forum.groupdocs.com/c/editor/20)

## Read more

* [Edit one worksheet, keep the workbook: surgical edits with an HTML round-trip](https://blog.groupdocs.com/editor/edit-one-worksheet-or-slide-keep-the-rest-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

