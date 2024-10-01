---

id: create-editabledocument-from-file-or-markup
url: editor/nodejs-java/create-editabledocument-from-file-or-markup
title: Create EditableDocument from File or Markup
weight: 4
description: "This article explains how to create an instance of the EditableDocument class from HTML files from disk or from HTML markup with resources using GroupDocs.Editor for Node.js via Java API."
keywords: Edit HTML document, GroupDocs.Editor
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to Create EditableDocument from File or Markup using GroupDocs.Editor
        description: Learn how to create an instance of EditableDocument from HTML files or markup with resources using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to Create EditableDocument from File or Markup using GroupDocs.Editor in Node.js via Java
        description: Step-by-step guide to create EditableDocument from HTML files or markup with resources
        steps:
        - name: Load the Edited HTML File from Disk
          text: Use the EditableDocument.fromFile() method to create an instance from an HTML file saved on disk, optionally specifying the resource folder.
        - name: Create EditableDocument from HTML Markup and Resources in Memory
          text: Use the EditableDocument.fromMarkup() method by providing the HTML markup string and an array of resources (images, stylesheets, etc.).
        - name: Create EditableDocument from Body Markup and Resource Folder
          text: Use the EditableDocument.fromBodyMarkupAndResourceFolder() method by providing the inner BODY content and the path to the resource folder.
---

> This demonstration shows how to create an instance of the `EditableDocument` class from HTML files on disk or from HTML markup with resources in Node.js via Java.

## Introduction

