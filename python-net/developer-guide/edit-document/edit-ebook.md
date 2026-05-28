---
id: edit-ebook
url: editor/python-net/edit-ebook
title: How to edit e-Book file
linkTitle: How to edit e-Book file
weight: 56
description: "This article demonstrates how to edit e-Book files using the Python programming language."
keywords: GroupDocs.Editor e-Book support, ebook format, editing electronic books, edit e-Book, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit e-Book document in the GroupDocs.Editor
        description: How to edit e-Book document using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to edit content of the e-Book document in the GroupDocs.Editor in Python
        description: Learn how to edit the content of an e-Book document with different editing options using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load the desired e-Book document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired e-Book document into it. Loading options are not needed.
        - name: Prepare an EbookEditOptions class
          text: Create an instance of the EbookEditOptions class and adjust its properties to meet your needs if necessary, for example enabling pagination or language information.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the editor.edit method with a previously prepared EbookEditOptions and obtain an instance of the EditableDocument class, which is ready for editing.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable class methods of the class.
        - name: Prepare an EbookSaveOptions class
          text: Create an instance of the EbookSaveOptions class with the desired output e-Book format and adjust its properties to meet your needs if necessary.
        - name: Save the edited document with the editor.save method
          text: Pass an instance of EditableDocument with the content of the edited document, an instance of EbookSaveOptions, and a destination byte stream or file path to the editor.save method for saving the document.
---
## Introduction

GroupDocs.Editor for Python via .NET supports 3 formats from the e-Book family:

1. [MOBI](https://docs.fileformat.com/ebook/mobi/) (MobiPocket),
2. [AZW3](https://docs.fileformat.com/ebook/azw3/), also known as Kindle Format 8 (KF8),
3. [ePub](https://docs.fileformat.com/ebook/epub/) (Electronic Publication).

All three formats are fully supported on both import (load) and export (save).

## Load e-Book files for edit

GroupDocs.Editor for Python via .NET does not contain loading options either for the whole e-Book formats family or for the specific e-Book formats — users should specify e-Books through a file path or a byte stream without any loading options at all.

```python
from groupdocs.editor import Editor

# Load from a file path
editor_from_path = Editor("book.epub")

# Load from a binary stream
with open("book.epub", "rb") as stream:
    editor_from_stream = Editor(stream)
```

## Edit e-Book files

There is a common edit options class for the whole e-Book formats family — the [`EbookEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebookeditoptions) class. The content of this class resembles the content of the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class, because [`EbookEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebookeditoptions) contains a subset of options from it — `enable_pagination` and `enable_language_information` — and, as in [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions), they are disabled (`False`) by default.

- `enable_pagination` — allows enabling or disabling pagination in the resultant HTML document. By default it is disabled (`False`). This option controls how exactly the content of the e-Book will be converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) representation while edited — in the _float_ (`False`) or in the _paged_ (`True`) mode.
- `enable_language_information` — allows exporting (`True`) or not exporting (`False`) the language information to the resultant HTML markup. By default it is disabled (`False`). This is useful when an e-Book contains text in different languages, and you want to preserve this language-specific metainformation while editing the document in the WYSIWYG-editor.

Like for all supported document formats, the [`EbookEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebookeditoptions) are optional, and the user may call the parameterless [`edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method — in this case the default [`EbookEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebookeditoptions) is implicitly applied.

## Save e-Book files after edit

Saving e-Books is performed like for all other formats. When the e-Book content was edited by the client in the WYSIWYG-editor and sent back to the server-side, it should be passed to the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), and then this instance should be passed to the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.

For saving in one of the e-Book formats the [`EbookSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebooksaveoptions) class should be used. This class is common for all supported e-Book formats within the e-Book family: MOBI, AZW3, and ePub. It has one constructor with a mandatory parameter — the desired output format, which should be specified as one of the [`EBookFormats`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/ebookformats) values: `MOBI`, `AZW3`, or `EPUB`. Once the instance was created, this format can be obtained and changed using the `output_format` property.

The [`EbookSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebooksaveoptions) class also has two other properties:

- `split_heading_level` of integer type controls how (if at all) to split the content of the e-Book into packages in the resultant file. It does not affect the representation of a file opened in any e-Book reader; rather, it is about the internal structure of the e-Book file. The default value is `2`. Setting it to `0` disables splitting.
- `export_document_properties` of boolean type controls whether to export built-in and custom document properties inside the resultant e-Book file. The default `False` value disables exporting document properties, so the resultant document will be a bit smaller in size.

The runnable example below loads an ePub file, edits it with default options, and saves the edited version back to the ePub format.

{{< tabs "code-example-edit-ebook">}}
{{< tab "edit_ebook.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import EBookFormats
from groupdocs.editor.options import EbookEditOptions, EbookSaveOptions

def edit_ebook():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load an input ePub file into the Editor
    with Editor("./sample-ebook.epub") as editor:
        # Create and adjust the e-Book edit options
        edit_options = EbookEditOptions()
        edit_options.enable_pagination = True
        edit_options.enable_language_information = True

        # Edit the e-Book and obtain an EditableDocument
        editable = editor.edit(edit_options)

        # Edit the content programmatically (in practice this is done in a WYSIWYG-editor)
        html = editable.get_embedded_html()
        edited = EditableDocument.from_markup(html.replace("</p>", " (edited)</p>", 1))

        # Create ePub save options and tune them
        save_options = EbookSaveOptions(EBookFormats.EPUB)
        save_options.export_document_properties = True
        save_options.split_heading_level = 3

        # Save the edited document back to the ePub format
        editor.save(edited, "./edited-ebook.epub", save_options)

        editable.dispose()
        edited.dispose()

if __name__ == "__main__":
    edit_ebook()
```
{{< /tab >}}
{{< tab "sample-ebook.epub" >}}  
{{< tab-text >}}
`sample-ebook.epub` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-ebook/sample-ebook.epub) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-ebook.epub" >}}  
```text
Binary file (EPUB, 29 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-ebook/edit_ebook/edited-ebook.epub)
{{< /tab >}}
{{< /tabs >}}

The same approach is used to save into the AZW3 or MOBI formats — just create the [`EbookSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebooksaveoptions) instance with `EBookFormats.AZW3` or `EBookFormats.MOBI` in the constructor.

## Extracting metainfo from e-Book files

Like for all supported formats, GroupDocs.Editor for Python via .NET provides the ability to detect document metainfo for all supported e-Book formats by using the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method of the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class. When a valid e-Book was loaded into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance, [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) returns a metadata view corresponding to the `EbookDocumentInfo` type, which defines the `format`, `page_count`, `size`, and `is_encrypted` properties.

- The `format` property returns an [`EBookFormats`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/ebookformats) value, which for e-Books can be `MOBI`, `AZW3`, or `EPUB`.
- The `page_count` property returns an **approximate** number of pages in the case of MOBI or AZW3, or the number of chapters in the case of ePub. The returned number should be treated carefully and approximately.
- The `size` property returns the number of bytes of the e-Book file.
- The `is_encrypted` property always returns `False`, because e-Books cannot be encrypted with a password.

```python
from groupdocs.editor import Editor

with Editor("./sample-ebook.epub") as editor:
    info = editor.get_document_info()
    print("Format:", info.format.name)
    print("Page count:", info.page_count)
    print("Size:", info.size)
```
