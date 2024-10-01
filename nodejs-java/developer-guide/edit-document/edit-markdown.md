---

id: edit-markdown
url: editor/nodejs-java/edit-markdown
title: Edit Markdown Documents
linktitle: Edit Markdown Documents
weight: 95
description: "This guide demonstrates how to edit the content of Markdown documents/files like common text documents using GroupDocs.Editor for Node.js via Java."
keywords: Edit Markdown, Edit Markdown file, Edit Markdown document, Edit MD, Edit MD file, Edit MD document, Import Markdown, Export markdown
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to Edit Markdown Document using GroupDocs.Editor
        description: How to edit Markdown documents using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to Edit Content of a Markdown Document using GroupDocs.Editor in Node.js
        description: Learn how to edit content of a Markdown (MD) document with different editing options using GroupDocs.Editor in Node.js step by step
        steps:
        - name: Load the desired Markdown document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, passing the desired Markdown document into it by a file path or a stream.          
        - name: Implement the IMarkdownImageLoadCallback interface
          text: If the input Markdown document has images, to preserve them it is necessary to create an IMarkdownImageLoadCallback implementation that will provide image data to GroupDocs.Editor.          
        - name: Prepare a MarkdownEditOptions instance
          text: Create an instance of the MarkdownEditOptions class and specify the previously created implementation of IMarkdownImageLoadCallback.
        - name: Call editor.edit() and send the obtained EditableDocument to the WYSIWYG editor
          text: Invoke the editor.edit() method by specifying the previously prepared MarkdownEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG editor.
        - name: Edit the document in the WYSIWYG editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG editor, which is running on the client-side (in a web browser), and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class.
        - name: Prepare a MarkdownSaveOptions instance
          text: Create an instance of the MarkdownSaveOptions class and adjust its properties to meet your needs if necessary. Select saving images as embedded in base64 encoding (exportImagesAsBase64 property) or as external files in the specified folder (imagesFolder property). All properties are optional.
        - name: Save the edited document with editor.save() method
          text: Pass an instance of EditableDocument with the content of the edited document, an instance of MarkdownSaveOptions, and a destination byte stream or file path to the editor.save() method for saving the document.
---

> This example demonstrates the standard open-edit-save pipeline with Markdown (MD) documents, using different options at every step.

## Introduction

