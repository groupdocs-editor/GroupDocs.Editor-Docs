---
id: deleting-worksheets-from-spreadsheet
url: editor/python-net/deleting-worksheets-from-spreadsheet
title: Deleting worksheets from spreadsheet
linkTitle: Deleting worksheets from spreadsheet
weight: 3
description: "This article describes the feature of the GroupDocs.Editor for Python via .NET - deleting (removing) one or many worksheets from the loaded and edited spreadsheet (workbook) during its saving to the output format"
keywords: Edit Excel, delete tab from Excel workbook, remove tab from Excel workbook, delete worksheet from spreadsheet, remove worksheet from spreadsheet, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
A **spreadsheet**, also known as a **workbook** — is a [family of the document formats](https://docs.fileformat.com/spreadsheet/), designed to work with tabular data. [XLS](https://docs.fileformat.com/spreadsheet/xls/), [XLSX](https://docs.fileformat.com/spreadsheet/xlsx/), [ODS](https://docs.fileformat.com/spreadsheet/ods/), [CSV](https://docs.fileformat.com/spreadsheet/csv/) formats are the most common examples of such document formats, while Microsoft Excel, LibreOffice Calc, Apache OpenOffice Calc are examples of table processors — programs, which allow the creation and editing of such documents. GroupDocs.Editor has an ability to edit existing spreadsheet documents, to create new spreadsheets from scratch, and also to delete worksheets from the edited spreadsheet during saving.

Need to keep in mind that not every spreadsheet may have the worksheets. For example, text-based separator-delimited formats like [CSV](https://docs.fileformat.com/spreadsheet/csv/) and [TSV](https://docs.fileformat.com/spreadsheet/tsv/) are basically the text files, they have no worksheets, so the worksheet cannot be removed from them. But almost all of the binary spreadsheet formats like XLS, XLSX and ODS do have them.

In GroupDocs.Editor the user edits one worksheet at a time — the spreadsheet [is loaded](https://docs.groupdocs.com/editor/python-net/load-document/) to the constructor of the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class, then worksheet is specified by its number in the [`SpreadsheetEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheeteditoptions), and then document is converted to the editable form, represented by the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class. Straight and simple. However, when the content of the worksheet was edited by the user and passed back to the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class, there are two options:

1. Users can save the edited content of the worksheet as a new spreadsheet with this single worksheet inside. This is the default behaviour that was present in the GroupDocs.Editor from its beginning.
2. Users can save the edited worksheet by inserting it to the original spreadsheet. This is described in a [separate article](https://docs.groupdocs.com/editor/python-net/inserting-edited-worksheet-into-existing-spreadsheet/). This second option also has two sub-options:
    1. The edited worksheet can *replace* the original worksheet in the input spreadsheet. For instance, in the loaded spreadsheet with 3 worksheets the user has chosen the 2nd one for edit; this worksheet was edited and then inserted back to the input spreadsheet, replacing the original 2nd worksheet onto the edited one.
    2. The edited worksheet can be inserted into the input spreadsheet to stay *together* with the original one. For instance, in the loaded spreadsheet with 3 worksheets the user has chosen the 2nd one for edit; this worksheet was edited and then inserted back to the input spreadsheet, so now the spreadsheet contains 4 worksheets: the first, fourth, and two versions of the second (original and edited).

When the 2nd option (inserting edited worksheet into the original spreadsheet) is used, users also can delete particular worksheet(s) from it. The [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) class has a [`worksheet_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumberstodelete/) property of `list[int]` type. By default it has a `None` value, which means that no worksheets should be removed. However, when it has one or more valid worksheet numbers, the worksheets with these numbers are removed while calling the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.

Need to mention that the worksheet numbers in a [`worksheet_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumberstodelete/) property are 1-based. For instance, for removing the first and fourth worksheets the [`worksheet_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumberstodelete/) property should have a `[1, 4]` value.

With the worksheet removal feature the GroupDocs.Editor performs the next algorithm during saving the spreadsheet:

1. User edits a content of the worksheet in the WYSIWYG-editor, passes the edited content to the instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class [using one of its static methods](https://docs.groupdocs.com/editor/python-net/create-editabledocument-from-file-or-markup/), creates and adjusts the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions), and passes the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) instance, and output stream or file path for writing to the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.
2. If the value of the [`worksheet_number`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property in the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) instance is set to the default value `0`, the GroupDocs.Editor generates a new spreadsheet with one worksheet. The [`worksheet_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumberstodelete/) property is ignored regardless of its value.
3. If the value of the [`worksheet_number`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property in the [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) instance has some non-zero value, the GroupDocs.Editor treats it as a command to insert the edited worksheet into the original spreadsheet.
4. Depending on the [`insert_as_new_worksheet`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/) property, the GroupDocs.Editor replaces old worksheet, specified by the value of the [`worksheet_number`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property, on the new one, taken from the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, or puts the new worksheet to be placed among existing worksheets without replacing any of them.
5. Finally, when all worksheet insertions and rearrangements are finished, the GroupDocs.Editor reads the value of the [`worksheet_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumberstodelete/) property, iterates over all numbers and removes specified worksheets from the spreadsheet.
6. The final spreadsheet, with updated and deleted worksheets, is written to the output stream or file.

The example below shows a full roundtrip of an input XLSX file: a spreadsheet is loaded, its first worksheet is converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), then HTML-markup is emitted from the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, modified, and the edited HTML-markup is converted back to another [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance. This edited worksheet is then inserted into the original spreadsheet, while the [`worksheet_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumberstodelete/) property is set to remove the original second worksheet during saving.

{{< tabs "code-example-deleting-worksheets-from-spreadsheet">}}
{{< tab "deleting_worksheets_from_spreadsheet.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.options import SpreadsheetLoadOptions, SpreadsheetEditOptions, SpreadsheetSaveOptions
from groupdocs.editor.formats import SpreadsheetFormats

def deleting_worksheets_from_spreadsheet():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load input spreadsheet to the Editor and specify loading options
    with Editor("./sample-spreadsheet.xlsx", SpreadsheetLoadOptions()) as editor:
        # Prepare edit options and set the 1st worksheet to edit
        edit_options = SpreadsheetEditOptions()
        edit_options.worksheet_index = 0  # index is 0-based, so this is the 1st worksheet

        # Generate EditableDocument with the original content of the 1st worksheet
        worksheet_opened_for_edit = editor.edit(edit_options)

        # Get the HTML-markup from the EditableDocument with the original content
        original_html = worksheet_opened_for_edit.get_embedded_html()

        # Emulate HTML content editing in a WYSIWYG-editor in the browser or somewhere else
        edited_html = original_html.replace("</body>", "<p>Edited content</p></body>")

        # Generate EditableDocument with the edited content
        worksheet_after_edit = EditableDocument.from_markup(edited_html)

        # Prepare save options that delete a worksheet during saving
        save_options = SpreadsheetSaveOptions(SpreadsheetFormats.XLSX)
        # A non-zero worksheet_number is required so the spreadsheet is rebuilt and deletions are applied
        save_options.worksheet_number = 1  # 1-based; insert the edited worksheet at the 1st position
        save_options.insert_as_new_worksheet = True  # keep the edited worksheet alongside the original ones
        save_options.worksheet_numbers_to_delete = [1]  # delete the 1st original worksheet (1-based)

        # Save the spreadsheet with the deleted worksheet
        editor.save(worksheet_after_edit, "./edited-spreadsheet.xlsx", save_options)

        worksheet_opened_for_edit.dispose()
        worksheet_after_edit.dispose()

    print("Saved spreadsheet with a deleted worksheet to edited-spreadsheet.xlsx")

if __name__ == "__main__":
    deleting_worksheets_from_spreadsheet()
```
{{< /tab >}}
{{< tab "sample-spreadsheet.xlsx" >}}  
{{< tab-text >}}
`sample-spreadsheet.xlsx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-excel/deleting-worksheets-from-spreadsheet/sample-spreadsheet.xlsx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-spreadsheet.xlsx" >}}  
```text
Binary file (XLSX, 35 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-excel/deleting-worksheets-from-spreadsheet/deleting_worksheets_from_spreadsheet/edited-spreadsheet.xlsx)
{{< /tab >}}
{{< /tabs >}}
