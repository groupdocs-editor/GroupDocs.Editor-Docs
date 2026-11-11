---
id: mcp
url: editor/mcp
title: GroupDocs.Editor MCP Server
weight: 6
description: "GroupDocs.Editor MCP server lets AI agents like Claude, Cursor, and Copilot make targeted edits to Word, Excel, PowerPoint and PDF documents — find and replace that keeps the formatting, Word forms, one worksheet or slide at a time — locally on your machine."
keywords: document editing MCP server, edit Word document with AI agent, find and replace docx keep formatting, fill Word form AI, edit Excel worksheet Claude, edit PDF with AI locally
productName: GroupDocs.Editor MCP Server
hideChildren: True
toc: True
---

**GroupDocs.Editor MCP server** lets AI agents like Claude, Cursor, and Copilot **make targeted edits to documents** — find and replace that keeps the formatting, fill Word forms, edit one worksheet or slide and put it back, turn a PDF into editable Word — in Word, Excel, PowerPoint, PDF, e-book and email formats, **locally on your machine**, without rewriting the document. 

Run it with one command. The Docker image is self-contained — the runtime and every native dependency the engine needs are inside it:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-editor/editor-net-mcp:latest
```

With the .NET 10 SDK installed, the same server also runs without Docker:

```bash
dnx GroupDocs.Editor.Mcp --yes
```

Both are the **.NET** build of the server and run on Windows, Linux, and macOS. Other platforms will each get their own launcher — see [Install for your platform](#install-for-your-platform).

Or use the [guided installer]({{< ref "editor/mcp/getting-started/_index.md" >}}) to register the server in your AI client, verify the setup, and configure shared folders in one pass.

Then ask your agent, in plain language:

> In contract.docx, replace ACME Corp with Globex Ltd and keep the formatting.

## What you can do

Eight tools (full details in the [tools reference]({{< ref "editor/mcp/tools-reference/_index.md" >}})):

| The user asks… | The agent calls | It gets back |
|---|---|---|
| "How many worksheets? Is it protected?" | [`get_document_info`]({{< ref "editor/mcp/tools-reference/get-document-info.md" >}}) | family, pages / worksheets / slides, protection, save targets |
| "Change X to Y, keep the formatting" | [`replace_text`]({{< ref "editor/mcp/tools-reference/replace-text.md" >}}) | the number of occurrences changed, and an edited copy |
| "Open the Q3 sheet / slide 2 / pages 1–2 for editing" | [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) | an editable HTML file of that part |
| "Put it back" / "save it as Word" | [`save_from_html`]({{< ref "editor/mcp/tools-reference/save-from-html.md" >}}) | the original with that part replaced, or a new document |
| "What fields does this form have?" | [`get_form_fields`]({{< ref "editor/mcp/tools-reference/get-form-fields.md" >}}) | names, types, current values, drop-down options |
| "Fill the form for Amara" | [`fill_form_fields`]({{< ref "editor/mcp/tools-reference/fill-form-fields.md" >}}) | the fields filled, the names it did not know |
| "Can it save this as EPUB?" | [`get_supported_formats`]({{< ref "editor/mcp/tools-reference/get-supported-formats.md" >}}) | the formats per family, or for one file |
| "Why the evaluation line?" | [`get_license_status`]({{< ref "editor/mcp/tools-reference/get-license-status.md" >}}) | the licensing mode |

## One edit, two calls

The pattern behind every scenario — captured from the server:

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

The agent never re-types the document. It names the change, the engine applies it to the original file, and the response says how many places changed — so a find that matched nothing is visible, not silently "done".

## Edit, don't regenerate

Ask a model to "change three words in this contract" without an editing tool and it reads the text and writes a new document: styles, tables and headers re-created from memory, and any clause it re-typed can drift without anyone noticing. An editing tool inverts that. The document stays the original file; only the named change is applied; the result is a copy with a count.

```text
Regenerate the document                Edit the document
-------------------------------------  -------------------------------------
agent reads the text, writes a new     agent names the change: find/replace,
  file from memory                       one worksheet, one form field
