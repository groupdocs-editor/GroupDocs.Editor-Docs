---
id: mcp-troubleshooting-faq
url: editor/mcp/troubleshooting-faq
title: Troubleshooting & FAQ
weight: 5
description: "Solutions to the most common GroupDocs.Editor MCP server issues — server not appearing in the client, startup failures, missing native dependencies, and first-launch timeouts."
keywords: MCP server not showing up in Claude Desktop, Claude can't see MCP tools, MCP server failed to start, dnx command not found, libgdiplus not found error, edit Word document with AI agent, find and replace docx keep formatting, fill Word form AI, edit PDF with AI locally
productName: GroupDocs.Editor MCP Server
toc: True
---

Solutions to the most common GroupDocs.Editor MCP server issues — server not appearing in the client, startup failures, missing native dependencies, and first-launch timeouts.

{{< alert style="info" >}}
**Platform-specific troubleshooting:** runtime problems depend on which build you run. For the `dnx` runner, native graphics libraries, and the Docker channel, see [Troubleshooting (.NET)]({{< ref "editor/net/mcp/troubleshooting.md" >}}). The issues on this page apply to every platform.
{{< /alert >}}

## Why is my MCP server not showing up in Claude Desktop?

1. **Restart the client** — every client reads its MCP config only at startup.
2. Check the config file location for your OS ([per-client reference]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}})) and that the entry sits under the right root key (`mcpServers` for Claude Desktop/Cursor/Windsurf, `servers` for VS Code/VS 2022).
3. Validate the JSON — a trailing comma silently breaks the whole file. If you used the [installer](https://github.com/groupdocs/GroupDocs.Mcp.Installer), a timestamped `.bak` of your previous config sits next to the file for comparison.

## The first tool call is slow or fails once, then works

A **cold cache**: on the very first use the server's package or image is still downloading while the client is already waiting on the connection. Warming it once fixes it for good — the exact command depends on your build: [.NET]({{< ref "editor/net/mcp/troubleshooting.md" >}}#the-first-tool-call-is-slow-or-fails-once-and-then-works).

## The server fails to start, or a runtime dependency is missing

These are properties of the build you run rather than of MCP, so the fixes live with the platform:

| Symptom | Where the fix is |
|---|---|
| `dnx: command not found` | [.NET troubleshooting]({{< ref "editor/net/mcp/troubleshooting.md" >}}#dnx-command-not-found) — `dnx` ships inside the .NET 10 SDK |
| Fonts missing or substituted in PDF output on Linux | [.NET troubleshooting]({{< ref "editor/net/mcp/troubleshooting.md" >}}#fonts-are-missing-or-substituted-in-pdf-output-linux) — install the fonts, or use the Docker image |
| "docker daemon not reachable" | [.NET troubleshooting]({{< ref "editor/net/mcp/troubleshooting.md" >}}#docker-daemon-not-reachable) — start Docker Desktop or `dockerd` |

## The agent says a file does not exist

Pass the **file name**, not a full path from your machine: the server resolves names inside its configured storage folder. When a name is not found the tool responds with the list of files it can see, so the agent can correct itself — check that list against [`GROUPDOCS_MCP_STORAGE_PATH`]({{< ref "editor/net/mcp/configuration.md" >}}).

## Why did one occurrence not change?

`replace_text` matches text **within one formatting run**. A phrase whose words carry different formatting — *ACME* in italics, *Corp* not — is two runs to the engine, and the search does not see it. In our test 6 of 7 occurrences changed and the seventh was exactly that case. Running `replace_text` again does not find it either, so "no matches" on a second run is **not** an all-clear. Check the edited copy with [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) instead: its plain-text preview joins the runs, so a leftover *ACME Corp* shows up there. For a phrase split across runs, use [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) and [`save_from_html`]({{< ref "editor/mcp/tools-reference/save-from-html.md" >}}), which edit the text whatever its formatting.

## Where is the edited file?

In the **output folder** — `GROUPDOCS_MCP_OUTPUT_PATH`, which defaults to the storage folder itself — as `<name>_edited.<ext>`, or under the name you ask for (`outputName`). It lands in the folder's root even when the input sat in a sub-folder: editing `contracts/msa.docx` writes `msa_edited.docx`. Nothing is ever overwritten — not the original, and not an earlier copy: when the name is taken, the server adds a number (`msa_edited (1).docx`).

## What does replace_text keep in a Word document?

`replace_text` opens the document in the engine's editing model (HTML), replaces the text there, and saves a new copy from it. We measured what a Word document keeps on server 26.10.0, with a file built in Word:

| Kept | Changed or lost |
|---|---|
| headings and styles, bold and italics, tables, images, hyperlinks, bookmarks | the **footer** — dropped from the copy, page-number field included |
| the header, footnotes and comments — their text is searched and replaced too | a **content control** — flattened to plain text, and its text is not replaced |
| existing tracked changes | a tracked change loses its **author** |
| a page-number field in the body | a date field becomes fixed text |

For a document whose footer matters, extract it with `extract_html` and `"pagination": true`, edit the HTML, and save it with `save_from_html`: in paginated mode the footer and its page number come back. [`fill_form_fields`]({{< ref "editor/mcp/tools-reference/fill-form-fields.md" >}}) does not go through HTML and keeps footers.

## Does it track changes?

No. The edited copy carries the new text, not a redline. To show a reviewer what changed, compare the original and the edited copy with [GroupDocs.Comparison](/comparison/mcp/).

## Can it edit a PDF?

By converting it: the PDF becomes flowing HTML, and the edit is saved as DOCX (or back to PDF). Layout-heavy PDFs do not round-trip perfectly, which is why saving to Word is the recommended path — [turn a PDF into an editable Word document]({{< ref "editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document.md" >}}).

## Does it need libgdiplus on Linux?

No. GroupDocs.Editor draws through managed code with its native libraries inside the package. On Linux it needs real fonts only when you save to PDF or XPS (`libfontconfig1`, `ttf-mscorefonts-installer`); the Docker image has them. The family's Linux setup script installs `libgdiplus` as well, which does no harm.

## Which format can I save to?

It depends on the family: a Word or PDF source saves to DOCX, DOC, RTF, ODT, PDF, XPS, TXT, Markdown, MHTML, EPUB, MOBI and AZW3; a workbook to XLSX, XLS, ODS, CSV, TSV, PDF and more; a presentation to PPTX, PPT, ODP, PDF and XPS. [`get_supported_formats`]({{< ref "editor/mcp/tools-reference/get-supported-formats.md" >}}) with the file returns the exact list.

## Verifying an installation end-to-end

Ask your agent *"list your GroupDocs editor tools and the license status"* — it should name `get_document_info`, `replace_text`, `extract_html`, `save_from_html`, `get_form_fields`, `fill_form_fields`, `get_supported_formats`, `get_license_status`. For a scripted check that performs the real MCP handshake and a live call through the engine, see [verifying a .NET installation]({{< ref "editor/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end).

## Still stuck?

Post your config (redact license paths) and the client name in the [Editor forum](https://forum.groupdocs.com/c/editor/20). Bugs: [GitHub issues](https://github.com/groupdocs-editor/GroupDocs.Editor.Mcp/issues).
