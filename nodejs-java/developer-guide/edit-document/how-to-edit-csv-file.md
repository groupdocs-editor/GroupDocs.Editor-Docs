---

id: how-to-edit-csv-file
url: editor/nodejs-java/how-to-edit-csv-file
title: How to Edit CSV File
linkTitle: How to Edit CSV File
weight: 50
description: "This guide demonstrates how to edit CSV, TSV, comma-separated values, and other text files with different settings and many other powerful features of GroupDocs.Editor for Node.js via Java."
keywords: Edit CSV, Edit TSV, Edit Semicolon-separated value file, Edit Whitespace-separated value file
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to Edit CSV and All Other DSV Files Using GroupDocs.Editor
        description: How to edit documents in CSV and all other DSV formats using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java 
    showVideo: True
    howTo:
        name: How to Edit Content of Documents in CSV and All Other DSV Formats Using GroupDocs.Editor in Node.js
        description: Learn how to edit content of documents in DSV (Delimiter-Separated Values) formats, including CSV, TSV, and others using GroupDocs.Editor in Node.js step by step
        steps:
        - name: Load the Desired Delimiter-Separated Values (DSV) File into the Editor Class
          text: Create an instance of the Editor class using the most suitable constructor overload by passing the desired text file into it. LoadOptions are not needed.
        - name: Prepare a DelimitedTextEditOptions Class
          text: Create an instance of the DelimitedTextEditOptions class and adjust its properties to meet your needs if necessary. While creating the DelimitedTextEditOptions instance, you should specify a separator (delimiter) string (may be a single character), which is used in the loaded DSV. For example, for CSV it will be a comma character (","), for TSV—a tab character, and so on.
        - name: Call editor.edit() and Send the Obtained EditableDocument to the WYSIWYG Editor
          text: Invoke the editor.edit() method by specifying the previously prepared DelimitedTextEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML markup and extract resources from this instance using corresponding instance methods, and pass all this data to the HTML-based WYSIWYG editor.
        - name: Edit the Document in the WYSIWYG Editor and Send the Edited Content Back to the Server-Side
          text: Make all necessary edits in the content of the DSV spreadsheet in the HTML-based WYSIWYG editor, which is running on the client-side (in a web browser), and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an Instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited DSV content into the most suitable static methods of the class.
        - name: Prepare a DelimitedTextSaveOptions Class
          text: Create an instance of the DelimitedTextSaveOptions class and adjust its properties to meet your needs if necessary. While creating the DelimitedTextSaveOptions instance, you must choose the desired separator (delimiter) string. It may not be the same as the one specified in the DelimitedTextEditOptions before; select any desired string separator that you need.
        - name: Save the Edited DSV Spreadsheet with the editor.save() Method
          text: Pass an instance of EditableDocument with the content of the edited DSV spreadsheet, an instance of DelimitedTextSaveOptions, and a destination byte stream or file path to the editor.save() method for saving the DSV file.
---

> This guide explains all necessary aspects and options regarding processing DSV spreadsheets (Delimiter-Separated Values) using GroupDocs.Editor for Node.js via Java.

## Introduction

DSV (Delimiter-Separated Values) documents are a specific form of text-based spreadsheets that use delimiters (separators) to separate values. Due to their nature, **GroupDocs.Editor** processes this class of documents separately from usual binary spreadsheets. Unlike typical spreadsheets, DSV documents have only a single tab (worksheet) and cannot be encoded. However, any non-empty string may be treated as a separator, so you always need to specify it explicitly.

The most common types of DSV are:

1. **CSV (Comma-Separated Values)**
2. **TSV (Tab-Separated Values)**
3. **Semicolon-Separated Values**
4. **Whitespace-Separated Values**
5. **...and any other delimiter**

GroupDocs.Editor supports DSV with any separator, which can be a character or a set of characters (string).

## Loading a CSV File for Editing

Unlike WordProcessing and Spreadsheet documents, DSV documents are loaded into the `Editor` class without any loading options. They are simple text files by their nature, so there is nothing to adjust:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Specify the input CSV file path
const inputCsvPath = "C://input//spreadsheet.csv";

