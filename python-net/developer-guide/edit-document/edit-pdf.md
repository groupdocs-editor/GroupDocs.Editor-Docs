---
id: edit-pdf
url: editor/python-net/edit-pdf
title: Edit PDF
linkTitle: Edit PDF document
weight: 14
description: "This guide demonstrates how to edit the content of PDF files like common text documents using GroupDocs.Editor for Python via .NET."
keywords: Edit PDF, Edit PDF file, Edit PDF document, Edit PDF content, Edit PDF text, Edit text in PDF, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit PDF document in the GroupDocs.Editor
        description: How to edit PDF document using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to edit content of the PDF document in the GroupDocs.Editor in Python
        description: Learn how to edit the content of a PDF document in different editing modes and with different editing options using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load the desired PDF document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired PDF document into it.
        - name: Prepare a PdfEditOptions class
          text: Create an instance of the PdfEditOptions class and adjust its properties to meet your needs if necessary. For example, enable or disable images, select a page range, set a pagination mode.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the editor.edit method with a previously prepared PdfEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML markup and extract resources from this instance using the corresponding instance methods, and pass all this data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in the WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on the client-side (in a web-browser), and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable class methods of the class.
        - name: Prepare a PdfSaveOptions class
          text: Create an instance of the PdfSaveOptions class and adjust its properties to meet your needs if necessary. All properties are optional, so you may use a default PdfSaveOptions instance. But with options you can protect the resultant document, set a compliance level or embed fonts.
        - name: Save the edited document with the editor.save method
          text: Pass an instance of EditableDocument with the content of the edited document, an instance of PdfSaveOptions, and a destination byte stream or file path to the editor.save method for saving the document.
---
> This example demonstrates the standard open-edit-save pipeline with PDF documents, using different options on every step.

## Introduction

