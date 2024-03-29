---
id: edit-excel
url: editor/java/edit-excel
title: Edit Excel Spreadsheet
weight: 46
description: "This guide demonstrates how to edit XLS, XLSX, XLSM, XLSB, ODS, SXC spreadsheets with hidden worksheets, protect edited spreadsheet with password and many other powerful features of GroupDocs.Editor for Java."
keywords: Edit Excel, Edit spreadsheet, Edit XLS, Edit XLSX, Edit XLSM, Edit XLSB, Edit ODS, Edit SXC
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit Excel workbook in the GroupDocs.Editor
        description: How to edit Excel workbook using the GroupDocs.Editor in Java language
        productCode: editor
        productPlatform: net 
    showVideo: True
    howTo:
        name: How to edit content of the Excel workbook in the GroupDocs.Editor in Java
        description: Learn how to edit content of the worksheets from the Excel workbook using the GroupDocs.Editor in Java step by step
        steps:
        - name: Load desired Excel workbook to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired  Excel workbook into it.
        - name: Prepare a SpreadsheetEditOptions class
          text: Create an instance of the SpreadsheetEditOptions class and adjust its properties to meet your needs if necessary.
        - name: Select desired worksheet (tab) for editing
          text: Using the SpreadsheetEditOptions you should select the desired worksheet (tab), that should be edited, using the "setWorksheetIndex()" method.
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.Edit method with specifying a previously prepared SpreadsheetEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the content of the worksheet in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited worksheet content into the most suitable static methods of the class
        - name: Prepare a SpreadsheetSaveOptions class
          text: Create an instance of the SpreadsheetSaveOptions class and adjust its properties to meet your needs if necessary. You need to choose the format of the output workbook — this is the only mandatory parameter, that must be specified in the constructor. Also using the "setWorksheetNumber()" and "setInsertAsNewWorksheet()" methods you can choose how to insert the edited worksheet into the output workbook — replace the original worksheet with the edited one, or inject a new edited worksheet to keep it along with old original simultaneously.
        - name: Save edited Excel workbook with Editor.save method
          text: Pass an instance of EditableDocument with content of the edited Excel workbook, instance of the SpreadsheetSaveOptions, and a destination byte stream or file path to the Editor.Save method for saving the workbook.
---
> This demonstration shows and explains all necessary moments and options regarding processing the Spreadsheet documents.

## Introduction

Spreadsheet documents are presented by many formats: XLS, XLSX, XLSM, XLSB, ODS, SXC and other, which are supported by **[GroupDocs.Editor](https://products.groupdocs.com/editor/java)**. There are separate load options, edit options and save options for all Spreadsheet documents. Please note that GroupDocs.Editor distinguish Spreadsheet documents from textual Delimiter-Separated documents (DSV) like CSV, TSV, semicolon-separated, whitespace-separated and so on. Such document types have a distinct set of options and are not reviewed in this article.

Spreadsheet documents have one critically important distinction compared to WordProcessing or textual documents — they have tabs (also called worksheets) instead of pages, and these tabs are separate and independent from each other. Tabs have no valid and correct representation in HTML markup. This means that WYSIWYG HTML-editors don't allow to edit multi-tabbed spreadsheet at one moment, — it is allowed to edit only one tab per time. Save edited tab, and only then switch to another one. Because GroupDocs.Editor is designed for editing documents on client-side, it should be consistent with such approach. As a result, multi-tabbed Spreadsheet document can be loaded to the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class, but [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance stores a content of only one tab. In other words, user loads multi-tabbed spreadsheet to the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class, but for editing two tabs, for example, he should call the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor).[edit()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) method two times: first one for the first tab and second invoke for the second tab. As a result, with these two calls user obtains two [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instances — first one contains first tab, and the second one — second.

As WordProcessing documents, Spreadsheet documents can have password-protection and can be protected from editing; GroupDocs.Editor supports all these features.

## Loading Spreadsheet documents

In order to load spreadsheet to the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class, user should use the [SpreadsheetLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetloadoptions) class. It is not necessary in some general cases — even without [SpreadsheetLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetloadoptions) the GroupDocs.Editor is able to recognize spreadsheet format and apply appropriate default load options automatically. But when spreadsheet is encoded, the [SpreadsheetLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetloadoptions) is the only way to set a password and load the document properly.
Example below shows such situation: loading the password-protected spreadsheet:

```java
String inputXlsxPath = "C://input//spreadsheet.xlsx";
SpreadsheetLoadOptions loadOptions = new SpreadsheetLoadOptions();
loadOptions.setPassword("password");
Editor editor = new Editor(inputXlsxPath, loadOptions);
```

There can be several scenarios here:

1. If password is specified, but document is not password-protected, the password will be ignored.
2. If document is password-protected, but password is not specified, the [PasswordRequiredException](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/passwordrequiredexception) will be thrown later during editing.
3. If document is password-protected,and password is specified, but is incorrect, the [IncorrectPasswordException](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/incorrectpasswordexception) will be thrown later during editing.

## Editing Spreadsheet documents

[SpreadsheetEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheeteditoptions) should be used for editing Spreadsheet documents. If parameterless overload of the `Editor.edit()` method is used, the first tab will be opened for editing. [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheeteditoptions) allows to specify the tab for opening by 0-based index and a boolean flag that indicates whether it is necessary to process hidden worksheets. This is demonstrated below:

```java
SpreadsheetEditOptions editOptions1 = new SpreadsheetEditOptions();
editOptions1.setWorksheetIndex(0); //index is 0-based, so this is 1st tab
editOptions1.setExcludeHiddenWorksheets(true);

SpreadsheetEditOptions editOptions2 = new SpreadsheetEditOptions();
editOptions2.setWorksheetIndex(1); //index is 0-based, so this is 2nd tab
editOptions2.setExcludeHiddenWorksheets(false);

EditableDocument firstTab = editor.edit(editOptions1);
EditableDocument secondTab = editor.edit(editOptions2);
```

## Saving Spreadsheet documents

[SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) class is designed for saving the Spreadsheet documents. Its constructor contains one mandatory parameter: document type, that is represented by the [SpreadsheetFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/spreadsheetformats) struct. With this option class user can also specify password; in such case output document will be encoded. Another possibility — to set the write protection for the document, also with the password. All these features are shown in the code below:

```java
SpreadsheetSaveOptions saveOptions = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsm);
saveOptions.setPassword("new password");
saveOptions.setWorksheetProtection(new WorksheetProtection(WorksheetProtectionType.All, "write password"));
```
