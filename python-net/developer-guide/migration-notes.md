---
id: migration-notes
url: editor/python-net/migration-notes
title: Migration Notes
linkTitle: Migration Notes
weight: 300
description: "Notes on the relationship between the GroupDocs.Editor for Python via .NET API and the underlying .NET API."
keywords: GroupDocs.Editor Python migration, snake_case, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
## Overview

GroupDocs.Editor for Python via .NET is a thin Python binding over the GroupDocs.Editor for .NET engine. As a result, its object model, classes, and workflow mirror the .NET API one-to-one. There is no separate legacy Python API to migrate away from — if you already know the .NET API (or are following the .NET examples), the Python usage will be immediately familiar.

## Key differences from the .NET API

When translating .NET code or documentation to Python, keep the following conventions in mind:

* **Naming** — properties and methods use `snake_case` in Python, which is automatically mapped to the .NET `PascalCase`. For example, `Editor.Edit()` becomes `editor.edit()`, `EditableDocument.GetEmbeddedHtml()` becomes `editable.get_embedded_html()`, and the `EnablePagination` property becomes `enable_pagination`.
* **Context managers** — use `with Editor(...) as editor:` so the native document handle is released automatically. `EditableDocument` is disposable too, and exposes a `dispose()` method.
* **Factories** — the static factory methods become Python class methods: `EditableDocument.from_markup(html)`, `EditableDocument.from_markup_and_resource_folder(html, folder)`, and `EditableDocument.from_file(html_path, folder)`.
* **Options families** — pick the load / edit / save options class that matches the document family. The save options type — not the file extension — controls the output format.
* **Enums** — format and option enums are case-insensitive and lazy-loaded, for example `WordProcessingFormats.DOCX` and `MailMessageOutput.ALL`.
* **Streams** — pass `open("file", "rb")` or `io.BytesIO(data)` where the .NET API expects a `Stream`.

## The core workflow

The fundamental load → edit → save pipeline is identical to .NET:

```python
from groupdocs.editor import Editor, EditableDocument
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

with Editor("document.docx") as editor:
    # Obtain the editable document from the original DOCX document
    editable = editor.edit()
    html_content = editable.get_embedded_html()
    # Pass html_content to a WYSIWYG editor and edit there...

    # Save the edited document to some WordProcessing format
    save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
    editor.save(editable, "edited.docx", save_options)
```

For more code examples and specific use cases, please refer to our [Developer Guide]({{< ref "editor/python-net/developer-guide/_index.md" >}}) documentation or the [GitHub](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Python-via-.NET) samples and showcases.
