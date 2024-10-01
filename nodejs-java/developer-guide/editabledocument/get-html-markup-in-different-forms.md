---

id: get-html-markup-in-different-forms
url: editor/nodejs-java/get-html-markup-in-different-forms
title: Get HTML Markup in Different Forms
weight: 1
description: "Learn how to get edited document HTML markup - body without head tag, content in raw and base64 form, and others using GroupDocs.Editor for Node.js via Java API."
keywords: Get html content, get html body, get html markup
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
---

> This guide demonstrates how to open an input document, convert it to an intermediate `EditableDocument`, and get HTML markup in different forms depending on client requirements using GroupDocs.Editor for Node.js via Java.

## Preparations

When an input document is loaded into the `Editor` class and opened for editing by transforming it into the intermediate `EditableDocument` class, it is possible to generate and get HTML markup in different forms. The code below shows various methods to achieve this.

First, you need to load the document into the `Editor` class and open it for editing, as demonstrated in the code below.

```javascript
// Import necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Specify the input file path
const inputFilePath = 'C:\\input_path\\document.docx'; // Path to some document

// Create load options
const loadOptions = new groupdocsEditor.WordProcessingLoadOptions();

// Create an Editor instance with the input file and load options
const editor = new groupdocsEditor.Editor(inputFilePath, loadOptions);

// Open the document for editing with format-specific edit options
const editOptions = new groupdocsEditor.WordProcessingEditOptions();
const document = editor.edit(editOptions);
```

The code above prepares a ready-to-use instance of the `EditableDocument` class, which contains the original document in its own intermediate format and is able to generate HTML markup in different forms.

## Getting Whole HTML Content

The most default and standard method for generating HTML markup is the parameterless `getContent()` method:

```javascript
const htmlContent = document.getContent();
```

If the document has external resources (stylesheets, fonts, images), they are referenced via different HTML elements: stylesheets are specified through `<link>` elements, while images through `<img>`. When using the `getContent()` method, such external resources will be referenced by external links. For example:

```html
<link href="stylesheet.css" rel="stylesheet"/>
<img src="image.png"/>
```

Quite often on the web server where such HTML will be edited, resources are processed by specific HTTP handlers. In such cases, it is required to adjust paths to such endpoints. A more advanced overload of the `getContent()` method can help:

```javascript
const externalImagesPrefix = "http://www.mywebsite.com/images/id=";
const externalCssPrefix = "http://www.mywebsite.com/css/id=";
const prefixedHtmlContent = document.getContent(externalImagesPrefix, externalCssPrefix);
```

In the example above, the specified prefixes will be added to every external link in the document's markup. For example, with the code above, the links will be as follows:

```html
<link href="http://www.mywebsite.com/css/id=stylesheet.css" rel="stylesheet"/>
<img src="http://www.mywebsite.com/images/id=image.png"/>
```

## Getting HTML BODY Content

Many HTML WYSIWYG editors are not able to process the whole HTML document with the `<head>` section and so on. They are able only to process the inner content of the HTML `<body>` element. To obtain such a part of the HTML markup, the `EditableDocument` class contains the `getBodyContent()` method, which, like the previous one, has two overloads, as shown below:

```javascript
const bodyContent = document.getBodyContent();
const externalImagesPrefix = "http://www.mywebsite.com/images/id=";
const prefixedBodyContent = document.getBodyContent(externalImagesPrefix);
```

The first parameterless overload, like the previous one, leaves links to the external images intact. The second, which accepts the external resource prefix, adds this prefix to every URL in the `src` attribute of every `<img>` tag found inside the HTML `<body>` markup.

## Getting Base64-Encoded Content

Sometimes it is necessary to obtain all the content of the document with all used resources into one single string. GroupDocs.Editor allows you to do this:

```javascript
const embeddedHtmlContent = document.getEmbeddedHtml();
```

In such a string, all stylesheets will be placed into `<style>` elements in the HTML `<head>` section, and all images in `<img>` elements will be serialized with base64 encoding and placed directly in the `src` attributes. All fonts and images used in stylesheets will also be serialized and stored in appropriate locations in the corresponding stylesheet. Such a string will be fully autonomous and self-sufficient.

## Conclusion

This guide has explained different ways of obtaining HTML markup from a document in different forms using GroupDocs.Editor for Node.js via Java. Depending on your application's requirements, you can choose the most suitable method to extract the HTML content for editing or displaying purposes.

---

By following this guide, you can effectively work with the `EditableDocument` class to get HTML markup in various forms, facilitating seamless integration with HTML-based WYSIWYG editors or other applications.

**Note:** Be sure to replace the paths and variables in the code examples with the actual values used in your application.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.
