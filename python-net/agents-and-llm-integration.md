---
id: agents-and-llm-integration
url: editor/python-net/agents-and-llm-integration
title: Agents and LLM Integration
linkTitle: Agents and LLMs
weight: 9
description: "GroupDocs.Editor for Python via .NET is AI agent and LLM friendly — machine-readable documentation, an MCP server, AGENTS.md shipped inside the pip package, and runnable code examples for AI-driven document editing pipelines."
keywords: AI, LLM, agent, RAG, MCP, machine-readable, documentation, Claude, GPT, Copilot, AGENTS.md, document editing, GroupDocs.Editor, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---

## AI agent and LLM friendly

GroupDocs.Editor for Python via .NET is designed to work seamlessly with AI agents, LLMs, and automated code generation tools. The library ships machine-readable documentation in multiple formats — including an `AGENTS.md` file inside the pip package itself — so that AI assistants can discover and use the API without manual guidance.

## MCP server

GroupDocs provides an [MCP (Model Context Protocol) server](https://docs.groupdocs.com/mcp) that enables LLMs to query the documentation on demand instead of loading it all at once. This saves tokens and lets your AI assistant fetch only the information it needs for the current task.

To connect your AI tool to the MCP server, add the GroupDocs endpoint to your MCP configuration:

{{< tabs "mcp-setup">}}
{{< tab "Claude Code / Claude Desktop" >}}
```json
// Claude Code:    ~/.claude/settings.json (or project .mcp.json)
// Claude Desktop: ~/Library/Application Support/Claude/claude_desktop_config.json
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "GitHub Copilot" >}}
```json
// .vscode/mcp.json in your project root
{
  "servers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "Cursor" >}}
```json
// .cursor/mcp.json in your project root
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "Generic MCP" >}}
```json
// Any MCP-compatible client
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< /tabs >}}

