---
id: save-document
url: editor/net/save-document
title: Save document
weight: 8
description: "This article demonstrates how to save edited text documents, spreadsheets and presentations with GroupDocs.Editor for .NET API."
keywords: Save edited document, edit document, GroupDocs.Editor
productName: GroupDocs.Editor for .NET
hideChildren: False
structuredData:
    showOrganization: True
    application:    
        name: Save document in the GroupDocs.Editor
        description: Save document using the GroupDocs.Editor in C# language
        productCode: editor
        productPlatform: net 
    showVideo: True
    howTo:
        name: How to save document using the GroupDocs.Editor in C#
        description: Learn how to save document using the GroupDocs.Editor in C# step by step
        steps:
        - name: Submit edited HTML-document to the server-side
          text: After document was edited on the client-side with WYSIWYG HTML-editor, its edited HTML-markup and resources must be submitted to the server-side, because the GroupDocs.Editor is a server-based middleware.
        - name: Create EditableDocument instance from edited content
          text: Once the content of edited document is transferred to the server-side, you should create an instance of EditableDocument class from this content by using the most appropriate static method from this type.
        - name: Create and adjust the appropriate SaveOptions
          text: Depending on what format the input document had and which output format you want to obtain, you need to create an appropriate inheritor of the ISaveOptions interface and and adjust it by selecting the desired format, protection, and other options.
        - name: Invoke Editor.Save method
          text: Finally, when edited document is stored within the EditableDocument instance and SaveOptions are prepared, you should invoke the desired overload of the Editor.Save method (first one saves the document to the file, while second — to the byte stream).
---
> This article describes how to obtain edited document content from client, process it and save to the resultant document of some specified format.

In a nutshell, the core process of document editing, where a user types some text across the pages of the document, inserts images, makes some edits, removes words or paragraphs, or moves some document parts from one location to another, is performed in some 3rd party software with GUI outside of the GroupDocs.Editor. This software may be, for example, but not limiting to:

- a web-based WYSIWYG HTML-editor, that is usually a pure client-side application, written on JavaScript and running in the browser. This may be, for example, a TinyMCE or CKEditor;
- desktop (like WinForms or WPF) application;
- mobile application, running on Android or iOS.

The core requirement for this application is to be able to open, view and edit HTML content. The GroupDocs.Editor on its side accepts various document formats (WordProcessing, Spreadsheet, Presentation, and many more) and prepares them for editing in external applications by converting to the HTML markup and placing it in the [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) container. So when calling the [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor).[Edit()](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) method, the [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) with the **original content** inside it is generated.

Then the user obtains this **original content** from the [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument), pushes it to the external HTML-editor, makes edits, and when these edits are done, a user has the **modified content**. In the GroupDocs.Editor terminology saving a document means obtaining the **modified content** and generating the document in output format (WordProcessing, Spreadsheet, Presentation, and many more) from it. And this article explains how to do this.

In short, saving a document implies the next three steps:

