---
id: how-to-edit-csv-file
url: editor/python-net/how-to-edit-csv-file
title: How to edit CSV file
linkTitle: How to edit CSV file
weight: 50
description: "This guide demonstrates how to edit CSV, TSV, comma-separated value and other text files with different settings and many other powerful features of GroupDocs.Editor for Python via .NET."
keywords: Edit CSV, Edit TSV, Edit Semicolon-separated value file, Edit Whitespace-separated value file, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit CSV and all other DSV files in the GroupDocs.Editor
        description: How to edit documents in CSV and all other DSV formats using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to edit content of the documents in CSV and all other DSV formats in the GroupDocs.Editor in Python
        description: Learn how to edit the content of documents in DSV (Delimiter-Separated Values) formats, including CSV, TSV, and all others, using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load the desired Delimiter-Separated Values (DSV) file into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired text file into it. Load options are not needed.
        - name: Prepare a DelimitedTextEditOptions class
          text: Create an instance of the DelimitedTextEditOptions class and specify a separator (delimiter). For CSV it is a comma character, for TSV a tab character, and so on. Adjust the other properties to meet your needs if necessary.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the editor.edit method with a previously prepared DelimitedTextEditOptions and obtain an instance of the EditableDocument class, which is ready for editing.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited DSV content into the most suitable class methods of the class.
        - name: Prepare a DelimitedTextSaveOptions class
          text: Create an instance of the DelimitedTextSaveOptions class and choose the desired separator (delimiter) string. It does not have to be the same separator as in the DelimitedTextEditOptions.
        - name: Save the edited DSV spreadsheet with the editor.save method
          text: Pass an instance of EditableDocument with the content of the edited DSV spreadsheet, an instance of DelimitedTextSaveOptions, and a destination byte stream or file path to the editor.save method for saving the DSV file.
---
> This demonstration shows and explains all the necessary moments and options regarding processing DSV spreadsheets (Delimiter-Separated Values) like CSV and others.

## Introduction

DSV (Delimiter-Separated Values) files are a specific form of text-based spreadsheets with delimiters (separators). Due to their nature, [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) processes this class of documents separately from usual binary spreadsheets. In contrast to usual spreadsheets, DSV documents due to their textual nature have only a single tab (worksheet) and cannot be encoded. Any non-empty string may be treated as a separator, so the user always needs to specify it explicitly.

The most common types of DSV are:

1. CSV (Comma-Separated Values)
2. TSV (Tab-Separated Values)
3. Semicolon-separated values
4. Whitespace-separated values
5. ...and any other

GroupDocs.Editor supports DSV with any separator, which can be a single character or a set of characters (string).

## Loading a CSV file for edit

Unlike WordProcessing and Spreadsheet documents, DSV documents are loaded into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class without any loading options. They are simple text files by their nature, so there is nothing to adjust:

```python
from groupdocs.editor import Editor

editor = Editor("spreadsheet.csv")
```

## Edit a CSV file

In order to open any DSV document for editing, the user must use the [`DelimitedTextEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/delimitedtexteditoptions) class, whose single constructor has one mandatory parameter — a separator (delimiter) string, which should not be `None` or an empty string. There are also several optional properties:

- `convert_date_time_data` and `convert_numeric_data` are boolean flags that indicate how to treat numbers. GroupDocs.Editor can recognize digits within cells and treat them as numbers or date-time values. By default this recognition is disabled, but the user can turn it on.
- `treat_consecutive_delimiters_as_one` is a boolean flag that determines how consecutive delimiters should be treated — as several (default, `False`) or as a single one (`True`).
- `optimize_memory_usage` is a boolean flag with a different purpose. By default, GroupDocs.Editor algorithms are tuned for maximum performance. However, in some rare cases the user may need to load a very large DSV file. By enabling this flag, the user switches GroupDocs.Editor to use other processing algorithms, which consume a relatively low amount of memory at the cost of lower performance.

The runnable example below loads a CSV file, edits it with comma as the delimiter and numeric recognition enabled, and then saves the edited content to a TSV file (a DSV with a tab separator).

{{< tabs "code-example-how-to-edit-csv-file">}}
{{< tab "edit_csv.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.options import DelimitedTextEditOptions, DelimitedTextSaveOptions

def edit_csv():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load an input CSV file into the Editor
    with Editor("./cars.csv") as editor:
        # Create the DSV edit options with a comma separator
        edit_options = DelimitedTextEditOptions(",")
        edit_options.convert_numeric_data = True
        edit_options.treat_consecutive_delimiters_as_one = True

        # Edit the CSV document and obtain an EditableDocument
        editable = editor.edit(edit_options)

        # Edit the content programmatically (in practice this is done in a WYSIWYG-editor)
        html = editable.get_content()
        edited = EditableDocument.from_markup(html)

        # Create DSV save options with a tab separator (TSV)
        save_options = DelimitedTextSaveOptions("\t")
        save_options.trim_leading_blank_row_and_column = True
        save_options.keep_separators_for_blank_row = False

        # Save the edited content to the TSV format
        editor.save(edited, "./edited-cars.tsv", save_options)

        editable.dispose()
        edited.dispose()

if __name__ == "__main__":
    edit_csv()
```
{{< /tab >}}
{{< tab "cars.csv" >}}  
{{< tab-text >}}
`cars.csv` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/how-to-edit-csv-file/cars.csv) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-cars.tsv" >}}  
```text
﻿1997	Ford	E350	ac, abs, moon	3000
1999	Chevy	Venture «Extended Edition»		4900
1996	Jeep	Grand Cherokee	MUST SELL! air, moon roof, loaded	4799
2014	SsangYong	Kyron	good car!	1998
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/how-to-edit-csv-file/edit_csv/edited-cars.tsv)
{{< /tab >}}
{{< /tabs >}}

## Save a CSV file after editing

After being edited, an input DSV can be saved back to a DSV (not necessarily with the same separator) or to any supported Spreadsheet document. In order to save a document to the DSV format, the user must use the [`DelimitedTextSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/delimitedtextsaveoptions) class, which, like [`DelimitedTextEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/delimitedtexteditoptions), has one constructor with a mandatory string parameter — a separator (delimiter), which should not be `None` or an empty string.

There are also other properties:

1. `encoding` — allows specifying the encoding of the generated DSV. By default, if not specified, it is UTF-8.
2. `trim_leading_blank_row_and_column` — a boolean flag that indicates whether leading blank rows and columns should be trimmed, like what MS Excel does.
3. `keep_separators_for_blank_row` — a boolean flag that indicates whether separators should be output for a blank row. The default value is `False`, which means the content for a blank row will be empty.

The edited content can also be saved to a Spreadsheet format such as XLSM. For this, a [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) instance with the desired output format is used:

```python
from groupdocs.editor.formats import SpreadsheetFormats
from groupdocs.editor.options import SpreadsheetSaveOptions

xlsm_save_options = SpreadsheetSaveOptions(SpreadsheetFormats.XLSM)
editor.save(edited, "./edited-cars.xlsm", xlsm_save_options)
```
