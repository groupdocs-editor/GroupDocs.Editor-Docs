---
id: load-document
url: editor/net/load-document
title: Load document
weight: 4
description: "Following this guide you will learn how to load document from local disk or file stream for editing with GroupDocs.Editor for .NET API."
keywords: Load document with GroupDocs.Editor, Load and edit document, edit document, edit spreadsheet, edit presentation
productName: GroupDocs.Editor for .NET
hideChildren: False
structuredData:
    showOrganization: True
    application:    
        name: Load document into GroupDocs.Editor
        description: Load document into GroupDocs.Editor for subsequent usage on C# language
        productCode: editor
        productPlatform: net 
    showVideo: True
    howTo:
        name: How to load document to the GroupDocs.Editor in C#
        description: Learn how to load document to the GroupDocs.Editor in C# step by step
        steps:
        - name: Prepare a LoadOptions instance if needed 
          text: If input document is password-protected or requires some adjusting using load, or it is required to specify its format family explicitly, create an appropriate inheritor of the ILoadOptions interface
        - name: Invoke the appropriate Editor class constructor
          text: Pass a filepath as a string or a byte stream through delegate into the most appropriate overload of the constructor of GroupDocs.Editor.Editor class
        - name: Make all necessary edits and savings
          text: After document is successfully loaded, you can get its metainfo, generate its editable version, and finally save it to the resultant file
---
> This article describes how to load input document to the [**GroupDocs.Editor**](https://products.groupdocs.com/editor/net) and how to apply load options.

First of all the input document, which should be accessible as a byte stream or through valid file path, should be loaded into the GroupDocs.Editor by creating an instance of the [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class through one of the constructor overloads.  
If the input document is presented as a stream, it should be loaded through delegate. Source code below shows two ways of loading documents: from path and from stream.

```csharp
//through path
string inputFilePath = "C:\\input_path\\document.docx"; //path to some document
Editor editor = new Editor(inputFilePath);

//through stream
FileStream inputStream = System.IO.File.OpenRead(inputFilePath);
Editor editor = new Editor(delegate { return inputStream; });
```

When two overloads from example above are used, GroupDocs.Editor automatically detects the format of input document and applies the most appropriate default loading options for the input document.  
However, it is possible and even recommended to specify correct loading options explicitly using constructor overloads, which accept two parameters. Like streams, loading options should be specified through delegates.  
Source code below shows using such options.

```csharp
//through path
string inputFilePath = "C:\\input_path\\document.docx"; //path to some document
WordProcessingLoadOptions wordLoadOptions = new WordProcessingLoadOptions();
Editor editor = new Editor(inputFilePath, delegate { return wordLoadOptions; }); //passing path and load options (via delegate) to the constructor

//through stream
MemoryStream inputStream = new MemoryStream();//obtained from somewhere
SpreadsheetLoadOptions spreadsheetLoadOptions = new SpreadsheetLoadOptions();
Editor editor = new Editor(delegate { return inputStream; }, delegate { return spreadsheetLoadOptions; });
```

Please note that not all document formats have appropriate classes, that represent load options. As for version 22.7, only WordProcessing, Spreadsheet and Presentation family formats, as well as a distinct PDF format, have load options. For other document formats, such as DSV, TXT or XML, there are no load options.

| Format family | Example formats | Load options class |
| --- | --- | --- |
| WordProcessing | DOC, DOCX, DOCM, DOT, ODT | [`WordProcessingLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingloadoptions) |
| Spreadsheet | XLS, XLSX, XLSM, XLSB | [`SpreadsheetLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetloadoptions) |
| Presentation | PPT, PPTX, PPS, POT | [`PresentationLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationloadoptions) |
| Fixed-layout format | PDF | [`PdfLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfloadoptions) |

Using load options is the only way for working with password-protected input documents. Any document can be loaded into the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance, even encoded document without the password. However, on the next step — opening for editing, — the exception will be thrown. GroupDocs.Editor handles passwords and encoded documents in the next way:

1. If document is not encoded, password is ignored anyway, whether or not it was specified.
2. If document is password-protected, but password is not specified, the [`PasswordRequiredException`](https://reference.groupdocs.com/editor/net/groupdocs.editor/passwordrequiredexception) will be thrown later during editing.
3. If document is password-protected, and password is specified, but is incorrect, the [`IncorrectPasswordException`](https://reference.groupdocs.com/editor/net/groupdocs.editor/incorrectpasswordexception) will be thrown later during editing.

Example below shows specifying password for opening some password-protected WordProcessing document.

```csharp
Stream inputStream = GetDocumentStreamFromSomewhere();
WordProcessingLoadOptions wordLoadOptions = new WordProcessingLoadOptions();
wordLoadOptions.Password = "correct_password";
Editor editor = new Editor(inputFilePath, delegate { return wordLoadOptions; });
```

{{< alert style="info" >}}Same approach is applicable for Spreadsheet, Presentation, and PDF documents too.{{< /alert >}}
