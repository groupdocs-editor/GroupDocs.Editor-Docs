---
id: working-with-formats
url: editor/java/working-with-formats
title: Working with formats
weight: 80
description: "This article explains document formats and format families supported by GroupDocs.Editor for Java and how to operate them in Java code."
keywords: GroupDocs.Editor supported formats, editable document formats
productName: GroupDocs.Editor for Java
hideChildren: False
---
> This article describes classes and interfaces of [**GroupDocs.Editor**](https://products.groupdocs.com/editor/java), which represent all supportable family formats and individual formats.

GroupDocs.Editor supports different document formats, all of them are conditionally divided onto several family formats:

1. WordProcessing formats, which includes DOC, DOCX, DOCM, RTF, ODT etc.
2. Spreadsheet formats, which includes XLS, XLT, XLSX, XLSM, XLTX, ODS etc.
3. Delimiter-Separated Values (DSV) formats, also known as delimited text, that are text-based form of spreadsheets, and includes CSV, TSV, semicolon-delimited, whitespace-delimited etc.
4. Presentation formats, which includes PPT, PPS, POT, PPTX, PPTM etc.
5. Text-based formats, which includes TXT, HTML, XML etc.
6. Fixed-layout formats, which includes only PDF at this time (for version 19.12).

For representing some of these format families the [Formats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/)namespace and [IDocumentFormat](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/idocumentformat) interface exists, along with four inheritors of this interface:

1. [WordProcessingFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/wordprocessingformats), which is common for all WordProcessing formats.
2. [SpreadsheetFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/spreadsheetformats), which is common for all Spreadsheet formats.
3. [PresentationFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/presentationformats), which is common for all Presentation formats.
4. [TextualFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/textualformats), which is common for all text-based formats, including plain text (TXT), markup formats (XML and HTML), and all Delimiter-Separated Values (DSV) formats.

All these types are immutable structs, all of them have text properties, which reflect their names and file extensions, all of them are equatable. Every struct has a set of static fields, each one represents one specific format within given family format. For every struct there is a `FromExtension` static method, that obtains a file extension, parses it and returns an instance of appropriate format. Every struct has an `All` static field, which allows enumeration over all specific formats within given family format.

Example below demonstrates all main features for the [WordProcessingFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/wordprocessingformats) struct; same is applicable for all other structs.

```java
//fetching one format
WordProcessingFormats dotm = WordProcessingFormats.Dotm;
System.out.println(String.format("DOT Macro-enabled: Name is {0}, extension is {1}", dotm.getName(), dotm.getExtension()));
//iterating over all formats within WordProcessing family
for (WordProcessingFormats oneFormat : (Iterable<WordProcessingFormats>) WordProcessingFormats.All) {
    System.out.println(String.format("Name is {0}, extension is {1}", oneFormat.getName(), oneFormat.getExtension()));
}
//Parsing from extension
WordProcessingFormats expectedDocm = WordProcessingFormats.fromExtension(".docm");
```
