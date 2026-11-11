---
id: mcp-supported-formats
url: editor/mcp/supported-formats
title: Supported formats
weight: 4
description: "The GroupDocs.Editor MCP server edits Word, Excel, PowerPoint, PDF, text, e-book and email formats — DOCX, XLSX, PPTX, PDF, ODT, CSV, Markdown, EPUB, EML, MSG and more — and saves them to 20+ formats, locally."
keywords: MCP server supported formats, edit docx xlsx pptx with AI, PDF to docx MCP, edit Markdown EPUB email AI agent, document editing formats
productName: GroupDocs.Editor MCP Server
toc: True
---

The server opens these formats for editing and saves them to the formats on the right — the list [`get_supported_formats`]({{< ref "editor/mcp/tools-reference/get-supported-formats.md" >}}) returns on server 26.10.0. The library's canonical matrix: [supported document formats]({{< ref "editor/net/getting-started/supported-document-formats.md" >}}).

| Family | Opens | Saves to | Parts |
|---|---|---|---|
| Word processing | DOCX, DOC, DOCM, DOT, DOTX, DOTM, ODT, OTT, RTF, WordML | DOCX, DOC, DOCM, DOT, DOTX, DOTM, RTF, ODT, OTT, PDF, XPS, TXT, MD, MHTML, EPUB, MOBI, AZW3 | the whole document; form fields |
| Spreadsheet | XLSX, XLS, XLSB, XLSM, XLT, XLTX, XLTM, XLAM, ODS, FODS, SXC, CSV, TSV, DIF | XLSX, XLS, XLSM, XLSB, XLTX, XLTM, XLT, ODS, CSV, TSV, PDF, XPS, TXT, MHTML | **one worksheet** per call |
| Presentation | PPTX, PPT, PPTM, PPS, PPSX, PPSM, POT, POTX, POTM, ODP, OTP | PPTX, PPT, PPTM, PPS, PPSX, PPSM, POT, POTX, POTM, ODP, PDF, XPS | **one slide** per call |
| PDF (fixed layout) | PDF | the Word-processing list, PDF included | a **page range** |
| Text and markup | TXT, MD, HTML, MHTML, XML, JSON, CHM | the Word-processing list | the whole document |
| E-book | EPUB, MOBI, AZW3 | the Word-processing list | the whole document |
| Email | EML, EMLX, MSG, MHTML, ICS, VCF, MBOX, PST, OST, OFT, TNEF | EML, EMLX, MSG, MHTML | the message |

**Four rules that explain most results**

* **Workbooks and presentations are edited one part at a time.** [`replace_text`]({{< ref "editor/mcp/tools-reference/replace-text.md" >}}) and [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) take `worksheet` or `slide` (default 1); [`save_from_html`]({{< ref "editor/mcp/tools-reference/save-from-html.md" >}}) splices the part back with `sourceFile`.
* **PPTX and email need the original to save.** HTML alone becomes DOCX, XLSX, PDF, TXT, Markdown or EPUB; a presentation or a message needs `sourceFile`.
* **PDF is converted, not patched.** It becomes flowing HTML; save the edit as DOCX when fidelity matters — [turn a PDF into Word]({{< ref "editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document.md" >}}).
* **Form fields are Word only.** [`get_form_fields`]({{< ref "editor/mcp/tools-reference/get-form-fields.md" >}}) reads DOCX, DOC, DOCM, DOT, DOTX, RTF and ODT.
