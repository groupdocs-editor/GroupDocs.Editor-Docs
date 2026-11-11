---
id: mcp-uc-find-and-replace-across-a-folder
url: editor/mcp/use-cases/find-and-replace-across-a-folder
title: How to find and replace text across a folder of documents without breaking the formatting
linkTitle: Replace across a folder
weight: 2
description: "Rename a company, a product or a version string in every document of a folder with an AI agent over MCP, locally: Word, Excel and PowerPoint, formatting kept, a count per file."
keywords: find and replace across multiple documents, rename company in all contracts AI, bulk replace docx keep formatting, replace text in folder of Word files, AI agent batch edit documents
productName: GroupDocs.Editor MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to find and replace text across a folder of documents without breaking the formatting"
        description: "Rename a company, a product or a version string in every document of a folder with an AI agent over MCP, locally: Word, Excel and PowerPoint, formatting kept, a count per file."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Editor MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "In every document in the contracts folder, plus budget.xlsx and review.pptx, replace ACME Corp with Globex Ltd as a whole word. Tell me how many changes per file."
---

When a supplier renames itself, every contract, rate card and slide deck that names it needs the same edit. Run the GroupDocs.Editor MCP server locally and ask your agent: it calls `replace_text` once per file, keeps the formatting, and tells you how many places changed in each — including the files where nothing matched:

```bash
dnx GroupDocs.Editor.Mcp --yes
```

with `GROUPDOCS_MCP_STORAGE_PATH` pointing at your documents folder — or the Docker image, `ghcr.io/groupdocs-editor/editor-net-mcp`. Per-client config: [Register in AI clients]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}}).

> In every document in the contracts folder, plus budget.xlsx and review.pptx, replace ACME Corp with Globex Ltd as a whole word. Tell me how many changes per file.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "editor/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* The server registered in your AI client, the documents inside the storage folder — [quick start]({{< ref "editor/mcp/getting-started/_index.md" >}}).
* An agent that can list the folder (its own file tools), or the file names in the prompt.

## The calls

```text
for each file the agent finds in contracts/ and the shared folder:
  replace_text {"find":"ACME Corp","replace":"Globex Ltd","wholeWord":true}
    msa-northwind.docx    -> Replaced 6  -> msa-northwind_edited.docx
    nda-northwind.docx    -> Replaced 1  -> nda-northwind_edited.docx
    welcome-letter.docx   -> no matches  -> nothing written
    budget.xlsx  (sheet 1)-> Replaced 1  -> budget_edited.xlsx
    review.pptx  (slide 1)-> Replaced 1  -> review_edited.pptx
  the agent reports: 9 changes in 4 files, 1 file without the name
```

## What the agent sees

Captured from server 26.10.0 against a synthetic set: an agreement and an NDA in `contracts/`, a welcome letter that never mentions the supplier, a three-worksheet budget and a three-slide deck. The server prints the full path inside your storage folder; it is shortened to `…/` here, and the evaluation line every response starts with is trimmed unless shown.

```text
-> replace_text {"file":{"filePath":"contracts/msa-northwind.docx"},
                 "find":"ACME Corp","replace":"Globex Ltd","wholeWord":true}
<- Replaced 6 occurrence(s) of 'ACME Corp' in 'msa-northwind.docx'
   Saved to: …/msa-northwind_edited.docx

-> replace_text {"file":{"filePath":"contracts/welcome-letter.docx"}, ...}
<- Text replacement found no matches for 'ACME Corp' in
   'welcome-letter.docx'. No file was written.

-> replace_text {"file":{"filePath":"budget.xlsx"}, ..., "worksheet":1}
<- Replaced 1 occurrence(s) of 'ACME Corp' in worksheet 1 of 3 of
   'budget.xlsx'
   Saved to: …/budget_edited.xlsx

-> replace_text {"file":{"filePath":"review.pptx"}, ..., "slide":3}
<- Text replacement failed for 'review.pptx' (find='ACME Corp'): Evaluation
   mode edits only the first 2 slides of a presentation; slide 3 is beyond
   them. Set GROUPDOCS_LICENSE_PATH, or the metered keys, to edit every part.
```