See [https://docs.groupdocs.com/mcp](https://docs.groupdocs.com/mcp) for full setup instructions and the list of available tools.

## AGENTS.md — built into the package

The `groupdocs-editor-net` pip package includes an `AGENTS.md` file at `groupdocs/editor/AGENTS.md`. AI coding assistants that scan installed packages (such as Claude Code, Cursor, GitHub Copilot) can automatically discover the API surface, usage patterns, and troubleshooting tips.

After installing the package, you can find it at:

```bash
pip show -f groupdocs-editor-net | grep AGENTS
```

The full content of that file is reproduced in the [AGENTS.md reference](#agentsmd-reference) section below.

## Machine-readable documentation

Every documentation page is available as a plain Markdown file that AI tools can fetch and process directly:

| Resource | URL |
|---|---|
| Full documentation (single file) | `https://docs.groupdocs.com/editor/python-net/llms-full.txt` |
| Full documentation (all products) | `https://docs.groupdocs.com/llms-full.txt` |
| Individual page (any page) | Append `.md` to the page URL, e.g. `https://docs.groupdocs.com/editor/python-net.md` |
| Quick start guide | `https://docs.groupdocs.com/editor/python-net/quick-start-guide.md` |

### How to use with AI tools

Point your AI assistant to the full documentation file for comprehensive context:

```
Fetch https://docs.groupdocs.com/editor/python-net/llms-full.txt and use it
as a reference for GroupDocs.Editor for Python via .NET API.
```

Or reference individual pages for focused tasks:

```
Read https://docs.groupdocs.com/editor/python-net/quick-start-guide.md
and help me edit a DOCX and save it back in Python.
```

## Why GroupDocs.Editor is a good building block for AI document pipelines

LLMs and RAG systems work best with structured text, not binary office files. GroupDocs.Editor converts Word, Excel, PowerPoint, PDF, email, eBook, and text documents into clean, editable HTML/CSS — and writes the edited result back to the original format. That round-trip makes it a natural component in agent-driven document workflows:

- **Extract structured content** — convert a document to HTML and pull headings, tables, and lists for downstream parsers or embeddings.
- **Programmatic edits** — let an agent rewrite the HTML body (fix text, inject content, redact) and save it straight back to DOCX/XLSX/PPTX without losing formatting.
- **Format conversion via HTML** — save an `EditableDocument` with a different `*SaveOptions` to convert (for example DOCX → PDF or DOCX → Markdown) for OCR, vision models, or token-efficient input.
- **Inspect before processing** — read format, page count, size, and encryption status with `get_document_info()` so the agent can branch on metadata.

A typical agent step looks like:

```python
from groupdocs.editor import Editor, EditableDocument
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions, MarkdownSaveOptions

with Editor("inbox/incoming.docx") as editor:
    # Step 1: inspect — let the agent decide what to do based on metadata
    info = editor.get_document_info()
    print("format:", info.format.name, "pages:", info.page_count)

    # Step 2: convert to editable HTML, let the agent edit the markup
    editable = editor.edit()
    edited_html = editable.get_body_content().replace("DRAFT", "FINAL")

    # Step 3a: save the edited markup back to the original format
    editor.save(EditableDocument.from_markup(edited_html), "staging/incoming.docx",
                WordProcessingSaveOptions(WordProcessingFormats.DOCX))

    # Step 3b: or export clean Markdown for a RAG pipeline
    editor.save(editable, "staging/incoming.md", MarkdownSaveOptions())
```

For end-to-end examples — including loading, editing each document family, working with the `EditableDocument`, and saving/converting — see the [Developer Guide]({{< ref "editor/python-net/developer-guide" >}}). Every code block on those pages has a runnable counterpart in the [examples repository](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Python-via-.NET).

## AGENTS.md reference

The content below is the same `AGENTS.md` file that ships inside the `groupdocs-editor-net` package. Copy it into your project as `AGENTS.md` or point your AI assistant to this page.

````markdown
# GroupDocs.Editor for Python via .NET -- AGENTS.md

> Instructions for AI agents working with this package.

Load a document, convert it to editable HTML/CSS, edit the markup, then save it back to the original format or convert to another -- Word, Excel, PowerPoint, PDF, email, eBook, and text/markup formats, all without MS Office or OpenOffice installed.

## Install

```bash
pip install groupdocs-editor-net
```

**Python**: 3.5 - 3.14 | **Platforms**: Windows, Linux, macOS

## Resources

| Resource | URL |
|---|---|
| Documentation | https://docs.groupdocs.com/editor/python-net/ |
| LLM-optimized docs | https://docs.groupdocs.com/editor/python-net/llms-full.txt |
| API reference | https://reference.groupdocs.com/editor/python-net/ |
| Code examples | https://docs.groupdocs.com/editor/python-net/developer-guide/ |
| Release notes | https://releases.groupdocs.com/editor/python-net/release-notes/ |
| PyPI | https://pypi.org/project/groupdocs-editor-net/ |
| Free support forum | https://forum.groupdocs.com/c/editor/ |
| Temporary license | https://purchase.groupdocs.com/temporary-license |

## MCP Server

If your environment has MCP configured, you can connect your AI tool to the GroupDocs documentation server for on-demand API lookups:

```json
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```

Works with Claude Code (`~/.claude/settings.json`), Cursor (`.cursor/mcp.json`), VS Code Copilot (`.vscode/mcp.json`), and any MCP-compatible client. If MCP is unavailable, fall back to the LLM-optimized docs URL above and this file -- both are shipped inside the wheel.

## Imports

```python
from groupdocs.editor import (
    License, Metered, Editor, EditableDocument, FormFieldManager,
    EncryptedException, IncorrectPasswordException, PasswordRequiredException, InvalidFormatException,
)
from groupdocs.editor.formats import (
    WordProcessingFormats, SpreadsheetFormats, PresentationFormats,
    FixedLayoutFormats, EBookFormats, EmailFormats, TextualFormats, FormatFamilies,
)
from groupdocs.editor.options import (
    # Load options
    WordProcessingLoadOptions, SpreadsheetLoadOptions, PresentationLoadOptions, PdfLoadOptions,
    # Edit options
    WordProcessingEditOptions, SpreadsheetEditOptions, PresentationEditOptions, PdfEditOptions,
    EbookEditOptions, EmailEditOptions, MarkdownEditOptions, TextEditOptions, XmlEditOptions, DelimitedTextEditOptions,
    # Save options
    WordProcessingSaveOptions, SpreadsheetSaveOptions, PresentationSaveOptions, PdfSaveOptions,
    HtmlSaveOptions, MhtmlSaveOptions, MarkdownSaveOptions, XpsSaveOptions, TextSaveOptions,
    EbookSaveOptions, EmailSaveOptions, DelimitedTextSaveOptions,
)
from groupdocs.editor.metadata import (
    IDocumentInfo, WordProcessingDocumentInfo, SpreadsheetDocumentInfo,
    PresentationDocumentInfo, FixedLayoutDocumentInfo, TextualDocumentInfo,
    EmailDocumentInfo, EbookDocumentInfo, MarkdownDocumentInfo,
)
```

## Load + Edit + Save (the core workflow)

`Editor` is the entry point. The flow is always: **open → `edit()` → manipulate HTML → `save()`**. Use it as a context manager so the native document handle is released.

```python
from groupdocs.editor import Editor, EditableDocument
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingLoadOptions, WordProcessingSaveOptions

with Editor("input.docx", WordProcessingLoadOptions()) as editor:
    editable = editor.edit()                 # -> EditableDocument
    html = editable.get_embedded_html()      # full self-contained HTML

    edited_html = html.replace("Hello", "Goodbye")

    after_edit = EditableDocument.from_markup(edited_html)
    save_opts = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
    editor.save(after_edit, "output.docx", save_opts)
```

**Editor constructor.** `Editor(file_path)`, `Editor(file_path, load_options)`, or `Editor(stream, load_options)`. The load-options type should match the input family (`WordProcessingLoadOptions` for DOCX, `SpreadsheetLoadOptions` for XLSX, etc.). Omitting load options lets the engine auto-detect.

**`editor.save(input_document, file_path, save_options)`** writes the (possibly modified) `EditableDocument` to disk. The `save_options` type — not the file extension — decides the output format.

## Per-format quick recipes

### Word Processing (DOC, DOCX, RTF, ODT, …)

```python
from groupdocs.editor.options import WordProcessingLoadOptions, WordProcessingEditOptions

with Editor("input.docx", WordProcessingLoadOptions()) as editor:
    eo = WordProcessingEditOptions()
    eo.enable_pagination = False
    eo.enable_language_information = True
    editable = editor.edit(eo)
    body = editable.get_body_content()       # <body>…</body> only
```

### Spreadsheet (XLS, XLSX, ODS, CSV, …) — edit one worksheet at a time

```python
from groupdocs.editor.options import SpreadsheetLoadOptions, SpreadsheetEditOptions

with Editor("book.xlsx", SpreadsheetLoadOptions()) as editor:
    eo = SpreadsheetEditOptions()
    eo.worksheet_index = 0                   # 0-based
    eo.exclude_hidden_worksheets = True
    html = editor.edit(eo).get_content()
```

### Presentation (PPT, PPTX, ODP, …) — edit one slide at a time

```python
from groupdocs.editor.options import PresentationLoadOptions, PresentationEditOptions

with Editor("deck.pptx", PresentationLoadOptions()) as editor:
    eo = PresentationEditOptions()
    eo.slide_number = 0                      # 0-based
    eo.show_hidden_slides = True
    html = editor.edit(eo).get_content()
```

### PDF input → HTML

```python
from groupdocs.editor.options import PdfLoadOptions, PdfEditOptions

lo = PdfLoadOptions()                        # lo.password = "..." for encrypted PDFs
with Editor("input.pdf", lo) as editor:
    eo = PdfEditOptions()
    eo.enable_pagination = True
    html = editor.edit(eo).get_content()
```

### Email (EML, MSG, MBOX, …)

```python
from groupdocs.editor.options import EmailEditOptions, EmailSaveOptions, MailMessageOutput

with Editor("message.eml") as editor:
    eo = EmailEditOptions()
    eo.mail_message_output = MailMessageOutput.ALL   # body, subject, to/cc/bcc, attachments, …
    html = editor.edit(eo).get_content()
```

## Round-trip vs. convert

There is no separate "convert" call — saving an `EditableDocument` with a *different* save-options family converts via the HTML intermediate. Same input, different `save_options` ⇒ different output format.

```python
from groupdocs.editor import Editor, EditableDocument
from groupdocs.editor.options import (
    WordProcessingSaveOptions, PdfSaveOptions, MarkdownSaveOptions,
)
from groupdocs.editor.formats import WordProcessingFormats

with Editor("input.docx") as editor:
    editable = editor.edit()

    editor.save(editable, "same.docx", WordProcessingSaveOptions(WordProcessingFormats.DOCX))  # round-trip
    editor.save(editable, "out.pdf",  PdfSaveOptions())        # DOCX -> PDF
    editor.save(editable, "out.md",   MarkdownSaveOptions())   # DOCX -> Markdown
```

To feed *modified* markup back into a save call, wrap it with `EditableDocument.from_markup(html)` (or `from_markup_and_resource_folder(html, folder)` / `from_file(html_path, folder)` when the HTML references external images/fonts on disk).

## EditableDocument resources

An `EditableDocument` exposes its extracted assets as collections you can iterate (`for r in coll` / `len(coll)`):

| Property | Contents |
|---|---|
| `images` | embedded raster/vector images |
| `fonts` | extracted fonts |
| `css` | stylesheets |
| `audio` | audio resources (e.g. from presentations) |
| `all_resources` | everything above, combined |

```python
with Editor("input.docx") as editor:
    editable = editor.edit()
    print("images:", len(editable.images), "css:", len(editable.css))
    # write HTML + every resource (images/fonts/css) into a folder:
    editable.save("page.html", "page_resources")
```

## Document info without editing

`get_document_info()` returns a lightweight **`StructView`** — a `dict` subclass exposing both `snake_case` attribute access and the raw PascalCase dict keys. Fields: `format` (nested: `name`, `extension`, `mime`, `format_family`), `page_count`, `size`, `is_encrypted`.

```python
with Editor("input.docx") as editor:
    info = editor.get_document_info()        # password="..." for encrypted files
    print("pages:", info.page_count, "size:", info.size,
          "encrypted:", info.is_encrypted, "format:", info.format.name)
    # dict access still works for back-compat: info["PageCount"], info["Format"]["Name"]
```

## Licensing

```python
from groupdocs.editor import License

# From file
License().set_license("path/to/license.lic")

# From stream
with open("license.lic", "rb") as f:
    License().set_license(f)
```

Or auto-apply: `export GROUPDOCS_LIC_PATH="path/to/license.lic"`

**Evaluation vs licensed.** Without a license the library still runs, but output is restricted: PDF output carries an evaluation watermark, other formats show an equivalent evaluation mark, and there is a page/document-count cap. Set `GROUPDOCS_LIC_PATH` (or call `License().set_license(...)`) and re-run to clear it. A 30-day full license is free: https://purchase.groupdocs.com/temporary-license

## API Reference

### Editor

| Method | Returns | Description |
|---|---|---|
| `Editor(file_path / stream [, load_options])` | | Open by path or binary stream; optional `*LoadOptions` matching the input family. Use as a context manager. |
| `edit([edit_options])` | `EditableDocument` | Convert the document to editable HTML/CSS; optional `*EditOptions` (pagination, worksheet/slide selection, …). |
| `save(input_document, file_path, save_options)` | `None` | Write the `EditableDocument` out; the `*SaveOptions` type decides the output format. |
| `get_document_info([password])` | `StructView` | `dict` subclass with both `snake_case` attrs and PascalCase keys: `format` (nested), `page_count`, `size`, `is_encrypted`. No full edit pass needed. |
| `form_field_manager` | `FormFieldManager` | Read/update form fields (Word processing). |

### EditableDocument

| Method | Returns | Description |
|---|---|---|
| `get_content()` | `str` | Full HTML document. |
| `get_body_content([external_images_template])` | `str` | `<body>` inner markup only. |
| `get_css_content([img_prefix, font_prefix])` | `list` | CSS stylesheet(s) as strings. |
| `get_embedded_html()` | `str` | Self-contained HTML with images/CSS inlined. |
| `save(html_file_path[, resources_folder_path])` | `None` | Persist HTML (+ resources) to disk. |
| `dispose()` | `None` | Release native resources (handled by `with`). |
| `images` / `fonts` / `css` / `audio` / `all_resources` | collection | Extracted resources. |
| `from_markup(html)` *(classmethod)* | `EditableDocument` | Build an editable doc from modified HTML. |
| `from_markup_and_resource_folder(html, folder)` *(classmethod)* | `EditableDocument` | …with on-disk resources. |
| `from_file(html_path, folder)` *(classmethod)* | `EditableDocument` | …from an HTML file + resource folder. |

### License / Metered

`License().set_license(path_or_stream)` · `Metered().set_metered_key(public, private)` · `Metered.get_consumption_quantity()` · `Metered.get_consumption_credit()`

## Key Patterns

- **Properties**: use `snake_case` -- auto-mapped to .NET `PascalCase`
- **Context managers**: `with Editor(...) as e:` ensures the document handle is released; `EditableDocument` is disposable too
- **Options families**: pick the `*LoadOptions` / `*EditOptions` / `*SaveOptions` that matches the document family; `*SaveOptions` controls the *output* format
- **Modified markup**: round-trip edited HTML through `EditableDocument.from_markup(html)` / `from_file(...)` before `editor.save(...)`
- **Streams**: pass `open("file", "rb")` or `io.BytesIO(data)` where .NET expects a Stream; `BytesIO` is updated after `save(stream)`
- **Enums**: case-insensitive, lazy-loaded (e.g., `WordProcessingFormats.DOCX`, `MailMessageOutput.ALL`)
- **Collections**: `for r in editable.images` and `len(editable.css)` work on .NET collections
- **Callbacks**: Python functions work for handler interfaces whose methods return `None`. Returning a .NET `Stream` from a Python callback is **not** supported by the binding -- use the file-path / resource-folder `save` overloads instead.

## Platform Requirements

| Platform | Requirements |
|---|---|
| Windows | None |
| Linux | `apt install libgdiplus libfontconfig1 ttf-mscorefonts-installer` |
| macOS | `brew install mono-libgdiplus` |

## Troubleshooting

**Output is watermarked / a few pages only** -- you are running unlicensed (evaluation mode). Apply a license / set `GROUPDOCS_LIC_PATH`.

**`PasswordRequiredException` / `IncorrectPasswordException`** -- the document is encrypted. Set the password on the load options: `lo = WordProcessingLoadOptions(); lo.password = "..."; Editor(path, lo)` (or pass `password=` to `get_document_info`).

**`System.Drawing.Common is not supported`** -- install libgdiplus: `sudo apt install libgdiplus` (Linux) / `brew install mono-libgdiplus` (macOS)

**`Gdip` type initializer exception** -- outdated libgdiplus: `brew reinstall mono-libgdiplus` (macOS)

**Garbled text / missing fonts** -- install fonts: `sudo apt install ttf-mscorefonts-installer fontconfig && sudo fc-cache -f`

**`DllNotFoundException: libSkiaSharp`** -- a stale system copy conflicts with the bundled version. Rename it: `sudo mv /usr/local/lib/libSkiaSharp.dylib /usr/local/lib/libSkiaSharp.dylib.bak`

**`DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` errors** -- do NOT set this. Install ICU: `sudo apt install libicu-dev`

**`TypeLoadException`** -- reinstall: `pip install --force-reinstall groupdocs-editor-net`

**Still stuck?** Post your question at https://forum.groupdocs.com/c/editor/ -- the development team responds directly.
````

## See also

- [Quick Start Guide]({{< ref "editor/python-net/getting-started/quick-start-guide" >}}) — your first edit in five minutes
- [Developer Guide]({{< ref "editor/python-net/developer-guide" >}}) — runnable examples for every API surface
- [API Reference](https://reference.groupdocs.com/editor/python-net) — full class and method documentation
