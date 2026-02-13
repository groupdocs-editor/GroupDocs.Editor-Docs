---
id: deleting-worksheets-from-spreadsheet
url: editor/java/deleting-worksheets-from-spreadsheet
title: Deleting worksheets from spreadsheet
weight: 3
description: "This article describes the new feature of the GroupDocs.Editor for Java version 26.1 - deleting (removing) one or many worksheets from the loaded and edited spreadsheet (workbook) during its saving to the output format"
keywords: Edit Excel, delete tab from Excel workbook, remove tab from Excel workbook, delete worksheet from spreadsheet, remove worksheet from spreadsheet
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
A **spreadsheet**, also known as a **workbook** — is a [family of the document formats](https://docs.fileformat.com/spreadsheet/), designed to work with tabular data. [XLS](https://docs.fileformat.com/spreadsheet/xls/), [XLSX](https://docs.fileformat.com/spreadsheet/xlsx/), [ODS](https://docs.fileformat.com/spreadsheet/ods/), [CSV](https://docs.fileformat.com/spreadsheet/csv/) formats are the most common examples of such document formats, while Microsoft Excel, LibreOffice Calc, Apache OpenOffice Calc are examples of table processors — programs, which allow the creation and editing of such documents. GroupDocs.Editor has an ability to edit existing spreadsheet documents from its beginning, and starting from the [version 24.6](https://releases.groupdocs.com/editor/net/release-notes/2024/groupdocs-editor-for-net-24-6-release-notes/) it has obtained an ability not only to edit already existing spreadsheets, but also to create new spreadsheets from scratch. And starting from the [version 26.1](https://releases.groupdocs.com/editor/java/release-notes/2026/groupdocs-editor-for-java-26-1-release-notes/) the GroupDocs.Editor has obtained a feature to delete worksheets from the edited spreadsheet during saving.

Need to keep in mind that not every spreadsheet may have the worksheets. For example, text-based separator-delimited formats like [CSV](https://docs.fileformat.com/spreadsheet/csv/) and [TSV](https://docs.fileformat.com/spreadsheet/tsv/) are basically the text files, they have no worksheets, so the worksheet cannot be removed from them. But almost all of the binary spreadsheet formats like XLS, XLSX and ODS do have them.

In GroupDocs.Editor the user edits one worksheet at a time — the spreadsheet [is loaded](https://docs.groupdocs.com/editor/java/load-document/) to the constructor of the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class, then worksheet is specified by its number in the [`SpreadsheetEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheeteditoptions/), and then document is converted to the editable form, represented by the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) class. Straight and simple. However, when the content of the worksheet was edited by the user and passed back to the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) class, there are two options:

1. Users can save the edited content of the worksheet as a new spreadsheet with this single worksheet inside. This is the default behaviour that was present in the GroupDocs.Editor from its beginning.
2. Users can save the edited worksheet by inserting it to the original spreadsheet. This was made possible starting from the [version 20.10](https://releases.groupdocs.com/editor/net/release-notes/2020/groupdocs-editor-for-net-20-10-release-notes/) and is described in a [separate article](https://docs.groupdocs.com/editor/java/inserting-edited-worksheet-into-existing-spreadsheet/). This second option also has two sub-options:
    1. The edited worksheet can *replace* the original worksheet in the input spreadsheet. For instance, in the loaded spreadsheet with 3 worksheets the user has chosen the 2nd one for edit; this worksheet was edited and then inserted back to the input spreadsheet, replacing the original 2nd worksheet onto the edited one.
    2. The edited worksheet can be inserted into the input spreadsheet to stay *together* with the original one. For instance, in the loaded spreadsheet with 3 worksheets the user has chosen the 2nd one for edit; this worksheet was edited and then inserted back to the input spreadsheet, so now the spreadsheet contains 4 worksheets: the first, fourth, and two versions of the second (original and edited).

Starting from the [version 26.1](https://releases.groupdocs.com/editor/java/release-notes/2026/groupdocs-editor-for-java-26-1-release-notes/), when the 2nd option (inserting edited worksheet into the original spreadsheet), users also can delete particular worksheet(s) from it. A new property was added to the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) class — [`WorksheetNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setWorksheetNumbersToDelete-int---) of `int[]` type. By default it has a `null` value, which means that no worksheets should be removed. However, when it has one or more valid worksheet numbers, the worksheets with these numbers are removed while calling the [`Editor.save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#save-java.io.OutputStream-) method.

Need to mention that the worksheet numbers in a [`WorksheetNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setWorksheetNumbersToDelete-int---) property are 1-based. For instance, for removing the first and fourth worksheets the [`WorksheetNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setWorksheetNumbersToDelete-int---) property should have a `new int[2] {1, 4}` value.

With the new worksheet removal feature the GroupDocs.Editor performs the next algorithm during saving the spreadsheet:

1. User edits a content of the worksheet in the WYSIWYG-editor, passes the edited content to the instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) class [using one of its static methods](https://docs.groupdocs.com/editor/java/create-editabledocument-from-file-or-markup/), creates and adjusts the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/), and passes the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instance, [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) instance, and output stream or file path for writing to the [`Editor.save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#save-java.io.OutputStream-) method.
2. If the value of the [`WorksheetNumber`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setWorksheetNumber-int-) property in the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) instance is set to the default value `0`, the GroupDocs.Editor generates a new spreadsheet with one worksheet. The [`WorksheetNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setWorksheetNumbersToDelete-int---) property is ignored regardless of its value.
3. If the value of the [`WorksheetNumber`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setWorksheetNumber-int-) property in the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) instance has some non-zero value, the GroupDocs.Editor treats it as a command to insert the edited worksheet into the original spreadsheet.
4. Depending on the [`InsertAsNewWorksheet`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setInsertAsNewWorksheet-boolean-) property, the GroupDocs.Editor replaces old worksheet, specified by the value of the [`WorksheetNumber`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setWorksheetNumber-int-) property, on the new one, taken from the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instance, or puts the new worksheet to be placed among existing worksheets without replacing any of them.
5. Finally, when all worksheet insertions and rearrangements are finished, the GroupDocs.Editor reads the value of the [`WorksheetNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/#setWorksheetNumbersToDelete-int---) property, iterates over all numbers and removes specified worksheets from the spreadsheet.
6. The final spreadsheet, with updated and deleted worksheets, is written to the output stream or file.

Example below shows full roundtrip of input XLSX file: spreadsheet with 3 worksheets inside is loaded, its second worksheet is converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/), then HTML-markup is emitted from the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instance, modified, and then edited HTML-markup is converted back to the another [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instance. From this point two instances of the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) class are created — one is intended for generating a first version of an output spreadsheet with a new worksheet and without deletion and finally it has 4 worksheets in total (3 original + 1 edited). The second [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheetsaveoptions/) instance has the WorksheetNumbersToDelete property set to remove the first and last worksheets, so the 2nd version of an output spreadsheet will have 2 worksheets in total (1 original + 1 edited).

{{< tabs "code-example">}}
{{< tab "DeleteWorksheetsFromLoadedAndEditedSpreadsheet.java" >}}  
```java
import com.groupdocs.editor.EditableDocument;
import com.groupdocs.editor.Editor;
import com.groupdocs.editor.examples.Constants;
import com.groupdocs.editor.formats.SpreadsheetFormats;
import com.groupdocs.editor.options.SpreadsheetEditOptions;
import com.groupdocs.editor.options.SpreadsheetLoadOptions;
import com.groupdocs.editor.options.SpreadsheetSaveOptions;

public class DeleteWorksheetsFromLoadedAndEditedSpreadsheet {
    public static void edit() {
	
        SpreadsheetLoadOptions loadOptions = new SpreadsheetLoadOptions();

        Editor editor = new Editor("Input3Worksheets.xlsx", loadOptions);
        // Prepare edit options: open 2nd worksheet (WorksheetIndex is 0-based)
        SpreadsheetEditOptions editOptions = new SpreadsheetEditOptions();
        editOptions.setWorksheetIndex(1);

        try {

            EditableDocument worksheet2OpenedForEdit = editor.edit(editOptions);
            // Get embedded HTML of the worksheet
            String originalHtml = worksheet2OpenedForEdit.getEmbeddedHtml();

            // Emulate HTML edit
            String editedHtml = originalHtml.replace("2nd row", "Edited 2nd row at 1st column");

            // Build EditableDocument from edited markup
            try {
                EditableDocument worksheet2AfterEdit = EditableDocument.fromMarkup(editedHtml, null);
                // ---- Save #1: insert edited worksheet as new, without deleting anything
                SpreadsheetSaveOptions saveOptionsWithoutDelete = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx);
                saveOptionsWithoutDelete.setWorksheetNumber(2);          // 1-based
                saveOptionsWithoutDelete.setInsertAsNewWorksheet(true);  // don't overwrite, insert new
                String outputWorksheets_without_delete = Constants.getOutputDirectoryPath("Output4Worksheets-without-delete.xlsx");
                editor.save(
                        worksheet2AfterEdit,
                        outputWorksheets_without_delete,
                        saveOptionsWithoutDelete
                );

                // ---- Save #2: insert edited worksheet as new, and delete 1st + last worksheet
                SpreadsheetSaveOptions saveOptionsWithDelete = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx);
                saveOptionsWithDelete.setWorksheetNumber(2);            // 1-based
                saveOptionsWithDelete.setInsertAsNewWorksheet(true);

                // After insertion, total worksheets become 4 => delete #1 and #4
                saveOptionsWithDelete.setWorksheetNumbersToDelete(new int[]{1, 4});
                String outputWorksheets_with_delete = Constants.getOutputDirectoryPath("Output2Worksheets-with-delete.xlsx");
                editor.save(
                        worksheet2AfterEdit,
                        outputWorksheets_with_delete,
                        saveOptionsWithDelete
                );
            }finally {
                worksheet2OpenedForEdit.dispose();
            }
        }finally {
            editor.dispose();
        }
    }

    public static void main(String[] args){
        edit();
    }
}
```
{{< /tab >}}
{{< tab "Input3Worksheets.xlsxz" >}}  
{{< tab-text >}}
`Input3Worksheets.xlsx` is sample file used in this example. Click [here](/editor/java/sample-files/Input3Worksheets.xlsx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Output4Worksheets-without-delete.xlsx" >}}  
{{< tab-text >}}
`Output4Worksheets-without-delete.xlsx` is edited document without deleted worksheets. Click [here](/editor/java/sample-files/Output4Worksheets-without-delete.xlsx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Output2Worksheets-with-delete.xlsx" >}}  
{{< tab-text >}}
`Output2Worksheets-with-delete.xlsx` is edited document with deleted worksheets. Click [here](/editor/java/sample-files/Output2Worksheets-with-delete.xlsx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}
