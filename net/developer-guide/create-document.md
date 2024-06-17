---
id: create-document
url: editor/net/create-document
title: Create new document by format
weight: 8
description: "This article demonstrates how to Create new document documents, spreadsheets and presentations with GroupDocs.Editor for .NET API."
keywords: Create new document, edit document, GroupDocs.Editor
productName: GroupDocs.Editor for .NET
hideChildren: False
structuredData:
    showOrganization: True
    application:    
        name: Creating New Documents by Format
        description: Create new document using the GroupDocs.Editor in C# language
        productCode: editor
        productPlatform: net 
    showVideo: True
    howTo:
        name: How to create new document using the GroupDocs.Editor in C#
        description: Learn how to create new document using the GroupDocs.Editor in C# step by step
        steps:
        - name: Prepare a Callback function to save the new document stream if needed 
          text: Callback function allow to save the new document stream
        - name: Invoke the appropriate Editor class constructor
          text: Pass a Callback function delegate into the most appropriate overload of the constructor of GroupDocs.Editor.Editor class
        - name: Make all necessary edits and savings
          text: After document is successfully loaded, you can get its metainfo, generate its editable version, and finally save it to the resultant file
---


GroupDocs.Editor for .NET empowers developers to effortlessly create and edit documents in various formats, including WordProcessing, Spreadsheet, Presentation, Ebook, and Email. This article provides a comprehensive guide on utilizing GroupDocs.Editor to obtain a new document in a specified format, extracting content from a client, processing it, and ultimately saving it to the resultant document.

## Getting Started with GroupDocs.Editor

To initiate the document creation process, the [**GroupDocs.Editor**](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/#constructors) constructor is employed. This constructor enables the creation of documents in different formats. Let's explore the key components of the code and how it handles various  [Document Formats](https://docs.groupdocs.com/editor/net/supported-document-formats/).

### 1. WordProcessing Document:

```csharp
Stream memoryStream = new MemoryStream();

// Create a new WordProcessing document and save it using a callback Action<Stream>.
using (Editor editor = new Editor(WordProcessingFormats.Docx))
{
    // Edit the WordProcessing document with default options.
    EditableDocument defaultWordProcessingDoc = editor.Edit();

    // Edit the WordProcessing document with specified options and some defined settings.
    WordProcessingEditOptions wordProcessingEditOptions = new WordProcessingEditOptions();
    wordProcessingEditOptions.EnablePagination = false;  // Disable pagination for the document.
    wordProcessingEditOptions.EnableLanguageInformation = true;  // Enable language information for the document.
    wordProcessingEditOptions.FontExtraction = FontExtractionOptions.ExtractAllEmbedded;  // Extract all embedded fonts.

    EditableDocument editableWordProcessingDocument = editor.Edit(wordProcessingEditOptions);
    editor.Save(memoryStream);
}
```

### 2. Spreadsheet Document:

```csharp
Stream memoryStream = new MemoryStream();

// Create a new Spreadsheet document and save it.
using (Editor editor = new Editor(SpreadsheetFormats.Xlsx))
{
    // Edit the Spreadsheet document with default options.
    EditableDocument defaultEditableSpreadsheetDocument = editor.Edit();

    // Edit the Spreadsheet document with specified options and some defined settings.
    SpreadsheetEditOptions spreadsheetEditOptions = new SpreadsheetEditOptions();
    spreadsheetEditOptions.WorksheetIndex = 0;
    spreadsheetEditOptions.ExcludeHiddenWorksheets = true;

    EditableDocument editableSpreadsheetDocument = editor.Edit(spreadsheetEditOptions);
    editor.Save(memoryStream);
}
```

### 3. Presentation Document:

```csharp
Stream memoryStream = new MemoryStream();

// Create a new Presentation document and save it.
using (Editor editor = new Editor(PresentationFormats.Pptx))
{
    // Edit the Presentation document with default options.
    EditableDocument defaultEditablePresentationDocument = editor.Edit();

    // Edit the Presentation document with specified options and some defined settings.
    PresentationEditOptions presentationEditOptions = new PresentationEditOptions();
    presentationEditOptions.ShowHiddenSlides = false;
    presentationEditOptions.SlideNumber = 0;

    EditableDocument editablePresentationDocument = editor.Edit(presentationEditOptions);
    editor.Save(memoryStream);
}
```

### 4. Ebook Document:

```csharp
Stream memoryStream = new MemoryStream();

// Create a new Ebook document and save it.
using (Editor editor = new Editor(EBookFormats.Epub))
{
    // Edit the Ebook document with default options.
    EditableDocument defaultEditableEbookDocument = editor.Edit();

    // Edit the Ebook document with specified options and some defined settings.
    EbookEditOptions ebookEditOptions = new EbookEditOptions();
    ebookEditOptions.EnablePagination = false;
    ebookEditOptions.EnableLanguageInformation = true;

    EditableDocument editableEbookDocument = editor.Edit(ebookEditOptions);
    editor.Save(memoryStream);
}
```

### 5. Email Document:

```csharp
Stream memoryStream = new MemoryStream();

// Create a new Email document and save it.
using (Editor editor = new Editor(EmailFormats.Eml))
{
    // Edit the Email document with default options.
    EditableDocument defaultEditableEmailDocument = editor.Edit();

    // Edit the Email document with specified options and some defined settings.
    EmailEditOptions emailEditOptions = new EmailEditOptions();
    emailEditOptions.MailMessageOutput = MailMessageOutput.All;

    EditableDocument editableEmailDocument = editor.Edit(emailEditOptions);
    editor.Save(memoryStream);
}
```

## Conclusion

This article has provided a step-by-step guide on leveraging GroupDocs.Editor for .NET to create new documents in various formats. By utilizing the constructor and specified edit options, developers can seamlessly integrate document creation and editing capabilities into their applications. Whether it's a WordProcessing, Spreadsheet, Presentation, Ebook, or Email document, GroupDocs.Editor simplifies the process, offering flexibility and efficiency in document manipulation. For more in-depth information, refer to the official [GroupDocs.Editor documentation](https://docs.groupdocs.com/editor/net/).
