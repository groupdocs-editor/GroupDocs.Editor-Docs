---
id: edit-document
url: editor/python-net/edit-document
title: Edit document
linkTitle: Edit document
weight: 6
description: "Follow this guide and learn how to edit text documents, spreadsheets and presentations using GroupDocs.Editor for Python via .NET API features."
keywords: Edit document, edit presentation, edit spreadsheet, GroupDocs.Editor, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: Edit document in the GroupDocs.Editor
        description: Edit document using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to edit document using the GroupDocs.Editor in Python
        description: Learn how to edit document using the GroupDocs.Editor in Python step by step
        steps:
        - name: Create the appropriate edit options instance
          text: Each format family has its own edit options class. You need to create an edit options class that corresponds to the format family of the input document.
        - name: Adjust the edit options
          text: When the edit options are created, you need to adjust them to meet your needs — select pagination mode (for WordProcessing documents), desired tab (Spreadsheet), slide (Presentation), or separator (Delimiter-separated values) etc.
        - name: Invoke the editor.edit method
          text: After the document is successfully loaded into the Editor class instance and the appropriate edit options are ready, call the editor.edit method with the specified options and obtain an instance of the generated EditableDocument.
        - name: Obtain HTML markup and resources from EditableDocument
          text: When the EditableDocument is generated, use its methods and properties to obtain the HTML markup and all related HTML resources (stylesheets, fonts, images, audio) in order to send and use them in the WYSIWYG HTML-editor.
---
> This article describes how to open a previously loaded document for editing, which options should be applied, and how to send the document content to the WYSIWYG HTML-editor or any other editing application.

When a document is loaded into an instance of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class, it is possible to open it for editing. In terms of [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net), opening a document for editing implies creating an instance of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class by calling the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor).[`edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) instance method. The `edit()` method can be called with a single parameter — an edit options instance — or without any parameter at all.

Each format family has its own edit options class. They are listed in the table below.

| Format family | Example formats | Edit options class |
| --- | --- | --- |
| WordProcessing | DOC, DOCX, DOCM, DOT, ODT | [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) |
| Spreadsheet | XLS, XLSX, XLSM, XLSB | [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheeteditoptions) |
| Delimiter-Separated Values (DSV) | CSV, TSV | [DelimitedTextEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/delimitedtexteditoptions) |
| Presentation | PPT, PPTX, PPS, POT | [PresentationEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationeditoptions) |
| Plain Text documents | TXT | [TextEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/texteditoptions) |
| Fixed-layout format | PDF | [PdfEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfeditoptions) |
| Fixed-layout format | XPS (including OpenXPS) | [XpsEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xpseditoptions) |
| XML | Any XML-based format like CSPROJ, SVG, and so on | [XmlEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xmleditoptions) |
| eBook | MOBI, AZW3, ePub | [EbookEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebookeditoptions) |
| Email | EML, EMLX, TNEF, MSG, HTML, MHTML, ICS, VCF, PST, MBOX, OFT | [EmailEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emaileditoptions) |

The parameterless `edit()` overload chooses the most appropriate default edit options based on the input document format.

The [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance holds a version of the input document, converted to an internal intermediate format according to the edit options. When it is ready, it can emit HTML, CSS, and other appropriate content that can be passed directly to the WYSIWYG editor. This is demonstrated below.

```python
from groupdocs.editor import Editor
from groupdocs.editor.options import WordProcessingLoadOptions, WordProcessingEditOptions

editor = Editor("document.docx", WordProcessingLoadOptions())
opened_document = editor.edit()  # with default edit options

edit_options = WordProcessingEditOptions()
edit_options.enable_language_information = True
edit_options.enable_pagination = True

another_opened_document = editor.edit(edit_options)
```

Several [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instances can be generated from a single [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance with different edit options. For example, for a WordProcessing document, the first time a user can call the [`edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method with the paged mode disabled, and the second time with it enabled. In other words, several different [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) representations of a single original document can be generated. Another example — there can be multiple [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instances for a single input Spreadsheet document, where each instance represents a different tab of the Spreadsheet document. Such an example is shown below.

```python
from groupdocs.editor import Editor
from groupdocs.editor.options import SpreadsheetLoadOptions, SpreadsheetEditOptions

editor = Editor("spreadsheet.xlsx", SpreadsheetLoadOptions())

edit_options1 = SpreadsheetEditOptions()
edit_options1.worksheet_index = 0  # index is 0-based, so this is the 1st tab
edit_options1.exclude_hidden_worksheets = True

edit_options2 = SpreadsheetEditOptions()
edit_options2.worksheet_index = 1  # index is 0-based, so this is the 2nd tab
edit_options2.exclude_hidden_worksheets = False

first_tab = editor.edit(edit_options1)
second_tab = editor.edit(edit_options2)
```

When the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance is ready, it can emit HTML markup, CSS markup, and other resources in different forms for passing them to the client-side WYSIWYG HTML-editor or any other application that can edit HTML documents. This is briefly shown in the example below.

```python
document = editor.edit()
body_content = document.get_body_content()
only_images = document.images
all_resources_together = document.all_resources
```

For more information about obtaining HTML markup and resources from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), please visit the [Get HTML markup in different forms]({{< ref "editor/python-net/developer-guide/editabledocument/get-html-markup-in-different-forms.md" >}}) and [Save HTML to folder]({{< ref "editor/python-net/developer-guide/editabledocument/save-html-to-folder.md" >}}) articles.
