---
id: edit-word
url: editor/nodejs-java/edit-word
title: Edit Word Documents in Node.js and Java
linktitle: Edit Word Document
weight: 45
description: "This guide demonstrates how to edit DOC, DOT, DOCX, DOCM, DOTX, ODT, RTF documents with font extraction, different pagination modes, and many other powerful features of GroupDocs.Editor for Node.js and Java."
keywords: Edit DOCX, Edit DOC, Edit RTF, Edit ODT, Edit Word document
productName: GroupDocs.Editor for Node.js and Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit Word document in GroupDocs.Editor
        description: How to edit Word document using GroupDocs.Editor in Node.js and Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to edit content of a Word document using GroupDocs.Editor in Node.js and Java
        description: Learn how to edit content of Word documents in different editing modes using GroupDocs.Editor for Node.js and Java step by step
        steps:
        - name: Load the desired Word document into the Editor class
          text: Create an instance of the Editor class by passing the desired Word document into it.
        - name: Prepare WordProcessingEditOptions class
          text: Create an instance of WordProcessingEditOptions and adjust its properties to meet your needs.
        - name: Select desired paginal mode
          text: Choose the desired pagination mode during editing — in float mode, the entire document will be represented as one single page, while paginal mode preserves the original page separation.
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the Editor.edit method with WordProcessingEditOptions and obtain an instance of EditableDocument, ready for editing. Generate HTML-markup and extract resources to pass to the WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, then submit the edited content and resources back to the server-side.
        - name: Create an instance of EditableDocument
          text: Create an instance of EditableDocument by passing the edited document content into the appropriate static methods of the class.
        - name: Prepare a WordProcessingSaveOptions class
          text: Create an instance of WordProcessingSaveOptions and adjust its properties as needed. Choose the format for the output document and set the pagination mode.
        - name: Save the edited document with Editor.save
          text: Pass an instance of EditableDocument with the edited content, along with WordProcessingSaveOptions, and save the document to the desired file path.

---

## How to edit a Word document?

GroupDocs.Editor for Node.js and Java supports editing WordProcessing formats such as DOC, DOCX, DOT, DOCM, DOTX, ODT, and RTF. It allows two processing modes:

- **float** (default)
- **paged** (also known as **paginal**)

In float mode, the document appears as a single page when loaded into the WYSIWYG editor. In paged mode, the document is displayed with its original page breaks, similar to Microsoft Word.

### Loading WordProcessing documents

Start by loading the document into the `Editor` class. Here’s an example of loading a password-protected DOCX file:

```javascript
const groupdocs = require('groupdocs-editor');
const Editor = groupdocs.Editor;
const WordProcessingLoadOptions = groupdocs.options.WordProcessingLoadOptions;

// Load options for a password-protected document
const loadOptions = new WordProcessingLoadOptions();
loadOptions.password = "some_password_to_open_document";

// Load the document into the Editor
const inputPath = "C://input_path/document.docx";
const editor = new Editor(inputPath, loadOptions);

---

## How to edit a Word document?

GroupDocs.Editor for Node.js supports editing WordProcessing formats such as DOC, DOCX, DOT, DOCM, DOTX, ODT, and RTF. It allows two processing modes:

- **float** (default)
- **paged** (also known as **paginal**)

In float mode, the document appears as a single page when loaded into the WYSIWYG editor. In paged mode, the document is displayed with its original page breaks, similar to Microsoft Word.

### Loading WordProcessing documents

Start by loading the document into the `Editor` class. Here’s an example of loading a password-protected DOCX file:

```javascript
const groupdocs = require('groupdocs-editor');
const Editor = groupdocs.Editor;
const WordProcessingLoadOptions = groupdocs.options.WordProcessingLoadOptions;

// Load options for a password-protected document
const loadOptions = new WordProcessingLoadOptions();
loadOptions.password = "some_password_to_open_document";

// Load the document into the Editor
const inputPath = "C://input_path/document.docx";
const editor = new Editor(inputPath, loadOptions);
```

If the document is unprotected, the password will be ignored. If the document is protected but no password is provided, an error will be thrown during editing.

### Editing WordProcessing documents

Once the document is loaded, you can edit it by creating `WordProcessingEditOptions`:

```javascript
const WordProcessingEditOptions = groupdocs.options.WordProcessingEditOptions;

let editOptions = new WordProcessingEditOptions();
editOptions.fontExtraction = groupdocs.options.FontExtractionOptions.ExtractEmbeddedWithoutSystem;
editOptions.enableLanguageInformation = true;
editOptions.enablePagination = true;
```

Explanation:
- `fontExtraction`: Extracts fonts used in the document. 
- `enableLanguageInformation`: Enables language information extraction for spell-checking.
- `enablePagination`: Switches from the default float mode to paged mode.

Next, edit the document using the prepared options:

```javascript
let editableDocument = editor.edit(editOptions);
```

To extract the HTML markup and resources separately:

```javascript
let originalContent = editableDocument.getContent();
let allResources = editableDocument.getAllResources();
```

### Modifying document content

After editing the document in the WYSIWYG-editor on the client side, pass the modified content back to the server:

```javascript
let editedContent = originalContent.replace("document", "edited document");
let updatedDocument = groupdocs.EditableDocument.fromMarkup(editedContent, allResources);
```

### Saving the Word document after editing

To save the edited document, you first need to create `WordProcessingSaveOptions`:

```javascript
const WordProcessingSaveOptions = groupdocs.options.WordProcessingSaveOptions;

let saveOptions = new WordProcessingSaveOptions(groupdocs.formats.WordProcessingFormats.Docm);
saveOptions.password = "password";
saveOptions.enablePagination = true;
saveOptions.locale = "en-US";
saveOptions.optimizeMemoryUsage = true;
saveOptions.protection = new groupdocs.options.WordProcessingProtection(
    groupdocs.options.WordProcessingProtectionType.ReadOnly, 
    "write_password"
);
```

Explanation:
1. `WordProcessingFormats.Docm`: Sets the format to DOCM.
2. `password`: Encrypts the document with a password.
3. `enablePagination`: Preserves pagination in the saved document.
4. `locale`: Sets the locale.
5. `optimizeMemoryUsage`: Optimizes memory for large documents.
6. `protection`: Adds write protection to the document.

Finally, save the document:

```javascript
let outputPath = "C://output_path/edited_document.docm";
editor.save(updatedDocument, outputPath, saveOptions);
```
