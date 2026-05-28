---
id: float-and-paginal-modes
url: editor/python-net/float-and-paginal-modes
title: Float and paginal modes
linkTitle: Float and paginal modes
weight: 4
description: "This article explains pros and cons of float and paginal document editing modes when edit Word documents with GroupDocs.Editor for Python via .NET API."
keywords: Edit Word document in float mode, Edit Word document page by page, Edit Word, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---

## How to enable different document edit modes?

WordProcessing module of [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net), that is responsible for converting all WordProcessing formats to [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instances and backward (from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) to some of WordProcessing format), contains two modes: *float* and *paginal* (also known as *paged*), where the first one — float, is default. These modes are presented by two properties with the same name and type:

```python
enable_pagination  # bool
```

At first, such property is present in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class. In this case this option is responsible for the selected mode during the forward (WordProcessing to [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument)) conversion.

Secondly, such property is present in the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class and is responsible for the selected mode during the backward ([EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) to WordProcessing) conversion.

The main "rule of thumb" for the end-user is to preserve the same mode during full document roundtrip and not to change it. In other words, if input document was converted to the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) (followed by the HTML emitting from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance) in the *float* mode, then the resultant [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance (obtained from a document, edited on client-side) should be converted back to the WordProcessing format also in the *float* mode (same rule for *paginal* mode too).

The main distinction of these two modes lies in the form of representation of the input WordProcessing document. By default all WordProcessing formats internally have no pages and page division, they are internally represented as a float smoothness and pageless structure. However, they contain page-related information like headers, footers, page watermarks, page numbering formatting info etc. And when WordProcessing document is opened in some desktop or browser-based text processor like MS Word, OpenOffice or Google Docs, this text processor dynamically and "on the fly" splits the document onto multiple pages, which are re-calculated every time when user is performing some edits in the document content. For example, when user removes a line from the beginning of a huge document, the empty space is "collapsing", and all subsequent content is like "moving upward" to fill the gap, where the removed line was located. Same real-time calculations are valid also for situations when content is added, — newly inserted content is pushing the existing content to move downward, with creating new pages if necessary.

The problem is that most of the used client-side browser-based JavaScript HTML WYSIWYG editors like TinyMCE or CKEditor do not support pages, page separation and described calculations at all. They support only documents with pageless structure. That's why GroupDocs.Editor supports two modes:

* In the *float* mode the WordProcessing document is represented in pageless form. Content is not separated onto pages. There are no page-related entities like headers, footers, watermarks and page numbers. This mode is the most suitable for almost all widespread WYSIWYG HTML-editors and that's why this mode is default.
* In the *paginal* mode the WordProcessing document is represented as a set of pages, where content is divided onto pages in the same way as MS Word does. All page-related entities like headers, footers, watermarks and page numbers are present. This mode should be turned on manually (as it is described above) and is fitting for scenarios where end-user is able to process such content in some appropriate and suitable way, for example, in his own-made HTML editing software.

## Complete example

The example below loads the sample document and edits it in the *paginal* mode by setting `enable_pagination` to `True`. To switch to the default *float* mode, set this property to `False` (or simply leave the default).

{{< tabs "code-example-float-and-paginal-modes">}}
{{< tab "edit_in_paginal_mode.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingEditOptions

def edit_in_paginal_mode():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Enable the paginal (paged) edit mode; set False for the default float mode
    edit_options = WordProcessingEditOptions()
    edit_options.enable_pagination = True

    with Editor("./sample-document.docx") as editor:
        editable = editor.edit(edit_options)
        html = editable.get_content()
        print("Edited in paginal mode, HTML markup length:", len(html))
        editable.dispose()

if __name__ == "__main__":
    edit_in_paginal_mode()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/float-and-paginal-modes/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edit-in-paginal-mode.txt" >}}  
```text
Edited in paginal mode, HTML markup length: 32503
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/float-and-paginal-modes/edit_in_paginal_mode/edit-in-paginal-mode.txt)
{{< /tab >}}
{{< /tabs >}}

## Paginal edit mode in PDF

Along with the family of WordProcessing formats, GroupDocs.Editor supports PDF as one of the output (resultant) formats. In other words, an input WordProcessing document may be opened, edited, but saved not only to the WordProcessing, but also to the PDF. The [PdfSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfsaveoptions) class contains the same `enable_pagination` boolean flag, which, like the same in WordProcessing, has a `False` default value, meaning *float* mode, while a `True` value means *paginal* mode. If input WordProcessing document was converted to [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) in *paginal* mode, the output PDF should be generated in *paginal* mode too, with enabled `enable_pagination` flag.