1. Obtain the **modified content** from somewhere (HTML-editor or any other storage or method) and create an instance of [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) from it. [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) actually serves as an object-oriented wrapper of the document content.
2. Select a desired output format, into which the **modified content** should be saved, and optional parameters.
3. Save the **modified content** to the document of previously chosen format by specified file path or into specified stream using the [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor).[Save()](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method.

These three steps are explained in detail and with code samples below.

## Obtaining the modified content

Different content editors provide the content in different forms. Some may return HTML markup as a [`System.String`](https://learn.microsoft.com/en-us/dotnet/api/system.string), while the external resources like stylesheet(s) and/or image(s) are located in some specific folder as files. Others may return both main content and resources as a collection of byte streams. Some HTML-editors may generate a single string, which already contains all HTML markup with resources baked into it using base64 encoding. There may be unlimited possibilities to do that, and thus the [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class has different static factories, which obtain **modified content** of the HTML document in different forms on input and according to this generate the [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance:

1. [`FromFile` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/fromfile) is designed for opening HTML-documents from disk — it obtains path to \*.html file (that contains HTML-markup) and an optional path to corresponding resource folder that contains different resources, like stylesheets, images, font files, audio files and so on.
2. [`FromMarkup` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkup) is designed for opening HTML-documents from memory — it obtains the HTML-markup as a [`System.String`](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0) and an optional list of [IHtmlResource](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources/ihtmlresource) items, like stylesheets, images, fonts, audio resources and so on.
3. [`FromMarkupAndResourceFolder` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkupandresourcefolder) is designed for opening HTML-documents from mixed storages — it obtains the HTML-markup as a [`System.String`](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0), but resources are obtained from a path to corresponding resource folder, which, unlike previous methods, is mandatory (such directory should exist).

More information about creating [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instances from files or markup with resources can be found in corresponding article "[Create EditableDocument from file or markup]({{< ref "editor/net/developer-guide/editabledocument/create-editabledocument-from-file-or-markup.md" >}})".

## Create and adjust saving options

When the [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance with the **modified content** is created, it's time to save it to the resultant document of some defined format, and the GroupDocs.Editor needs to know this format. Like with load and edit options, every family format has its own class that implements [ISaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/isaveoptions) interface. These classes are listed below.

| Format family | Example formats | Save options class | Format class |
| --- | --- | --- | --- |
| WordProcessing | DOC, DOCX, DOCM, DOT, ODT | [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingsaveoptions) | [WordProcessingFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/wordprocessingformats) |
| Spreadsheet | XLS, XLSX, XLSM, XLSB | [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions) | [SpreadsheetFormat](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/spreadsheetformats) |
| Delimiter-Separated Values (DSV) | CSV, TSV | [DelimitedTextSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/delimitedtextsaveoptions) | [TextualFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/textualformats) |
| Presentation | PPT, PPTX, PPS, POT | [PresentationSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions) | [PresentationFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/presentationformats) |
| Plain Text documents | TXT | [TextSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/textsaveoptions) | [TextualFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/textualformats) |
| Fixed-layout format | PDF | [PdfSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions) | [FixedLayoutFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/fixedlayoutformats) |
| Fixed-layout format | XPS | [XpsSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xpssaveoptions) | [FixedLayoutFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/fixedlayoutformats) |
| Email | EML, EMLX, TNEF, MSG, HTML, MHTML, ICS, VCF, PST, MBOX, OFT | [EmailSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emailsaveoptions) | [EmailFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/emailformats) |
| e-Books | ePub, Mobi, AZW3 | [EbookSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebooksaveoptions/) | [EBookFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats) |

So, let’s say it is necessary to save the **modified content** to the document of DOCX format. DOCS is the part of WordProcessing family. So the instance of the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingsaveoptions) class should be created, and the [WordProcessingFormats.Docx](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/wordprocessingformats/docx/) value should be specified in its constructor. Like this:

```csharp
WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Docx);
```

If it is also necessary to encode the resultant document with the password, it can be done using one line of code:

```csharp
saveOptions.Password = "sone-password";
```

Of course, different formats have different options. For example, there is a `Password` property in [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingsaveoptions), [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions), [PresentationSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions), but there is no anything similar in XpsSaveOptions, [EmailSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emailsaveoptions), [TextSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/textsaveoptions), and [EbookSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebooksaveoptions/), because these formats do not support password protection.

Also need to mention that the format of input document with **original content** and format of the resultant document with **modified content** may be different. For example, the original document can be some WordProcessing formats, while the output document can be TXT or PDF. Or the original document can be a PDF, while output — DOCX. Same transitions are allowed between Spreadsheets and DSV (two-ways). But, of course, they are not allowed, where formats are theoretically incompatible in their essence, like WordProcessing and Spreadsheet.

## Saving modified content to the document

Finally, when the instance of [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class with modified content inside is created, and the format of the resultant document is defined, it is possible to generate this resultant document using the [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor).[Save()](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method. This method has two overloads. These overloads differ only with the way how output document is specified: as path, where file should be created, or as a byte stream, into which the document content should be written. All other parameters are the same.

Here is a signature:
```csharp
Save(EditableDocument inputDocument, string filePath, ISaveOptions saveOptions)
Save(EditableDocument inputDocument, Stream outputDocument, ISaveOptions saveOptions)
```

Here:
- The 1st parameter — `EditableDocument inputDocument` — is a [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance with **modified content** inside, created on the 1st step.
- The 2nd parameter is a [stream](https://learn.microsoft.com/en-us/dotnet/api/system.io.stream), into which the resultant document of defined format should be written, or a file path, where the resultant document of defined format should be stored.
- The 3rd parameter is an instance of some save options, which defines the format of the resultant document and additional adjustments, created on the 2nd step.

## Complete code example

Because the WYSIWYG HTML-editor is not a part of the GroupDocs.Editor, it is hard to provide a lightweight code example with fully functional editing. So in this sample the content will be edited programmatically, using a [`String.Replace`](https://learn.microsoft.com/ru-ru/dotnet/api/system.string.replace) method.

```csharp
// Create Editor class by loading an input document in DOCX format by path
Editor editor = new Editor("input.docx");

// Open document for edit and obtain EditableDocument
EditableDocument original = editor.Edit();

// Get the original content as a string
string originalContent = original.GetEmbeddedHtml();

// Get the modified content by editing original content
string modifiedContent = originalContent.Replace("old", "new");

// Create EditableDocument from modified content
EditableDocument modified = EditableDocument.FromMarkup(modifiedContent, null);

// Create and adjust 2 different saving options
WordProcessingSaveOptions docxSaveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Docx);
PdfSaveOptions pdfSaveOptions = new PdfSaveOptions();

// Save modified content to the 2 documents
editor.Save(modified, "output.docx", docxSaveOptions);
editor.Save(modified, "output.pdf", pdfSaveOptions);

// Dispose all
original.Dispose();
modified.Dispose();
editor.Dispose();
```

In this example 2 different save options are created and two different [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor).[Save()](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) calls are made for saving the modified content to the DOCX and PDF formats.