[Markdown](https://docs.fileformat.com/word-processing/md/) is a lightweight markup language that has become popular recently. Markdown files with a `*.md` extension are plain text files containing special syntax, supporting text formatting, tables, lists, images, and more. There are several dialects of Markdown, including GFM, CommonMark, Multi-Markdown, and others.

GroupDocs.Editor for Node.js via Java fully supports the Markdown format on both import and export, including its auto-detection.

GroupDocs.Editor for Node.js via Java supports the following Markdown features, which mostly follow the CommonMark specification and are represented as appropriate styles or direct formatting:

- Headings
- Blockquotes
- Code blocks
- Horizontal rules
- Bold emphasis
- Italic emphasis
- Strikethrough formatting
- Numbered and bulleted lists
- Tables
- Internal images, stored with base64 encoding
- External images

## Loading

Loading Markdown documents into the `Editor` class is straightforward and similar to other formats. There are no dedicated load options for the Markdown format; it is enough to specify the file itself through a file path or an input stream.

The code example below shows loading the same Markdown file into two `Editor` instances through a file path and a byte stream, and then checking how GroupDocs.Editor will detect the format of the specified file.

```javascript
// Import necessary modules
const fs = require('fs');
const path = require('path');
const groupdocsEditor = require('groupdocs-editor');

// Specify the file name and path
const filename = 'sample.md';
const inputPath = path.join('path', 'to', 'markdown', filename);

// Load from file path
const editorFromPath = new groupdocsEditor.Editor(inputPath);

// Load from stream
const fileStream = fs.createReadStream(inputPath);
const editorFromStream = new groupdocsEditor.Editor(fileStream);

// Get document info
const info = editorFromPath.getDocumentInfo();

if (info.getFormat() === groupdocsEditor.TextualFormats.Md) {
    console.log('The document format is Markdown.');
} else {
    console.log('The document format is not Markdown.');
}

// Dispose of editors when done
editorFromPath.dispose();
editorFromStream.dispose();
```

## Editing

There is a special class `MarkdownEditOptions` for editing Markdown files. It is not mandatory when editing a document; you can use the `editor.edit()` overload without parameters—GroupDocs.Editor will automatically detect the format and apply the default options.

However, specifying custom `MarkdownEditOptions` may be essential when the input Markdown file has external images; "external" means that images are stored elsewhere, and in the Markdown code there are _links_ to these images. When `MarkdownEditOptions` are not specified and correctly configured (explained below), GroupDocs.Editor will not be able to locate such images. To reference all external images, an instance of `MarkdownEditOptions` should be created, and the `setImageLoadCallback()` property should be correctly specified.

To do this, you must create your own class that implements the `IMarkdownImageLoadCallback` interface. This interface defines a single method `processImage()`, which receives a `MarkdownImageLoadArgs` object and must return a value of the `MarkdownImageLoadingAction` enumeration.

The working mechanism is as follows. GroupDocs.Editor parses an input Markdown file line by line, character by character. When it encounters a link to an external image, it creates an instance of `MarkdownImageLoadArgs` with all data about this image—its filename (or relative path, or URL) and a boolean flag indicating whether it is a local link (in the filesystem) or a web link. Then, when `MarkdownEditOptions` have the `setImageLoadCallback()` property specified with your implementation of `IMarkdownImageLoadCallback`, GroupDocs.Editor invokes the `processImage()` method by passing the prepared `MarkdownImageLoadArgs` into it. GroupDocs.Editor then "waits" until your code makes a decision—a return value of the `MarkdownImageLoadingAction` enumeration.

If your implementation returns `MarkdownImageLoadingAction.Skip`, then the image will be skipped—the resultant document will have an "empty area" where the image should be.

If your implementation returns `MarkdownImageLoadingAction.Default`, then GroupDocs.Editor will try to load the image by itself—this is possible if, for example, the link to the image is an absolute path or URL.

If your implementation returns `MarkdownImageLoadingAction.UserProvided`, then this means that your code must provide the image data by itself, specifying it in the `setData()` method of `MarkdownImageLoadArgs`. In that case, GroupDocs.Editor will process the specified binary data for this image.

The code example below shows exactly the last scenario, where you create your own implementation of the `IMarkdownImageLoadCallback` interface.

Let's say we have the following file and folder structure:

- root/`input.md`
- root/images/`image1.png`
- root/images/`image2.jpeg`

The `input.md` is a Markdown file we want to open and edit, located in the "root" folder, and it uses (references) two raster images `image1.png` and `image2.jpeg`, located in the "images" subfolder.

```javascript
// Import necessary modules
const fs = require('fs');
const path = require('path');
const groupdocsEditor = require('groupdocs-editor');

// Define the custom image loader class
class MdImageLoader {
    constructor(imagesFolder) {
        this.imagesFolder = imagesFolder;
    }

    // Implement the processImage method
    processImage(args) {
        const imagePath = path.join(this.imagesFolder, path.basename(args.getImageFileName()));
        const imageData = fs.readFileSync(imagePath);
        args.setData(imageData);
        return groupdocsEditor.MarkdownImageLoadingAction.UserProvided;
    }
}

function editingMarkdown() {
    const inputMdPath = path.join('root', 'input.md');
    const imagesFolder = path.join('root', 'images');

    // Create the edit options
    const editOptions = new groupdocsEditor.MarkdownEditOptions();
    editOptions.setImageLoadCallback(new MdImageLoader(imagesFolder));

    const editor = new groupdocsEditor.Editor(inputMdPath);

    const beforeEdit = editor.edit(editOptions);

    // Ensure there are 2 images
    console.log('Number of images:', beforeEdit.getImages().length);
    console.log('First image type:', beforeEdit.getImages()[0].getType().getFileExtension());
    console.log('Second image type:', beforeEdit.getImages()[1].getType().getFileExtension());

    const originalHtmlContent = beforeEdit.getEmbeddedHtml();

    // Send the 'originalHtmlContent' to the client-side WYSIWYG editor,
    // obtain the edited version, and create a new EditableDocument from it
    const editedHtmlContent = originalHtmlContent; // For demonstration purposes

    const afterEdit = groupdocsEditor.EditableDocument.fromMarkup(editedHtmlContent, null);

    // Ensure 2 images are still present
    console.log('Number of images after edit:', afterEdit.getImages().length);
    console.log('First image type after edit:', afterEdit.getImages()[0].getType().getFileExtension());
    console.log('Second image type after edit:', afterEdit.getImages()[1].getType().getFileExtension());

    // Save to DOCX, for example
    const saveOptions = new groupdocsEditor.WordProcessingSaveOptions(groupdocsEditor.WordProcessingFormats.Docx);
    const outputDocxPath = path.join('root', `Output.${saveOptions.getOutputFormat().getExtension()}`);

    editor.save(afterEdit, outputDocxPath, saveOptions);

    // Dispose of resources
    beforeEdit.dispose();
    afterEdit.dispose();
    editor.dispose();
}

// Call the function
editingMarkdown();
```

In this example, there is a user-created `MdImageLoader` class. It is initialized with the images folder path, and in the `processImage` method, the file content is read and provided to the `MarkdownImageLoadArgs` object through the `setData()` method.

## Saving

GroupDocs.Editor also supports saving into the Markdown format. Like for any other format, to save into Markdown you must create an instance of the `MarkdownSaveOptions` class and specify it in the `editor.save()` method.

If the document intended for saving in Markdown format has images, they should be handled in a way similar to that described above. But there is no callback for saving the images here. Instead, you have three choices:

1. **Ignore images**: They will be absent in the output.
2. **Save images inside the Markdown code**: Images will be stored in base64 encoding within the Markdown document.
3. **Save images as separate files**: Images are saved in the specified folder, and the Markdown code contains references to these image files.

To do this, the `MarkdownSaveOptions` class has several properties:

- `setExportImagesAsBase64(boolean value)`: If set to `true`, the content of the images will be embedded inside the output Markdown as base64. If this flag is set to `true`, it has the highest priority, and the `setImagesFolder()` property is ignored.
- `setImagesFolder(String path)`: Specifies the path to the folder where images should be saved. This property works when `setExportImagesAsBase64(false)` is set.

The example below shows opening an input DOCX file for editing and saving it into three different Markdown output files, each with its own saving properties.

```javascript
// Import necessary modules
const fs = require('fs');
const path = require('path');
const groupdocsEditor = require('groupdocs-editor');

const filename = 'SampleDoc.docx';
const inputPath = path.join('some-path', filename);

const outputFolder = 'path/to/output/folder';

const outputMdEmbeddedFilePath = path.join(outputFolder, 'Output_Markdown_Embedded.md');
const outputMdExternalFilePath = path.join(outputFolder, 'Output_Markdown_External.md');
const outputMdAbsentFilePath = path.join(outputFolder, 'Output_Markdown_Absent.md');

const editor = new groupdocsEditor.Editor(inputPath);
const beforeEdit = editor.edit();
// Send content to client-side WYSIWYG editor, edit it there, send back to server-side
const afterEdit = groupdocsEditor.EditableDocument.fromMarkup(beforeEdit.getEmbeddedHtml(), null);

// Saving to "embedded" version, where all images are embedded in MD with base64 encoding
{
    const mdSaveOptionsEmbedded = new groupdocsEditor.MarkdownSaveOptions();
    mdSaveOptionsEmbedded.setExportImagesAsBase64(true);
    editor.save(afterEdit, outputMdEmbeddedFilePath, mdSaveOptionsEmbedded);
}

// Saving to "external" version, where images are stored as separate files, and MD contains links to these files
{
    const mdSaveOptionsExternal = new groupdocsEditor.MarkdownSaveOptions();
    mdSaveOptionsExternal.setImagesFolder(outputFolder);
    editor.save(afterEdit, outputMdExternalFilePath, mdSaveOptionsExternal);
}

// Saving to "absent" version, where images are skipped and not present in MD
{
    const mdSaveOptionsAbsent = new groupdocsEditor.MarkdownSaveOptions();
    // Using an in-memory buffer to avoid saving images
    const outputMdAbsentTemp = [];
    editor.save(afterEdit, (data) => outputMdAbsentTemp.push(data), mdSaveOptionsAbsent);
    const outputMdAbsentContent = Buffer.concat(outputMdAbsentTemp);
    fs.writeFileSync(outputMdAbsentFilePath, outputMdAbsentContent);
}

// Dispose of resources
beforeEdit.dispose();
afterEdit.dispose();
editor.dispose();
```

In this example, for the "absent" version, the document content is saved to an in-memory buffer and then written to a file. This approach avoids GroupDocs.Editor's default behavior of saving images when a file stream is provided.

## Roundtrip

Because the Markdown format is supported on import and export, it is possible to perform a roundtrip scenario with it—open a Markdown file for editing, edit it, and then save the edited version to the Markdown format again. The example below demonstrates such a scenario.

```javascript
// Import necessary modules
const fs = require('fs');
const path = require('path');
const groupdocsEditor = require('groupdocs-editor');

// Define the custom image loader class
class MdImageLoader {
    constructor(imagesFolder) {
        this.imagesFolder = imagesFolder;
    }

    // Implement the processImage method
    processImage(args) {
        const imagePath = path.join(this.imagesFolder, path.basename(args.getImageFileName()));
        const imageData = fs.readFileSync(imagePath);
        args.setData(imageData);
        return groupdocsEditor.MarkdownImageLoadingAction.UserProvided;
    }
}

function markdownRoundtrip() {
    const inputFolderPath = 'path/to/input/folder';
    const outputFolder = 'path/to/output/folder';
    const outputMdPath = path.join(outputFolder, 'Output.md');

    const filename = 'ComplexImage.md';
    const inputPath = path.join(inputFolderPath, filename);

    const editOptions = new groupdocsEditor.MarkdownEditOptions();
    editOptions.setImageLoadCallback(new MdImageLoader(inputFolderPath));

    const saveOptions = new groupdocsEditor.MarkdownSaveOptions();
    saveOptions.setTableContentAlignment(groupdocsEditor.MarkdownTableContentAlignment.Center);
    saveOptions.setImagesFolder(outputFolder);

    const editor = new groupdocsEditor.Editor(inputPath);

    const doc = editor.edit(editOptions);

    console.log('Number of images:', doc.getImages().length);

    // Edit 'doc' in WYSIWYG editor and obtain its edited version
    // For demonstration, we use the same document
    const editedDoc = doc;

    editor.save(editedDoc, outputMdPath, saveOptions);

    // Dispose of resources
    doc.dispose();
    editor.dispose();
}

// Call the function
markdownRoundtrip();
```

In this example, we perform a roundtrip: we load a Markdown document with images, edit it, and save it back to Markdown format, ensuring that images are correctly handled.

---

By following this guide, you can effectively edit Markdown documents using GroupDocs.Editor for Node.js via Java, customizing the editing process according to your specific needs.

**Note:** Be sure to replace the paths in the code examples with the actual paths on your system.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.
