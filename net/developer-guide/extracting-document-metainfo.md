---
id: extracting-document-metainfo
url: editor/net/extracting-document-metainfo
title: Extracting document metainfo
weight: 10
description: "Following this guide you will learn how to obtain basic document metadata like pages count, size, file type before editing it with GroupDocs.Editor for .NET API."
keywords: Extract document metadata, Get document info, obtain basic document metadata
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: Extract document metainfo in the GroupDocs.Editor
        description: Extract information about document format and other properties using the GroupDocs.Editor in C# language
        productCode: editor
        productPlatform: net 
    showVideo: True
    howTo:
        name: How to extract document metainfo using the GroupDocs.Editor in C#
        description: Learn how to extract information about document format and other properties using the GroupDocs.Editor in C# step by step
        steps:
        - name: Load desired document to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired document into it.
        - name: Invoke the Editor.GetDocumentInfo method
          text: Once the document is loaded, call the Editor.GetDocumentInfo method and specify an optional password for the document into it, if document is password-protected.		  
        - name: Examine obtained document info
          text: The Editor.GetDocumentInfo method returns the inheritor of the IDocumentInfo interface, which has the type, that is the most appropriate for the document format. For example, for the input document in WordProcessing format the GetDocumentInfo will return an instance of a WordProcessingDocumentInfo class with information about page count, protection, exact format, and some other data.
---
> This demonstration shows and explains usage of the [GetDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo) method, that extracts meta info from the document.

## Introduction

In some situations it is required to grab meta info from the document before actually editing it. For example, user wants to edit last tab of the multi-tabbed spreadsheet, but he doesn't know, how many tabs the document contains. Ir it is unclear for the user, is the document password-protected or not. For such situations [**GroupDocs.Editor**](https://products.groupdocs.com/editor/net) provides a [GetDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo) method, that returns detailed meta info (metadata) about the specified document.

## Using the method

In order to grab the meta info from the document, it should firstly be loaded into the `Editor` class. Then [GetDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo)() should be called. This method obtains one parameter — password as a string. If document is encoded and user knows the password, he can specify it here. For other cases the null or empty string can be passed. Code example below demonstrates the usage:

```csharp
Editor editor = new Editor("C://input/document.docx");
IDocumentInfo infoDocxWithoutPassword = editorDocx.GetDocumentInfo(null);
IDocumentInfo infoDocxWithPassword = editorDocx.GetDocumentInfo("password"); 
```

There can be several scenarios here regarding whether document is encoded or not, and did user specified a password:

1. If password is specified, but document is not password-protected, or the document format doesn't support encoding at all, the password will be ignored.
2. If document is password-protected, but password is not specified, the [PasswordRequiredException](https://apireference.groupdocs.com/net/editor/groupdocs.editor/passwordrequiredexception) will be thrown while calling [GetDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo)().
3. If document is password-protected,and password is specified, but is incorrect, the [IncorrectPasswordException](https://apireference.groupdocs.com/net/editor/groupdocs.editor/incorrectpasswordexception) will be thrown while calling [GetDocumentInfo()](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo).

## Explaining resulting type

[GetDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo)() method returns a [IDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo). This is interface, that stores meta info about one particular document and contains the next properties:

1. [PageCount](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo/properties/pagecount). This is a positive number, that returns page count for WordProcessing, PDF and XPS documents, tabs (worksheets) count for Spreadsheets, slides count for Presentations and a number `1` for pageless documents like XML or TXT.
2. [Size](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo/properties/size). Document size in bytes.
3. [IsEncrypted](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo/properties/isencrypted). A boolean flag that indicates whether document is encrypted with the password or not. If document is of type, that doesn't support encryption at all, like CSV or XML, this property always returns `false`.
4. [Format](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo/properties/format). Returns info about the format itself.

There are eight inheritors of the [IDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo) interface, all are structs:

1. [WordProcessingDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/wordprocessingdocumentinfo) — common for all WordProcessing family formats.
2. [SpreadsheetDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/spreadsheetdocumentinfo) — common for all Spreadsheet family formats.
3. [PresentationDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/presentationdocumentinfo) — common for all Presentation family formats.
4. [TextualDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/textualdocumentinfo) — common for all textual types, including all DSV (like CSV and TSV), XML, HTML, and plain text.
5. [FixedLayoutDocumentInfo](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/fixedlayoutdocumentinfo) - common for all documents with a fixed-layout format, this includes only PDF and XPS.
6. [EmailDocumentInfo](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/emaildocumentinfo) - common for all Email family formats, like EML, MSG, VCF, PST, MBOX and others.
7. [EbookDocumentInfo](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo) - common for all eBook family formats like MOBI and ePub.
8. [MarkdownDocumentInfo](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/markdowndocumentinfo) - special struct, that is dedicated especially for the Markdown (MD) textual format.


One important thing to note: if [GetDocumentInfo()](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo) returns `NULL` value instead of some of [IDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo) inheritors, this means that specified document is not supported by the GroupDocs.Editor and thus cannot be opened for editing or saved.

## Explaining document format

[IDocumentInfo](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo) interface contains a [`Format` property](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo/properties/format) of [IDocumentFormat](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/idocumentformat) type. [IDocumentFormat](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/idocumentformat) is an interface, that is common for all format descriptors. It is designed for indicating one particular document format and stores format name, extension, MIME-code, and has equality operators.

Each inheritor of [IDocumentFormat](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/idocumentformat) interface delivers three properties, all of a [System.String](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0) type: 
1. [Name](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/idocumentformat/properties/name), that provides name of the format.
2. [Extension](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/idocumentformat/properties/extension), that provides a format extension.
3. [Mime](https://apireference.groupdocs.com/editor/net/groupdocs.editor.formats/idocumentformat/properties/mime), that provides a MIME-code for a particular format

[`IDocumentFormat` interface](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/idocumentformat) has seven inheritors, all of them are structs:
1. [WordProcessingFormats](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/wordprocessingformats) — holds all formats from WordProcessing family.
2. [SpreadsheetFormats](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/spreadsheetformats) — holds all formats from Spreadsheet family.
3. [PresentationFormats](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/spreadsheetformats) — holds all formats from Presentation family.
4. [TextualFormats](https://apireference.groupdocs.com/net/editor/groupdocs.editor.formats/textualformats) — holds all formats with text-based nature.
5. [FixedLayoutFormats](https://apireference.groupdocs.com/editor/net/groupdocs.editor.formats/fixedlayoutformats) - holds all formats from the fixed-layout format family. This includes only PDF and XPS.
6. [EBookFormats](https://apireference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats) - holds all eBool (Electronic book) formats like Mobi and ePub.
7. [EmailFormats](https://apireference.groupdocs.com/editor/net/groupdocs.editor.formats/emailformats) - holds all email (electronic mail) formats like EML and MSG.

