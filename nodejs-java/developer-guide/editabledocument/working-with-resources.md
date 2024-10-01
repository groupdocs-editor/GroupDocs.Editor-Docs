---

id: working-with-resources
url: editor/nodejs-java/working-with-resources
title: Working with Resources
weight: 3
description: "This article demonstrates and explains different operations with resources, including retrieving, adjusting, and saving them in different scenarios when editing documents with GroupDocs.Editor for Node.js via Java."
keywords: Working with HTML resources, Edit document, GroupDocs.Editor
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to Work with Resources using GroupDocs.Editor
        description: Learn how to retrieve, adjust, and save resources when editing documents using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
---

> This guide demonstrates and explains different operations with resources, including retrieving, adjusting, and saving them in different scenarios using GroupDocs.Editor for Node.js via Java.

## Introduction

Almost all documents of any type have resources. First of all, images; some document formats also hold fonts. Even for plain text documents (TXT), when converting them to HTML for editing, there will be one stylesheet, which is treated as a resource. **GroupDocs.Editor** allows you to work with resources during the editing phase when a document is loaded into the `Editor` class and opened for editing by generating the `EditableDocument` instance, which is produced by the `editor.edit()` method. An instance of `EditableDocument` can be treated as an input document converted to an internal intermediate format, with possibilities to generate HTML markup and resources for passing them to the client-side WYSIWYG HTML editor. GroupDocs.Editor classifies all resources into four groups:

1. **Images**, including raster (PNG, BMP, JPEG, GIF, ICON) and vector (SVG and WMF).
2. **Fonts**, including TTF, EOT, WOFF, WOFF2.
3. **Textual resources**: only CSS.
4. **Audio files**: only MP3 (appeared in version 22.6).

Every resource type has its own distinct class with metadata, constructors, and other methods. They all are accessible through the `groupdocs-editor` module.

## Preparations

Let's prepare an `EditableDocument` instance by loading and editing an input WordProcessing document. The following code demonstrates how to achieve this:

```javascript
// Import necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Specify the input DOCX file path
const inputDocxPath = 'C://input/document.docx';

// Create an instance of Editor with optional load options
const editor = new groupdocsEditor.Editor(inputDocxPath, new groupdocsEditor.WordProcessingLoadOptions());

// Prepare edit options
const editOptions = new groupdocsEditor.WordProcessingEditOptions();

// Enable maximum font extraction
editOptions.setFontExtraction(groupdocsEditor.FontExtractionOptions.ExtractAll);

// Create EditableDocument instance
const beforeEdit = editor.edit(editOptions);
```

In this example:

- We import the `groupdocs-editor` module.
- Specify the path to the input WordProcessing document.
- Create an instance of the `Editor` class.
- Prepare `WordProcessingEditOptions` and set font extraction to extract all fonts.
- Call the `editor.edit()` method to create an `EditableDocument` instance.

## Obtaining Resources

Now that the `EditableDocument` instance is ready, it is possible to obtain resources from it. The `EditableDocument` provides several ways to do this.

First of all, resources can be retrieved by their type. The `EditableDocument` contains methods for each resource type:

- `getImages()`: Returns an array of `IImageResource` instances, which is common for all images, raster and vector.
- `getFonts()`: Returns an array of `FontResourceBase` instances, which is common for all fonts.
- `getCss()`: Returns an array of `CssText` instances, representing CSS stylesheets.
- `getAudio()`: Returns an array of `Mp3Audio` instances, representing MP3 audio files (added in version 21.10).

Secondly, all resources can be obtained with a single method `getAllResources()`. It returns an array of `IHtmlResource` instances, which is common for all HTML resources, including images, fonts, stylesheets, and audio files. The collection returned by the `getAllResources()` method is essentially a concatenation of the previous resource arrays.

Here's the source code:

```javascript
// Get images
const images = beforeEdit.getImages();

// Get fonts
const fonts = beforeEdit.getFonts();

// Get stylesheets
const stylesheets = beforeEdit.getCss();

// Get audio resources (if any)
const audioFiles = beforeEdit.getAudio();

// Get all resources together
const allTogether = beforeEdit.getAllResources();
```

If you want to manually save the instance of `EditableDocument` as an HTML file with resources, you may use code like this:

```javascript
// Import necessary modules
const fs = require('fs');
const path = require('path');

// Define the output folder
const outputFolder = 'C://output//document_resources//';

// Ensure the output folder exists
if (!fs.existsSync(outputFolder)) {
    fs.mkdirSync(outputFolder, { recursive: true });
}

// Save images
images.forEach((oneImage) => {
    const imagePath = path.join(outputFolder, oneImage.getFilenameWithExtension());
    fs.writeFileSync(imagePath, oneImage.getByteContent());
});

// Save fonts
fonts.forEach((oneFont) => {
    const fontPath = path.join(outputFolder, oneFont.getFilenameWithExtension());
    fs.writeFileSync(fontPath, oneFont.getByteContent());
});

// Save stylesheets
stylesheets.forEach((oneStylesheet) => {
    const stylesheetPath = path.join(outputFolder, oneStylesheet.getFilenameWithExtension());
    fs.writeFileSync(stylesheetPath, oneStylesheet.getTextContent());
});

// Save audio files (if any)
audioFiles.forEach((oneAudio) => {
    const audioPath = path.join(outputFolder, oneAudio.getFilenameWithExtension());
    fs.writeFileSync(audioPath, oneAudio.getByteContent());
});

// Save the HTML content
fs.writeFileSync('C://output//document.html', beforeEdit.getContent());
```

In this example:

- We import the `fs` and `path` modules to handle file system operations.
- Define the output folder where resources will be saved.
- Ensure the output folder exists, creating it if necessary.
- Iterate over each resource type (images, fonts, stylesheets, audio) and save each resource to the output folder.
- Save the HTML content to a file.

## CSS Resources

There is also a way designed especially for stylesheets. Stylesheets can contain external resources too, which are presented as links with URLs. For example, they can reference images, fonts, and other stylesheets. In such cases, it is necessary to adjust such links. To cope with this, the `EditableDocument` contains two overloads of the `getCssContent()` method.

- **First overload**: Parameterless, returns an array of strings, each representing one stylesheet. In most cases, the document has only one stylesheet, so the array will have only one item.
- **Second overload**: Accepts two string parameters: `externalImagesPrefix` and `externalFontsPrefix`. The first parameter is designated for images, while the second is for fonts.

Both overloads are shown in the example below:

```javascript
// Get stylesheets without prefixes
const stylesheetsWithoutPrefixes = beforeEdit.getCssContent();

// Define external prefixes
const externalImagesPrefix = "http://www.mywebsite.com/images/id=";
const externalFontsPrefix = "http://www.mywebsite.com/fonts/id=";

// Get stylesheets with prefixes
const stylesheetsWithPrefixes = beforeEdit.getCssContent(externalImagesPrefix, externalFontsPrefix);
```

In this example:

- We first retrieve the stylesheets without any external resource prefixes.
- Then, we define external prefixes for images and fonts.
- Use the `getCssContent()` method with the prefixes to adjust the resource links within the stylesheets.

## Conclusion

By following this guide, you can effectively work with resources when editing documents using GroupDocs.Editor for Node.js via Java. You can retrieve, adjust, and save images, fonts, stylesheets, and audio files associated with your documents. This enables you to have full control over the resources when integrating with client-side WYSIWYG HTML editors or for other purposes.

**Note:** Be sure to replace the paths in the code examples with the actual paths used in your application.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.
