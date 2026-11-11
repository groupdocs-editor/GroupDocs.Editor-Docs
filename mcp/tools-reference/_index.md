---
id: mcp-tools-reference
url: editor/mcp/tools-reference
title: Tools reference
weight: 2
description: "Complete reference of every tool the GroupDocs.Editor MCP server exposes to AI agents, with parameters, example prompts, and results."
keywords: MCP tools document editing, replace_text MCP tool, extract_html save_from_html, fill Word form fields MCP, edit documents tools for AI agents
productName: GroupDocs.Editor MCP Server
generated: true
serverVersion: 26.10.0
toc: True
---

Complete reference of every tool the GroupDocs.Editor MCP server exposes to AI agents, with parameters, example prompts, and results. Captured from a live `tools/list` call against server version **26.10.0** (raw capture: `tools-list.generated.json` in this section's source).

| Tool | What it does |
|---|---|
| [`get_document_info`]({{< ref "editor/mcp/tools-reference/get-document-info.md" >}}) | Returns the format family, pages (worksheets, slides), protection and the formats it can be saved to |
| [`replace_text`]({{< ref "editor/mcp/tools-reference/replace-text.md" >}}) | Finds and replaces text and saves an edited copy, keeping formatting; one worksheet or slide at a time |
| [`extract_html`]({{< ref "editor/mcp/tools-reference/extract-html.md" >}}) | Turns a document — or one worksheet, slide or PDF page range — into editable HTML |
| [`save_from_html`]({{< ref "editor/mcp/tools-reference/save-from-html.md" >}}) | Saves edited HTML as a document, or splices one edited worksheet or slide back into the original |
| [`get_form_fields`]({{< ref "editor/mcp/tools-reference/get-form-fields.md" >}}) | Lists a Word form's fields with their names, types and current values |
| [`fill_form_fields`]({{< ref "editor/mcp/tools-reference/fill-form-fields.md" >}}) | Fills Word form fields by name and saves a filled copy; reports unknown names |
| [`get_supported_formats`]({{< ref "editor/mcp/tools-reference/get-supported-formats.md" >}}) | Lists the formats the engine opens and saves, or the save targets for one file |
| [`get_license_status`]({{< ref "editor/mcp/tools-reference/get-license-status.md" >}}) | Reports the active licensing mode and, under metered licensing, consumption |

## The FileInput shape

Every tool takes its document through the same `file` object — pass **either** a name from your storage folder **or** inline content:

```json
{ "file": { "filePath": "contract.docx" } }
```

| Field | Type | Description |
|---|---|---|
| `filePath` | string | File path or name in the configured storage folder |
| `fileContent` | string | Base64-encoded file content (alternative to `filePath`) |
| `fileName` | string | Original filename with extension — required with `fileContent`. Since **26.9.0** it also works on its own, resolved from the storage folder exactly like `filePath` |

You rarely write this JSON yourself: the AI agent does, from your plain-language prompt. Missing files are not an error to fear — the tool responds with the list of available files so the agent can correct itself.
