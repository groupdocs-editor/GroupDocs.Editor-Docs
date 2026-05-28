---
id: inserting-edited-worksheet-into-existing-spreadsheet
url: editor/python-net/inserting-edited-worksheet-into-existing-spreadsheet
title: Inserting edited worksheet into existing spreadsheet
linkTitle: Inserting edited worksheet into existing spreadsheet
weight: 1
description: "This article describes the feature of the GroupDocs.Editor for Python via .NET - inserting an edited worksheet into existing spreadsheet"
keywords: Edit Excel, insert tab into Excel, insert worksheet into Excel, insert worksheet into workbook, insert worksheet into spreadsheet, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
By default and from the moment when a Spreadsheet module was released to public, the full spreadsheet editing pipeline was the next:

1. Loading spreadsheet in a form of a file or stream into constructor of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class.
2. Selecting a specific worksheet to edit and specifying its index in the [worksheet_index](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheeteditoptions/worksheetindex) property of [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheeteditoptions) class.
3. Opening a spreadsheet for editing by calling [editor.edit()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method, passing [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheeteditoptions) instance with selected worksheet into it, and obtaining [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance from this method.
4. Emitting HTML and CSS markup, which represents a content of an edited document, from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, using different methods like [get_content()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getcontent).
5. Passing this HTML and CSS markup into the WYSIWYG editor, which is located on client-side and is running in the browser.
6. End-user edits the document content in the WYSIWYG editor.
7. Edited document content, in form of HTML and CSS markup, is passed back to the server-side.
8. Creating a new instance of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class by passing the edited content, obtained from server, into the [from_file](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/fromfile), [from_markup](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup), or [from_markup_and_resource_folder](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkupandresourcefolder/) static methods (depending from content).
9. Creating an instance of [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) class with format of output spreadsheet file.
10. Calling an [editor.save](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method by passing the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), output stream for the document, and a [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) options, into it. This will generate a new spreadsheet, which contains only one single worksheet — those worksheet, that was edited on the client-side and which content was passed via [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class into this method.

The last 10th step can be altered — the edited worksheet can be inserted into the original spreadsheet, which was loaded on the 1st step.

[SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) class contains two properties: integer [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) and boolean flag [insert_as_new_worksheet](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/). Both of them have "usual" default values: `worksheet_number` has `0` and `insert_as_new_worksheet` is set to `False`.

```python
save_options.worksheet_number = 0
save_options.insert_as_new_worksheet = False
```

By default, when these properties are not touched or at least [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) has a default value `0`, the GroupDocs.Editor will generate new single-worksheet spreadsheet, as before. However, if [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property contains number, distinct from `0`, and valid spreadsheet is loaded into [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class (it is expected to be the original spreadsheet, which was edited, but it actually can be any spreadsheet, even those, which has no relation to the original), then edited worksheet will be **inserted** into given spreadsheet.

## worksheet_number property

[worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property, if it is not a zero, defines, where, at which exact position in the given spreadsheet the new edited worksheet should be inserted. [insert_as_new_worksheet](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/) parameter, which is a boolean flag, determines, how this worksheet should be inserted: should it replace the existing worksheet, that is located on specified position (`False`, default value), or it should be injected between existing worksheets, without rewriting them, and thus increasing the total amount of worksheets in the given spreadsheet by one.

Because default `0` value of [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) parameter is reserved, actual worksheet numbering starts from `1`. This is different from widely used 0-based indexing, however, makes sense, thus it is not an index of a worksheet, but rather number of a worksheet, the same as MS Excel uses, for instance. This means that, for example, for given spreadsheet, that consists of 5 worksheets, 1st one has a `1` worksheet number, and 5th — `5`. If user has specified a worksheet number, which exceeds the total amount of worksheets in spreadsheet, this number will be automatically adjusted to the latest. This means that if, for example, for the same 5-worksheet spreadsheet the user will specify a `6`, `7` or even a very big value, it will be internally set a `5` — number of the latest worksheet.

Along with positive worksheet numbers, the [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property also supports negative numbering, which implies count from the end of spreadsheet. In this case, `-1` is treated as the last worksheet, `-2` — the last but one, and so on. Again, like with positive numbering, in case when number exceeds the amount of worksheets, it will be adjusted to the closest — first worksheet for negative numbers.

Part of source code below explains this numbering system:

```python
save_options = SpreadsheetSaveOptions(SpreadsheetFormats.XLSX)

# let's say we have a spreadsheet with 5 worksheets

save_options.worksheet_number = 0   # default value, given spreadsheet will be ignored and new will be created

# positive numbering
save_options.worksheet_number = 1   # first worksheet
save_options.worksheet_number = 2   # second worksheet
save_options.worksheet_number = 3   # third worksheet
save_options.worksheet_number = 4   # fourth worksheet
save_options.worksheet_number = 5   # fifth worksheet
save_options.worksheet_number = 6   # fifth worksheet, because value '6' exceeds the worksheets amount '5' and thus is adjusted to the closest

# negative numbering
save_options.worksheet_number = -1  # fifth worksheet, which is first from end (last)
save_options.worksheet_number = -2  # fourth worksheet, which is second from end (last but one)
save_options.worksheet_number = -3  # third worksheet, which is third from end
save_options.worksheet_number = -4  # second worksheet, which is fourth from end
save_options.worksheet_number = -5  # first worksheet, which is fifth from end
save_options.worksheet_number = -6  # first worksheet, because value '-6' exceeds the worksheets amount '5' and thus is adjusted to the closest
```

## insert_as_new_worksheet property

[insert_as_new_worksheet](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/) property complements the previously described [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) property and is ignored, when [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) is set to `0`. If [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber) points on the worksheet of specific number, the [insert_as_new_worksheet](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/) determines how to treat this worksheet number and how to insert the worksheet:

* [insert_as_new_worksheet](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/) property has default `False` value, the existing worksheet in given spreadsheet will be completely erased, and the content of the new worksheet (which is located in the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance) will be putted on this place. As a result, a spreadsheet will preserve the same untouched amount of worksheets, but one of its worksheets (specified by the [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber)) will be replaced onto new one.
* If [insert_as_new_worksheet](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/) property has `True` value, the edited worksheet, obtained from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, will be injected among existing worksheets in given spreadsheet, so its amount of worksheets will be incremented by one. New worksheet is inserted at position, specified by [worksheet_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetnumber), and all subsequent worksheets (following or preceding) will be shifted to the end or to the beginning accordingly, depending on positive or negative numbering.

Source code below shows, how worksheet number is treated when [insert_as_new_worksheet](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/insertasnewworksheet/) property is enabled:

```python
save_options = SpreadsheetSaveOptions(SpreadsheetFormats.XLSX)

# let's say we have a spreadsheet with 5 worksheets

save_options.worksheet_number = 0   # default value, given spreadsheet will be ignored, as well as insert_as_new_worksheet

save_options.insert_as_new_worksheet = True  # enabling the adding of worksheet instead of replacing

# positive numbering
save_options.worksheet_number = 1   # new worksheet is injected as first, while all following (including 'old' 1st) are shifting to the end
save_options.worksheet_number = 2   # new worksheet is injected as second, while 2nd, 3rd, 4th and 5th are shifting to the end
save_options.worksheet_number = 3   # new worksheet is injected as third, while 3rd, 4th and 5th are shifting to the end
save_options.worksheet_number = 4   # new worksheet is injected as fourth, while 4th and 5th are shifting to the end
save_options.worksheet_number = 5   # new worksheet is injected as fifth, while 5th is shifting to the end and becomes 6th
save_options.worksheet_number = 6   # new worksheet is injected as sixth, it already becomes the latest, none of existing worksheets are shifting to the end
save_options.worksheet_number = 7   # same as previous

# negative numbering
save_options.worksheet_number = -1  # new worksheet is injected as first from end (it becomes sixth if starting from beginning), none of existing worksheets are shifting to the end
save_options.worksheet_number = -2  # new worksheet is injected as second from end (it becomes fifth if starting from beginning), following single worksheet is shifting to the end
save_options.worksheet_number = -3  # new worksheet is injected as third from end (it becomes fourth if starting from beginning), two following worksheets are shifting to the end
save_options.worksheet_number = -4  # new worksheet is injected as fourth from end (it becomes third if starting from beginning), three following worksheets are shifting to the end
save_options.worksheet_number = -5  # new worksheet is injected as fifth from end (it becomes second if starting from beginning), four following worksheets are shifting to the end
save_options.worksheet_number = -6  # new worksheet is injected as sixth from end (it becomes first if starting from beginning), five following worksheets are shifting to the end
save_options.worksheet_number = -7  # same as previous
```

The complete example below loads a spreadsheet, edits its first worksheet, and saves the result by inserting the edited worksheet into the original spreadsheet as a new worksheet, keeping the original worksheets intact.

{{< tabs "code-example-inserting-edited-worksheet-into-existing-spreadsheet">}}
{{< tab "inserting_edited_worksheet_into_existing_spreadsheet.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.options import SpreadsheetLoadOptions, SpreadsheetEditOptions, SpreadsheetSaveOptions
from groupdocs.editor.formats import SpreadsheetFormats

def inserting_edited_worksheet_into_existing_spreadsheet():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load input spreadsheet to the Editor and specify loading options
    with Editor("./sample-spreadsheet.xlsx", SpreadsheetLoadOptions()) as editor:
        # Prepare edit options and set the 1st worksheet to edit
        edit_options = SpreadsheetEditOptions()
        edit_options.worksheet_index = 0  # index is 0-based, so this is the 1st worksheet

        # Generate EditableDocument with the original content of the worksheet
        worksheet_opened_for_edit = editor.edit(edit_options)

        # Get the HTML-markup from the EditableDocument with the original content
        original_html = worksheet_opened_for_edit.get_embedded_html()

        # Emulate HTML content editing in a WYSIWYG-editor in the browser or somewhere else
        edited_html = original_html.replace("</body>", "<p>Edited content</p></body>")

        # Generate EditableDocument with the edited content
        worksheet_after_edit = EditableDocument.from_markup(edited_html)

        # Prepare save options that insert the edited worksheet into the original spreadsheet
        save_options = SpreadsheetSaveOptions(SpreadsheetFormats.XLSX)
        save_options.worksheet_number = 1  # 1-based; insert at the 1st position
        save_options.insert_as_new_worksheet = True  # keep the edited worksheet alongside the original ones

        # Save the spreadsheet with the inserted edited worksheet
        editor.save(worksheet_after_edit, "./edited-spreadsheet.xlsx", save_options)

        worksheet_opened_for_edit.dispose()
        worksheet_after_edit.dispose()

    print("Saved spreadsheet with the inserted edited worksheet to edited-spreadsheet.xlsx")

if __name__ == "__main__":
    inserting_edited_worksheet_into_existing_spreadsheet()
```
{{< /tab >}}
{{< tab "sample-spreadsheet.xlsx" >}}  
{{< tab-text >}}
`sample-spreadsheet.xlsx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-excel/inserting-edited-worksheet-into-existing-spreadsheet/sample-spreadsheet.xlsx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-spreadsheet.xlsx" >}}  
```text
Binary file (XLSX, 66 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-excel/inserting-edited-worksheet-into-existing-spreadsheet/inserting_edited_worksheet_into_existing_spreadsheet/edited-spreadsheet.xlsx)
{{< /tab >}}
{{< /tabs >}}

### Additional notes

It is worth mentioning that the described feature doesn't modify the original spreadsheet document, which was originally loaded into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class through its constructor. When saving a spreadsheet and inserting the edited worksheet into existing spreadsheet, the GroupDocs.Editor creates a full and exact copy of the original document, and only then adds or replaces the worksheet onto the edited. So the original document is not touched in any case.

From this point it is clear that the GroupDocs.Editor cannot insert edited worksheet into existing spreadsheet document, if this document is not available. For example, original spreadsheet document was loaded into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class, then opened for edit and stored somewhere for consequent editing. Then, in order to create an output spreadsheet from edited document, a new [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance was created from another document. In such case, if new loaded document is not a spreadsheet, the feature with inserting a worksheet will not work (because there is no source, into which the worksheet can be inserted). Also this means that for such scenario the "output" source spreadsheet may not be the same document as the "original" source spreadsheet. For example, it is absolutely legal and working scenario, when user initially loads a spreadsheet named "A" into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class, edits (let's say) 2nd worksheet from it, then creates a new instance of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class and loads another spreadsheet named "B" into it, and finally creates an output document from "B", where edited worksheet is injected on 5th position.

[SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) class contains properties for protecting a worksheet from editing: [password](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/password) and [worksheet_protection](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions/worksheetprotection). When these options are applied and inserting an edited worksheet into existing spreadsheet is applied too, the worksheet protection is applied only to the edited worksheet, which is inserting — all other worksheets are untouched.

```python
save_options = SpreadsheetSaveOptions(SpreadsheetFormats.XLSX)
save_options.worksheet_number = 1
save_options.insert_as_new_worksheet = True
save_options.password = "new password"  # the output document will be encoded with this password
save_options.worksheet_protection = worksheet_protection  # write-protection applied only to the inserted edited worksheet
```