Four things to notice:

* **The count is per file**, and a file without the name says so and writes nothing — the agent can report *"the welcome letter does not mention ACME"* instead of a vague *"done"*.
* **Workbooks and decks are edited one part per call.** The budget names the supplier on worksheet 1; the deck on slides 1 and 3. Ask the agent to check each part (`get_document_info` gives the count).
* **Evaluation mode stops at part 2**, with an error that says so. Licensed, slide 3 is edited like the others.
* **Edited copies land in the output folder's root** — `msa-northwind_edited.docx`, not `contracts/msa-northwind_edited.docx`. Use `outputName` to keep a naming scheme.

## Precise matches

| You want | Arguments | Captured result |
|---|---|---|
| the name, not a word that contains it | `"wholeWord": true` | 6 in the agreement, 1 in the NDA |
| any capitalisation | `"find": "acme corp", "matchCase": false` | `Replaced 1 occurrence(s) of 'acme corp'` |
| a version string, exactly | `"find": "v2\\.3\\b", "regex": true, "replace": "v2.4"` | `Replaced 1 occurrence(s) of 'v2\.3\b'` |
| the edit as a PDF to send | `"format": "pdf"` | `Saved to: …/nda-northwind_edited.pdf` |

`regex` takes .NET regular expressions; the replacement may use `$1`-style groups.

## Expected result

One edited copy per file that contained the name, a per-file count, and a list of files that did not — in the time it takes to make a call per file.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| A file's count is one short of what you see | an occurrence is split across formatting runs (half italic) | edit that file through [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) |
| *"…slide 3 is beyond them."* | evaluation mode edits parts 1–2 | license the server — [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}) |
| Two runs overwrite each other's output | both wrote `<name>_edited.<ext>` to the root | give each call an `outputName` |

## FAQ

**Can it edit a whole folder in one call?** No — one document per call, which is what makes the per-file count possible. The agent loops.

**Does it edit headers, footers and tables?** Tables, yes (two of the agreement's six occurrences were in the fee table). Check headers and footers in the result for your documents.

**Can it work on a code repository's documents?** Yes — point the storage folder at the repository and ask, for example, to bump `v2.3` to `v2.4` in the release-notes DOCX and the spec XLSX. Add the edited copies to `.gitignore` or rename them over the originals deliberately.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
For wording changes across documents in ./documents, call groupdocs-editor
replace_text once per file and per worksheet/slide; use wholeWord for names.
Report a count per file and list files with no match. Never re-type a document.
```

## Honest limits

* **Evaluation mode:** every edited document starts with an evaluation banner, a saved workbook gains an `Evaluation Warning` worksheet, and only worksheets and slides 1–2 can be edited (a PDF extraction holds two pages). [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}).
* **One formatting run:** `replace_text` does not see a phrase whose words are formatted differently; use the HTML round-trip for those.
* **No tracked changes:** the edited copy carries the new text, not a redline.
* **PDF editing is a conversion:** text and structure come across; exact page layout does not.

## Related

* [Edit documents with AI agents]({{< ref "editor/mcp/use-cases/edit-documents-with-ai-agents.md" >}})
* [Fill Word forms from data]({{< ref "editor/mcp/use-cases/fill-word-forms-from-data.md" >}})
* [Edit one worksheet or slide]({{< ref "editor/mcp/use-cases/edit-one-worksheet-or-slide-and-put-it-back.md" >}})
* [Turn a PDF into editable Word]({{< ref "editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document.md" >}})
* [On-premise deployment]({{< ref "editor/mcp/use-cases/on-premise-document-editing.md" >}})
* [Tools reference]({{< ref "editor/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "editor/mcp/troubleshooting-faq.md" >}})
* Questions: [Editor forum](https://forum.groupdocs.com/c/editor/20)

## Read more

* [Rename a party across 40 contracts without touching the formatting](https://blog.groupdocs.com/editor/find-replace-across-contracts-keep-formatting-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