When working with [**GroupDocs.Editor**](https://products.groupdocs.com/editor/nodejs-java) in the usual way by loading, opening, editing, and saving documents, instances of the `EditableDocument` class are produced by the `editor.edit()` method and accepted by the `editor.save()` method. However, in some cases, it is required to create an `EditableDocument` instance from existing HTML markup with optional resources.

For example, a document was loaded into the `Editor` class, opened for editing, and then the `EditableDocument` was saved to disk as a set of HTML files and connected resources. Then this HTML document was passed to a WYSIWYG editor, edited, and saved back to disk as modified HTML. Or the raw output from the WYSIWYG editor was saved to a string variable. To save it to some final format like DOCX or XLSX, you need to pass the document to the `editor.save()` method in the form of an `EditableDocument` instance. This means you should create an instance of the `EditableDocument` class manually.

The `EditableDocument` class contains three public static methods for creating its instances:

- `fromFile()`
- `fromMarkup()`
- `fromBodyMarkupAndResourceFolder()`

This article reviews all of them.

## Opening from File

Let's suppose that we have an HTML file with edited document content saved on disk. There is also a folder with resources (images, fonts, stylesheets) that are used by this document, and the document has correct links to these resources.

Let's say the HTML document is named "document.html" and is located in the "input" folder. The resource folder is located in the same "input" folder and is named "document_resources". Importantly, the HTML markup from "document.html" has proper links to files from the "document_resources" folder. For example, there will be:

```html
<link rel="stylesheet" type="text/css" href="document_resources/style.css" />
```

in the "document.html", and the "document_resources" folder actually contains the "style.css" file.

In that case, creating the `EditableDocument` instance is straightforward:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Specify the input HTML file path
const inputHtmlPath = 'C://input//document.html';

// Create an EditableDocument from the file
const editableDocument = groupdocsEditor.EditableDocument.fromFile(inputHtmlPath);
```

The `fromFile()` method accepts two parameters:

- The first is the path to the HTML file.
- The second is the path to the resource folder (optional).

If the HTML file contains correct links, as described above, the second parameter is not necessary; GroupDocs.Editor will scan the links and automatically find the resource folder and correct resources. However, if the path to the resource folder doesn't match the links in the HTML file, it is possible to specify a resource folder forcibly, as illustrated below:

```javascript
// Specify the input HTML file and resource folder paths
const inputHtmlPath = 'C://input//document.html';
const inputResourceFolderPath = 'C://input//document_resources/';

// Create an EditableDocument from the file and resource folder
const editableDocument = groupdocsEditor.EditableDocument.fromFile(inputHtmlPath, inputResourceFolderPath);
```

If the HTML file contains a link to a resource that is not present in the resource folder, it will be omitted.

## Opening from Markup

In some cases, the edited HTML document is not present as a file. It may be stored in a database, obtained from remote storage, or something else. In such cases, the `EditableDocument` class provides the `fromMarkup()` method. This method has two parameters:

- `newHtmlContent`: A string that contains raw HTML markup. This parameter is mandatory.
- `resources`: An array of prepared resources (images, fonts, stylesheets) that are used or may be used in the HTML document. You need to prepare this collection yourself. This parameter is optional; you may pass an empty array. As with the previous method, if the HTML markup refers to a resource that is not found in the "resources" collection, it will be skipped (omitted).

The code example below demonstrates this approach:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// The HTML markup as a string
const inputHtmlMarkup = '<html><head><title>Edited document</title></head><body>...</body></html>';

// Prepare resources
const fs = require('fs');
const path = require('path');

// Assuming you have the resources as files or in memory
const image1Content = fs.readFileSync('path/to/image1.jpg');
const image2Content = fs.readFileSync('path/to/image2.png');
const stylesheetContent = 'p { color: black; text-align: left; } ...';

// Create resource instances
const image1 = new groupdocsEditor.JpegImage('image1.jpg', image1Content);
const image2 = new groupdocsEditor.PngImage('image2.png', image2Content);
const stylesheet = new groupdocsEditor.CssText('stylesheet.css', stylesheetContent, 'UTF-8');

// Add resources to an array
const allResources = [image1, image2, stylesheet];

// Create an EditableDocument from the markup and resources
const editableDocument = groupdocsEditor.EditableDocument.fromMarkup(inputHtmlMarkup, allResources);
```

## Opening from Inner BODY HTML Content and Resource Folder

All previously described methods of creating instances of the `EditableDocument` class assume that both the HTML file or string with HTML markup contains a valid, well-formed HTML document according to all W3C requirements. Such a document should contain an HTML Document Definition (DOCTYPE) and a root `<html>` element that, in turn, has two and only two children: `<head>` (with document meta-information) and `<body>` (with document content). All stylesheets are included and/or embedded in the `<head>` element (`<link>` and/or `<style>` elements, respectively), and are 'used by' content markup (by using `class` and `id` attributes, in most cases).

However, most client-side WYSIWYG HTML editors like TinyMCE and CKEditor work only with the inner content of the `<body>` element: they can obtain only such markup on input and produce such markup on output. External stylesheets are usually included in the HTML editor settings or somewhere else, but not through HTML markup (except for the inline styles in the `style` attribute).

To pass HTML `<body>` markup *into* the HTML editor, there is a `getBodyContent()` method, which returns the inner content of the HTML `<body>` element. Conversely, to obtain HTML markup *from* the HTML editor and create a new `EditableDocument` instance with this markup, the `fromBodyMarkupAndResourceFolder()` method is used. This method obtains two mandatory parameters:

- `htmlBodyContent`: A string that contains raw HTML markup located inside the `<body>` element (between `<body>` and `</body>` tags, without these opening/closing tags themselves). This should be valid markup.
- `resourceFolderPath`: A string that contains the full path to the folder with all external resources for the opening document. If the folder doesn't exist, an exception will be thrown.

Because inner `<body>` markup doesn't contain stylesheets (neither included nor embedded), these stylesheets should be referenced in another way. To obtain them, the second parameter exists; you should specify a full path to the folder that contains images, fonts, and stylesheets used by the HTML document specified in the first parameter. GroupDocs.Editor scans this folder, finds `.css` files, opens them, parses their content, and if this content is valid, applies these stylesheets to the document. If a stylesheet references external images and/or fonts, GroupDocs.Editor will try to find these resources in this resource folder too. Also, if the HTML markup specified in the first parameter contains external images referenced using an `<img>` element, GroupDocs.Editor will try to find these images in this resource folder too. In other words, the `fromBodyMarkupAndResourceFolder()` method "assumes" that the resource folder contains resources for that specific HTML document, whose inner `<body>` content was specified in the first parameter.

Here's how you can use this method:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// The inner BODY HTML content as a string (without <body> tags)
const htmlBodyContent = '<p>This is the edited content...</p>';

// Specify the resource folder path
const resourceFolderPath = 'C://input//document_resources/';

// Create an EditableDocument from body markup and resource folder
const editableDocument = groupdocsEditor.EditableDocument.fromBodyMarkupAndResourceFolder(htmlBodyContent, resourceFolderPath);
```

By using these methods, you can create `EditableDocument` instances from different sources, enabling you to save the edited content back to the desired output format using the `editor.save()` method.

---

**Note:** In this guide, we replaced any links to previous versions with "20.6" as per your instruction. The `fromBodyMarkupAndResourceFolder()` method was introduced in the **20.6 version** of GroupDocs.Editor for Node.js via Java.

---

By following this guide, you can effectively create `EditableDocument` instances from HTML files or markup, which allows you to save edited documents back to various formats using GroupDocs.Editor for Node.js via Java.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.

---