styles, tables, headers re-typed       everything else is the original file
silent drift in clauses it re-wrote    a count of what changed, nothing else
no record of what changed              no match = no file written
```

## Scenarios

* [Edit documents with AI agents]({{< ref "editor/mcp/use-cases/edit-documents-with-ai-agents.md" >}}) — start here: inspect, replace, check the count.
* [Find and replace across a folder]({{< ref "editor/mcp/use-cases/find-and-replace-across-a-folder.md" >}}) — rename a party in every contract, keep the formatting.
* [Fill Word forms from data]({{< ref "editor/mcp/use-cases/fill-word-forms-from-data.md" >}}) — read the field names, fill, see what was not filled.
* [Edit one worksheet or slide and put it back]({{< ref "editor/mcp/use-cases/edit-one-worksheet-or-slide-and-put-it-back.md" >}}) — the rest of the workbook untouched.
* [Turn a PDF into an editable Word document]({{< ref "editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document.md" >}}) — what "edit this PDF" should mean.
* [On-premise architecture]({{< ref "editor/mcp/use-cases/on-premise-document-editing.md" >}}) — what stays on the machine, and what travels.

## Install for your platform

Installation, prerequisites, and client configuration are platform-specific; the tools and licensing model are the same everywhere.

| Platform | Status | Install and setup |
|---|---|---|
| .NET | **Available** | [MCP server for .NET]({{< ref "editor/net/mcp/_index.md" >}}) |
| Java | Planned | [Tell us you need it](https://forum.groupdocs.com/c/editor/20) |
| Python | Planned | [Tell us you need it](https://forum.groupdocs.com/c/editor/20) |
| Node.js | Planned | [Tell us you need it](https://forum.groupdocs.com/c/editor/20) |

## Supported AI clients

| Client | How it connects |
|---|---|
| Claude Desktop | `claude_desktop_config.json` |
| Claude Code | `claude mcp add` CLI |
| VS Code / GitHub Copilot | user-level or workspace `mcp.json` |
| Visual Studio 2022 (17.14+) | `.mcp.json` in the solution root |
| Cursor | `~/.cursor/mcp.json` |
| Windsurf | `~/.codeium/windsurf/mcp_config.json` |
| Cline | Cline MCP settings |
| Codex CLI | `codex mcp add` CLI |
| JetBrains Rider | manual registration (Settings → AI Assistant → MCP) |

Exact config blocks for every client: [Register in AI clients]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}}).

## How it works

```text
+--------------+          +--------------------+         +------------------+
|  AI client   |  stdio   | MCP server process | reads / | local filesystem |
| (Claude, VS  | <----->  | (GroupDocs engine) | <-----> | storage / output |
| Code, agent) | JSON-RPC |   child process    |  writes |     folders      |
+--------------+          +--------------------+         +------------------+
```

The server uses MCP's **local stdio transport**: your AI client starts the server as a child process and talks to it over standard input/output. No inbound ports, no external endpoints, no telemetry — the data path is *agent → local server → local filesystem*. Edited copies are written next to the originals, which are never overwritten. Details: [On-premise architecture]({{< ref "editor/mcp/use-cases/on-premise-document-editing.md" >}}).

## Delivery channels

| | Docker | NuGet (`dnx`) |
|---|---|---|
| Prerequisites | Docker only | .NET 10 SDK; on Linux, fonts for PDF/XPS output |
| Native dependencies | bundled in the image, fonts included | inside the package — **no `libgdiplus` needed** |
| Package | `ghcr.io/groupdocs-editor/editor-net-mcp` | `GroupDocs.Editor.Mcp` on NuGet |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) | any OS with .NET 10 |

## Resources

* [Quick start]({{< ref "editor/mcp/getting-started/_index.md" >}}) · [Use cases]({{< ref "editor/mcp/use-cases/_index.md" >}}) · [Supported formats]({{< ref "editor/mcp/supported-formats.md" >}}) · [Troubleshooting & FAQ]({{< ref "editor/mcp/troubleshooting-faq.md" >}})
* GitHub: [server source](https://github.com/groupdocs-editor/GroupDocs.Editor.Mcp) · [installer](https://github.com/groupdocs/GroupDocs.Mcp.Installer) · [integration tests](https://github.com/groupdocs-editor/GroupDocs.Editor.Mcp.Tests)
* [NuGet package](https://www.nuget.org/packages/GroupDocs.Editor.Mcp) · [Docker image](https://github.com/orgs/groupdocs-editor/packages/container/package/editor-net-mcp) · [MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.groupdocs-editor/groupdocs-editor-mcp)
* The library behind it: [GroupDocs.Editor for .NET]({{< ref "editor/net/_index.md" >}})
* Questions: [Editor forum](https://forum.groupdocs.com/c/editor/20)
