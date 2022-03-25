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

Unlike WordProcessing and Spreadsheet documents, DSV documents are loaded into the [Editor](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class without any loading options. They are simple text files by their nature, so it is nothing to adjust:

```java
String inputCsvPath = "C://input//spreadsheet.csv";
Editor editor = new Editor(inputCsvPath);
```

## Editing CSV file

In order to open any DSV document for edit, user must use [DelimitedTextEditOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions) class, which single constructor has one mandatory parameter — string separator (delimiter), that should not be NULL or empty string. There are also several optional properties. Two properties — [ConvertDateTimeData](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions/properties/convertdatetimedata) and [ConvertNumericData](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions/properties/convertnumericdata) — are boolean flags, which indicate how to treat numbers. GroupDocs.Editor can recognize digits within cells and treat them as numbers or date-time values. By default this recognition is disabled, but user can turn it on. Next property — [TreatConsecutiveDelimitersAsOne](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions/properties/treatconsecutivedelimitersasone), — is also a boolean flag, which determines how the consecutive delimiters should be treated — as several (default - false) or as a single one (true).

Last property — [OptimizeMemoryUsage](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions/properties/optimizememoryusage), — is also a boolean flag, but with completely different purpose. By default GroupDocs.Editor algorithms are tuned for maximum performance for reducing the latency time; memory consumption has lower priority. However in some rare cases user may need to load and open very huge DSV, several hundreds of MiBs or even close to GiB. In such cases GroupDocs.Editor (Java in fact) may throw an OutOfMemoryException. For coping with such use-cases the [OptimizeMemoryUsage](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions/properties/optimizememoryusage) flag was introduced. By enabling it user switches GroupDocs.Editor to use another processing algorithms, which consume relatively low amount of memory at the cost of lower performance.

Example below demonstrates using the [DelimitedTextEditOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions) for editing CSV document, where comma character is a delimiter:

```java
DelimitedTextEditOptions editOptions = new DelimitedTextEditOptions(",");
editOptions.setConvertDateTimeData(false);
editOptions.setConvertNumericData(true);
editOptions.setTreatConsecutiveDelimitersAsOne(true);
editOptions.setOptimizeMemoryUsage(true);

EditableDocument document = editor.edit(editOptions);
```

## Save edited CSV file

After being edited, input DSV can be saved back to DSV (not necessary with the same separator) or to any supportable Spreadsheet document. In order to save document to DSV format user must use the [DelimitedTextSaveOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions) class, which, like the [DelimitedTextEditOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtexteditoptions), has one constructor with mandatory string parameter — separator (delimiter), that should not be NULL or empty string.

There are also other properties:

1. [Encoding](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions/properties/encoding). Allows to specify the encoding of generated DSV. By default, if not specified, is UTF8.
2. [TrimLeadingBlankRowAndColumn](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions/properties/trimleadingblankrowandcolumn). Boolean flag, that indicates whether leading blank rows and columns should be trimmed like what MS Excel does.
3. [KeepSeparatorsForBlankRow](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/delimitedtextsaveoptions/properties/keepseparatorsforblankrow). Boolean flag, that indicates whether separators should be output for blank row. Default value is false which means the content for blank row will be empty.

Example below demonstrates loading CSV, opening it to the [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance, and saving to TSV and XLSM.

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
