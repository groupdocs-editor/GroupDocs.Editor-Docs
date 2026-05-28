---
id: edit-txt
url: editor/python-net/edit-txt
title: Edit TXT
linkTitle: Edit text files
weight: 60
description: "This guide demonstrates how to edit plain text files with encoding, lists recognition, pagination and other powerful features of GroupDocs.Editor for Python via .NET."
keywords: Edit TXT file, Edit plain text, Edit TXT programmatically, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit text file in the GroupDocs.Editor
        description: How to edit plain text TXT file using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to edit content of the plain text (TXT) file in the GroupDocs.Editor in Python
        description: Learn how to edit the content of a plain text (TXT) file using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load the desired text file into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired text file into it. Load options are not needed.
        - name: Prepare a TextEditOptions class
          text: Create an instance of the TextEditOptions class and adjust its properties to meet your needs if necessary. You can specify an encoding of the text document, direction of the text flow, pagination mode, and other options.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the editor.edit method with a previously prepared TextEditOptions and obtain an instance of the EditableDocument class, which is ready for editing.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited text content into the most suitable class methods of the class.
        - name: Prepare a TextSaveOptions class
          text: Create an instance of the TextSaveOptions class and adjust its properties to meet your needs if necessary. You can specify an encoding of the text document, and other options.
        - name: Save the edited text file with the editor.save method
          text: Pass an instance of EditableDocument with the content of the edited text file, an instance of TextSaveOptions, and a destination byte stream or file path to the editor.save method for saving the text file.
---
> This demonstration shows how to load, open for editing, and save text documents, and explains the edit and save options and their purpose.

## How to edit a TXT file?

Textual documents are simple Plain Text flat files (TXT) that contain no images, pages, paragraphs, lists, tables, and so on. However, users can create some primitive formatting like lists with leading markers, left indents with whitespaces, tables with pseudo-graphics, paragraphs with line breaks, and so on. [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) can recognize some of these structures. Another feature that GroupDocs.Editor provides is the ability to save an edited TXT document not only back to TXT, but also to WordProcessing.

## Loading a text file for edit

Unlike WordProcessing and Spreadsheet documents, text documents are loaded into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class without any loading options. They are simple text files by their nature, so there is nothing to adjust:

```python
from groupdocs.editor import Editor

editor = Editor("file.txt")
```

## Edit a text file

In order to open a text document for editing by creating an [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, the [`TextEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/texteditoptions) class is used. This class has several options (properties), described below:

1. `encoding` — allows setting the character encoding of the input text document. By default, if not specified, it is UTF-8.
2. `recognize_lists` — a boolean flag that indicates how exactly numbered list items are recognized. If this option is set to `False` (default value), the list recognition algorithm detects list paragraphs when list numbers end with either a dot, a right bracket, or bullet symbols. If this option is set to `True`, whitespaces are also used as list number delimiters.
3. `leading_spaces` — a flag that indicates how consecutive leading spaces should be handled: convert to a left indent (default value), preserve as consecutive spaces, or trim.
4. `trailing_spaces` — a flag that indicates how consecutive trailing spaces should be handled: truncate (default value) or trim.
5. `enable_pagination` — a boolean flag that allows enabling the paged view of the document. By their nature all text documents are pageless, however, GroupDocs.Editor allows splitting them into pages, like MS Word does.
6. `direction` — a flag which allows specifying the direction of the text flow in the input plain text document. By default it is left-to-right.

The runnable example below demonstrates using this options class to open the input text document for editing, getting the HTML content, editing it programmatically, and then saving the edited content back to a TXT file.

{{< tabs "code-example-edit-txt">}}
{{< tab "edit_txt.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.options import TextEditOptions, TextSaveOptions

def edit_txt():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load an input text file into the Editor
    with Editor("./sample.txt") as editor:
        # Create and adjust the text edit options
        edit_options = TextEditOptions()
        edit_options.enable_pagination = True
        edit_options.recognize_lists = True

        # Edit the text file and obtain an EditableDocument
        editable = editor.edit(edit_options)

        # Edit the content programmatically (in practice this is done in a WYSIWYG-editor)
        html = editable.get_content()
        edited = EditableDocument.from_markup(html.replace("This is a sample plain text file", "This is an edited sample plain text file"))

        # Create text save options and tune them
        save_options = TextSaveOptions()
        save_options.preserve_table_layout = True

        # Save the edited content back to the TXT format
        editor.save(edited, "./edited.txt", save_options)

        editable.dispose()
        edited.dispose()

if __name__ == "__main__":
    edit_txt()
```
{{< /tab >}}
{{< tab "sample.txt" >}}  
{{< tab-text >}}
`sample.txt` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-txt/sample.txt) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited.txt" >}}  
```text
﻿This is an edited sample plain text file
 
There is one empty line above this text.  Two spaces.   Three spaces.
New line, which is preceded by 5 consecutive spaces. External link: https://rozetka.com.ua/final_pm_2d_black/p34087151/#tab=characteristics
New line again, but at this time 1 tab char and 1 space char.
One tab.
Two tabs.
Three tabs.
 
 
[TRUNCATED]
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-txt/edit_txt/edited.txt)
{{< /tab >}}
{{< /tabs >}}

## Save a text file after edit

After being edited, a text document can be saved back as TXT or as WordProcessing. For saving back to the TXT format the user must use the [`TextSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/textsaveoptions) class, which has the following properties:

1. `encoding` — the character encoding of the text document, which will be applied when saving it. By default, if not specified, it is UTF-8.
2. `add_bidi_marks` — a boolean flag that determines whether to add bi-directional marks before each BiDi run when saving in plain text format.
3. `preserve_table_layout` — a boolean flag that specifies whether GroupDocs.Editor should try to preserve the layout of tables when saving in plain text format. The default value is `False`.

The edited content can also be saved to a WordProcessing format. For this, a [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) instance with the desired output format is used:

```python
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

word_save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCM)
editor.save(edited, "./edited.docm", word_save_options)
```

As a result, after running these examples the user will have versions of the edited document in the TXT and DOCM formats.
