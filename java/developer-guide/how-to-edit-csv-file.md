---
id: how-to-edit-csv-file
url: editor/java/how-to-edit-csv-file
title: How to edit CSV file
linkTitle: How to edit CSV file
weight: 50
description: "This guide demonstrates how to edit CSV, TSV, comma-separated value and other text files with different settings and many other powerful features of GroupDocs.Editor for Java."
keywords: Edit CSV, Edit TSV, Edit Semicolon-separated value file, Edit Whitespace-separated value file
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit CSV and all other DSV files in the GroupDocs.Editor
        description: How to edit documents in CSV and all other DSV formats using the GroupDocs.Editor in Java language
        productCode: editor
        productPlatform: java 
    showVideo: True
    howTo:
        name: How to edit content of the documents in CSV and all other DSV formats in the GroupDocs.Editor in Java
        description: Learn how to edit content of the documents in DSV (Delimiter-Separated Values) formats, including CSV, TSV and all others using the GroupDocs.Editor in Java step by step
        steps:
        - name: Load desired Delimiter-Separated Values (DSV) file to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired  text file into it. LoadOptions are not needed.
        - name: Prepare a DelimitedTextEditOptions class
          text: Create an instance of the DelimitedTextEditOptions class and adjust its properties to meet your needs if necessary. While creating the DelimitedTextEditOptions instance, you should specify a separator (delimiter) string (may be a single character), which is used in the loaded DSV. For example, for the CSV it will be a comma character (","), for TSV — a tab character, and so on.
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.edit method with specifying a previously prepared DelimitedTextEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the content of the DSV spreadsheet in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited DSV content into the most suitable static methods of the class
        - name: Prepare a DelimitedTextSaveOptions class
          text: Create an instance of the DelimitedTextSaveOptions class and adjust its properties to meet your needs if necessary. While creating the DelimitedTextSaveOptions instance, you must choose the desired separator (delimiter) string. It may not be the same as it was specified in the DelimitedTextEditOptions before, select any desired string separator that you need.
        - name: Save edited DSV spreadhseet with Editor.save method
          text: Pass an instance of EditableDocument with content of the edited DSV spreadsheet, instance of the DelimitedTextSaveOptions, and a destination byte stream or file path to the Editor.save method for saving the DSV file.
---
> This demonstration shows and explains all necessary moments and options regarding processing DSV spreadsheets (Delimiter-separated values)

## Introduction

DSV (Delimiter-Separated Values) documents are specific form of text-based spreadsheets with delimiters (separators). Due of their nature the [**GroupDocs.Editor**](https://products.groupdocs.com/editor/java) processes this class of documents separately from usual binary spreadsheets. In counterpart to usual spreadsheets, DSV documents due to their textual nature have only single tab (worksheet) and cannot be encoded. But any non-empty string may be treated as separator, so user always need to specify it explicitly.
The most common types of DSV are:

1. CSV (Comma-Separated value)
2. TSV (Tab-Separated value)
3. Semicolon-separated value
4. Whitespace-separated value
5. ...and any other

GroupDocs.Editor supports DSV with any separator, which can be character or a set of characters (string).

## Loading CSV file for edit

Unlike WordProcessing and Spreadsheet documents, DSV documents are loaded into the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class without any loading options. They are simple text files by their nature, so it is nothing to adjust:

```java
String inputCsvPath = "C://input//spreadsheet.csv";
Editor editor = new Editor(inputCsvPath);
```

## Editing CSV file

In order to open any DSV document for edit, user must use [DelimitedTextEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions) class, which single constructor has one mandatory parameter — string separator (delimiter), that should not be NULL or empty string. There are also several optional properties. Two properties — [getConvertDateTimeData()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions#getConvertDateTimeData--) and [getConvertNumericData()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions#getConvertNumericData--) — are boolean flags, which indicate how to treat numbers. GroupDocs.Editor can recognize digits within cells and treat them as numbers or date-time values. By default this recognition is disabled, but user can turn it on. Next property — [getTreatConsecutiveDelimitersAsOne()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions#getTreatConsecutiveDelimitersAsOne--), — is also a boolean flag, which determines how the consecutive delimiters should be treated — as several (default - false) or as a single one (true).

Last property — [getOptimizeMemoryUsage()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions#getOptimizeMemoryUsage--), — is also a boolean flag, but with completely different purpose. By default GroupDocs.Editor algorithms are tuned for maximum performance for reducing the latency time; memory consumption has lower priority. However in some rare cases user may need to load and open very huge DSV, several hundreds of MiBs or even close to GiB. In such cases GroupDocs.Editor (Java in fact) may throw an OutOfMemoryException. For coping with such use-cases the [getOptimizeMemoryUsage()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions#getOptimizeMemoryUsage--) flag was introduced. By enabling it user switches GroupDocs.Editor to use another processing algorithms, which consume relatively low amount of memory at the cost of lower performance.

Example below demonstrates using the [DelimitedTextEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions) for editing CSV document, where comma character is a delimiter:

```java
DelimitedTextEditOptions editOptions = new DelimitedTextEditOptions(",");
editOptions.setConvertDateTimeData(false);
editOptions.setConvertNumericData(true);
editOptions.setTreatConsecutiveDelimitersAsOne(true);
editOptions.setOptimizeMemoryUsage(true);

EditableDocument document = editor.edit(editOptions);
```

## Save edited CSV file

After being edited, input DSV can be saved back to DSV (not necessary with the same separator) or to any supportable Spreadsheet document. In order to save document to DSV format user must use the [DelimitedTextSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions) class, which, like the [DelimitedTextEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions), has one constructor with mandatory string parameter — separator (delimiter), that should not be NULL or empty string.

There are also other properties:

1. [getEncoding()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions#getEncoding--). Allows to specify the encoding of generated DSV. By default, if not specified, is UTF8.
2. [getTrimLeadingBlankRowAndColumn()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions#getTrimLeadingBlankRowAndColumn--). Boolean flag, that indicates whether leading blank rows and columns should be trimmed like what MS Excel does.
3. [getKeepSeparatorsForBlankRow()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions#getKeepSeparatorsForBlankRow--). Boolean flag, that indicates whether separators should be output for blank row. Default value is false which means the content for blank row will be empty.

Example below demonstrates loading CSV, opening it to the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance, and saving to TSV and XLSM.

```java
String inputCsvPath = "C://input//spreadsheet.csv";
Editor editor = new Editor(inputCsvPath);

DelimitedTextEditOptions editOptions = new DelimitedTextEditOptions(",");
EditableDocument document = editor.edit(editOptions);

DelimitedTextSaveOptions tsvSaveOptions = new DelimitedTextSaveOptions("\t");
tsvSaveOptions.setTrimLeadingBlankRowAndColumn(true);
tsvSaveOptions.setKeepSeparatorsForBlankRow(false);

SpreadsheetSaveOptions xlsmSaveOptions = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsm);

String tsvSavePath = "C://output//spreadsheet.tsv";
String xlsmSavePath = "C://output//spreadsheet.xlsm";

editor.save(document, tsvSavePath, tsvSaveOptions);
editor.save(document, xlsmSavePath, xlsmSaveOptions);
```

In this example output "'spreadsheet.tsv" will have a UTF-8 encoding.
