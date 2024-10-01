---
id: inserting-edited-worksheet-into-existing-spreadsheet
url: editor/nodejs-java/inserting-edited-worksheet-into-existing-spreadsheet
title: Inserting edited worksheet into existing spreadsheet in Node.js
weight: 1
description: "This article describes how to insert an edited worksheet into an existing spreadsheet using GroupDocs.Editor for Node.js."
keywords: Edit Excel, insert tab into Excel, insert worksheet into Excel, insert worksheet into spreadsheet
productName: GroupDocs.Editor for Node.js
hideChildren: False
toc: True

---

## Overview

Starting from version 24.6, GroupDocs.Editor for Node.js introduces the ability to insert an edited worksheet into an existing spreadsheet, changing the default behavior of overwriting the entire workbook with a new single worksheet. This feature provides more flexibility in managing Excel spreadsheets when editing individual worksheets.

### Default Editing Pipeline

the typical workflow for editing a spreadsheet was:

1. Load the spreadsheet into the `Editor` class using a file or stream.
2. Specify the worksheet to edit by setting its index in the `SpreadsheetEditOptions`.
3. Use the `Editor.edit()` method to get the `EditableDocument` containing the HTML and CSS representation of the selected worksheet.
4. Send the HTML/CSS content to a client-side WYSIWYG editor.
5. Once editing is complete, retrieve the modified HTML/CSS content from the client-side and pass it back to the server.
6. Create an instance of `EditableDocument` with the modified content.
7. Use `SpreadsheetSaveOptions` to configure how the spreadsheet should be saved.
8. Call `Editor.save()` to save the changes, creating a new spreadsheet that contains the edited worksheet only.

### New Feature: Inserting Edited Worksheet

This process can be enhanced by inserting the edited worksheet into the original spreadsheet instead of generating a new one with only the edited worksheet. 

The `SpreadsheetSaveOptions` class has two new properties:

- `worksheetNumber`: This defines the position where the edited worksheet should be inserted.
- `insertAsNewWorksheet`: A boolean flag that determines whether to replace an existing worksheet or insert the edited worksheet as a new one.

```javascript
let worksheetNumber = 1; // Set worksheet number
let insertAsNewWorksheet = true; // Insert the edited worksheet as a new one
```

If `worksheetNumber` is `0`, a new single-worksheet spreadsheet will be created. However, if a valid spreadsheet is loaded and `worksheetNumber` is greater than `0`, the edited worksheet will be inserted into the existing spreadsheet.

## WorksheetNumber Property

The `worksheetNumber` property determines where in the spreadsheet the new worksheet will be inserted. Worksheet numbering starts at `1`, similar to how Excel numbers worksheets, not the zero-based index typically used in programming.

If the number exceeds the number of worksheets in the spreadsheet, it will be adjusted to the last worksheet.

```javascript
let saveOptions = new SpreadsheetSaveOptions();
saveOptions.worksheetNumber = 2; // Insert into second worksheet
```

Additionally, negative numbers can be used to count from the end of the spreadsheet, where `-1` is the last worksheet, `-2` is the second to last, and so on. 

Example with positive and negative numbering:

```javascript
saveOptions.worksheetNumber = -1; // Insert as last worksheet
saveOptions.worksheetNumber = 5;  // Insert into fifth worksheet (or adjust if fewer exist)
```

## InsertAsNewWorksheet Property

The `insertAsNewWorksheet` property determines whether to replace the worksheet at the specified position or insert a new one.

- If `insertAsNewWorksheet` is `false`, the worksheet at the specified position is replaced by the edited one.
- If `insertAsNewWorksheet` is `true`, the edited worksheet is inserted, and existing worksheets are shifted accordingly.

```javascript
saveOptions.insertAsNewWorksheet = true;  // Enable inserting the edited worksheet as new
```

For example, inserting a worksheet in various positions:

```javascript
saveOptions.worksheetNumber = 3;  // Insert at the third worksheet
saveOptions.insertAsNewWorksheet = true;  // Keep existing worksheets and insert new one
```

### Example Code for Node.js

```javascript
const groupdocs = require('groupdocs-editor');
const Editor = groupdocs.Editor;
const SpreadsheetSaveOptions = groupdocs.options.SpreadsheetSaveOptions;
const SpreadsheetEditOptions = groupdocs.options.SpreadsheetEditOptions;

// Load the existing spreadsheet into the Editor
const editor = new Editor('Sample.xlsx');

// Prepare options for editing the spreadsheet
const editOptions = new SpreadsheetEditOptions();
editOptions.worksheetIndex = 1;  // Edit the second worksheet (0-based index)

// Get the editable document (HTML/CSS)
const editableDocument = editor.edit(editOptions);

// Simulate editing the document and retrieving it back as HTML
const editedContent = '<html>...edited content...</html>';
const resources = [];

// Create a new EditableDocument instance with the edited content
const updatedDocument = EditableDocument.fromMarkup(editedContent, resources);

// Prepare save options
const saveOptions = new SpreadsheetSaveOptions();
saveOptions.worksheetNumber = 2;  // Insert as second worksheet
saveOptions.insertAsNewWorksheet = true;  // Insert as new worksheet

// Save the updated spreadsheet
editor.save(updatedDocument, 'UpdatedSample.xlsx', saveOptions);
```

## Additional Notes

This new feature doesn't modify the original spreadsheet loaded into the `Editor` class. When the edited worksheet is inserted into the spreadsheet, the original document is copied, and the worksheet is added to the copy. Thus, the original spreadsheet remains unchanged.

It’s also important to note that inserting an edited worksheet requires that the loaded document is a valid spreadsheet. The `worksheetNumber` and `insertAsNewWorksheet` properties must be properly configured for the feature to work as expected.
