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
          text: Pass a filepath as a string or a byte stream into the most appropriate overload of the constructor of GroupDocs.Editor.Editor class
        - name: Make all necessary edits and savings
          text: After document is successfully loaded, you can get its metainfo, generate its editable version, and finally save it to the resultant file
---
> The text provides a comprehensive guide on how to load documents using the GroupDocs.Editor for .NET API. Below is a revised version that improves readability, consistency, and clarity.

---

### Load Document

This guide explains how to load a document from a local disk or file stream for editing using the GroupDocs.Editor for .NET API.

#### Introduction

In this article, you will learn how to load an input document into [**GroupDocs.Editor**](https://products.groupdocs.com/editor/net) and apply load options.

### Loading Documents

To load an input document, which should be accessible either as a byte stream or through a valid file path, you can create an instance of the [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class using one of its constructor overloads. Below are examples of loading documents from a file path and from a stream.

```csharp
// Load document from file path
string inputFilePath = "C:\\input_path\\document.docx"; // Path to the document
Editor editor = new Editor(inputFilePath);

// Load document from stream
FileStream inputStream = System.IO.File.OpenRead(inputFilePath);
Editor editor = new Editor(inputStream);
```

When using the constructor overloads shown above, GroupDocs.Editor automatically detects the format of the input document and applies the most suitable default loading options. However, it is recommended to specify the correct loading options explicitly by using constructor overloads that accept two parameters. Here is how you can do this:

```csharp
// Load document from file path with load options
string inputFilePath = "C:\\input_path\\document.docx"; // Path to the document
WordProcessingLoadOptions wordLoadOptions = new WordProcessingLoadOptions();
Editor editor = new Editor(inputFilePath, wordLoadOptions); // Passing path and load options to the constructor

// Load document from stream with load options
MemoryStream inputStream = new MemoryStream(); // Stream obtained from somewhere
SpreadsheetLoadOptions spreadsheetLoadOptions = new SpreadsheetLoadOptions();
Editor editor = new Editor(inputStream, spreadsheetLoadOptions);
```

### Load Options

Please note that not all document formats have associated classes for load options. As of version 22.7, only WordProcessing, Spreadsheet, Presentation formats, and a distinct PDF format have specific load options classes. Other formats, such as DSV, TXT, or XML, do not have load options.

| Format Family      | Example Formats                      | Load Options Class                                                                                      |
|--------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------|
| WordProcessing     | DOC, DOCX, DOCM, DOT, ODT            | [`WordProcessingLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingloadoptions) |
| Spreadsheet        | XLS, XLSX, XLSM, XLSB                | [`SpreadsheetLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetloadoptions)         |
| Presentation       | PPT, PPTX, PPS, POT                  | [`PresentationLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationloadoptions)       |
| Fixed-layout format| PDF                                  | [`PdfLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfloadoptions)                         |

### Handling Password-Protected Documents

Using load options is essential when working with password-protected documents. Any document can be loaded into the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance, even if it is password-protected. However, if the password is not handled correctly, an exception will be thrown during the editing process. Here’s how GroupDocs.Editor handles passwords:

1. If the document is not password-protected, any specified password will be ignored.
2. If the document is password-protected but no password is specified, a [`PasswordRequiredException`](https://reference.groupdocs.com/editor/net/groupdocs.editor/passwordrequiredexception) will be thrown during editing.
3. If the document is password-protected and an incorrect password is provided, an [`IncorrectPasswordException`](https://reference.groupdocs.com/editor/net/groupdocs.editor/incorrectpasswordexception) will be thrown during editing.

The example below demonstrates how to specify a password for opening a password-protected WordProcessing document:

```csharp
Stream inputStream = GetDocumentStreamFromSomewhere();
WordProcessingLoadOptions wordLoadOptions = new WordProcessingLoadOptions();
wordLoadOptions.Password = "correct_password";
Editor editor = new Editor(inputFilePath, wordLoadOptions);
```

{{< alert style="info" >}}The same approach applies to Spreadsheet, Presentation, and PDF documents as well.{{< /alert >}}

---

This revision enhances clarity, improves structure, and ensures that the instructions are easy to follow.