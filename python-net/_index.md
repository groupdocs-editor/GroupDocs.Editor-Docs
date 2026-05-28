---
id: home
url: editor/python-net
title: GroupDocs.Editor for Python via .NET
weight: 1
description: "Native Python library that loads Word, Excel, PowerPoint, PDF, email, eBook, and text documents, converts them to editable HTML/CSS, and saves them back — or to another format — on Windows, Linux, and macOS. No Microsoft Office or OpenOffice required."
keywords: GroupDocs.Editor, Python via .NET, edit documents, edit Word, edit Excel, edit PowerPoint, edit PDF, HTML round-trip, document editing, on-premise, Windows, Linux, macOS, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: true
toc: True
structuredData:
    showOrganization: true
---

<img src="/logo/128x128/groupdocs-editor-python.png" width="110" height="110" alt="groupdocs-editor-python-via-net-home" align="left" style="margin: 0 30px 30px 0"/>

<img src="https://img.shields.io/pypi/v/groupdocs-editor-net?label=GroupDocs.Editor%20PyPI" alt="PyPI package">
<img src="https://img.shields.io/pypi/dm/groupdocs-editor-net?label=pypi%20downloads" alt="PyPI downloads">

{{< button style="primary" link="https://releases.groupdocs.com/editor/python-net/release-notes/" >}} <svg class="gdoc-icon gdoc-product-doc__btn-icon"><use xlink:href="/img/groupdocs-stack.svg#document"></use></svg> Release notes {{< /button >}}
{{< button style="primary" link="https://pypi.org/project/groupdocs-editor-net/" >}} {{< icon "gdoc_download" >}} Download from PyPI {{< /button >}}
{{< button style="primary" link="https://products.groupdocs.app/editor/total" >}} <svg class="gdoc-icon gdoc-product-doc__btn-icon"><use xlink:href="/img/groupdocs-stack.svg#app"></use></svg> Online app {{< /button >}}

[GroupDocs.Editor for Python via .NET](https://products.groupdocs.com/editor/python-net/) is a document-editing API built around an HTML round-trip: load a document, convert it to clean, editable HTML/CSS, edit that markup in any WYSIWYG editor or programmatically, then save it back to the original format — or convert it to another. It works with Word processing, spreadsheets, presentations, PDF, email, eBooks, and text/markup formats through one unified API, with no MS Office or OpenOffice installation required.

<div style="clear:left"></div>

## Quick example

```python
from groupdocs.editor import Editor

# Load a document, convert it to editable HTML, and read its body content
with Editor("document.docx") as editor:
    editable = editor.edit()
    html = editable.get_body_content()
    print(html)
```

## Features

- **HTML round-trip editing**: Convert any supported document to editable HTML/CSS and save it back without losing fidelity.
- **Multi-format**: Word processing, spreadsheets, presentations, PDF, email, eBooks, and text/markup, all behind one API.
- **Format conversion**: Save an edited document with a different `*SaveOptions` to convert it (for example DOCX → PDF or DOCX → Markdown) via the HTML intermediate.
- **Granular editing**: Edit a single worksheet, a single slide, or a page range; toggle pagination and language metadata.
- **Resource extraction**: Pull out a document's images, fonts, CSS, and audio as separate resources.

## Supported File Formats

GroupDocs.Editor supports a wide range of file formats. For a complete list, see the [full list of supported formats]({{< ref "editor/python-net/getting-started/supported-document-formats.md" >}}).

- **Word Processing** (DOC, DOCX, DOCM, DOT, ODT, RTF)
- **Spreadsheets** (XLS, XLSX, XLSM, ODS, CSV, TSV)
- **Presentations** (PPT, PPTX, PPS, ODP)
- **Fixed-Layout** (PDF, XPS)
- **Email** (EML, MSG, MBOX, MHTML)
- **eBooks** (EPUB, MOBI, AZW3)
- **Text & Markup** (HTML, XML, JSON, Markdown, TXT)

## Getting Started

To get started, refer to the [System Requirements]({{< ref "editor/python-net/getting-started/system-requirements.md" >}}), [Supported Document Formats]({{< ref "editor/python-net/getting-started/supported-document-formats.md" >}}), [Installation]({{< ref "editor/python-net/getting-started/installation" >}}), and [Quick Start Guide]({{< ref "editor/python-net/getting-started/quick-start-guide" >}}) sections for setup instructions.

## Developer Guide

For practical code examples and explanations of the load–edit–save workflow across every supported family, see the [Developer Guide]({{< ref "editor/python-net/developer-guide" >}}) section. It covers loading documents, editing Word/Excel/PowerPoint/PDF/email/eBook/text content, working with the `EditableDocument`, and managing form fields.

## Technical Support

If you experience any issues or have suggestions, please refer to the [Technical Support]({{< ref "editor/python-net/technical-support" >}}) topic. This topic provides multiple support channels tailored to your needs.
