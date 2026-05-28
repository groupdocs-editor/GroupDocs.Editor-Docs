---
id: quick-start-guide
url: editor/python-net/quick-start-guide
title: Quick Start Guide
linkTitle: Quick Start Guide
second_title: A simple example of how to use GroupDocs.Editor for Python via .NET
weight: 5
keywords: quick start, hello world, get started, first edit, edit DOCX, DOCX to PDF, document info, pip install, venv, virtual environment, GroupDocs.Editor, python
description: "Set up a virtual environment, install groupdocs-editor-net, and run three minimal examples — round-trip edit a DOCX, convert DOCX → PDF through HTML, and read document info — in under five minutes."
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---

This guide provides a quick overview of how to set up and start using GroupDocs.Editor for Python via .NET. The library loads a document, converts it to editable HTML/CSS, lets you edit that markup, and saves it back to the original format — or to a different one.

## Prerequisites

To proceed, make sure you have:

1. **Configured** environment as described in the [System Requirements]({{< ref "editor/python-net/getting-started/system-requirements.md" >}}) topic.
2. **Optionally** you may [Get a Temporary License](https://purchase.groupdocs.com/temporary-license/) to test all the product features.

## Set Up Your Development Environment

For best practices, use a virtual environment to manage dependencies in Python applications. Learn more about virtual environment at [Create and Use Virtual Environments](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#create-and-use-virtual-environments) documentation topic.

### Create and Activate a Virtual Environment

Create a virtual environment:

{{< tabs "example1">}}
{{< tab "Windows" >}}
```ps
py -m venv .venv
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m venv .venv
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m venv .venv
```
{{< /tab >}}
{{< /tabs >}}

Activate a virtual environment:

{{< tabs "example2">}}
{{< tab "Windows" >}}
```ps
.venv\Scripts\activate
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
source .venv/bin/activate
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
source .venv/bin/activate
```
{{< /tab >}}
{{< /tabs >}}

### Install `groupdocs-editor-net` Package

After activating the virtual environment, run the following command in your terminal to install the latest version of the package:

{{< tabs "example3">}}
{{< tab "Windows" >}}
```ps
py -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< /tabs >}}

Ensure the package is installed successfully. You should see the message

```bash
Successfully installed groupdocs-editor-net-*
```

## Example 1: Edit a Word document

To quickly test the library, let's load a DOCX file, edit its HTML, and save it back to DOCX.

{{< tabs "demo_app_edit_word_document">}}
{{< tab "edit_word_document.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

def edit_word_document():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load the document
    with Editor("./sample-document.docx") as editor:
        # Convert the document to editable HTML
        editable = editor.edit()
        html = editable.get_embedded_html()

        # Edit the HTML markup (rename the document title)
        edited_html = html.replace("Title of the document", "Title of the edited document")

        # Build an editable document from the modified markup and save it back to DOCX
        after_edit = EditableDocument.from_markup(edited_html)
        editor.save(after_edit, "./edited-document.docx", WordProcessingSaveOptions(WordProcessingFormats.DOCX))

if __name__ == "__main__":
    edit_word_document()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/getting-started/quick-start-guide/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-document.docx" >}}  
```text
Binary file (DOCX, 49 KB)
```
[Download full output](/editor/python-net/_output_files/getting-started/quick-start-guide/edit_word_document/edited-document.docx)
{{< /tab >}}
{{< /tabs >}}

Your folder tree should look similar to the following directory structure:

```Directory
📂 demo-app
 ├──edit_word_document.py
 ├──sample-document.docx
 └──GroupDocs.Editor.lic (Optionally)
```

### Run the App

{{< tabs "run_the_app_edit_word_document">}}
{{< tab "Windows" >}}
```ps
py edit_word_document.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 edit_word_document.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 edit_word_document.py
```
{{< /tab >}}
{{< /tabs >}}

After running the app you can deactivate virtual environment by executing `deactivate` or closing your shell.

### Explanation
- `Editor("./sample-document.docx")`: Loads the document into the editor.
- `editor.edit()`: Converts the document to an `EditableDocument` and `get_embedded_html()` returns a self-contained HTML string.
- `EditableDocument.from_markup(edited_html)`: Wraps the modified markup back into an editable document.
- `editor.save(..., WordProcessingSaveOptions(WordProcessingFormats.DOCX))`: Saves the edited document back to DOCX.

## Example 2: Convert a document to another format

In this example we convert a DOCX file to PDF. GroupDocs.Editor converts through its HTML intermediate — saving an `EditableDocument` with a different `*SaveOptions` produces a different output format.

{{< tabs "demo_app_convert_word_to_pdf">}}
{{< tab "convert_word_to_pdf.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import PdfSaveOptions

def convert_word_to_pdf():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load the document
    with Editor("./sample-document.docx") as editor:
        # Convert the document to editable HTML
        editable = editor.edit()

        # Save the editable document as PDF
        editor.save(editable, "./sample-document.pdf", PdfSaveOptions())

if __name__ == "__main__":
    convert_word_to_pdf()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/getting-started/quick-start-guide/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "sample-document.pdf" >}}  
```text
Binary file (PDF, 155 KB)
```
[Download full output](/editor/python-net/_output_files/getting-started/quick-start-guide/convert_word_to_pdf/sample-document.pdf)
{{< /tab >}}
{{< /tabs >}}

Your folder tree should look similar to the following directory structure:

```Directory
📂 demo-app
 ├──convert_word_to_pdf.py
 ├──sample-document.docx
 └──GroupDocs.Editor.lic (Optionally)
```

### Run the App

{{< tabs "run_the_app_convert_word_to_pdf">}}
{{< tab "Windows" >}}
```ps
py convert_word_to_pdf.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 convert_word_to_pdf.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 convert_word_to_pdf.py
```
{{< /tab >}}
{{< /tabs >}}

### Explanation
- `editor.edit()`: Converts the source document to an `EditableDocument`.
- `PdfSaveOptions()`: Selects PDF as the output format.
- `editor.save(editable, "./sample-document.pdf", PdfSaveOptions())`: Writes the document out as PDF through the HTML intermediate.

## Example 3: Read document information

Sometimes you only need a document's metadata. `get_document_info()` returns format, page count, size, and encryption status without a full edit pass.

{{< tabs "demo_app_get_document_info">}}
{{< tab "get_document_info.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def get_document_info():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load the document and read its metadata
    with Editor("./sample-document.docx") as editor:
        info = editor.get_document_info()

        print("Format:", info.format.name)
        print("Extension:", info.format.extension)
        print("Pages:", info.page_count)
        print("Size, bytes:", info.size)
        print("Encrypted:", info.is_encrypted)

if __name__ == "__main__":
    get_document_info()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/getting-started/quick-start-guide/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "get-document-info.txt" >}}  
```text
Format: Office Open XML WordProcessingML Macro-Free Document (DOCX)
Extension: docx
Pages: 3
Size, bytes: 49455
Encrypted: False
```
[Download full output](/editor/python-net/_output_files/getting-started/quick-start-guide/get_document_info/get-document-info.txt)
{{< /tab >}}
{{< /tabs >}}

Your folder tree should look similar to the following directory structure:

```Directory
📂 demo-app
 ├──get_document_info.py
 ├──sample-document.docx
 └──GroupDocs.Editor.lic (Optionally)
```

### Run the App

{{< tabs "run_the_app_get_document_info">}}
{{< tab "Windows" >}}
```ps
py get_document_info.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 get_document_info.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 get_document_info.py
```
{{< /tab >}}
{{< /tabs >}}

### Explanation
- `editor.get_document_info()`: Returns a lightweight view of the document's metadata.
- The view supports `snake_case` attribute access (`info.page_count`, `info.format.name`) as well as dict-style access for the underlying PascalCase keys (`info["PageCount"]`).

## Next Steps

After completing the basics, explore additional resources to enhance your usage:
- [Supported Document Formats]({{< ref "editor/python-net/getting-started/supported-document-formats.md" >}}): Review the full list of supported file types.
- [Licensing and Subscription]({{< ref "editor/python-net/getting-started/licensing-and-subscription.md" >}}): Check details on licensing and evaluation.
- [Developer Guide]({{< ref "editor/python-net/developer-guide" >}}): Dive into loading, editing, and saving documents of every supported family.
- [Technical Support]({{< ref "editor/python-net/technical-support" >}}): Contact support for assistance if you encounter issues.
