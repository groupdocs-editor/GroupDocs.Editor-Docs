---
id: load-document
url: editor/python-net/load-document
title: Load document
linkTitle: Load document
weight: 4
description: "Following this guide you will learn how to load a document from a local disk or file stream for editing with GroupDocs.Editor for Python via .NET API."
keywords: Load document with GroupDocs.Editor, Load and edit document, edit document, edit spreadsheet, edit presentation, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: Load document into GroupDocs.Editor
        description: Load document into GroupDocs.Editor for subsequent usage on Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to load document to the GroupDocs.Editor in Python
        description: Learn how to load document to the GroupDocs.Editor in Python step by step
        steps:
        - name: Prepare a LoadOptions instance if needed
          text: If the input document is password-protected or requires some adjusting using load options, or it is required to specify its format family explicitly, create an appropriate load options class.
        - name: Invoke the appropriate Editor class constructor
          text: Pass a file path as a string or a binary stream into the most appropriate overload of the constructor of the Editor class.
        - name: Make all necessary edits and savings
          text: After the document is successfully loaded, you can get its metainfo, generate its editable version, and finally save it to the resultant file.
---

This guide explains how to load a document from a local disk or file stream for editing using the GroupDocs.Editor for Python via .NET API.

## Introduction

In this article, you will learn how to load an input document into [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) and apply load options.

## Loading Documents

To load an input document, which should be accessible either as a binary stream or through a valid file path, create an instance of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class using one of its constructor overloads. Below are examples of loading documents from a file path and from a stream.

```python
from groupdocs.editor import Editor

# Load document from a file path
editor = Editor("document.docx")

# Load document from a binary stream
with open("document.docx", "rb") as stream:
    editor = Editor(stream)
```

When using the constructor overloads shown above, GroupDocs.Editor automatically detects the format of the input document and applies the most suitable default loading options. However, it is recommended to specify the correct loading options explicitly by using constructor overloads that accept two parameters. Here is how you can do this:

```python
from groupdocs.editor import Editor
from groupdocs.editor.options import WordProcessingLoadOptions, SpreadsheetLoadOptions

# Load document from a file path with load options
word_load_options = WordProcessingLoadOptions()
editor = Editor("document.docx", word_load_options)

# Load document from a stream with load options
spreadsheet_load_options = SpreadsheetLoadOptions()
with open("spreadsheet.xlsx", "rb") as stream:
    editor = Editor(stream, spreadsheet_load_options)
```

The following complete example loads a document from a local file path with load options and prints its basic metadata:

{{< tabs "code-example-load-document">}}
{{< tab "load_document.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingLoadOptions

def load_document():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Prepare load options for the WordProcessing family
    load_options = WordProcessingLoadOptions()

    # Load the document from a local file path with load options
    with Editor("./sample-document.docx", load_options) as editor:
        info = editor.get_document_info()
        print("Loaded:", info.format.name, "-", info.page_count, "page(s)")

if __name__ == "__main__":
    load_document()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/load-document/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "load-document.txt" >}}  
```text
Loaded: Office Open XML WordProcessingML Macro-Free Document (DOCX) - 3 page(s)
```
[Download full output](/editor/python-net/_output_files/developer-guide/load-document/load_document/load-document.txt)
{{< /tab >}}
{{< /tabs >}}

## Load Options

Please note that not all document formats have associated classes for load options. Only the WordProcessing, Spreadsheet, Presentation families, and a distinct PDF format have specific load options classes. Other formats, such as DSV, TXT, or XML, do not have load options.

| Format Family       | Example Formats            | Load Options Class                                                                                                              |
|---------------------|----------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| WordProcessing      | DOC, DOCX, DOCM, DOT, ODT  | [`WordProcessingLoadOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingloadoptions) |
| Spreadsheet         | XLS, XLSX, XLSM, XLSB      | [`SpreadsheetLoadOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetloadoptions)       |
| Presentation        | PPT, PPTX, PPS, POT        | [`PresentationLoadOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationloadoptions)     |
| Fixed-layout format | PDF                        | [`PdfLoadOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfloadoptions)                       |

## Handling Password-Protected Documents

Using load options is essential when working with password-protected documents. Any document can be loaded into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance, even if it is password-protected. However, if the password is not handled correctly, an exception will be thrown during the editing process. Here is how GroupDocs.Editor handles passwords:

1. If the document is not password-protected, any specified password will be ignored.
2. If the document is password-protected but no password is specified, a [`PasswordRequiredException`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/passwordrequiredexception) will be thrown during editing.
3. If the document is password-protected and an incorrect password is provided, an [`IncorrectPasswordException`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/incorrectpasswordexception) will be thrown during editing.

The example below demonstrates how to specify a password for opening a password-protected WordProcessing document:

```python
from groupdocs.editor import Editor
from groupdocs.editor.options import WordProcessingLoadOptions

word_load_options = WordProcessingLoadOptions()
word_load_options.password = "correct_password"
editor = Editor("protected-document.docx", word_load_options)
```

{{< alert style="info" >}}The same approach applies to Spreadsheet, Presentation, and PDF documents as well.{{< /alert >}}
