---
id: introduction
url: editor/java/introduction
title: Introduction
weight: 1
description: "This is an introduction into edit document techniques explanation like main stages of document opening, editing and saving results within Java applications."
keywords: Edit document programmatically, Edit document Java, Edit document principles
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: Introduction to GroupDocs.Editor usage
        description: Description of the purposes and the most basic principles of the GroupDocs.Editor
        productCode: editor
        productPlatform: java 
    showVideo: True 
---
> This article explains the most common and fundamental principles of GroupDocs.Editor, how it works, what is its purpose, and how it should be used.

[**GroupDocs.Editor**](https://products.groupdocs.com/editor/java) is a Java GUI-less class library, which means that it has only programmatic interface (API). This fact means that in order to edit a document user must use GroupDocs.Editor in conjunction with some 3rd-party editor application, through which GUI the end-user is able to edit document content. For GroupDocs.Editor it is not important which exactly editor software is used. But because GroupDocs.Editor is aimed on web-development, it has the only requirement — 3rd-party editor should be compatible with HTML documents.

In order to edit a document with GroupDocs.Editor, user must perform several sequential steps: load document into GroupDocs.Editor (using optional load options), open document for editing (with optional edit options), generate HTML markup with resources (using different options and settings), and pass this markup to the 3rd-party WYSIWYG HTML-editor. Then end-user edits the document content, and when he finish editing and submits the edited document, this modified markup should be transferred back to the GroupDocs.Editor and converted to the output document of desired format.

From the GroupDocs.Editor perspective, this pipeline can be conditionally divided onto three main stages, that are described below.

## Loading document into the GroupDocs.Editor

On the *[loading document]({{< ref "editor/java/developer-guide/load-document.md" >}})* stage user should create an instance of [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class and pass an input document (through file path or byte stream) along with document load options. Loading options are not required and GroupDocs.Editor can automatically detect document format and select the most appropriate default options for the given format. But it is recommended to specify them explicitly. They are inevitable when trying to load password-protected documents.

```java
String inputFilePath = "C:\\input_path\\document.docx"; //path to some document
Editor editor = new Editor(inputFilePath); //passing path to the constructor, default WordProcessingLoadOptions will be applied automatically
```

After this stage document is ready to be opened and edited.

## Opening a document for editing

Because GroupDocs.Editor is GUI-less library, document cannot be edited directly into it. But in order to edit document in WYSIWYG HTML-editor, GroupDocs.Editor needs to generate an HTML-version of a document, because any WYSIWYG editor can work only with HTML/CSS markup. When instance of [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class is created on the 1st stage, user should *[open document for editing]({{< ref "editor/java/developer-guide/edit-document" >}})*

by calling an [edit()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor#edit--) method of [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class. This method returns an instance of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class. This class can be described as a converted version of input document, that is stored in internal intermediate format, compatible with all formats, that GroupDocs.Editor supports. With [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) user can obtain HTML markup of the input document with different options, stylesheets, images, fonts, save HTML-document to disk, and other things. It is implied that HTML-markup, emitted by [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument), then is passed into the client-side WYSIWYG HTML-editor, where end-user can actually edit the document.

Like with loading, [edit()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor#edit--) method obtains optional [IEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ieditoptions) inheritors, that controls how exactly the document will be opened for edit.

```java
WordProcessingEditOptions editOptions = new WordProcessingEditOptions();
editOptions.setEnableLanguageInformation(true);

EditableDocument readyToEdit = editor.edit(editOptions);
```

After this stage document is ready to be passed to the WYSIWYG HTML-editor and its content can be edited by the end-user.

## Saving a document

*[Saving a document]({{< ref "editor/java/developer-guide/save-document.md" >}})* is a final stage, which occurs when document content was edited in the WYSIWYG HTML-editor (or any other software, this has no difference for GroupDocs.Editor) and should be saved back as a document of some format (like DOCX, PDF, or XLSX, for example). At this stage user should create a new instance of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class with HTML-markup and resources of edited version of the original document, that was obtained from end-user. [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class contains several static methods, that allows to create its instances from HTML documents, that may be presented in different forms. And when [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance is ready, it is possible to save it as an ordinary document using a [save()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor#save--) method of [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class.

```java
EditableDocument afterEdit = EditableDocument.fromMarkup("<body>HTML content of the document...</body>", null);
String outputFilePath = "C:\\output_path\\document.rtf";
WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Rtf);
editor.save(afterEdit, outputFilePath, saveOptions);
```

Unlike with previous load options and edit options, save options are mandatory, because GroupDocs.Editor needs to know exact document format for saving.

## Detecting document type

Sometimes it is necessary to *[detect a document type and extract its metadata]({{< ref "editor/java/developer-guide/extracting-document-metainfo.md" >}})* before sending it for editing. For such scenarios GroupDocs.Editor allows to detect document type and extract its the most necessary metainfo depending on document type:

1. Is document encoded or not;
2. Exact document format;
3. Document size;
4. Number of pages (tabs);
5. Text encoding, if document is textual.

In order to detect document type and gather its meta info, user should load a desired document into the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class and then call a [getDocumentInfo()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getdocumentinfo--) method.

## Describing options

On every stage user can adjust (tune) the processing by different options:

1. [ILoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/iloadoptions) for *loading* document.
2. [IEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ieditoptions) for opening document for *editing*.
3. [ISaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/isaveoptions) for *saving* edited document.

Some of these options may be optional in specific cases, some are mandatory. For example, it is possible to load a document into the `Editor` class without loading options, — in such case GroupDocs.Editor will try to detect the document format automatically and apply the most appropriate default options for detected document format.

### Describing family formats

All document formats, which GroupDocs.Editor supports, are grouped into family formats. Each family format has lot of common features, so there is no options for each format — only for family format. Relation between formats, family formats, import/export formats and options is illustrated in the table below.

| Family format | Supported formats | Load | Save | Load options | Edit options | Save options | Metadata |
| --- | --- | --- | --- | --- | --- | --- | --- |
| WordProcessing | DOC, DOCX, DOCM, DOT,DOTX, DOTM, RTF,WordprocessingML Flat XML, ODT, OTT, Word 2003 XML | ![(tick)](/editor/java/images/check.png) | ![(tick)](/editor/java/images/check.png) | [WordProcessingLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingloadoptions) | [WordProcessingEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions) | [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions) | [WordProcessingDocumentInfo](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/wordprocessingdocumentinfo) |
| Spreadsheet | XLS, XLT, XLSX, XLSM, XLSB, XLTX, XLTM, XLAM,SpreadsheetML XML, ODS, FODS, SXC, DIF | ![(tick)](/editor/java/images/check.png) | ![(tick)](/editor/java/images/check.png) | [SpreadsheetLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetloadoptions) | [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheeteditoptions) | [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) | [SpreadsheetDocumentInfo](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/spreadsheetdocumentinfo) |
| DSV | CSV, TSV, semicolon-separated,whitespace-separated, arbitrary separator | ![(tick)](/editor/java/images/check.png) | ![(tick)](/editor/java/images/check.png) | N/A | [DelimitedTextEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions) | [DelimitedTextSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions) | N/A |
| Presentation | PPT, PPTX, PPTM, PPS, PPSX, PPSM,POT, POTX, POTM, ODP, OTP | ![(tick)](/editor/java/images/check.png) | ![(tick)](/editor/java/images/check.png) | [PresentationLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationloadoptions) | [PresentationEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions) | [PresentationSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions) | [PresentationDocumentInfo](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/presentationdocumentinfo) |
| XML | Any XML document | ![(tick)](/editor/java/images/check.png) | ![(error)](/editor/java/images/error.png) | N/A | [XmlEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions) | N/A | [TextualDocumentInfo](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo) |
| TXT | Any text document | ![(tick)](/editor/java/images/check.png) | ![(tick)](/editor/java/images/check.png) | N/A | [TextEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/texteditoptions) | [TextSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/textsaveoptions) | [TextualDocumentInfo](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo) |
| Fixed-layout format | PDF | ![(error)](/editor/java/images/error.png) | ![(tick)](/editor/java/images/check.png) | N/A | N/A | [PdfSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions) | N/A |

### Additional materials

Detailed information about every stage of document processing along with source code examples, options explanations and so on, can be found in the next articles:

1. [Load document]({{< ref "editor/java/developer-guide/load-document.md" >}}
.    )
2. [Edit document]({{< ref "editor/java/developer-guide/edit-document" >}})
3. [Save document]({{< ref "editor/java/developer-guide/save-document.md" >}})

Complete description of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class, all its possibilities, members and purpose, along with source code example, is located in the next articles:

* [Working with EditableDocument]({{< ref "editor/java/developer-guide/editabledocument/_index.md" >}})
* [Get HTML markup in different forms]({{< ref "editor/java/developer-guide/editabledocument/get-html-markup-in-different-forms.md" >}})
* [Save HTML to folder]({{< ref "editor/java/developer-guide/editabledocument/save-html-to-folder.md" >}})
* [Working with resources]({{< ref "editor/java/developer-guide/editabledocument/working-with-resources.md" >}})
* [Create EditableDocument from file or markup]({{< ref "editor/java/developer-guide/editabledocument/create-editabledocument-from-file-or-markup.md" >}})

Detailed review of all supported family formats together with explaining their load/edit/save options, illustrated with source code, can be found in the next articles:

* [Working with WordProcessing documents]({{< ref "editor/java/developer-guide/edit-document/edit-word" >}})
* [Working with Spreadsheets]({{< ref "editor/java/developer-guide/edit-document/edit-excel" >}})
* [Working with DSV]({{< ref "editor/java/developer-guide/edit-document/how-to-edit-csv-file.md" >}})
* [Working with Presentations]({{< ref "editor/java/developer-guide/edit-document/edit-powerpoint" >}})
* [Working with text documents]({{< ref "editor/java/developer-guide/edit-document/edit-txt.md" >}})
