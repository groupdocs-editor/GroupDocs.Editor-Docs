---

id: save-html-to-folder
url: editor/nodejs-java/save-html-to-folder
title: Save HTML to Folder
weight: 2
description: "This article explains how to save an edited document in HTML form to a folder on local disk using GroupDocs.Editor for Node.js via Java features."
keywords: Save edited HTML, Save edited document, Save HTML to folder
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to Save HTML to Folder using GroupDocs.Editor
        description: Learn how to save edited documents as HTML files with resource folders using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to Save HTML to Folder using GroupDocs.Editor in Node.js via Java
        description: Step-by-step guide to save edited documents as HTML files with resource folders
        steps:
        - name: Load the Input Document into the Editor
          text: Create an instance of the Editor class by specifying the input file path and optional load options.
        - name: Open the Document for Editing
          text: Use the editor.edit() method with appropriate edit options to obtain an EditableDocument instance.
        - name: Save the EditableDocument as HTML to Disk
          text: Extract the HTML content and resources from the EditableDocument and save them to disk.
        - name: Dispose of Resources
          text: Dispose of the EditableDocument and Editor instances to free up resources.

---

> This guide demonstrates how to open an input document, convert it to an intermediate `EditableDocument`, and save it to disk as an HTML file with a resource folder using GroupDocs.Editor for Node.js via Java.

Almost all HTML WYSIWYG client-side editors are able to open HTML documents from disk (from a path). **GroupDocs.Editor for Node.js via Java** allows you to open any supported document, convert it to HTML, and save it to disk, which may be very useful for subsequent editing in a WYSIWYG editor. The code below demonstrates such an example.

```javascript
// Import necessary modules
const fs = require('fs');
const path = require('path');
const groupdocsEditor = require('groupdocs-editor');

// Specify the input file path
const inputFilePath = 'C:\\input_path\\document.docx'; // Path to some document

// Create load options
const loadOptions = new groupdocsEditor.WordProcessingLoadOptions();

// Create an Editor instance with the input file and load options
const editor = new groupdocsEditor.Editor(inputFilePath, loadOptions);

// Open the document for editing with format-specific edit options
const editOptions = new groupdocsEditor.WordProcessingEditOptions();
const editableDocument = editor.edit(editOptions);

// Get the HTML content
const htmlContent = editableDocument.getContent();

// Specify the output HTML file path
const outputHtmlFilePath = 'C:\\output_path\\document.html';

// Save the HTML content to a file
fs.writeFileSync(outputHtmlFilePath, htmlContent);

// If there are resources (images, stylesheets, fonts), save them to a folder
const resourcesFolderPath = 'C:\\output_path\\document_files';

// Ensure the resources folder exists
if (!fs.existsSync(resourcesFolderPath)) {
    fs.mkdirSync(resourcesFolderPath, { recursive: true });
}

// Get all resources
const resources = editableDocument.getAllResources();

// Save each resource to the resources folder
resources.forEach((resource) => {
    const resourceName = resource.getResourceName();
    const resourceContent = resource.getContent();
    const resourcePath = path.join(resourcesFolderPath, resourceName);
    fs.writeFileSync(resourcePath, resourceContent);
});

// Dispose of resources
editableDocument.dispose();
editor.dispose();
```

In this example, we load an input WordProcessing (DOCX) document into the `Editor` class with load options specific for this document family type—`WordProcessingLoadOptions`. Then, the document is converted to an `EditableDocument` using the `editor.edit()` method, which, in turn, uses document-specific `WordProcessingEditOptions`. We then obtain the HTML content and save it to an HTML file on disk, specified by an absolute path.

Please note that **GroupDocs.Editor** does not automatically create an accompanying folder with resources in Node.js via Java, so we manually extract the resources from the `EditableDocument` and save them to a specified folder. The resources may include images, stylesheets, and fonts used by the HTML document.

By the way, don't forget to dispose of all resources when you're done to free up system resources.

```javascript
// Dispose of resources
editableDocument.dispose();
editor.dispose();
```

---

By following this guide, you can effectively save edited documents as HTML files along with their associated resources using GroupDocs.Editor for Node.js via Java. This is particularly useful when you need to provide the HTML content to client-side WYSIWYG editors or for other purposes that require the HTML files to be stored on disk.

**Note:** Be sure to replace the paths in the code examples with the actual paths on your system.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.
