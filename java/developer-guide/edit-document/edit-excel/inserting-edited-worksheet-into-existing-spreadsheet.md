---
id: inserting-edited-worksheet-into-existing-spreadsheet
url: editor/java/inserting-edited-worksheet-into-existing-spreadsheet
title: Inserting edited worksheet into existing spreadsheet
weight: 1
description: "This article describes the new feature of the GroupDocs.Editor for java version 20.11 - inserting an edited worksheet into existing spreadsheet"
keywords: Edit Excel, insert tab into Escel, insert worksheet into Excel, insert worksheet into spreadsheet
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
By default and from the moment when a Spreadsheet module was released to public, the full spreadsheet editing pipeline was the next:

1. Loading spreadsheet in a form of a file or stream into constructor of the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class.
2. Selecting a specific worksheet to edit and specifying its index in the [getWorksheetIndex()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheeteditoptions/#getWorksheetIndex()) property of [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheeteditoptions) class.
3. Opening a spreadsheet for editing by calling [Editor.edit()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) method, passing [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheeteditoptions) instance with selected worksheet into it, and obtaining [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance from this method.
4. Emitting HTML and CSS markup, which represents a content of an edited document, from [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance, using different methods like [getContent()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#getContent--).
5. Passing this HTML and CSS markup into the WYSIWYG editor, which is located on client-side and is running in the browser.
6. End-user edits the document content in the WYSIWYG editor.
7. Edited document content, in form of HTML and CSS markup, is passed back to the server-side.
8. Creating a new instance of the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class by passing the edited content, obtained from server, into the [fromFile()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromFile-java.lang.String-java.lang.String-), [fromMarkup()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromMarkup-java.lang.String-java.util.List-com.groupdocs.editor.htmlcss.resources.IHtmlResource--), or [fromMarkupAndResourceFolder()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromMarkupAndResourceFolder-java.lang.String-java.lang.String-) static methods (depending from content).
9. Creating an instance of [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) class with format of output spreadsheet file.
10. Calling an [Editor.save()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/#save-com.groupdocs.editor.EditableDocument-java.io.OutputStream-com.groupdocs.editor.options.ISaveOptions-) method by passing the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument), output stream for the document, and a [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) options, into it. This will generate a new spreadsheet, which contains only one single worksheet — those worksheet, that was edited on the client-side and which content was passed via [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class into this method.

Before GroupDocs.Editor version 20.11 this was the only pipeline, accessible for the customer.

Starting from the version 20.11, the last 10th step can be altered — edited worksheet can be inserted into original spreadsheet, which was loaded on the 1st step.

[SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) class now contains two new properties: integer [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) and boolean flag [getInsertAsNewWorksheet()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getInsertAsNewWorksheet--). Both of them have "usual" default values: `getWorksheetNumber()` has `0` and `getInsertAsNewWorksheet()` is set to `false`.

```java
public int getWorksheetNumber() { get; set; }
public boolean getInsertAsNewWorksheet() { get; set; }
```

By default, when these properties are not touched or at least [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) has a default value '0', the GroupDocs.Editor will generate new single-worksheet spreadsheet, as in previous versions. However, if [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) property contains number, distinct from '0', and valid spreadsheet is loaded into [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class (it is expected to be the original spreadsheet, which was edited, but it actually can be any spreadsheet, even those, which has no relation to the original), then edited worksheet will be **inserted** into given spreadsheet.

## WorksheetNumber property

[getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) property, if it is not a zero, defines, where, at which exact position in the given spreadsheet the new edited worksheet should be inserted. [getInsertAsNewWorksheet()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getInsertAsNewWorksheet--) parameter, which is a boolean flag, determines, how this worksheet should be inserted: should it replace the existing worksheet, that is located on specified position ('false', default value), or it should be injected between existing worksheets, without rewriting them, and thus increasing the total amount of worksheets in the given spreadsheet by one.

Because default `0` value of [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) parameter is reserved, actual worksheet numbering starts from `1`. This is different from widely used 0-based indexing, however, makes sense, thus it is not an index of a worksheet, but rather number of a worksheet, the same as MS Excel uses, for instance. This means that, for example, for given spreadsheet, that consists of 5 worksheets, 1st one has a '1' worksheet number, and 5th — `5`. If user has specified a worksheet number, which exceeds the total amount of worksheets in spreadsheet, this number will be automatically adjusted to the latest. This means that if, for example, for the same 5-worksheet spreadsheet the user will specify a '6', '7' or even 'Int32.MaxValue', it will be internally set a `5` — number of the latest worksheet.

Along with positive worksheet numbers, the [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) property also supports negative numbering, which implies count from the end of spreadsheet. In this case, `-1` is treated as the last worksheet, `-2` — the last but one, and so on. Again, like with positive numbering, in case when number exceeds the amount of worksheets, it will be adjusted to the closest — first worksheet for negative numbers.

Part of source code below explains this numbering system:

```java
SpreadsheetSaveOptions saveOptions = new SpreadsheetSaveOptions(Formats.SpreadsheetFormats.Xlsx)
  
//let's say we have a spreadsheet with 5 worksheets
  
saveOptions.setWorksheetNumber(0);  // default value, given spreadsheet will be ignored and new will be created
  
//positive numbering
saveOptions.setWorksheetNumber(1);  // first worksheet
saveOptions.setWorksheetNumber(2);  // second worksheet
saveOptions.setWorksheetNumber(3);  // third worksheet
saveOptions.setWorksheetNumber(4);  // fourth worksheet
saveOptions.setWorksheetNumber(5);  // fifth worksheet
saveOptions.setWorksheetNumber(6);  // fifth worksheet, because value '6' exceeds the worksheets amount '5' and thus is adjusted to the closest
  
//negative numbering
saveOptions.setWorksheetNumber(-1);  // fifth worksheet, which is first from end (last)
saveOptions.setWorksheetNumber(-2);  // fourth worksheet, which is second from end (last but one)
saveOptions.setWorksheetNumber(-3);  // third worksheet, which is third from end
saveOptions.setWorksheetNumber(-4);  // second worksheet, which is fourth from end
saveOptions.setWorksheetNumber(-5);  // first worksheet, which is fifth from end
saveOptions.setWorksheetNumber(-6);  // first worksheet, because value '-6' exceeds the worksheets amount '5' and thus is adjusted to the closest
```

## InsertAsNewWorksheet property

[getInsertAsNewWorksheet()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getInsertAsNewWorksheet--)  property complements the previously described [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) property and is ignored, when [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) is set to `0`. If [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--) points on the worksheet of specific number, the [getInsertAsNewWorksheet()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getInsertAsNewWorksheet--) determines how to treat this worksheet number and how to insert the worksheet:

* [getInsertAsNewWorksheet()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getInsertAsNewWorksheet--) property has default 'false' value, the existing worksheet in given spreadsheet will be completely erased, and the content of the new worksheet (which is located in the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance) will be putted on this place. As a result, a spreadsheet will preserve the same untouched amount of worksheets, but one of its worksheets (specified by the [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--)) will be replaced onto new one.
* If [getInsertAsNewWorksheet()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getInsertAsNewWorksheet--) property has 'true' value, the edited worksheet, obtained from [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance, will be injected among existing worksheets in given spreadsheet, so its amount of worksheets will be incremented by one. New worksheet is inserted at position, specified by [getWorksheetNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--), and all subsequent worksheets (following or preceding) will be shifted to the end or to the beginning accordingly, depending on positive or negative numbering.

Source code below shows, how worksheet number is treated when [getInsertAsNewWorksheet()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getInsertAsNewWorksheet--) property is enabled:

```java
SpreadsheetSaveOptions saveOptions = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx)
  
//let's say we have a spreadsheet with 5 worksheets
  
saveOptions.setWorksheetNumber(0);  // default value, given spreadsheet will be ignored, as well as InsertAsNewWorksheet
  
saveOptions.setInsertAsNewWorksheet(true);  //enabling the adding of worksheet instead of replacing
  
//positive numbering
saveOptions.setWorksheetNumber(1);  // new worksheet is injected as first, while all following (including 'old' 1st) are shifting to the end
saveOptions.setWorksheetNumber(2);  // new worksheet is injected as second, while 2nd, 3rh, 4th and 5th are shifting to the end
saveOptions.setWorksheetNumber(3);  // new worksheet is injected as third, while 3rh, 4th and 5th are shifting to the end
saveOptions.setWorksheetNumber(4);  // new worksheet is injected as fourth, while 4th and 5th are shifting to the end
saveOptions.setWorksheetNumber(5);  // new worksheet is injected as fifth, while 5th is shifting to the end and becomes 6th
saveOptions.setWorksheetNumber(6);  // new worksheet is injected as sixth, it already becomes the latest, none of existing worksheets are shifting to the end
saveOptions.setWorksheetNumber(7);  // same as previous
  
//negative numbering
saveOptions.setWorksheetNumber(-1);  // new worksheet is inject as first from end (it becomes sixth if starting from beginning), none of existing worksheets are shifting to the end
saveOptions.setWorksheetNumber(-2);  // new worksheet is inject as second from end (it becomes fifth if starting from beginning), following single worksheet is shifting to the end
saveOptions.setWorksheetNumber(-3);  // new worksheet is inject as third from end (it becomes fourth if starting from beginning), two following worksheets are shifting to the end
saveOptions.setWorksheetNumber(-4);  // new worksheet is inject as fourth from end (it becomes third if starting from beginning), three following worksheets are shifting to the end
saveOptions.setWorksheetNumber(-5);  // new worksheet is inject as fifth from end (it becomes second if starting from beginning), four following worksheets are shifting to the end
saveOptions.setWorksheetNumber(-6);  // new worksheet is inject as sixth from end (it becomes first if starting from beginning), five following worksheets are shifting to the end
saveOptions.setWorksheetNumber(-7);  // same as previous
```

### Additional notes

It is worth mentioning that the described new feature doesn't modify the original spreadsheet document, which was originally loaded into the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class through its constructor. When saving a spreadsheet and inserting the edited worksheet into existing spreadsheet, the GroupDocs.Editor creates a full and exact copy of the original document, and only then adds or replaces the worksheet onto the edited. So the original document is not touched in any case.

From this point it is clear that the GroupDocs.Editor cannot insert edited worksheet into existing spreadsheet document, if this document is not available. For example, original spreadsheet document was loaded into the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class, then opened for edit and stored somewhere for consequent editing. Then, in order to create an output spreadsheet from edited document, a new [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) instance was created from another document. In such case, if new loaded document is not a spreadsheet, the feature with inserting a worksheet will not work (because there is no source, into which the worksheet can be inserted). Also this means that for such scenario the "output" source spreadsheet may not be the same document as the "original" source spreadsheet. For example, it is absolutely legal and working scenario, when user initially loads a spreadsheet named "A" into the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class, edits (let's say) 2nd worksheet from it, then creates a new instance of the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class and loads another spreadsheet named "B" into it, and finally creates an output document from "B", where edited worksheet is injected on 5th position.

[SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) class contains properties for protecting a worksheet from editing: [getPassword()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getPassword--) and [getWorksheetProtection()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#getWorksheetNumber--). When this options are applied and inserting an edited worksheet into existing spreadsheet is applied too, the worksheet protection is applied only to the edited worksheet, which is inserting, — all other worksheets are untouched.