// Create an instance of the Editor
const editor = new groupdocsEditor.Editor(inputCsvPath);
```

## Editing a CSV File

To open any DSV document for editing, you must use the `DelimitedTextEditOptions` class, whose constructor has one mandatory parameter—a string separator (delimiter) that should not be null or an empty string. There are also several optional properties:

1. **convertDateTimeData**: Boolean flag indicating whether to recognize date-time values within cells. By default, this recognition is disabled.
2. **convertNumericData**: Boolean flag indicating whether to recognize numeric values within cells. By default, this recognition is disabled.
3. **treatConsecutiveDelimitersAsOne**: Boolean flag that determines how consecutive delimiters should be treated—as several (default `false`) or as a single one (`true`).
4. **optimizeMemoryUsage**: Boolean flag with a completely different purpose. By default, GroupDocs.Editor algorithms are tuned for maximum performance to reduce latency time; memory consumption has a lower priority. However, in some rare cases, you may need to load and open a very large DSV file, several hundreds of MBs or even close to a GB. In such cases, GroupDocs.Editor (or Java, in fact) may throw an OutOfMemoryException. To cope with such use-cases, the `optimizeMemoryUsage` flag was introduced. By enabling it, you switch GroupDocs.Editor to use alternative processing algorithms, which consume relatively low amounts of memory at the cost of lower performance.

The example below demonstrates using the `DelimitedTextEditOptions` for editing a CSV document where the comma character is the delimiter:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Prepare DelimitedTextEditOptions
const editOptions = new groupdocsEditor.DelimitedTextEditOptions(",");
editOptions.setConvertDateTimeData(false);
editOptions.setConvertNumericData(true);
editOptions.setTreatConsecutiveDelimitersAsOne(true);
editOptions.setOptimizeMemoryUsage(true);

// Open the document for editing
const document = editor.edit(editOptions);
```

## Saving the Edited CSV File

After being edited, the input DSV can be saved back to DSV (not necessarily with the same separator) or to any supported Spreadsheet document. To save the document to DSV format, you must use the `DelimitedTextSaveOptions` class, which, like the `DelimitedTextEditOptions`, has a constructor with a mandatory string parameter—the separator (delimiter) that should not be null or an empty string.

There are also other properties:

1. **encoding**: Allows specifying the encoding of the generated DSV. By default, if not specified, it is UTF-8.
2. **trimLeadingBlankRowAndColumn**: Boolean flag that indicates whether leading blank rows and columns should be trimmed, similar to what MS Excel does.
3. **keepSeparatorsForBlankRow**: Boolean flag that indicates whether separators should be output for a blank row. The default value is `false`, which means the content for a blank row will be empty.

The example below demonstrates loading a CSV file, opening it into an `EditableDocument` instance, and saving it to TSV and XLSM formats:

```javascript
// Import the necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Specify the input CSV file path
const inputCsvPath = "C://input//spreadsheet.csv";

// Create an instance of the Editor
const editor = new groupdocsEditor.Editor(inputCsvPath);

// Prepare DelimitedTextEditOptions
const editOptions = new groupdocsEditor.DelimitedTextEditOptions(",");
const document = editor.edit(editOptions);

// Prepare DelimitedTextSaveOptions for TSV
const tsvSaveOptions = new groupdocsEditor.DelimitedTextSaveOptions("\t");
tsvSaveOptions.setTrimLeadingBlankRowAndColumn(true);
tsvSaveOptions.setKeepSeparatorsForBlankRow(false);

// Prepare SpreadsheetSaveOptions for XLSM
const xlsmSaveOptions = new groupdocsEditor.SpreadsheetSaveOptions(groupdocsEditor.SpreadsheetFormats.Xlsm);

// Specify output paths
const tsvSavePath = "C://output//spreadsheet.tsv";
const xlsmSavePath = "C://output//spreadsheet.xlsm";

// Save the document as TSV
editor.save(document, tsvSavePath, tsvSaveOptions);

// Save the document as XLSM
editor.save(document, xlsmSavePath, xlsmSaveOptions);
```

In this example, the output file `'spreadsheet.tsv'` will have UTF-8 encoding.

---

By following this guide, you can effectively edit CSV and other DSV files using GroupDocs.Editor for Node.js via Java, customizing the editing process according to your specific needs.

> **Note:** Be sure to replace the paths in the code examples with the actual paths on your system.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.
