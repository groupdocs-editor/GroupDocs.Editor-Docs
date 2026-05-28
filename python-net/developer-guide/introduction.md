---
id: introduction
url: editor/python-net/introduction
title: Introduction
linkTitle: Introduction
weight: 2
description: "This article explains main principles and stages of editing documents programmatically with GroupDocs.Editor for Python via .NET API."
keywords: Edit document programmatically, Edit document Python, Edit document principles, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: Introduction to GroupDocs.Editor usage
        description: Description of the purposes and the most basic principles of the GroupDocs.Editor
        productCode: editor
        productPlatform: python-net
    showVideo: True
---
> This article explains the most common and fundamental principles of GroupDocs.Editor, how it works, what is its purpose, and how it should be used.

[**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) is a GUI-less class library, which means that it has only a programmatic interface (API). This fact means that in order to edit a document the user must use GroupDocs.Editor in conjunction with some 3rd-party editor application, through which GUI the end-user is able to edit document content. For GroupDocs.Editor it is not important which exactly editor software is used. But because GroupDocs.Editor is aimed at web-development, it has the only requirement — the 3rd-party editor should be compatible with HTML documents.

In order to edit a document with GroupDocs.Editor, the user must perform several sequential steps: load the document into GroupDocs.Editor (using optional load options), open the document for editing (with optional edit options), generate HTML markup with resources (using different options and settings), and pass this markup to the 3rd-party WYSIWYG HTML-editor. Then the end-user edits the document content, and when he finishes editing and submits the edited document, this modified markup should be transferred back to GroupDocs.Editor and converted to the output document of the desired format.

From the GroupDocs.Editor perspective, this pipeline can be conditionally divided into three main stages, that are described below.

## Loading document into the GroupDocs.Editor

On the *[loading document]({{< ref "editor/python-net/developer-guide/load-document.md" >}})* stage the user should create an instance of the [`Editor` class](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) and pass an input document (through a file path or binary stream) along with document load options. Loading options are not required and GroupDocs.Editor can automatically detect the document format and select the most appropriate default options for the given format. But it is recommended to specify them explicitly. They are inevitable when trying to load password-protected documents.

```python
from groupdocs.editor import Editor

# Passing a path to the constructor; default WordProcessingLoadOptions will be applied automatically
editor = Editor("document.docx")
```

After this stage the document is ready to be opened and edited.

## Opening a document for editing

