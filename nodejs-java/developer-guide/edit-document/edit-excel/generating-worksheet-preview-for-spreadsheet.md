---
id: generating-worksheets-preview-for-spreadsheet
url: editor/nodejs-java/generating-worksheets-preview-for-spreadsheet
title: Generating worksheets (tabs) preview for spreadsheet in Node.js
weight: 2
description: "This article describes how to generate a preview for any worksheet (tab) from an existing Excel spreadsheet in SVG format using GroupDocs.Editor for Node.js."
keywords: preview worksheet tab excel spreadsheet SVG
productName: GroupDocs.Editor for Node.js
hideChildren: False
toc: True

---


Starting from **[version 23.10](https://releases.groupdocs.com/editor/java/release-notes/2023/groupdocs-editor-for-java-23-10-release-notes/)**, GroupDocs.Editor for Node.js allows generating previews of any worksheet (also known as _tab_) within a spreadsheet document in SVG format. This feature enables users to view the content of the spreadsheet without editing it. The generated worksheet preview is non-editable using GroupDocs.Editor, but can be saved and viewed in any SVG-supporting software, such as browsers or desktop viewers.

This feature works in both trial and licensed modes without restrictions and doesn't consume credits or bytes during generation. GroupDocs.Editor also supports generating SVG previews for hidden worksheets.

To generate worksheet previews for a spreadsheet in GroupDocs.Editor for Node.js, follow these steps:

- Load the spreadsheet file into the `Editor` class.
- Call the `getDocumentInfo()` method and provide a password if the spreadsheet is protected.
- Use the returned `SpreadsheetDocumentInfo` object and invoke the `generatePreview(worksheetIndex)` method by specifying the zero-based index of the worksheet. If the index is out of range, an error will be thrown.
- The `generatePreview(worksheetIndex)` method returns a worksheet preview as an SVG image encapsulated in the `SvgImage` class. This class provides methods to retrieve the SVG content, save it to disk, or stream it.

The following code demonstrates how to load an unprotected spreadsheet, get the number of worksheets, generate previews for all worksheets, and save these previews to disk.

```javascript
// Import GroupDocs.Editor classes
const groupdocs = require('groupdocs-editor');
const Editor = groupdocs.Editor;

// Define the path to the input spreadsheet
const inputPath = 'Sample.xlsx';

// Load the spreadsheet file into the Editor constructor
const editor = new Editor(inputPath, new SpreadsheetLoadOptions());

// Get document info for the spreadsheet
const documentInfo = editor.getDocumentInfo();

// Ensure the document is in XLSX format
if (documentInfo.format !== groupdocs.formats.SpreadsheetFormats.Xlsx) {
    throw new Error('Unsupported format, please use an XLSX file.');
}

// Cast the document info to SpreadsheetDocumentInfo
const spreadsheetInfo = documentInfo.asSpreadsheetDocumentInfo();

// Get the total number of worksheets
const worksheetCount = spreadsheetInfo.getPageCount();

// Loop through all worksheets and generate previews
for (let worksheetIndex = 0; worksheetIndex < worksheetCount; worksheetIndex++) {
    // Generate an SVG preview for the current worksheet
    const svgPreview = spreadsheetInfo.generatePreview(worksheetIndex);

    // Save the SVG preview to disk
    svgPreview.save(`Sample_out_image_${worksheetIndex}.svg`);
}
```

## Additional Details

The worksheet preview feature is provided through a new public method in the `SpreadsheetDocumentInfo` class. This method, `generatePreview(worksheetIndex)`, takes the worksheet index and returns an `SvgImage` object containing the SVG representation of the worksheet.

If the user prefers a raster format (such as PNG) instead of the SVG vector format, the `SvgImage` class offers the `saveToPng()` method, which converts the SVG content into a PNG image.

Example for saving a worksheet preview as a PNG:

```javascript
// Save the SVG preview as PNG
svgPreview.saveToPng(`Sample_out_image_${worksheetIndex}.png`);
```

This feature makes it easy to generate worksheet previews for inspection and display purposes without requiring any changes to the actual spreadsheet content.