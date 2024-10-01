---

id: editabledocument
url: editor/nodejs-java/editabledocument
title: EditableDocument
weight: 100
description: "This documentation section explains features of the EditableDocument class when editing documents with GroupDocs.Editor for Node.js via Java API."
keywords: Edit document, Editable document
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
---

The `EditableDocument` class represents an input document of any supported format that has been converted to an internal intermediate format according to the edit options and is ready for editing in HTML WYSIWYG editors. The `EditableDocument` contains a variety of methods for emitting HTML markup, stylesheets, and resources with different settings to facilitate editing.

## HTML Markup

The `EditableDocument` class is capable of emitting HTML markup in different forms:

- **Whole document with header**: Includes the full HTML document with `<html>`, `<head>`, and `<body>` tags.
- **Only `<body>` content**: Extracts just the content within the `<body>` tags, suitable for embedding in other HTML documents.
- **Customized external links**: Allows tuning external links for images, fonts, and stylesheets.
- **All-embedded version**: Presents the entire HTML document with all resources embedded as a single string.

### Example: Getting HTML Markup in Different Forms

```javascript
// Import necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Load the document into the Editor
const editor = new groupdocsEditor.Editor('path/to/input/document.docx');

// Edit the document to get an EditableDocument instance
const editableDocument = editor.edit();

// Get the full HTML content including the header
const fullHtmlContent = editableDocument.getContent();

// Get only the body content
const bodyContent = editableDocument.getBodyContent();

// Get the HTML content with embedded resources
const embeddedHtmlContent = editableDocument.getEmbeddedHtml();

// Dispose of resources when done
editableDocument.dispose();
editor.dispose();
```

## Saving to Disk

For some WYSIWYG editors, loading HTML content requires specifying the content via a file path. Alternatively, you might need to save the document as HTML to disk instead of passing it directly to the WYSIWYG editor. In such cases, the `EditableDocument` class has the ability to save its content to disk as an ordinary HTML document, represented by a `.html` file and an accompanying folder with resources (images, stylesheets, fonts).

### Example: Saving HTML to a Folder

```javascript
// Import necessary modules
const fs = require('fs');
const path = require('path');
const groupdocsEditor = require('groupdocs-editor');

// Define output paths
const outputHtmlPath = 'path/to/output/document.html';
const resourcesFolderPath = 'path/to/output/resources';

// Ensure the resources folder exists
if (!fs.existsSync(resourcesFolderPath)) {
    fs.mkdirSync(resourcesFolderPath, { recursive: true });
}

// Save the HTML content to a file
fs.writeFileSync(outputHtmlPath, fullHtmlContent);

// Extract and save resources
const resources = editableDocument.getAllResources();
resources.forEach((resource) => {
    const resourcePath = path.join(resourcesFolderPath, resource.getResourceName());
    fs.writeFileSync(resourcePath, resource.getContent());
});

// Now the HTML file and its resources are saved to disk

// Dispose of resources when done
editableDocument.dispose();
editor.dispose();
```

## Obtaining Resources

When an input document of some format is converted to an `EditableDocument` and then to an HTML document, this HTML document contains not only HTML markup but also one or more stylesheets, images, and sometimes even fonts. All of these are called **resources**. The `EditableDocument` has several methods and properties specifically for working with them.

### Example: Working with Resources

```javascript
// Import necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Load the document into the Editor and edit it
const editor = new groupdocsEditor.Editor('path/to/input/document.docx');
const editableDocument = editor.edit();

// Get all resources
const allResources = editableDocument.getAllResources();

// Iterate over resources and process them
allResources.forEach((resource) => {
    console.log('Resource Name:', resource.getResourceName());
    console.log('Resource Type:', resource.getResourceType());
    // You can also get the content of the resource
    const content = resource.getContent();
    // Process the resource content as needed
});

// Dispose of resources when done
editableDocument.dispose();
editor.dispose();
```

## Creating New EditableDocument Instances

Opening a document for editing involves creating instances of the `EditableDocument` class, which are returned from the `editor.edit()` method. However, when the HTML content of the document has been sent to the WYSIWYG HTML editor and edited by the end-user, you need to create a new instance of the `EditableDocument` class to save it back to the output format.

The `EditableDocument` class provides several static methods that allow you to create instances from HTML documents:

- **From HTML file on disk**: If the edited HTML is saved as a `.html` file with a resource folder.
- **From HTML markup in memory**: If you have the HTML content and resources available in memory.
- **From WYSIWYG editor output**: If the editor provides the inner `<body>` HTML markup and resources.

### Example: Creating EditableDocument from Edited Content

```javascript
// Import necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

// Assume we have the edited HTML content and resources
const editedHtmlContent = '<html>...</html>'; // The edited HTML markup
const resources = []; // Array of resources (images, stylesheets, etc.)

// Create an EditableDocument from the edited content
const editableDocument = groupdocsEditor.EditableDocument.fromMarkup(editedHtmlContent, resources);

// Now you can save the edited document back to the desired format
const saveOptions = new groupdocsEditor.WordProcessingSaveOptions(groupdocsEditor.WordProcessingFormats.Docx);
editor.save(editableDocument, 'path/to/output/document.docx', saveOptions);

// Dispose of resources when done
editableDocument.dispose();
editor.dispose();
```

---

By understanding and utilizing the `EditableDocument` class, you can effectively manage the conversion between different document formats and HTML, facilitating seamless editing experiences in your applications using GroupDocs.Editor for Node.js via Java.

**Note:** Be sure to replace the paths and variables in the code examples with the actual values used in your application.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.