Because GroupDocs.Editor is a GUI-less library, a document cannot be edited directly within it. But in order to edit a document in a WYSIWYG HTML-editor, GroupDocs.Editor needs to generate an HTML-version of the document, because any WYSIWYG editor can work only with HTML/CSS markup. When an instance of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class is created on the 1st stage, the user should open the document for editing by calling the [`edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class. This method returns an instance of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class. This class can be described as a converted version of the input document, that is stored in an internal intermediate format, compatible with all formats that GroupDocs.Editor supports. With [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) the user can obtain HTML markup of the input document with different options, stylesheets, images, fonts, save an HTML-document to disk, and other things. It is implied that the HTML-markup, emitted by [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), is then passed into the client-side WYSIWYG HTML-editor, where the end-user can actually edit the document.

Like with loading, the [`edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method accepts optional edit options, that control how exactly the document will be opened for editing.

```python
from groupdocs.editor.options import WordProcessingEditOptions

edit_options = WordProcessingEditOptions()
edit_options.enable_language_information = True

ready_to_edit = editor.edit(edit_options)
```

After this stage the document is ready to be passed to the WYSIWYG HTML-editor and its content can be edited by the end-user.

## Saving a document

*[Saving a document]({{< ref "editor/python-net/developer-guide/save-document.md" >}})* is the final stage, which occurs when document content was edited in the WYSIWYG HTML-editor (or any other software, this makes no difference for GroupDocs.Editor) and should be saved back as a document of some format (like DOCX, PDF, or XLSX, for example). At this stage the user should create a new instance of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class with HTML-markup and resources of the edited version of the original document, that was obtained from the end-user. The [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class contains several class methods, that allow to create its instances from HTML documents, which may be presented in different forms. And when an [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance is ready, it is possible to save it as an ordinary document using the [`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class.

```python
from groupdocs.editor import EditableDocument
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

after_edit = EditableDocument.from_markup("<body>HTML content of the document...</body>")
save_options = WordProcessingSaveOptions(WordProcessingFormats.RTF)
editor.save(after_edit, "document.rtf", save_options)
```

Unlike the previous load options and edit options, save options are mandatory, because GroupDocs.Editor needs to know the exact document format for saving.

## Detecting document type

Sometimes it is necessary to *[detect a document type and extract its metadata]({{< ref "editor/python-net/developer-guide/extracting-document-metainfo.md" >}})* before sending it for editing. For such scenarios GroupDocs.Editor allows to detect the document type and extract its most necessary metainfo depending on the document type:

1. Is the document encoded or not;
2. Exact document format;
3. Document size;
4. Number of pages (tabs);
5. Text encoding, if the document is textual.

In order to detect the document type and gather its meta info, the user should load the desired document into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class and then call the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method.

## Describing options

On every stage the user can adjust (tune) the processing by different options:

1. Load options for *loading* a document.
2. Edit options for opening a document for *editing*.
3. Save options for *saving* an edited document.

Some of these options may be optional in specific cases, some are mandatory. For example, it is possible to load a document into the `Editor` class without load options — in such a case GroupDocs.Editor will try to detect the document format automatically and apply the most appropriate default options for the detected document format.

### Describing family formats

All document formats, which GroupDocs.Editor supports, are grouped into family formats. Each family format has a lot of common features, so there are no options for each format — only for the family format. The relation between formats, family formats, import/export formats and options is illustrated in the table below.

| Family format | Supported formats | Load | Save | Load options | Edit options | Save options | Metadata |
| --- | --- | --- | --- | --- | --- | --- | --- |
| WordProcessing | DOC, DOCX, DOCM, DOT, DOTX, DOTM, RTF, WordprocessingML Flat XML, ODT, OTT, Word 2003 XML | ![(tick)](/editor/python-net/images/check.png) | ![(tick)](/editor/python-net/images/check.png) | [WordProcessingLoadOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingloadoptions) | [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) | [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) | [WordProcessingDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/wordprocessingdocumentinfo) |
| Spreadsheet | XLS, XLT, XLSX, XLSM, XLSB, XLTX, XLTM, XLAM, SpreadsheetML XML, ODS, FODS, SXC, DIF | ![(tick)](/editor/python-net/images/check.png) | ![(tick)](/editor/python-net/images/check.png) | [SpreadsheetLoadOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetloadoptions) | [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheeteditoptions) | [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) | [SpreadsheetDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/spreadsheetdocumentinfo) |
| DSV | CSV, TSV, semicolon-separated, whitespace-separated, arbitrary separator | ![(tick)](/editor/python-net/images/check.png) | ![(tick)](/editor/python-net/images/check.png) | N/A | [DelimitedTextEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/delimitedtexteditoptions) | [DelimitedTextSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/delimitedtextsaveoptions) | N/A |
| Presentation | PPT, PPTX, PPTM, PPS, PPSX, PPSM, POT, POTX, POTM, ODP, OTP | ![(tick)](/editor/python-net/images/check.png) | ![(tick)](/editor/python-net/images/check.png) | [PresentationLoadOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationloadoptions) | [PresentationEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationeditoptions) | [PresentationSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions) | [PresentationDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/presentationdocumentinfo) |
| XML | Any XML document | ![(tick)](/editor/python-net/images/check.png) | ![(error)](/editor/python-net/images/error.png) | N/A | [XmlEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xmleditoptions) | N/A | [TextualDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/textualdocumentinfo) |
| TXT | Any text document | ![(tick)](/editor/python-net/images/check.png) | ![(tick)](/editor/python-net/images/check.png) | N/A | [TextEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/texteditoptions) | [TextSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/textsaveoptions) | [TextualDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/textualdocumentinfo) |
| Fixed-layout format | PDF | ![(tick)](/editor/python-net/images/check.png) | ![(tick)](/editor/python-net/images/check.png) | [PdfLoadOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfloadoptions) | [PdfEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfeditoptions) | [PdfSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfsaveoptions) | [FixedLayoutDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/fixedlayoutdocumentinfo) |
| Fixed-layout format | XPS (including OpenXPS) | ![(error)](/editor/python-net/images/error.png) | ![(tick)](/editor/python-net/images/check.png) | N/A | N/A | [XpsSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xpssaveoptions) | [FixedLayoutDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/fixedlayoutdocumentinfo) |
| e-Book | Mobi, AZW3, ePub | ![(tick)](/editor/python-net/images/check.png) | ![(tick)](/editor/python-net/images/check.png) | N/A | [EbookEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebookeditoptions) | [EbookSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebooksaveoptions) | [EbookDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/ebookdocumentinfo) |
| Email | EML, EMLX, TNEF, MSG, HTML, MHTML, ICS, VCF, PST, MBOX, OFT | ![(tick)](/editor/python-net/images/check.png) | ![(tick)](/editor/python-net/images/check.png) | N/A | [EmailEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emaileditoptions) | [EmailSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emailsaveoptions) | [EmailDocumentInfo](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/emaildocumentinfo) |


### Additional materials

Detailed information about every stage of document processing along with source code examples, options explanations and so on, can be found in the next articles:

1. [Load document]({{< ref "editor/python-net/developer-guide/load-document.md" >}})
2. [Save document]({{< ref "editor/python-net/developer-guide/save-document.md" >}})

Detailed reviews of the supported family formats and the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class, along with source code examples, can be found in the next articles:

* [Create and edit new WordProcessing document]({{< ref "editor/python-net/developer-guide/create-document.md" >}})
* [Extracting document metainfo]({{< ref "editor/python-net/developer-guide/extracting-document-metainfo.md" >}})
* [Working with formats]({{< ref "editor/python-net/developer-guide/working-with-formats.md" >}})
* [Working with HTML resources]({{< ref "editor/python-net/developer-guide/working-with-html-resources.md" >}})
* [How to edit Mobi file]({{< ref "editor/python-net/developer-guide/working-with-mobi-documents.md" >}})
