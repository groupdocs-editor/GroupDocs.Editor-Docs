---
id: save-html-to-folder
url: editor/python-net/save-html-to-folder
title: Save HTML to folder
linkTitle: Save HTML to folder
weight: 2
description: "This article explains how to save an edited document in HTML form to a folder on local disk using GroupDocs.Editor for Python via .NET features."
keywords: Save edited HTML, Save edited document, Save HTML to folder, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
> This demonstration shows how to open an input document, convert it to an intermediate [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), and save it to disk as an HTML file with a resource folder.

Almost all HTML WYSIWYG client-side editors are able to open an HTML document from disk (from a path). [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) allows to open any supportable document, convert it to HTML and save it to disk, which may be very useful for subsequently editing it in some WYSIWYG editor.

When the document is opened for editing with [`editor.edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit), the resulting [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) can be written to disk with its [`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/save) method. The method accepts the path to the output HTML file and the path to a folder where the resources (images, stylesheets, fonts) will be saved.

```python
from groupdocs.editor import Editor
from groupdocs.editor.options import WordProcessingLoadOptions

load_options = WordProcessingLoadOptions()
editor = Editor("document.docx", load_options)  # passing path and load options to the constructor
document = editor.edit()

# Save HTML markup together with resources into a folder
document.save("document.html", "document_resources")
```

In this example we load an input WordProcessing (DOCX) document into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class with load options, specific for this document family type - [`WordProcessingLoadOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingloadoptions). Then the document is converted to the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) using the [`edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method. In the last line the content is saved to the HTML file on disk, and all resources are placed into the specified folder.

## Complete code example

The example below loads a document, opens it for editing, and saves the HTML markup together with all of its resources into a folder.

{{< tabs "code-example-save-html-to-folder">}}
{{< tab "save_html_to_folder.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def save_html_to_folder():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        editable = editor.edit()

        # Write the HTML markup and every resource into a folder
        editable.save("output.html", "output_resources")

        editable.dispose()

    print("Saved HTML to output.html with resources in output_resources")

if __name__ == "__main__":
    save_html_to_folder()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/editabledocument/save-html-to-folder/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.html" >}}  
```text
<HTML><HEAD><META charset="utf-8" /><TITLE>Sample Document ver.1</TITLE><META name="author" content="neD" /><LINK href="output_resources/style.css" rel="stylesheet" type="text/css" /></HEAD><BODY><DIV class="documentMainContent"><DIV class="Section1"><H1 class="Heading_1"><A id="_Toc465352669" ><SPAN class="Default_Paragraph_Font text001">Title of the document</SPAN></A></H1><H2 class="Heading_2"><A id="_Toc465352671" ><SPAN class="Default_Paragraph_Font text002">Subtitle #1</SPAN></A></H2><P cl
[TRUNCATED]
```
[Download full output](/editor/python-net/_output_files/developer-guide/editabledocument/save-html-to-folder/save_html_to_folder/output.html)
{{< /tab >}}
{{< /tabs >}}
