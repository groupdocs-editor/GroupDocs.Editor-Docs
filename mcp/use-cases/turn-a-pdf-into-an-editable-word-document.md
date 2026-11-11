---
id: mcp-uc-turn-a-pdf-into-an-editable-word-document
url: editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document
title: How to turn a PDF into an editable Word document with an AI agent
linkTitle: PDF to editable Word
weight: 5
description: "Turn a PDF into an editable Word document with an AI agent over MCP, locally, then edit the Word copy: PDF to HTML to DOCX, what comes across, and the two-page evaluation limit."
keywords: edit PDF with AI agent, convert PDF to editable Word locally, PDF to docx AI, change text in PDF Claude, MCP PDF editing
productName: GroupDocs.Editor MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to turn a PDF into an editable Word document with an AI agent"
        description: "Turn a PDF into an editable Word document with an AI agent over MCP, locally, then edit the Word copy: PDF to HTML to DOCX, what comes across, and the two-page evaluation limit."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Editor MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Turn travel-policy.pdf into a Word document, then change the hotel ceiling from 160 EUR to 175 EUR."
---

*"Edit this PDF"* usually means *"change a few lines and send it again"*. A PDF is a page description, not a document you type into, so the reliable path is to convert it into Word, edit the Word copy, and save it as PDF again if needed. Run the GroupDocs.Editor MCP server locally and the agent does all three:

```bash
dnx GroupDocs.Editor.Mcp --yes
```

with `GROUPDOCS_MCP_STORAGE_PATH` pointing at your documents folder — or the Docker image, `ghcr.io/groupdocs-editor/editor-net-mcp`. Per-client config: [Register in AI clients]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}}).

> Turn travel-policy.pdf into a Word document, then change the hotel ceiling from 160 EUR to 175 EUR.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "editor/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* A PDF with a text layer (a scan without one has no words to edit) in the storage folder.
* The server registered in your AI client — [quick start]({{< ref "editor/mcp/getting-started/_index.md" >}}).

## The calls

```text
travel-policy.pdf (3 pages)
   |-- extract_html {"pages":"1-2"} --> travel-policy_pages1-2.html
   |-- save_from_html {"format":"docx"} --> travel-policy.docx
   '-- replace_text {"find":"160 EUR","replace":"175 EUR"}
          --> travel-policy_edited.docx      (edit the Word copy)
   a PDF is converted, not patched: text and structure come across,
   exact page layout does not
```

## What the agent sees

Captured from server 26.10.0 against a synthetic three-page policy. The server prints the full path inside your storage folder; it is shortened to `…/` here, and the evaluation line every response starts with is trimmed unless shown.

```text
-> get_document_info {"file":{"filePath":"travel-policy.pdf"}}
<- { "family": "FixedLayout", "pageCount": 3, "isEncrypted": false,
     "canSaveTo": [ "docx", "doc", "rtf", "odt", "pdf", "md", "epub", ... ] }

-> extract_html {"file":{"filePath":"travel-policy.pdf"},"pages":"1-2"}
<- Extracted 'travel-policy.pdf' as editable HTML
   Saved to: …/travel-policy_pages1-2.html

-> save_from_html {"htmlFile":{"filePath":"travel-policy_pages1-2.html"},
     "format":"docx","outputName":"travel-policy.docx"}
<- Saved 'travel-policy_pages1-2.html' as DOCX
   Saved to: …/travel-policy.docx

-> replace_text {"file":{"filePath":"travel-policy.docx"},
     "find":"160 EUR","replace":"175 EUR"}
<- Replaced 1 occurrence(s) of '160 EUR' in 'travel-policy.docx'
   Saved to: …/travel-policy_edited.docx
```

The Word copy carries the headings, paragraphs and figures of the policy; the edit is an ordinary `replace_text` on a Word document. To send it as a PDF again, add `"format": "pdf"` to the last call.

## Two pages per call in evaluation mode

Unlicensed, one extraction holds **at most two PDF pages**. Asking for `"pages": "1-3"` returned pages 1 and 2 and stopped at the first line of page 3 — **without an error**, only the evaluation line every response starts with. `"pages": "3"` on its own extracted page 3. So for a longer PDF in evaluation mode, extract two pages at a time; licensed, extract the whole document by leaving `pages` out.

## What comes across, and what does not

| Comes across | Does not, exactly |
|---|---|
| text, headings, paragraphs, lists, tables | the page layout — positions, columns, page breaks |
| fonts and emphasis, images (`skipImages: false`) | form fields, annotations, digital signatures |
| the reading order of a born-digital PDF | the words of a scan — there is no OCR |

`pagination: true` keeps page boundaries in the HTML when you need to match pages.

## Expected result

An editable `travel-policy.docx` and the edited copy, locally, in a few seconds — and a clear statement of the layout trade-off for the person who asked.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Page 3 is missing from the Word copy | evaluation mode: two pages per extraction, silently | extract `"3"` separately, or license the server |
| The Word copy is empty | a scanned PDF with no text layer | OCR it first |
| The layout shifted | a PDF is converted to flowing text | edit in Word and keep it as DOCX, or save back to PDF and check the pages |

## FAQ

**Can it change the PDF in place?** It can save the edited HTML straight back as PDF (`"format": "pdf"`), but that is still a conversion: the result is a new PDF built from the HTML, not the original with one line patched.

**Is anything uploaded?** No. The PDF, the HTML and the Word copy stay in your storage folder.

**Does it handle password-protected PDFs?** Yes — pass `password`.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
When asked to edit a PDF in ./documents, use groupdocs-editor: extract_html
(pages, two at a time unlicensed), save_from_html as docx, then edit the Word
copy. Tell the user that PDF layout does not round-trip exactly.
```

## Honest limits

* **Evaluation mode:** every edited document starts with an evaluation banner, a saved workbook gains an `Evaluation Warning` worksheet, and only worksheets and slides 1–2 can be edited (a PDF extraction holds two pages). [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}).
* **One formatting run:** `replace_text` does not see a phrase whose words are formatted differently; use the HTML round-trip for those.
* **No tracked changes:** the edited copy carries the new text, not a redline.
* **PDF editing is a conversion:** text and structure come across; exact page layout does not.

## Related

* [Edit documents with AI agents]({{< ref "editor/mcp/use-cases/edit-documents-with-ai-agents.md" >}})
* [Find and replace across a folder]({{< ref "editor/mcp/use-cases/find-and-replace-across-a-folder.md" >}})
* [Fill Word forms from data]({{< ref "editor/mcp/use-cases/fill-word-forms-from-data.md" >}})
* [Edit one worksheet or slide]({{< ref "editor/mcp/use-cases/edit-one-worksheet-or-slide-and-put-it-back.md" >}})
* [On-premise deployment]({{< ref "editor/mcp/use-cases/on-premise-document-editing.md" >}})
* [Tools reference]({{< ref "editor/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "editor/mcp/troubleshooting-faq.md" >}})
* Questions: [Editor forum](https://forum.groupdocs.com/c/editor/20)

## Read more

* [Edit a PDF with an AI agent? Turn it into Word first, locally](https://blog.groupdocs.com/editor/edit-pdf-with-ai-agent-convert-to-word-locally-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

