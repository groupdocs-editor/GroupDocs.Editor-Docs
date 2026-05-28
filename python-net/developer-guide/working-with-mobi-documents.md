---
id: how-to-edit-mobi-file
url: editor/python-net/how-to-edit-mobi-file
title: How to edit Mobi file
linkTitle: How to edit Mobi file
weight: 58
description: "This article demonstrates how to edit Mobi files using the Python programming language."
keywords: GroupDocs.Editor Mobi support, Mobi format, editing Mobi documents, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
## Introduction

Mobi format is an E-Book format, developed by the French company MobiPocket and based on XML. E-Books in this format can contain text with rich formatting, images, and different annotations like bookmarks, notes, highlights, corrections and so on. Mobi books can have DRM protection.

GroupDocs.Editor for Python via .NET is able to open (load) Mobi documents for editing, edit them, and save (export) documents back to the Mobi format, so Mobi is fully supported: on import, export and auto-detection. The closely related [AZW3 format](https://docs.fileformat.com/ebook/azw3/), also known as Kindle Format 8 (KF8), which may be considered a successor to Mobi, is supported on both import and export as well.

## Loading a Mobi file for editing

Despite being a distinct format, which doesn't belong to any of the existing format families, Mobi has no dedicated loading options. So for loading it into an instance of the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class, users should simply specify a path to the Mobi file or a stream with its content in the constructor:

```python
from groupdocs.editor import Editor

# Load from a file path
editor_from_path = Editor("book.mobi")

# Load from a binary stream
with open("book.mobi", "rb") as stream:
    editor_from_stream = Editor(stream)
```

There are no loading options, because Mobi has nothing to tune up during loading — it cannot have password protection, and can be processed only in one way.

## Editing a Mobi file

Because Mobi belongs to the e-Book format family, it uses common edit options for all e-Book formats — the [`EbookEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebookeditoptions). This class may be described as a truncated version of the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class, because [`EbookEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebookeditoptions) contains a subset of options from [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) — `enable_pagination` and `enable_language_information` and, as in [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions), they are disabled (`False`) by default.

They have exactly the same meaning as their "siblings" from [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions). `enable_pagination` allows switching between float and paginal mode in the resultant HTML document. `enable_language_information` allows enabling the export of language information in HTML. This is very useful for books, which have parts of text written in different languages.

An example of usage is below (let's assume that an [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance with a loaded Mobi document is already created):

```python
from groupdocs.editor.options import EbookEditOptions

edit_options = EbookEditOptions()
edit_options.enable_pagination = True
edit_options.enable_language_information = True
opened = editor.edit(edit_options)
# save it or pass it to the WYSIWYG-editor
```

## Saving a Mobi file after editing

For all the format families the saving procedure is the same — it is required to obtain the content of the edited document on the server-side, create an instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) from it, and then pass it to the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method. Because the Mobi format belongs to the e-Book formats family, in order to save a document in the Mobi format an [`EbookSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebooksaveoptions) class is required.

This class has a mandatory constructor with a single parameter — the desired [e-Book format](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/ebookformats/). For Mobi it should be `EBookFormats.MOBI`. All other parameters in the `EbookSaveOptions` class are in fact optional, but are nevertheless described below:

- `split_heading_level` of integer type controls the internal structure of the generated Mobi file: whether its internal content is divided into packages and, if yes, then how. This parameter exists because the content of a Mobi file is stored in packages, usually one package per chapter. By default this parameter is `2` — the most optimal.
- `export_document_properties` of boolean type also relates to the internal structure of the Mobi file — it decides whether to embed the built-in and custom document properties inside the resultant Mobi file (`True`) or not (`False`). By default it is `False` — do not embed.

Concluding: in order to save the edited document in the Mobi format, the user should create an instance of the [`EbookSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebooksaveoptions) class with the `EBookFormats.MOBI` argument in the constructor parameter, and then pass this instance to the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method along with the other arguments.

The code example below demonstrates the full round-trip: opening a Mobi document, editing it, and saving the edited version to the Mobi format.

{{< tabs "code-example-working-with-mobi-documents">}}
{{< tab "edit_mobi_document.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import EBookFormats
from groupdocs.editor.options import EbookEditOptions, EbookSaveOptions

def edit_mobi_document():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Create an Editor instance and load an input Mobi file
    with Editor("./sample-ebook.mobi") as editor:
        # Create edit options for the e-Book family
        edit_options = EbookEditOptions()
        edit_options.enable_pagination = True
        edit_options.enable_language_information = True

        # Edit the Mobi document and obtain an EditableDocument
        editable = editor.edit(edit_options)

        # Edit the content programmatically (in practice this is done in a WYSIWYG-editor)
        html = editable.get_embedded_html()
        edited = EditableDocument.from_markup(html.replace("Title of the document", "Title of the edited document"))

        # Create Mobi save options and tune them
        save_options = EbookSaveOptions(EBookFormats.MOBI)
        save_options.export_document_properties = True

        # Save the edited document in the Mobi format
        editor.save(edited, "./edited-ebook.mobi", save_options)

        editable.dispose()
        edited.dispose()

if __name__ == "__main__":
    edit_mobi_document()
```
{{< /tab >}}
{{< tab "sample-ebook.mobi" >}}  
{{< tab-text >}}
`sample-ebook.mobi` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/working-with-mobi-documents/sample-ebook.mobi) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-ebook.mobi" >}}  
```text
Binary file (MOBI, 28 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/working-with-mobi-documents/edit_mobi_document/edited-ebook.mobi)
{{< /tab >}}
{{< /tabs >}}

## Detecting a Mobi file

As for documents of all supported types, Mobi documents can be detected using the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method of the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class. When a valid Mobi document was loaded into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance, [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) returns a metadata view corresponding to the [`EbookDocumentInfo`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/ebookdocumentinfo) type, which defines four properties: `format`, `page_count`, `size`, and `is_encrypted`.

- The `format` property returns an [`EBookFormats`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/ebookformats) value, which for files with a `*.mobi` extension can be `MOBI` or `AZW3`.
- The `page_count` property returns an **approximate** number of pages in the case of MOBI or AZW3, or the number of chapters in the case of ePub. For Mobi it is approximate, because the Mobi format internally is a set of HTML documents (chapters), which are not separated into pages and have no strict page dimensions. So, for returning a page count for a Mobi document, GroupDocs.Editor assumes a standard A4 page size in portrait orientation, splits the existing document content onto such "papers", and then calculates the count. The returned number should be treated very carefully and approximately.
- The `size` property returns the number of bytes of the Mobi document.
- The `is_encrypted` property always returns `False`, because Mobi documents cannot be encrypted with a password, like PDF or Office Open XML.
