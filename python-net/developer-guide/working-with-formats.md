---
id: working-with-formats
url: editor/python-net/working-with-formats
title: Working with formats
linkTitle: Working with formats
weight: 80
description: "This article explains document formats and format families supported by GroupDocs.Editor for Python via .NET and how to operate them in Python code."
keywords: GroupDocs.Editor supported formats, editable document formats, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
> This article describes the classes and enums of [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net), which represent all supportable family formats and individual formats.

GroupDocs.Editor supports different document formats, all of them are conditionally divided into several family formats:

1. WordProcessing formats, which include DOC, DOCX, DOCM, RTF, ODT etc.
2. Spreadsheet formats, which include XLS, XLT, XLSX, XLSM, XLTX, ODS etc.
3. Delimiter-Separated Values (DSV) formats, also known as delimited text, that are a text-based form of spreadsheets, and include CSV, TSV, semicolon-delimited, whitespace-delimited etc.
4. Presentation formats, which include PPT, PPS, POT, PPTX, PPTM etc.
5. Text-based formats, which include TXT, HTML, XML etc.
6. Fixed-layout formats, which include PDF and XPS formats, where their representation is "baked" to be uniform on every platform.
7. eBook formats, which include Mobi, AZW3 and ePub.
8. Email formats, which include MSG, EML, PST and others.

For representing these format families, the [`groupdocs.editor.formats`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/) module provides a dedicated enum per family:

1. [WordProcessingFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/wordprocessingformats), which is common for all WordProcessing formats.
2. [SpreadsheetFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/spreadsheetformats), which is common for all Spreadsheet formats.
3. [PresentationFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/presentationformats), which is common for all Presentation formats.
4. [TextualFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/textualformats), which is common for all text-based formats, including plain text (TXT), markup formats (XML and HTML), and all Delimiter-Separated Values (DSV) formats.
5. [EBookFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/ebookformats), which is common for all E-book formats, including MOBI, AZW3, and ePub.
6. [FixedLayoutFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/fixedlayoutformats), which is common for only PDF and XPS (this includes OpenXPS).
7. [EmailFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/emailformats), which is common for all electronic mail formats.

These enums are case-insensitive and lazy-loaded. Every enum exposes a set of members, each one representing a specific format within the given family format — for example, `WordProcessingFormats.DOCX`, `SpreadsheetFormats.XLSX`, or `EBookFormats.MOBI`. A format member is the value you pass to the matching save options constructor to select the output format.

## Fetching a format

You import the family enum from the `groupdocs.editor.formats` module and access the desired format member by name. The member is then used wherever a target format is required, most notably in the save options constructor:

```python
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

# Fetch one format within the WordProcessing family
docm = WordProcessingFormats.DOCM

# Use it to select the output format
save_options = WordProcessingSaveOptions(docm)
```

The same approach applies to all other families:

```python
from groupdocs.editor.formats import (
    SpreadsheetFormats, PresentationFormats, EBookFormats,
    EmailFormats, FixedLayoutFormats, TextualFormats,
)
from groupdocs.editor.options import (
    SpreadsheetSaveOptions, PresentationSaveOptions, EbookSaveOptions,
)

xlsx_options = SpreadsheetSaveOptions(SpreadsheetFormats.XLSX)
pptx_options = PresentationSaveOptions(PresentationFormats.PPTX)
mobi_options = EbookSaveOptions(EBookFormats.MOBI)
```

## Reading format information

The format descriptor exposed by [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) provides text properties that reflect the format name, MIME code, and file extension:

```python
from groupdocs.editor import Editor

with Editor("document.docx") as editor:
    info = editor.get_document_info()
    print("Name:", info.format.name)
    print("Extension:", info.format.extension)
    print("MIME:", info.format.mime)
```

## Complete code example

The example below loads a document, reads the format descriptor that GroupDocs.Editor detected, and selects an output format from the WordProcessing family to convert the document.

{{< tabs "code-example-working-with-formats">}}
{{< tab "working_with_formats.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

def working_with_formats():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        # Read the detected format descriptor
        info = editor.get_document_info()
        print("Detected format:", info.format.name, "(", info.format.extension, ")")

        # Pick a target format from the WordProcessing family
        target_format = WordProcessingFormats.DOCM
        save_options = WordProcessingSaveOptions(target_format)

        # Convert the document to the selected format
        editable = editor.edit()
        editor.save(editable, "./converted-document.docm", save_options)

if __name__ == "__main__":
    working_with_formats()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/working-with-formats/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "converted-document.docm" >}}  
```text
Binary file (DOCM, 49 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/working-with-formats/working_with_formats/converted-document.docm)
{{< /tab >}}
{{< /tabs >}}
