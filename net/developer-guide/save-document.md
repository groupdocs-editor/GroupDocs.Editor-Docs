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

When end-user has finished document editing in the WYSIWYG HTML-editor (this is usually a pure client-side application, written on JavaScript), he submits the editing operation, and HTML markup with stylesheets, images, and maybe other resources are passed to the server-side. In order to generate a document of some output format, these resources should be passed to the [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument).

As it was shown in previous articles, instances of [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) are generated and returned by the [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor).[Edit()](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) method and then are used for emitting HTML markup and resources for passing them *to* the WYSIWYG-editor.  
However [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) also has a second purpose — to obtain edited content *from* WYSIWYG-editor. [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class has no public constructors; instead of them it has three static factories, which obtain HTML document in different forms onm input and according to this generate the `EditableDocument` instance:

1. [`FromFile` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/fromfile) is designed for opening HTML-documents from disk — it obtains path to \*.html file (that contains HTML-markup) and an optional path to corresponding resource folder that contains different resources, like stylesheets, images, font files, audio files and so on.
2. [`FromMarkup` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkup) is designed for opening HTML-documents from memory — it obtains the HTML-markup as a [`System.String`](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0) and an optional list of [IHtmlResource](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources/ihtmlresource) items, like stylesheets, images, fonts, audio resources and so on.
3. [`FromMarkupAndResourceFolder` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkupandresourcefolder) is designed for opening HTML-documents from mixed storages — it obtains the HTML-markup as a [`System.String`](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0), but resources are obtained from a path to corresponding resource folder, which, unlike previous methods, is mandatory (such directory should exist).

More information about creating [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instances from files or markup with resources can be found in corresponding article "[Create EditableDocument from file or markup]({{< ref "editor/net/developer-guide/working-with-editabledocument/create-editabledocument-from-file-or-markup.md" >}})".

When [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) is created, it can be converted to the output document. For doing this, user must use [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor).[Save()](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method, that has two overloads. These overloads differ only with the way how output document is specified: as path, where file should be created, or as a byte stream, into which the document content should be written. All other parameters are same. They are:

1. Instance of [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class, that holds a content of edited document.
2. Output document, that is specified as file path ([`System.String`](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0)) or byte stream ([`System.IO.Stream`](https://docs.microsoft.com/en-us/dotnet/api/system.io.stream?view=net-6.0)).
3. Mandatory save options, that are represented by one of inheritors of the [`ISaveOptions` interface](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/isaveoptions).

Like with load and edit options, every family format has its own class, that implements [ISaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/isaveoptions) interface. These classes are listed below.

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
| e-Books | ePub | [EpubSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/epubsaveoptions) | [EBookFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats) |
| e-Books | AZW3 | [Azw3SaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/azw3saveoptions) | [EBookFormats](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats) |

Source code below shows creating an instance of [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class and consequent saving a two versions of the document: one to the file and second — to the stream.

```csharp
string inputHtmlPath = "C:\input\document.html";
EditableDocument document = EditableDocument.FromFile(inputHtmlPath, null);

Editor editor = new Editor("C:\path\original.docx");

//save 1st version to file through path
string outputPath = "C:\\output_path\\document.rtf";
WordProcessingSaveOptions saveOptions1 = new WordProcessingSaveOptions(WordProcessingFormats.Rtf);
editor.Save(document, outputPath, saveOptions1);

//save 2nd version to stream
MemoryStream outputStream = new MemoryStream();
WordProcessingSaveOptions saveOptions2 = new WordProcessingSaveOptions(WordProcessingFormats.Docm);
editor.Save(document, outputStream, saveOptions2);
```

As you can see from example above, it is possible to create multiple output documents from a single [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) with different save options and different formats. And these output formats should not be strictly same as format of input document.

Even more: in some cases format family can also be different. For example, original document can be some of WordProcessing formats, while output document can be TXT or PDF. Or original document can be a PDF, while output — DOCX. Same transitions are allowed between Spreadsheets and DSV (two-ways). But, of course, they are not allowed, where formats are theoretically incompatible in their essence, like WordProcessing and Spreadsheet.
