---
id: extracting-document-metainfo
url: editor/python-net/extracting-document-metainfo
title: Extracting document metainfo
linkTitle: Extracting document metainfo
weight: 10
description: "Following this guide you will learn how to obtain basic document metadata like pages count, size, file type before editing it with GroupDocs.Editor for Python via .NET API."
keywords: Extract document metadata, Get document info, obtain basic document metadata, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: Extract document metainfo in the GroupDocs.Editor
        description: Extract information about document format and other properties using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to extract document metainfo using the GroupDocs.Editor in Python
        description: Learn how to extract information about document format and other properties using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load the desired document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired document into it.
        - name: Invoke the editor.get_document_info method
          text: Once the document is loaded, call the editor.get_document_info method and specify an optional password for the document, if the document is password-protected.
        - name: Examine the obtained document info
          text: The editor.get_document_info method returns a lightweight view of the document metadata that is the most appropriate for the document format. For example, for an input document in WordProcessing format it returns information about page count, protection, exact format, and some other data.
---
> This demonstration shows and explains the usage of the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method, that extracts meta info from the document.

## Introduction

In some situations it is required to grab meta info from a document before actually editing it. For example, the user wants to edit the last tab of a multi-tabbed spreadsheet, but he doesn't know how many tabs the document contains. Or it is unclear for the user whether the document is password-protected or not. For such situations [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) provides a [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method, that returns detailed meta info (metadata) about the specified document.

## Using the method

In order to grab the meta info from a document, it should firstly be loaded into the `Editor` class. Then [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) should be called. This method accepts one optional parameter — the password as a string. If the document is encoded and the user knows the password, he can specify it here. For other cases, the password can be omitted. The code example below demonstrates the usage:

```python
from groupdocs.editor import Editor

with Editor("document.docx") as editor:
    info_without_password = editor.get_document_info()
    info_with_password = editor.get_document_info(password="password")
```

There can be several scenarios here regarding whether the document is encoded or not, and whether the user specified a password:

1. If a password is specified, but the document is not password-protected, or the document format doesn't support encoding at all, the password will be ignored.
2. If the document is password-protected, but a password is not specified, the [`PasswordRequiredException`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/passwordrequiredexception) will be thrown while calling [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo).
3. If the document is password-protected, and a password is specified, but it is incorrect, the [`IncorrectPasswordException`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/incorrectpasswordexception) will be thrown while calling [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo).

## Explaining the resulting type

The [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method returns a lightweight view of the document metadata. It supports `snake_case` attribute access as well as dict-style access for the underlying PascalCase keys. It contains the next properties:

1. `page_count`. This is a positive number, that returns the page count for WordProcessing, PDF and XPS documents, tabs (worksheets) count for Spreadsheets, slides count for Presentations and a number `1` for pageless documents like XML or TXT.
2. `size`. The document size in bytes.
3. `is_encrypted`. A boolean flag that indicates whether the document is encrypted with a password or not. If the document is of a type that doesn't support encryption at all, like CSV or XML, this property always returns `False`.
4. `format`. Returns info about the format itself.

Internally GroupDocs.Editor provides a dedicated metadata type for every family format, all of which expose the four properties above:

1. [WordProcessingDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/wordprocessingdocumentinfo) — common for all WordProcessing family formats.
2. [SpreadsheetDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/spreadsheetdocumentinfo) — common for all Spreadsheet family formats.
3. [PresentationDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/presentationdocumentinfo) — common for all Presentation family formats.
4. [TextualDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/textualdocumentinfo) — common for all textual types, including all DSV (like CSV and TSV), XML, HTML, and plain text.
5. [FixedLayoutDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/fixedlayoutdocumentinfo) — common for all documents with a fixed-layout format, this includes only PDF and XPS.
6. [EmailDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/emaildocumentinfo) — common for all Email family formats, like EML, MSG, VCF, PST, MBOX and others.
7. [EbookDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/ebookdocumentinfo) — common for all eBook family formats like MOBI and ePub.
8. [MarkdownDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/markdowndocumentinfo) — a special type dedicated especially to the Markdown (MD) textual format.

One important thing to note: if [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) returns a `None` value, this means that the specified document is not supported by GroupDocs.Editor and thus cannot be opened for editing or saved.

## Explaining the document format

The metadata view contains a `format` property. The format descriptor indicates one particular document format and stores the format name, extension, and MIME-code. It delivers the next properties:

1. `name` — provides the name of the format.
2. `extension` — provides the format extension.
3. `mime` — provides the MIME-code for the particular format.
4. `format_family` — provides the family format the format belongs to.

The format descriptors are grouped by family:

1. [WordProcessingFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/wordprocessingformats) — holds all formats from the WordProcessing family.
2. [SpreadsheetFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/spreadsheetformats) — holds all formats from the Spreadsheet family.
3. [PresentationFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/presentationformats) — holds all formats from the Presentation family.
4. [TextualFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/textualformats) — holds all formats with a text-based nature.
5. [FixedLayoutFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/fixedlayoutformats) — holds all formats from the fixed-layout family. This includes only PDF and XPS.
6. [EBookFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/ebookformats) — holds all eBook (Electronic book) formats like Mobi and ePub.
7. [EmailFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/emailformats) — holds all email (electronic mail) formats like EML and MSG.

## Complete code example

The example below loads a document, reads its metadata without performing a full edit pass, and prints the most useful fields.

{{< tabs "code-example-extracting-document-metainfo">}}
{{< tab "extracting_document_metainfo.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def extracting_document_metainfo():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load the document and read its metadata
    with Editor("./sample-document.docx") as editor:
        info = editor.get_document_info()

        print("Format:", info.format.name)
        print("Extension:", info.format.extension)
        print("MIME:", info.format.mime)
        print("Pages:", info.page_count)
        print("Size, bytes:", info.size)
        print("Encrypted:", info.is_encrypted)

if __name__ == "__main__":
    extracting_document_metainfo()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/extracting-document-metainfo/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extracting-document-metainfo.txt" >}}  
```text
Format: Office Open XML WordProcessingML Macro-Free Document (DOCX)
Extension: docx
MIME: application/vnd.openxmlformats-officedocument.wordprocessingml.document
Pages: 3
Size, bytes: 49455
Encrypted: False
```
[Download full output](/editor/python-net/_output_files/developer-guide/extracting-document-metainfo/extracting_document_metainfo/extracting-document-metainfo.txt)
{{< /tab >}}
{{< /tabs >}}
