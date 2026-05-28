---
id: generating-worksheets-preview-for-spreadsheet
url: editor/python-net/generating-worksheets-preview-for-spreadsheet
title: Generating worksheets (tabs) preview for spreadsheet
linkTitle: Generating worksheets preview for spreadsheet
weight: 2
description: "This article describes how to generate a preview for any worksheet (tab) for the existing Excel spreadsheet in SVG format using GroupDocs.Editor for Python via .NET"
keywords: preview worksheet tab excel spreadsheet SVG, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
GroupDocs.Editor for Python via .NET allows to generate a preview for any worksheet (a.k.a. _tab_) in the spreadsheet (a.k.a. _workbook_) document in SVG format. With this feature the end-users are able to view and inspect the content of the spreadsheet without actually sending it for edit. This generated worksheet preview cannot be edited using the GroupDocs.Editor itself, but it can be saved and then viewed in any desktop or online image viewer as well as in the browser (because any modern browser actually supports viewing of SVG format).

This feature is working regardless of the licensing mode of the GroupDocs.Editor: it works the same for both trial and licensed mode, there are no trial limitations for this feature. While generating the worksheets preview, the GroupDocs.Editor doesn't write off the consumed bytes or credits.

Excel spreadsheets may have the so-called _hidden worksheets_ — GroupDocs.Editor generates an SVG preview for them too.

For generating the worksheets preview for a particular spreadsheet document the user must perform the next steps:

- Load a desired spreadsheet file to the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/) class.
- Call the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo/) method and specify a password of a loaded spreadsheet in case if this spreadsheet is protected with a password.
- In the obtained [`SpreadsheetDocumentInfo`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/spreadsheetdocumentinfo/) object invoke the `generate_preview(worksheet_index)` method and specify a zero-based _index_ (do not confuse with the worksheet _numbers_, which are 1-based) of the desired worksheet. If the specified index is lesser than 0 or exceeds the number of worksheets within a given spreadsheet, then an exception will be thrown.
- The `generate_preview(worksheet_index)` method returns a worksheet preview as an SVG vector image, that is encapsulated in the `SvgImage` class. This class has all necessary methods and properties to obtain the content of an SVG image in any desired form, save it to disk, stream and so on.

The snippet below illustrates opening an unprotected spreadsheet file, obtaining the number of all worksheets inside this spreadsheet, and then generating the previews for every worksheet in a loop. Then these previews are saved to the disk.

```python
import os
from groupdocs.editor import Editor

# Obtain a valid path to the spreadsheet file
input_path = "./sample-spreadsheet.xlsx"
output_folder = "./previews"

# Load spreadsheet file to the Editor constructor
with Editor(input_path) as editor:
    # Get document info for this file
    info_spreadsheet = editor.get_document_info()

    # Get the number of all worksheets
    worksheets_count = info_spreadsheet.page_count

    # Iterate through all worksheets and generate the preview on every iteration
    for worksheet_index in range(worksheets_count):
        # Generate one preview as an SVG image by worksheet index
        one_svg_preview = info_spreadsheet.generate_preview(worksheet_index)

        # Save the SVG preview to a file
        one_svg_preview.save(os.path.join(output_folder, one_svg_preview.filename_with_extension))
```

The worksheets preview feature is by its essence a method in the existing `SpreadsheetDocumentInfo` object, that obtains a worksheet index and returns an instance of the `SvgImage` class. If the end-user needs to obtain a preview of the worksheet in a raster format, but not in the vector, the `SvgImage` class also provides a method to convert the SVG content to the PNG format.

The complete example below loads a spreadsheet, opens its first worksheet for editing, and prints the length of the generated HTML content. This roundtrip confirms that the spreadsheet is read correctly before any preview is generated.

{{< tabs "code-example-generating-worksheet-preview-for-spreadsheet">}}
{{< tab "generating_worksheet_preview_for_spreadsheet.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import SpreadsheetEditOptions

def generating_worksheet_preview_for_spreadsheet():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load the spreadsheet file to the Editor constructor
    with Editor("./sample-spreadsheet.xlsx") as editor:
        # Prepare edit options and select the 1st worksheet
        edit_options = SpreadsheetEditOptions()
        edit_options.worksheet_index = 0  # index is 0-based, so this is the 1st worksheet

        # Open the worksheet for editing
        worksheet = editor.edit(edit_options)

        # Obtain the HTML content of the worksheet
        content = worksheet.get_content()
        print("Worksheet HTML content length:", len(content))

        worksheet.dispose()

if __name__ == "__main__":
    generating_worksheet_preview_for_spreadsheet()
```
{{< /tab >}}
{{< tab "sample-spreadsheet.xlsx" >}}  
{{< tab-text >}}
`sample-spreadsheet.xlsx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-excel/generating-worksheet-preview-for-spreadsheet/sample-spreadsheet.xlsx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "generating-worksheet-preview-spreadsheet.txt" >}}  
```text
Worksheet HTML content length: 42948
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-excel/generating-worksheet-preview-for-spreadsheet/generating_worksheet_preview_for_spreadsheet/generating-worksheet-preview-spreadsheet.txt)
{{< /tab >}}
{{< /tabs >}}
