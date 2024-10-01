---
id: generating-page-preview
url: editor/nodejs-java/generating-page-preview
title: Generating Page Preview for WordProcessing Documents
weight: 16
description: "Learn how to generate a preview for any page of an existing WordProcessing document in SVG format using GroupDocs.Editor for Node.js via Java."
keywords: preview page WordProcessing Word DOCX SVG
productName: GroupDocs.Editor for Node.js via Java
hideChildren: False
toc: True
---

Starting from [version 23.9](https://docs.groupdocs.com/editor/nodejs-java/groupdocs-editor-for-node-js-via-java-23-9-release-notes/), GroupDocs.Editor for Node.js via Java allows you to generate a preview for any page of a loaded WordProcessing document, such as DOC, DOCX, DOCM, RTF, ODT, and more. The preview is generated in SVG format, a vector graphics format supported by numerous image viewers and modern browsers.

This feature enables users to view and inspect any page of the imported WordProcessing document without actually editing it. The generated preview cannot be edited by GroupDocs.Editor itself; it can only be obtained in SVG format and saved as needed—to a byte stream or file.

This feature works regardless of the licensing mode of GroupDocs.Editor for Node.js via Java: it functions the same for both trial and licensed modes, and there are no trial limitations for this feature. While generating page previews, GroupDocs.Editor doesn’t consume any credits or impose any limitations.

With this addition, GroupDocs.Editor for Node.js via Java now supports generating previews for all three major office format families: WordProcessing, [Spreadsheet](https://docs.groupdocs.com/editor/nodejs-java/generating-worksheets-preview-for-spreadsheet/), and [Presentation](https://docs.groupdocs.com/editor/nodejs-java/generating-slides-preview-for-presentation/).

## Steps to Generate Page Previews

To generate page previews for a particular WordProcessing document, follow these steps:

1. **Load the WordProcessing File:**
   Load the desired WordProcessing file into the `Editor` class.

2. **Retrieve Document Information:**
   Call the `getDocumentInfo()` method. If the document is password-protected, specify the password.

3. **Cast to WordProcessingDocumentInfo:**
   Since the loaded file is a WordProcessing document (like DOCX, RTF, etc.), the `getDocumentInfo()` method returns an instance of the `WordProcessingDocumentInfo` class. Cast the returned `IDocumentInfo` to `WordProcessingDocumentInfo`.

4. **Generate Page Preview:**
   Use the `generatePreview(pageIndex)` method on the `WordProcessingDocumentInfo` instance, specifying a zero-based page index. If the index is out of range, an exception will be thrown.

5. **Save or Use the SVG Preview:**
   The `generatePreview(pageIndex)` method returns a page preview as an SVG image, encapsulated in the `SvgImage` class. Use the methods provided by this class to obtain the SVG content, save it to disk, stream it, etc.

## Code Sample

The following code sample demonstrates how to open an unprotected WordProcessing DOCX file, obtain the total number of pages, generate previews for each page, and save these previews to disk. Note that even in trial mode, GroupDocs.Editor generates previews for all pages without any trial watermarks.

```javascript
// Import necessary modules
const fs = require('fs');
const groupdocs_editor = require('groupdocs-editor');

// Obtain a valid full path to the WordProcessing file
const inputPath = "SampleDoc.docx";

// Load the WordProcessing file into the Editor
const editor = new groupdocs_editor.Editor(inputPath);

// Get document info for this file
const infoUncasted = editor.getDocumentInfo();

// Ensure it is a WordProcessing document (e.g., DOCX format)
if (infoUncasted.getFormat() === groupdocs_editor.WordProcessingFormats.Docx) {

    // Cast document info to WordProcessingDocumentInfo type
    const infoWordProcessing = infoUncasted;

    // Get the number of pages
    const pagesCount = infoWordProcessing.getPageCount();

    // Iterate through all pages and generate a preview for each
    for (let pageIndex = 0; pageIndex < pagesCount; pageIndex++) {

        // Generate one preview as an SVG image by page index
        const oneSvgPreview = infoWordProcessing.generatePreview(pageIndex);

        // Save the SVG preview to a file
        const outputFilePath = `outputFolder/Page_${pageIndex + 1}.svg`;
        fs.writeFileSync(outputFilePath, oneSvgPreview.getContent());

        console.log(`Page ${pageIndex + 1} preview saved to ${outputFilePath}`);
    }
} else {
    console.error("The provided document is not a WordProcessing format.");
}
```

**Note:** Make sure to replace `"SampleDoc.docx"` with the actual path to your WordProcessing document and `"outputFolder"` with the desired output directory.

## Handling Raster Images in Previews

WordProcessing formats can contain raster images. When generating an SVG preview for a page that includes raster images, these images are embedded inside the SVG using base64 encoding via the [data URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme).

## Converting SVG to PNG

If you need the page preview in a raster format instead of SVG, you can convert the SVG to PNG using the `saveToPng()` method available in the `SvgImage` class. This method converts the SVG content to PNG format and saves it to the specified output stream.

```javascript
// Continue from the previous code sample

// Generate one preview as an SVG image by page index
const oneSvgPreview = infoWordProcessing.generatePreview(pageIndex);

// Save the SVG preview as a PNG image
const pngOutputFilePath = `outputFolder/Page_${pageIndex + 1}.png`;
const pngStream = fs.createWriteStream(pngOutputFilePath);
oneSvgPreview.saveToPng(pngStream);
pngStream.end();

console.log(`Page ${pageIndex + 1} preview saved as PNG to ${pngOutputFilePath}`);
```

## Conclusion

The new page preview feature adds a valuable capability to GroupDocs.Editor for Node.js via Java, allowing you to generate and utilize previews of individual pages within WordProcessing documents. This can be particularly useful for applications that require document thumbnails, page-specific views, or any scenario where a visual representation of the document's pages is needed without loading the entire document into an editor.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.