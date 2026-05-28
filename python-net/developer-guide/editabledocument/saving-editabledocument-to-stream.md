---
id: saving-editabledocument-to-stream
url: editor/python-net/saving-editabledocument-to-stream
title: Saving EditableDocument to stream
linkTitle: Saving EditableDocument to stream
weight: 5
description: "This article shows and explains advanced techniques and approaches while working with EditableDocument, including saving the HTML markup and the accompanying resources using GroupDocs.Editor for Python via .NET."
keywords: Save EditableDocument to stream, Save HTML document to stream, GroupDocs.Editor, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
> This article shows and explains advanced techniques and approaches while working with EditableDocument, including saving its HTML markup and the accompanying resources.

## Introduction

The [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class is one of the most important among all types in GroupDocs.Editor. It is focused on producing HTML markup and saving it together with its resources, and it exposes several ways to do so. This article covers them.

## Saving the HTML markup to a file

The simplest way to persist an [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) is its [`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/save) method. When invoked with only the HTML file path, it writes the HTML markup, and the accompanying resources are placed next to it automatically.

```python
from groupdocs.editor import Editor

with Editor("document.docx") as editor:
    editable = editor.edit()
    # Write the HTML markup to a file
    editable.save("document.html")
```

## Obtaining the HTML markup as a string

When the markup is needed in memory rather than on disk — for example, to push it into a stream, a database, or an HTTP response — it can be obtained as a string with [`get_content()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getcontent), [`get_body_content()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getbodycontent), or the fully self-contained [`get_embedded_html()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getembeddedhtml). The resulting string can then be written into any binary stream, such as an `io.BytesIO`:

```python
import io
from groupdocs.editor import Editor

with Editor("document.docx") as editor:
    editable = editor.edit()

    # All resources are embedded inside a single self-contained string
    embedded_html = editable.get_embedded_html()

    # Write the markup into an in-memory binary stream
    stream = io.BytesIO()
    stream.write(embedded_html.encode("utf-8"))
    stream.seek(0)
```

When [`get_embedded_html()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getembeddedhtml) is used, the produced string is fully autonomous: stylesheets are embedded into the markup and images are serialized with base64 encoding directly into the `src` attributes, so no external resources are required.

## Complete code example

The example below loads a document, opens it for editing, and saves its HTML markup to a file.

{{< tabs "code-example-saving-editabledocument-to-stream">}}
{{< tab "saving_editabledocument_to_stream.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def saving_editabledocument_to_stream():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        editable = editor.edit()

        # Save the HTML markup to a file
        editable.save("output.html")

        # The same markup is also available in memory as a string
        embedded_html = editable.get_embedded_html()
        print("Embedded HTML length:", len(embedded_html))

        editable.dispose()

if __name__ == "__main__":
    saving_editabledocument_to_stream()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/editabledocument/saving-editabledocument-to-stream/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.html" >}}  
```text
<HTML><HEAD><META charset="utf-8" /><TITLE>Sample Document ver.1</TITLE><META name="author" content="neD" /><LINK href="output_resources/style.css" rel="stylesheet" type="text/css" /></HEAD><BODY><DIV class="documentMainContent"><DIV class="Section1"><H1 class="Heading_1"><A id="_Toc465352669" ><SPAN class="Default_Paragraph_Font text001">Title of the document</SPAN></A></H1><H2 class="Heading_2"><A id="_Toc465352671" ><SPAN class="Default_Paragraph_Font text002">Subtitle #1</SPAN></A></H2><P cl
[TRUNCATED]
```
[Download full output](/editor/python-net/_output_files/developer-guide/editabledocument/saving-editabledocument-to-stream/saving_editabledocument_to_stream/output.html)
{{< /tab >}}
{{< /tabs >}}