The [PDF documents, or documents in a Portable Document Format](https://docs.fileformat.com/pdf/), developed by Adobe Corp, are widely used all over the Internet and in document management systems. The PDF format has a crucial distinction from other formats such as [DOCX](https://docs.fileformat.com/word-processing/docx/), [TXT](https://docs.fileformat.com/word-processing/txt/), or [HTML](https://docs.fileformat.com/web/html/)/[CSS](https://docs.fileformat.com/web/css/) — it is a so-called fixed-layout format. The main purpose of PDF is to be platform-independent and store the exact representation of a document — wherever and whenever this document is opened, it should provide per-character and even per-pixel fidelity. This means that a document, once created, is "baked" in terms of its representation and editability. While you can freely edit any DOCX document by adding, removing, or moving any part of its content, PDF documents stay "frozen". Internally, a PDF document consists of pages, where every page contains a set of glyphs (visual characters), each having coordinates of where it is located on the page.

Concluding:
- Editing PDF documents like ordinary DOCX, TXT, or HTML is an extremely difficult and complex task.
- The quality of editing a PDF document may be very close to what we can do with usual text documents, but it will never be 100%, especially when the input PDF has quite complex formatting and content.
- Due to the complexity of the PDF format and the process of making it editable, this operation requires a lot of processing time and memory.

## In two words

Editing PDF documents is the same as editing any other document:

1. Load a PDF document into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class with [`PdfLoadOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfloadoptions), specifying a password if needed.
2. Edit the document using the [`editor.edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method with [`PdfEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfeditoptions) and obtain an instance of [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument).
3. Send the document content to the client-side, edit it there with a WYSIWYG-editor, and send the modified (edited) content back to the server-side.
4. Create an instance of [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) with the modified content and call [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) using [`PdfSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfsaveoptions).

## Loading

The [`PdfLoadOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfloadoptions) class is responsible for loading PDF files into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor). It has only one property — a `password` of a string type. By default it is `None` — no password is specified. This property is vital when an input document is encoded with a password. If a document is not encoded, the property value is ignored whether it was specified or not.

When the input PDF is not password-protected, `PdfLoadOptions` is not necessary at all — GroupDocs.Editor will automatically detect the PDF format and apply the default `PdfLoadOptions` by itself. However, specifying even a default `PdfLoadOptions` will speed up the document processing, because in this case GroupDocs.Editor will not spend processing time on the automatic format detection routine.

```python
from groupdocs.editor import Editor
from groupdocs.editor.options import PdfLoadOptions

# Create the default PDF loading options
load_options = PdfLoadOptions()

# Set a password
load_options.password = "some_password"

# Load a PDF without PDF load options
editor1 = Editor("protected.pdf")

# Load a PDF with PDF load options
editor2 = Editor("protected.pdf", load_options)
```

## Editing

Like for other format families in GroupDocs.Editor, there is a special [`PdfEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfeditoptions) class for editing PDF documents. The most useful properties are:

1. The `skip_images` boolean flag. By default it has a `False` value — images are not skipped and are preserved. However, if you need only textual information from the document, you can set this flag to `True`.
2. The `enable_pagination` boolean flag. This flag sets the document conversion mode: the **float** (default value is `False`) or **paginal** (`True`). When the float mode is selected, the document content is converted to a pageless (float) HTML document. When the paginal mode is selected, the pages of the document are preserved in the generated HTML document, like in a PDF viewer.
3. The `pages` property, which allows setting a page range that should be processed. By default all pages of the input document are processed.

If a default `PdfEditOptions` instance is acceptable for you, you may omit creating `PdfEditOptions` at all — just call the parameterless [`editor.edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) overload and GroupDocs.Editor will internally generate and apply the default `PdfEditOptions` for the input PDF document.

The runnable example below loads a PDF document, edits it with adjusted `PdfEditOptions`, and obtains the HTML content from the resultant [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument).

{{< tabs "code-example-edit-pdf">}}
{{< tab "edit_pdf.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import PdfLoadOptions, PdfEditOptions

def edit_pdf():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Prepare PDF load options (optional, speeds up format detection)
    load_options = PdfLoadOptions()

    # Load an input PDF document into the Editor
    with Editor("./sample-document.pdf", load_options) as editor:
        # Create and adjust the PDF edit options
        edit_options = PdfEditOptions()
        edit_options.enable_pagination = True
        edit_options.skip_images = False

        # Edit the PDF and obtain an EditableDocument
        editable = editor.edit(edit_options)

        # Obtain the HTML content (in practice it is sent to the WYSIWYG-editor)
        content = editable.get_content()
        print("Generated HTML content length:", len(content))

        editable.dispose()

if __name__ == "__main__":
    edit_pdf()
```
{{< /tab >}}
{{< tab "sample-document.pdf" >}}  
{{< tab-text >}}
`sample-document.pdf` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-pdf/sample-document.pdf) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edit-pdf.txt" >}}  
```text
Generated HTML content length: 110427
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-pdf/edit_pdf/edit-pdf.txt)
{{< /tab >}}
{{< /tabs >}}

## Saving

Like for other document formats, there is a special class responsible for saving PDF documents — the [`PdfSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfsaveoptions) class. It has the following properties:

1. `password` — allows you to protect the output PDF document with a specified password. By default it is `None` — password protection is not applied.
2. `compliance` — allows setting the PDF standards compliance level for the output PDF.
3. `optimize_memory_usage` — a boolean flag that modifies the generation of the output PDF document so that the process takes less memory at the cost of longer processing time. By default it has a `False` value.
4. `font_embedding` — responsible for embedding font resources into the resultant PDF document.

Unlike `PdfLoadOptions` and `PdfEditOptions`, which are optional, `PdfSaveOptions` is mandatory even if all its values are default. After editing, an [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) is created from the modified content and is then passed, together with the save options, to the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method:

```python
from groupdocs.editor import Editor, EditableDocument
from groupdocs.editor.options import PdfEditOptions, PdfSaveOptions

with Editor("./sample-document.pdf") as editor:
    original = editor.edit(PdfEditOptions())

    # Send the content to the WYSIWYG-editor and obtain the edited content (omitted here)
    edited = EditableDocument.from_markup(original.get_embedded_html())

    save_options = PdfSaveOptions()
    save_options.password = "some_password"
    save_options.optimize_memory_usage = True

    editor.save(edited, "./edited-document.pdf", save_options)

    original.dispose()
    edited.dispose()
```

## Different output formats

Keep in mind that when an input PDF was edited and you are going to save it, it is not necessary to save it exactly in the PDF format — you are free to choose any compatible format, like all WordProcessing formats, the text format, or eBook formats.

```python
from groupdocs.editor import Editor, EditableDocument
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import PdfSaveOptions, WordProcessingSaveOptions, TextSaveOptions

with Editor("./sample-document.pdf") as editor:
    edited = editor.edit()

    # Save to PDF
    editor.save(edited, "./edited-document.pdf", PdfSaveOptions())
    # Save to DOCX
    editor.save(edited, "./edited-document.docx", WordProcessingSaveOptions(WordProcessingFormats.DOCX))
    # Save to TXT
    editor.save(edited, "./edited-document.txt", TextSaveOptions())

    edited.dispose()
```

## Obtaining PDF document info

The article [Extracting document metainfo]({{< ref "editor/python-net/developer-guide/extracting-document-metainfo.md" >}}) describes the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method, which allows you to detect the document format and extract its metadata without editing it. This mechanism also works with PDF documents.

When [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) is called for an [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance that is loaded with a PDF document, the method returns a metadata view corresponding to the `FixedLayoutDocumentInfo` type — a common type for all fixed-layout documents, PDF and XPS in particular. It exposes the `format`, `page_count`, `size`, and `is_encrypted` properties. If the input PDF is encoded, its correct password should be specified in the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method.

```python
from groupdocs.editor import Editor

with Editor("./sample-document.pdf") as editor:
    info = editor.get_document_info()
    print("Format:", info.format.name)
    print("Page count:", info.page_count)
    print("Size:", info.size)
    print("Is encrypted:", info.is_encrypted)
```
