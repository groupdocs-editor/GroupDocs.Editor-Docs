---

id: edit-document
url: editor/nodejs-java/edit-document
title: Edit Document
weight: 6
description: "Follow this guide to learn how to edit text documents, spreadsheets, and presentations using GroupDocs.Editor for Node.js via Java API features."
keywords: Edit document, edit presentation, edit spreadsheet, GroupDocs.Editor
productName: GroupDocs.Editor for Node.js via Java
hideChildren: False
structuredData:
    showOrganization: True
    application:    
        name: Edit document using GroupDocs.Editor
        description: Edit document using GroupDocs.Editor in JavaScript
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to edit document using GroupDocs.Editor in JavaScript
        description: Learn how to edit documents using GroupDocs.Editor in JavaScript step by step
        steps:
        - name: Create appropriate EditOptions instance
          text: Each format family has its own implementation of EditOptions. You need to create an instance of the EditOptions class corresponding to the format family of the input document.
        - name: Adjust EditOptions
          text: Adjust the EditOptions to meet your needs—select pagination mode (for WordProcessing documents), desired worksheet (Spreadsheet), slide (Presentation), or separator (Delimiter-separated values), etc.
        - name: Invoke Editor.edit method
          text: After the document is successfully loaded into the Editor class instance and appropriate EditOptions are ready, call the Editor.edit method with specified options and obtain an instance of the generated EditableDocument.
        - name: Obtain HTML markup and resources from EditableDocument
          text: When the EditableDocument is generated, use its methods and properties to obtain HTML markup and all related HTML resources (stylesheets, fonts, images, audio) to send and use them in the WYSIWYG HTML editor.
---

> This article describes how to open a previously loaded document for editing, which options should be applied, and how to send document content to a WYSIWYG HTML editor or any other editing application.

When a document is loaded into an instance of the `Editor` class, it is possible to open it for editing. In terms of **GroupDocs.Editor**, opening a document for editing involves creating an instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.EditableDocument) class by calling the `Editor.edit()` method. There are two overloads of the `edit()` method. The first one accepts a single parameter—an instance of the `EditOptions` class.

Each format family has its own implementation of the `EditOptions` class. They are listed in the table below.

| Format Family                      | Example Formats                                         | Edit Options Class                                                     |
|------------------------------------|---------------------------------------------------------|------------------------------------------------------------------------|
| **WordProcessing**                 | DOC, DOCX, DOCM, DOT, ODT                               | [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.WordProcessingEditOptions) |
| **Spreadsheet**                    | XLS, XLSX, XLSM, XLSB                                   | [`SpreadsheetEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.SpreadsheetEditOptions)      |
| **Delimiter-Separated Values (DSV)** | CSV, TSV                                               | [`DelimitedTextEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.DelimitedTextEditOptions)    |
| **Presentation**                   | PPT, PPTX, PPS, POT                                     | [`PresentationEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.PresentationEditOptions)     |
| **Plain Text Documents**           | TXT                                                     | [`TextEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.TextEditOptions)                    |
| **XML**                            | Any XML-based format like CSPROJ, SVG, etc.             | [`XmlEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.XmlEditOptions)                      |
| **eBook**                          | MOBI, AZW3, EPUB                                        | [`EbookEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.EbookEditOptions)                  |
| **Email**                          | EML, EMLX, TNEF, MSG, HTML, MHTML, ICS, VCF, PST, MBOX, OFT | [`EmailEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.EmailEditOptions)                  |

The second overload is parameterless—it chooses the most appropriate default edit options based on the input document format.

The `EditableDocument` instance holds a version of the input document converted to an internal intermediate format according to the edit options. When it is ready, it can emit HTML, CSS, and other appropriate content that can be passed directly to the WYSIWYG editor. This is demonstrated below.

```javascript
// Import the GroupDocs.Editor module
const groupdocsEditor = require('groupdocs-editor');

// Path to the input document
const inputFilePath = "C:\\input_path\\document.docx";

// Load the document into the Editor
const editor = new groupdocsEditor.Editor(inputFilePath, new groupdocsEditor.WordProcessingLoadOptions());

// Edit the document with default edit options
const openedDocument = editor.edit(); // With default edit options

// Create and adjust custom edit options
const editOptions = new groupdocsEditor.WordProcessingEditOptions();
editOptions.setEnableLanguageInformation(true);
editOptions.setEnablePagination(true);

// Edit the document with custom edit options
const anotherOpenedDocument = editor.edit(editOptions);
```

Multiple `EditableDocument` instances can be generated from a single `Editor` instance with different edit options. For example, for a WordProcessing document, you can call the `edit()` method first with pagination disabled, and then with pagination enabled. In other words, you can generate several different `EditableDocument` representations of the single original document. Another example is generating multiple `EditableDocument` instances for a single input Spreadsheet document, where each instance represents a different worksheet of the Spreadsheet document. This is shown below.

```javascript
// Import the GroupDocs.Editor module
const groupdocsEditor = require('groupdocs-editor');

// Path to the input spreadsheet
const inputXlsxPath = "C://input/spreadsheet.xlsx";

// Load the spreadsheet into the Editor
const editor = new groupdocsEditor.Editor(inputXlsxPath, new groupdocsEditor.SpreadsheetLoadOptions());

// Create edit options for the first worksheet
const editOptions1 = new groupdocsEditor.SpreadsheetEditOptions();
editOptions1.setWorksheetIndex(0); // Index is 0-based, so this is the 1st worksheet
editOptions1.setExcludeHiddenWorksheets(true);

// Create edit options for the second worksheet
const editOptions2 = new groupdocsEditor.SpreadsheetEditOptions();
editOptions2.setWorksheetIndex(1); // Index is 0-based, so this is the 2nd worksheet
editOptions2.setExcludeHiddenWorksheets(false);

// Edit the first worksheet
const firstWorksheet = editor.edit(editOptions1);

// Edit the second worksheet
const secondWorksheet = editor.edit(editOptions2);
```

When the `EditableDocument` instance is ready, it can emit HTML markup, CSS markup, and other resources in different forms for passing them to the client-side WYSIWYG HTML editor or any other application that can edit HTML documents. This is briefly shown in the example below.

```javascript
// Edit the document
const document = editor.edit();

// Get the body content of the HTML
const bodyContent = document.getBodyContent();

// Get only the images from the document resources
const onlyImages = document.getImages();

// Get all resources (images, stylesheets, fonts, etc.)
const allResourcesTogether = document.getAllResources();
```

For more information about obtaining HTML markup and resources from `EditableDocument`, please visit the following articles:

- [Get HTML Markup in Different Forms](/editor/nodejs-java/developer-guide/editabledocument/get-html-markup-in-different-forms)
- [Working with Resources](/editor/nodejs-java/developer-guide/editabledocument/working-with-resources)
- [Save HTML to Folder](/editor/nodejs-java/developer-guide/editabledocument/save-html-to-folder)

