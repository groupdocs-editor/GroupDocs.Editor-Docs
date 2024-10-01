---

id: edit-txt
url: editor/nodejs-java/edit-txt
title: Edit TXT
linkTitle: Edit Text Files
weight: 60
description: "This guide demonstrates how to edit plain text files with encoding, lists recognition, pagination, and other powerful features of GroupDocs.Editor for Node.js via Java"
keywords: Edit TXT file, Edit plain text, Edit TXT programmatically
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit text file using GroupDocs.Editor
        description: How to edit plain text TXT file using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java 
    showVideo: True
    howTo:
        name: How to edit content of a plain text (TXT) file using GroupDocs.Editor in Node.js
        description: Learn how to edit content of a plain text (TXT) file using GroupDocs.Editor in Node.js step by step
        steps:
        - name: Load the desired text file into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload by passing the desired text file into it. LoadOptions are not needed.
        - name: Prepare a TextEditOptions instance
          text: Create an instance of the TextEditOptions class and adjust its properties to meet your needs if necessary. You can specify the encoding of the text document, direction of the text flow, pagination mode, and other options.
        - name: Call editor.edit() and send the obtained EditableDocument to the WYSIWYG editor
          text: Invoke the editor.edit() method by specifying the previously prepared TextEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML markup and extract resources from this instance using corresponding instance methods, and pass all this data to the HTML-based WYSIWYG editor.
        - name: Edit the document in the WYSIWYG editor and send the edited content back to the server-side
          text: Make all necessary edits in the content of the text file in the HTML-based WYSIWYG editor, which is running on the client-side (in a web browser), and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited text content into the most suitable static methods of the class.
        - name: Prepare a TextSaveOptions instance
          text: Create an instance of the TextSaveOptions class and adjust its properties to meet your needs if necessary. You can specify the encoding of the text document and other options.
        - name: Save the edited text file with the editor.save() method
          text: Pass an instance of EditableDocument with the content of the edited text file, an instance of TextSaveOptions, and a destination byte stream or file path to the editor.save() method for saving the text file.
---

> This demonstration shows how to load, open for editing, and save textual documents, explains edit and save options and their purpose.

## How to Edit a TXT File?

Textual documents are simple plain text flat files (TXT) that contain no images, pages, paragraphs, lists, tables, and so on. However, users can create some primitive formatting like lists with leading markers, left indents with whitespaces, tables with pseudo-graphics, paragraphs with line breaks, and so on. **GroupDocs.Editor** can recognize some of these structures. Another non-obvious feature that GroupDocs.Editor provides is the ability to save an edited TXT document not only back to TXT but also to WordProcessing formats.

## Loading Text File for Editing

Unlike WordProcessing and Spreadsheet documents, text documents are loaded into the `Editor` class without any loading options. They are simple text files by their nature, so there is nothing to adjust:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Specify the input TXT file path
const inputTxtPath = "C://input//file.txt";

// Create an instance of Editor
const editor = new groupdocsEditor.Editor(inputTxtPath);
```

## Edit Text File

In order to open a text document for editing by creating an `EditableDocument` instance, it is required to use the `TextEditOptions` class. This class has several options (properties) that are described below:

1. **Encoding** — allows setting the character encoding of the input text document. By default, if not specified, it is UTF-8.
2. **RecognizeLists** — boolean flag that indicates how exactly numbered list items are recognized. If this option is set to `false` (default value), the list recognition algorithm detects list paragraphs when list numbers end with either a dot, right bracket, or bullet symbols (such as "•", "*", "-" or "o"). If this option is set to `true`, whitespaces are also used as list number delimiters: the list recognition algorithm for Arabic style numbering (1., 1.1.2.) uses both whitespaces and dot (".") symbols.
3. **LeadingSpaces** — enum flag that indicates how consecutive leading spaces should be handled: convert to left indent (default value), preserve as consecutive spaces, or trim.
4. **TrailingSpaces** — enum flag that indicates how consecutive trailing spaces should be handled: truncated (default value) or trim.
5. **EnablePagination** — boolean flag that allows enabling paged view of the document. By their nature, all text documents are pageless; however, GroupDocs.Editor allows splitting them into pages, like MS Word does.
6. **Direction** — enum flag that allows specifying the direction of text flow in the input plain text document. By default, it is Left-to-Right.

The source code below demonstrates using this options class and opening an input text document for editing. Then, when the `EditableDocument` instance is ready, the example below demonstrates how to emit HTML markup from it, edit it, and create a new `EditableDocument` instance from the edited content:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Prepare TextEditOptions
const editOptions = new groupdocsEditor.TextEditOptions();
editOptions.setEncoding('UTF-8');
editOptions.setRecognizeLists(true);
editOptions.setLeadingSpaces(groupdocsEditor.TextLeadingSpacesOptions.ConvertToIndent);
editOptions.setTrailingSpaces(groupdocsEditor.TextTrailingSpacesOptions.Trim);
editOptions.setDirection(groupdocsEditor.TextDirection.Auto);

// Create EditableDocument instance
const beforeEdit = editor.edit(editOptions);

// Get HTML content
const originalTextContent = beforeEdit.getContent();

// Edit content
const updatedTextContent = originalTextContent.replace(/text/g, 'EDITED text');

// Get resources (only one stylesheet actually in this case)
const allResources = beforeEdit.getAllResources();

// Finally, create new EditableDocument
const afterEdit = groupdocsEditor.EditableDocument.fromMarkup(updatedTextContent, allResources);
```

## Save Text File After Editing

After being edited, the text document can be saved back as TXT or as a WordProcessing document. To save back to TXT format, you must use the `TextSaveOptions` class, which has three properties described below:

1. **Encoding** — character encoding of the text document, which will be applied for its saving. By default, if not specified, it is UTF-8.
2. **AddBidiMarks** — boolean flag that determines whether to add bi-directional marks before each BiDi run when saving in plain text format.
3. **PreserveTableLayout** — boolean flag that specifies whether GroupDocs.Editor should try to preserve the layout of tables when saving in the plain text format. The default value is `false`.

The source code below shows how to save the `EditableDocument` to both TXT and WordProcessing formats:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Prepare TextSaveOptions
const txtSaveOptions = new groupdocsEditor.TextSaveOptions();
txtSaveOptions.setAddBidiMarks(true);
txtSaveOptions.setPreserveTableLayout(true);

// Prepare WordProcessingSaveOptions
const wordSaveOptions = new groupdocsEditor.WordProcessingSaveOptions(groupdocsEditor.WordProcessingFormats.Docm);

// Specify output paths
const outputTxtPath = "C:\\output\\document.txt";
const outputWordPath = "C:\\output\\document.docm";

// Save the edited document as TXT
editor.save(afterEdit, outputTxtPath, txtSaveOptions);

// Save the edited document as WordProcessing format
editor.save(afterEdit, outputWordPath, wordSaveOptions);
```

---

By following this guide, you can effectively edit plain text files using GroupDocs.Editor for Node.js via Java, customizing the editing process according to your specific needs.
