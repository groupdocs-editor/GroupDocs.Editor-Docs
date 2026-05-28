---
id: create-document
url: editor/python-net/create-document
title: Create and edit new WordProcessing document
linkTitle: Create and edit new WordProcessing document
weight: 8
description: "This article demonstrates how to create and edit WordProcessing documents using GroupDocs.Editor for Python via .NET. It also covers supported formats like spreadsheets and presentations."
keywords: Create new document, edit document, WordProcessing, GroupDocs.Editor, DOCX, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: Create and edit new WordProcessing document
        description: Create and edit new documents using GroupDocs.Editor for Python via .NET
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to create and edit a new WordProcessing document using GroupDocs.Editor in Python
        description: Step-by-step guide for creating, editing, and saving DOCX documents using GroupDocs.Editor for Python via .NET
        steps:
        - name: Build an EditableDocument from HTML markup
          text: Use the EditableDocument.from_markup class method and pass an HTML markup string that represents the content of the new document.
        - name: Edit and save the document
          text: Modify the HTML content if needed, build an EditableDocument from it, and save the document to a stream or file with the appropriate save options.

---

This guide shows how to build a new document from HTML markup and save it in a specific format using the `Editor` and `EditableDocument` classes from GroupDocs.Editor for Python via .NET.

It is possible to create a new document in all major document formats, including text (DOCX), workbooks (XLSX), presentations (PPTX), e-books (EPUB) and emails (EML). See the full list of [supported document formats](https://docs.groupdocs.com/editor/python-net/supported-document-formats/), taking note of the "Create" column, which indicates whether it is possible to create a new document of a particular format.

## Steps to Create and Edit a Document

### 1. Build an editable document from HTML markup

The content of a new document is described as HTML markup. Wrap that markup into an [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) using the [`from_markup`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup) class method:

```python
from groupdocs.editor import EditableDocument

html = "<html><body><h1>New document</h1><p>Created with GroupDocs.Editor.</p></body></html>"
editable = EditableDocument.from_markup(html)
```

### 2. Modify the HTML content

If you obtained the markup from an existing source, you can modify it before building the editable document:

```python
updated_html = html.replace("New document", "My new document")
updated_doc = EditableDocument.from_markup(updated_html)
```

### 3. Save the final document

Choose the appropriate save options for the desired output format and save the editable document. Saving requires an [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance, whose [`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method writes the editable document to a file or stream:

```python
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
save_options.enable_pagination = True
```

## Complete code example

The example below builds an editable document from an HTML markup string, modifies its content, and saves the result to a DOCX file.

{{< tabs "code-example-create-document">}}
{{< tab "create_document.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

def create_document():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Describe the content of the new document as HTML markup
    html = "<html><body><h1>New document</h1><p>Created with GroupDocs.Editor.</p></body></html>"

    # Modify the HTML content if needed
    updated_html = html.replace("New document", "My new document")

    # Build an editable document from the modified markup
    editable = EditableDocument.from_markup(updated_html)

    # Prepare the save options for the WordProcessing family
    save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
    save_options.enable_pagination = True

    # Load a sample document to obtain an Editor instance, then save the new document
    with Editor("./sample-document.docx") as editor:
        editor.save(editable, "./new-document.docx", save_options)

    editable.dispose()

if __name__ == "__main__":
    create_document()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/create-document/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "new-document.docx" >}}  
```text
Binary file (DOCX, 8 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/create-document/create_document/new-document.docx)
{{< /tab >}}
{{< /tabs >}}

## Result

The code builds a DOCX document from HTML markup, modifies its body, and saves the final version to disk. You can optionally save it to an in-memory stream or return it via an API.

## See also

* [`Editor` class](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/)
* [`EditableDocument.from_markup`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup/)
* [`editor.save`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save/)
