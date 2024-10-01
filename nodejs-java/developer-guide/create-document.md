---

id: create-document
url: editor/nodejs-java/create-document
title: Create New Document by Format
weight: 8
description: "This article demonstrates how to create new documents, spreadsheets, and presentations with GroupDocs.Editor for Node.js via Java API."
keywords: Create new document, edit document, GroupDocs.Editor
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
structuredData:
    showOrganization: True
    application:
        name: Creating New Documents by Format
        description: Create new documents using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to create new documents using GroupDocs.Editor in Node.js via Java
        description: Learn how to create new documents using GroupDocs.Editor in Node.js via Java step by step
        steps:
        - name: Prepare a callback function to save the new document stream if needed
          text: A callback function allows you to save the new document stream
        - name: Invoke the appropriate Editor class constructor
          text: Pass a callback function delegate into the appropriate overload of the GroupDocs.Editor.Editor class constructor
        - name: Make all necessary edits and savings
          text: After the document is successfully loaded, you can get its meta-information, generate its editable version, and finally save it to the resultant file
---

**GroupDocs.Editor for Node.js via Java** empowers developers to effortlessly create and edit documents in various formats, including WordProcessing, Spreadsheet, Presentation, Ebook, and Email. This article provides a comprehensive guide on utilizing GroupDocs.Editor to create a new document in a specified format, process it, and ultimately save it to the resultant document.

## Getting Started with GroupDocs.Editor

To initiate the document creation process, the [**GroupDocs.Editor**](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/) constructor is employed. This constructor enables the creation of documents in different formats. Let's explore the key components of the code and how it handles various [Document Formats](https://docs.groupdocs.com/editor/nodejsjava/supported-document-formats/).

### 1. WordProcessing Document

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Create a new WordProcessing document
const editor = new groupdocsEditor.Editor(groupdocsEditor.WordProcessingFormats.Docx);

// Edit the WordProcessing document with default options
const defaultWordProcessingDoc = editor.edit();

// Edit the WordProcessing document with specified options and some defined settings
const wordProcessingEditOptions = new groupdocsEditor.WordProcessingEditOptions();
wordProcessingEditOptions.setEnablePagination(false);  // Disable pagination for the document
wordProcessingEditOptions.setEnableLanguageInformation(true);  // Enable language information for the document
wordProcessingEditOptions.setFontExtraction(groupdocsEditor.FontExtractionOptions.ExtractAllEmbedded);  // Extract all embedded fonts

const editableWordProcessingDocument = editor.edit(wordProcessingEditOptions);

// Save the document to a file
const outputFilePath = 'output.docx';
const saveOptions = new groupdocsEditor.WordProcessingSaveOptions(groupdocsEditor.WordProcessingFormats.Docx);

editor.save(editableWordProcessingDocument, outputFilePath, saveOptions);

// Dispose of resources
editableWordProcessingDocument.dispose();
editor.dispose();
```

### 2. Spreadsheet Document

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Create a new Spreadsheet document
const editor = new groupdocsEditor.Editor(groupdocsEditor.SpreadsheetFormats.Xlsx);

// Edit the Spreadsheet document with default options
const defaultEditableSpreadsheetDocument = editor.edit();

// Edit the Spreadsheet document with specified options and some defined settings
const spreadsheetEditOptions = new groupdocsEditor.SpreadsheetEditOptions();
spreadsheetEditOptions.setWorksheetIndex(0);
spreadsheetEditOptions.setExcludeHiddenWorksheets(true);

const editableSpreadsheetDocument = editor.edit(spreadsheetEditOptions);

// Save the document to a file
const outputFilePath = 'output.xlsx';
const saveOptions = new groupdocsEditor.SpreadsheetSaveOptions(groupdocsEditor.SpreadsheetFormats.Xlsx);

editor.save(editableSpreadsheetDocument, outputFilePath, saveOptions);

// Dispose of resources
editableSpreadsheetDocument.dispose();
editor.dispose();
```

### 3. Presentation Document

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Create a new Presentation document
const editor = new groupdocsEditor.Editor(groupdocsEditor.PresentationFormats.Pptx);

// Edit the Presentation document with default options
const defaultEditablePresentationDocument = editor.edit();

// Edit the Presentation document with specified options and some defined settings
const presentationEditOptions = new groupdocsEditor.PresentationEditOptions();
presentationEditOptions.setShowHiddenSlides(false);
presentationEditOptions.setSlideNumber(0);

const editablePresentationDocument = editor.edit(presentationEditOptions);

// Save the document to a file
const outputFilePath = 'output.pptx';
const saveOptions = new groupdocsEditor.PresentationSaveOptions(groupdocsEditor.PresentationFormats.Pptx);

editor.save(editablePresentationDocument, outputFilePath, saveOptions);

// Dispose of resources
editablePresentationDocument.dispose();
editor.dispose();
```

### 4. Email Document

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Create a new Email document
const editor = new groupdocsEditor.Editor(groupdocsEditor.EmailFormats.Eml);

// Edit the Email document with default options
const defaultEditableEmailDocument = editor.edit();

// Edit the Email document with specified options and some defined settings
const emailEditOptions = new groupdocsEditor.EmailEditOptions();
emailEditOptions.setMailMessageOutput(groupdocsEditor.MailMessageOutput.All);

const editableEmailDocument = editor.edit(emailEditOptions);

// Save the document to a file
const outputFilePath = 'output.eml';
const saveOptions = new groupdocsEditor.EmailSaveOptions(groupdocsEditor.EmailFormats.Eml);

editor.save(editableEmailDocument, outputFilePath, saveOptions);

// Dispose of resources
editableEmailDocument.dispose();
editor.dispose();
```

## Conclusion

This article has provided a step-by-step guide on leveraging GroupDocs.Editor for Node.js via Java to create new documents in various formats. By utilizing the constructor and specified edit options, developers can seamlessly integrate document creation and editing capabilities into their applications. Whether it's a WordProcessing, Spreadsheet, Presentation, Ebook, or Email document, GroupDocs.Editor simplifies the process, offering flexibility and efficiency in document manipulation. For more in-depth information, refer to the official [GroupDocs.Editor for Node.js via Java documentation](https://docs.groupdocs.com/editor/nodejsjava/).

---

**Note:** Make sure to replace `'output.docx'`, `'output.xlsx'`, `'output.pptx'`, and `'output.eml'` with the actual paths where you want to save your documents.

---

By following this guide, you can effectively create new documents in various formats using GroupDocs.Editor for Node.js via Java. This allows you to programmatically generate and manipulate documents within your Node.js applications.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java) repository on GitHub.

---