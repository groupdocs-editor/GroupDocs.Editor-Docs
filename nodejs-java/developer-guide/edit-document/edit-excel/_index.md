---
id: edit-excel
url: url: editor/nodejs-java/edit-excel
title: Edit Excel Spreadsheet in Node.js
weight: 46
description: "This guide demonstrates how to edit XLS, XLSX, XLSM, XLSB, ODS, SXC spreadsheets with hidden worksheets, protect edited spreadsheets with passwords, and many other powerful features of GroupDocs.Editor for Node.js."
keywords: Edit Excel, Edit spreadsheet, Edit XLS, Edit XLSX, Edit XLSM, Edit XLSB, Edit ODS, Edit SXC
productName: GroupDocs.Editor for Node.js
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit Excel workbooks in GroupDocs.Editor
        description: How to edit Excel workbooks using GroupDocs.Editor in Node.js
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to edit the content of Excel workbooks in GroupDocs.Editor for Node.js
        description: Learn how to edit the content of worksheets from Excel workbooks using GroupDocs.Editor in Node.js step by step
        steps:
        - name: Load the desired Excel workbook into the Editor class
          text: Create an instance of the Editor class by passing the desired Excel workbook into it.
        - name: Prepare SpreadsheetEditOptions
          text: Create an instance of SpreadsheetEditOptions and adjust its properties to suit your needs if necessary.
        - name: Select the desired worksheet (tab) for editing
          text: Use SpreadsheetEditOptions to select the desired worksheet (tab) for editing by specifying the worksheet index using the "setWorksheetIndex()" method.
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke Editor.edit, specify the prepared SpreadsheetEditOptions, and obtain an instance of EditableDocument, which is ready for editing. Generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server
          text: Edit the content of the worksheet in the HTML-based WYSIWYG-editor on the client-side (web browser), then submit the edited content and resources back to the server, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of EditableDocument by passing the edited worksheet content into the appropriate static methods of the class.
        - name: Prepare SpreadsheetSaveOptions
          text: Create an instance of SpreadsheetSaveOptions, adjust its properties, and choose the format of the output workbook — this is the only mandatory parameter that must be specified. Using "setWorksheetNumber()" and "setInsertAsNewWorksheet()", choose how to insert the edited worksheet into the output workbook — replace the original or inject a new edited worksheet.
        - name: Save the edited Excel workbook with Editor.save
          text: Pass an instance of EditableDocument with the content of the edited Excel workbook, an instance of SpreadsheetSaveOptions, and a destination file path or stream to the Editor.save method to save the workbook.
---

## Introduction

Spreadsheet documents come in various formats: XLS, XLSX, XLSM, XLSB, ODS, SXC, and others, all of which are supported by **[GroupDocs.Editor](https://products.groupdocs.com/editor/nodejs-java/)**. Each format requires separate load, edit, and save options. GroupDocs.Editor distinguishes between spreadsheet formats and delimiter-separated files like CSV, TSV, or whitespace-separated documents, which have their own set of options.

Spreadsheet documents have a crucial distinction from word processing or text-based documents — they contain worksheets (tabs) instead of pages. These tabs are independent of each other and cannot be represented simultaneously in an HTML markup. Therefore, only one tab can be edited at a time in a WYSIWYG HTML-editor. After editing a tab, the edited content must be saved before switching to another tab. 

Multi-tabbed spreadsheets can be loaded into the **Editor** class, but an instance of **EditableDocument** will only contain the content of one tab. To edit multiple tabs, the **Editor.edit()** method must be invoked for each tab, returning a separate **EditableDocument** for each.

Spreadsheet documents may have password protection, and GroupDocs.Editor supports this feature.

## Loading Spreadsheet Documents

To load a spreadsheet into the **Editor** class, use the **SpreadsheetLoadOptions** class. While this is not always necessary, it allows you to specify additional options such as passwords for protected spreadsheets.

Example for loading a password-protected spreadsheet:

```javascript
const groupdocs = require('groupdocs-editor');
const SpreadsheetLoadOptions = groupdocs.options.SpreadsheetLoadOptions;
const Editor = groupdocs.Editor;

const inputXlsxPath = 'C://input/spreadsheet.xlsx';
const loadOptions = new SpreadsheetLoadOptions();
loadOptions.password = 'password';
const editor = new Editor(inputXlsxPath, loadOptions);
```

There are several scenarios here:
1. If a password is specified but the document is not password-protected, the password will be ignored.
2. If the document is password-protected but the password is not specified, an error will be thrown during editing.
3. If the document is password-protected but the provided password is incorrect, an error will also be thrown during editing.

## Editing Spreadsheet Documents

To edit a spreadsheet, use the **SpreadsheetEditOptions** class. By default, the first tab will be opened for editing unless another is specified. You can set the tab for editing by its 0-based index and specify whether to exclude hidden worksheets.

Example:

```javascript
const SpreadsheetEditOptions = groupdocs.options.SpreadsheetEditOptions;

const editOptions1 = new SpreadsheetEditOptions();
editOptions1.worksheetIndex = 0;  // Index is 0-based, so this is the 1st tab
editOptions1.excludeHiddenWorksheets = true;

const editOptions2 = new SpreadsheetEditOptions();
editOptions2.worksheetIndex = 1;  // This is the 2nd tab
editOptions2.excludeHiddenWorksheets = false;

const firstTab = editor.edit(editOptions1);
const secondTab = editor.edit(editOptions2);
```

## Saving Spreadsheet Documents

Use the **SpreadsheetSaveOptions** class to save the edited spreadsheet. Its constructor requires one mandatory parameter: the format of the output document, represented by the **SpreadsheetFormats** structure. You can also specify a password for the output document or set write protection.

Example:

```javascript
const SpreadsheetSaveOptions = groupdocs.options.SpreadsheetSaveOptions;
const SpreadsheetFormats = groupdocs.formats.SpreadsheetFormats;

const saveOptions = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsm);
saveOptions.password = 'new password';
saveOptions.worksheetProtection = {
    protectionType: 'All',
    protectionPassword: 'write password'
};

editor.save(editableDocument, saveOptions, 'C://output/edited-spreadsheet.xlsm');
```