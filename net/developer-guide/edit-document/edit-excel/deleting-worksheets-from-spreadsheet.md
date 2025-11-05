---
id: deleting-worksheets-from-spreadsheet
url: editor/net/deleting-worksheets-from-spreadsheet
title: Deleting worksheets from spreadsheet
weight: 3
description: "This article describes the new feature of the GroupDocs.Editor for .NET version 25.11 - deleting (removing) one or many worksheets from the loaded and edited spreadsheet (workbook) during its saving to the output format"
keywords: Edit Excel, delete tab from Excel workbook, remove tab from Excel workbook, delete worksheet from spreadsheet, remove worksheet from spreadsheet
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
A **spreadsheet**, also known as a **workbook** — is a [family of the document formats](https://docs.fileformat.com/spreadsheet/), designed to work with tabular data. [XLS](https://docs.fileformat.com/spreadsheet/xls/), [XLSX](https://docs.fileformat.com/spreadsheet/xlsx/), [ODS](https://docs.fileformat.com/spreadsheet/ods/), [CSV](https://docs.fileformat.com/spreadsheet/csv/) formats are the most common examples of such document formats, while Microsoft Excel, LibreOffice Calc, Apache OpenOffice Calc are examples of table processors — programs, which allow the creation and editing of such documents. GroupDocs.Editor has an ability to edit existing spreadsheet documents from its beginning, and starting from the [version 24.6](https://releases.groupdocs.com/editor/net/release-notes/2024/groupdocs-editor-for-net-24-6-release-notes/) it has obtained an ability not only to edit already existing spreadsheets, but also to create new spreadsheets from scratch. And starting from the [version 25.11](https://releases.groupdocs.com/editor/net/release-notes/2025/groupdocs-editor-for-net-25-11-release-notes/) the GroupDocs.Editor has obtained a feature to delete worksheets from the edited spreadsheet during saving.

Need to keep in mind that not every spreadsheet may have the worksheets. For example, text-based separator-delimited formats like [CSV](https://docs.fileformat.com/spreadsheet/csv/) and [TSV](https://docs.fileformat.com/spreadsheet/tsv/) are basically the text files, they have no worksheets, so the worksheet cannot be removed from them. But almost all of the binary spreadsheet formats like XLS, XLSX and ODS do have them.

In GroupDocs.Editor the user edits one worksheet at a time — the spreadsheet [is loaded](https://docs.groupdocs.com/editor/net/load-document/) to the constructor of the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class, then worksheet is specified by its number in the [`SpreadsheetEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheeteditoptions), and then document is converted to the editable form, represented by the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class. Straight and simple. However, when the content of the worksheet was edited by the user and passed back to the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class, there are two options:

1. Users can save the edited content of the worksheet as a new spreadsheet with this single worksheet inside. This is the default behaviour that was present in the GroupDocs.Editor from its beginning.
2. Users can save the edited worksheet by inserting it to the original spreadsheet. This was made possible starting from the [version 20.10](https://releases.groupdocs.com/editor/net/release-notes/2020/groupdocs-editor-for-net-20-10-release-notes/) and is described in a [separate article](https://docs.groupdocs.com/editor/net/inserting-edited-worksheet-into-existing-spreadsheet/). This second option also has two sub-options:
    1. The edited worksheet can *replace* the original worksheet in the input spreadsheet. For instance, in the loaded spreadsheet with 3 worksheets the user has chosen the 2nd one for edit; this worksheet was edited and then inserted back to the input spreadsheet, replacing the original 2nd worksheet onto the edited one.
    2. The edited worksheet can be inserted into the input spreadsheet to stay *together* with the original one. For instance, in the loaded spreadsheet with 3 worksheets the user has chosen the 2nd one for edit; this worksheet was edited and then inserted back to the input spreadsheet, so now the spreadsheet contains 4 worksheets: the first, fourth, and two versions of the second (original and edited).

Starting from the [version 25.11](https://releases.groupdocs.com/editor/net/release-notes/2025/groupdocs-editor-for-net-25-11-release-notes/), when the 2nd option (inserting edited worksheet into the original spreadsheet), users also can delete particular worksheet(s) from it. A new property was added to the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions) class — `WorksheetNumbersToDelete` of `int[]` type. By default it has a `null` value, which means that no worksheets should be removed. However, when it has one or more valid worksheet numbers, the worksheets with these numbers are removed while calling the [`Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method.

Need to mention that the worksheet numbers in a `WorksheetNumbersToDelete` property are 1-based. For instance, for removing the first and fourth worksheets the `WorksheetNumbersToDelete` property should have a `new int[2] {1, 4}` value.

With the new worksheet removal feature the GroupDocs.Editor performs the next algorithm during saving the spreadsheet:

1. User edits a content of the worksheet in the WYSIWYG-editor, passes the edited content to the instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class [using one of its static methods](https://docs.groupdocs.com/editor/net/create-editabledocument-from-file-or-markup/), creates and adjusts the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions), and passes the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance, [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions) instance, and output stream or file path for writing to the [`Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method.
2. If the value of the [`WorksheetNumber`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property in the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions) instance is set to the default value `0`, the GroupDocs.Editor generates a new spreadsheet with one worksheet. The `WorksheetNumbersToDelete` property is ignored regardless of its value.
3. If the value of the [`WorksheetNumber`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property in the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions) instance has some non-zero value, the GroupDocs.Editor treats it as a command to insert the edited worksheet into the original spreadsheet.
4. Depending on the [`InsertAsNewWorksheet`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/) property, the GroupDocs.Editor replaces old worksheet, specified by the value of the [`WorksheetNumber`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property, on the new one, taken from the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance, or puts the new worksheet to be placed among existing worksheets without replacing any of them.
5. Finally, when all worksheets insertions and rearrangements are finished, the GroupDocs.Editor reads the value of the `WorksheetNumbersToDelete` property, iterates over all numbers and removes specified worksheets from the spreadsheet.
6. The final spreadsheet, with updated and deleted worksheets, is written to the output stream or file.

Example below shows full roundtrip of input XLSX file: spreadsheet with 3 worksheets inside is loaded, its second worksheet is converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument), then HTML-markup is emitted from the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance, edited, and then edited HTML-markup is converted back to the another [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance. From this point two instances of the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions) class are created — one is intended for generating a first version of an output spreadsheet with a new worksheet and without deletion and finally it has 4 worksheets in total (3 original + 1 edited). The second [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheetsaveoptions) instance has the WorksheetNumbersToDelete property set to remove the first and last worksheets, so the 2nd version of an output spreadsheet will have 2 worksheets in total (1 original + 1 edited).

Input XLSX as well as two versions of the output XLSX files may be downloaded here:

- [Input3Worksheets.xlsx](/editor/net/sample-files/Input3Worksheets.xlsx)
- [Output4Worksheets-without-delete.xlsx](/editor/net/sample-files/Output4Worksheets-without-delete.xlsx)
- [Output2Worksheets-with-delete.xlsx](/editor/net/sample-files/Output2Worksheets-with-delete.xlsx)

{{< tabs "DeleteWorksheetsFromLoadedAndEditedSpreadsheet">}}
{{< tab "C#" >}}
```csharp
using GroupDocs.Editor;
using GroupDocs.Editor.Formats;
using GroupDocs.Editor.Options;
// ...

// Load input spreadsheet to the Editor and specify loading options
using (Editor editor = new Editor("Input3Worksheets.xlsx", new SpreadsheetLoadOptions()))
{
    // Prepare edit options and set 2nd worksheet to edit
    SpreadsheetEditOptions editOptions = new SpreadsheetEditOptions();
    editOptions.WorksheetIndex = 1;//2nd worksheet to edit, here WorksheetIndex is 0-based due to historical reasons

    //generate EditableDocument with original content of 2nd worksheet
    using (EditableDocument worksheet2OpenedForEdit = editor.Edit(editOptions))
    {
        // Get the HTML-markup from the EditableDocument with original content
        string originalHtmlContentOf2ndWorksheet = worksheet2OpenedForEdit.GetEmbeddedHtml();
        
        //emulate HTML content editing in WYSIWYG-editor in browser or somewhere else
        string editedHtmlContentOf2ndWorksheet = originalHtmlContentOf2ndWorksheet.Replace("2nd row", "Edited 2nd row at 1st column");

        //generate EditableDocument with edited content of 2nd worksheet
        using (EditableDocument worksheet2AfterEdit = EditableDocument.FromMarkup(editedHtmlContentOf2ndWorksheet))
        {
            //prepare save options without deletions
            SpreadsheetSaveOptions saveOptionsWithoutDelete = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx);
            // let it be the 2nd worksheet...
            saveOptionsWithoutDelete.WorksheetNumber = 2;//here WorksheetNumber is 1-based
            //... and we also save the original 2nd worksheet, which is pushed to the 3rd position
            saveOptionsWithoutDelete.InsertAsNewWorksheet = true;

            // So now the spreadsheet must have 4 worksheets, not 3. Save it to file
            editor.Save(worksheet2AfterEdit, "Output4Worksheets-without-delete.xlsx", saveOptionsWithoutDelete);

            // Create another save options, with deletions at this time
            SpreadsheetSaveOptions saveOptionsWithDelete = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx);
            // let the new worksheet will be 2nd for now, but after deleting it will became the 1st later
            saveOptionsWithDelete.WorksheetNumber = 2;

            //... and we also save the original 2nd worksheet, which will be next sibling to the newly inserted
            saveOptionsWithDelete.InsertAsNewWorksheet = true;

            // delete the 1st and last worksheet
            saveOptionsWithDelete.WorksheetNumbersToDelete = new int[2]
            {
                1,//1st worksheet
                4//last worksheet is 4, not 3, because we inserted the edited 2nd worksheet as the new worksheet instance, without rewriting the original 2nd worksheet
            };
            // Save it again to distinct file. Output XLSX should have 2 worksheets now.
            editor.Save(worksheet2AfterEdit, "Output2Worksheets-with-delete.xlsx", saveOptionsWithDelete);
        }
    }
}
```
{{< /tab >}}
{{< tab "VB.NET">}}
```vb
Imports GroupDocs.Editor
Imports GroupDocs.Editor.Formats
Imports GroupDocs.Editor.Options
' ...

' Load input spreadsheet to the Editor and specify loading options
Using editor As New Editor("Input3Worksheets.xlsx", New SpreadsheetLoadOptions())
    ' Prepare edit options and set 2nd worksheet to edit
    Dim editOptions As New SpreadsheetEditOptions()
    editOptions.WorksheetIndex = 1 '2nd worksheet to edit, here WorksheetIndex is 0-based due to historical reasons

    ' generate EditableDocument with original content of 2nd worksheet
    Using worksheet2OpenedForEdit As EditableDocument = editor.Edit(editOptions)
        ' Get the HTML-markup from the EditableDocument with original content
        Dim originalHtmlContentOf2ndWorksheet As String = worksheet2OpenedForEdit.GetEmbeddedHtml()

        ' emulate HTML content editing in WYSIWYG-editor in browser or somewhere else
        Dim editedHtmlContentOf2ndWorksheet As String = originalHtmlContentOf2ndWorksheet.Replace("2nd row", "Edited 2nd row at 1st column")

        ' generate EditableDocument with edited content of 2nd worksheet
        Using worksheet2AfterEdit As EditableDocument = EditableDocument.FromMarkup(editedHtmlContentOf2ndWorksheet)
            ' prepare save options without deletions
            Dim saveOptionsWithoutDelete As New SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx)

            ' let it be the 2nd worksheet...
            saveOptionsWithoutDelete.WorksheetNumber = 2 'here WorksheetNumber is 1-based
            '... and we also save the original 2nd worksheet, which is pushed to the 3rd position
            saveOptionsWithoutDelete.InsertAsNewWorksheet = True

            ' So now the spreadsheet must have 4 worksheets, not 3. Save it to file
            editor.Save(worksheet2AfterEdit, "Output4Worksheets-without-delete.xlsx", saveOptionsWithoutDelete)

            ' Create another save options, with deletions at this time
            Dim saveOptionsWithDelete As New SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx)
            ' let the new worksheet will be 2nd for now, but after deleting it will became the 1st later
            saveOptionsWithDelete.WorksheetNumber = 2
            '... and we also save the original 2nd worksheet, which will be next sibling to the newly inserted
            saveOptionsWithDelete.InsertAsNewWorksheet = True

            ' delete the 1st and last worksheet
            saveOptionsWithDelete.WorksheetNumbersToDelete = New Int32(1) {
                1, '1st worksheet
                4 'last worksheet is 4, not 3, because we inserted the edited 2nd worksheet as the new worksheet instance, without rewriting the original 2nd worksheet
            }
            ' Save it again to distinct file. Output XLSX should have 2 worksheets now.
            editor.Save(worksheet2AfterEdit, "Output2Worksheets-with-delete.xlsx", saveOptionsWithDelete)
        End Using
    End Using
End Using
```
{{< /tab >}}
{{< /tabs >}}

