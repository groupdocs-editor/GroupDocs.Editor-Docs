---
id: memory-optimization-option
url: editor/python-net/memory-optimization-option
title: Memory optimization option
linkTitle: Memory optimization option
weight: 9
description: "This article explains how to optimize memory utilization when editing large Word documents using GroupDocs.Editor for Python via .NET API."
keywords: large document edit performance, memory optimization when edit document, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
By default [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) tries to perform computations and complete the task as fast as possible, and if this challenge requires a lot of memory to be used, GroupDocs.Editor does it. However, in some very specific cases, when the processing document is very huge, or the user machine has a very limited amount of free memory, an out-of-memory error may occur. In order to solve such a problem the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class contains the [`optimize_memory_usage`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions/) property:

```python
optimize_memory_usage  # bool
```

By default it has a `False` value, which means that the memory optimization is disabled for the sake of the best possible performance. By setting it to `True` the user can enable another document generating mechanism, which can significantly decrease memory consumption while generating large documents at the cost of slower generation time while performing the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.

## Complete example

The example below loads the sample document, edits it, and saves the result to DOCX with the `optimize_memory_usage` flag enabled.

{{< tabs "code-example-memory-optimization-option">}}
{{< tab "optimize_memory_usage.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

def optimize_memory_usage():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        original = editor.edit()
        modified = EditableDocument.from_markup(original.get_embedded_html())

        # Enable memory optimization for the saving process
        save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
        save_options.optimize_memory_usage = True

        editor.save(modified, "./optimized-document.docx", save_options)
        print("Saved the edited document with memory optimization enabled")

        original.dispose()
        modified.dispose()

if __name__ == "__main__":
    optimize_memory_usage()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/memory-optimization-option/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "optimized-document.docx" >}}  
```text
Binary file (DOCX, 49 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/memory-optimization-option/optimize_memory_usage/optimized-document.docx)
{{< /tab >}}
{{< /tabs >}}
