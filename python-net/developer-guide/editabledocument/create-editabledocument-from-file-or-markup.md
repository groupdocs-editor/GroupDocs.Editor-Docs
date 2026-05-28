---
id: create-editabledocument-from-file-or-markup
url: editor/python-net/create-editabledocument-from-file-or-markup
title: Create EditableDocument from file or markup
linkTitle: Create EditableDocument from file or markup
weight: 4
description: "This article explains how to create an instance of the EditableDocument class from HTML files on disk or from HTML markup with resources using GroupDocs.Editor for Python via .NET API."
keywords: Edit HTML document, GroupDocs.Editor, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
> This demonstration shows how to create an instance of the EditableDocument class from HTML files on disk or from HTML markup with resources.

## Introduction

When working with [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) in the usual way by loading, opening, editing and saving documents, the instances of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class are produced by the [Editor.edit()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method and accepted by the [Editor.save()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method. However, in some cases it is required to create an [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance from existing HTML markup with optional resources. For example, some document was loaded to the `Editor` class, opened for editing, and then the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) was saved to the disk as a set of an HTML file and connected resources. Then this HTML document was passed to the WYSIWYG-editor, edited, and saved back to the disk as modified HTML. Or the raw output from the WYSIWYG-editor was saved to a string variable. In order to save it to some final format like DOCX or XLSX, the user needs to pass the document to the [Editor.save()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method in the form of an [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance. This means that the user should create an instance of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class manually.

[EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) contains three public class methods for creating its instances: [`from_file`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/fromfile), [`from_markup`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup) and [`from_markup_and_resource_folder`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkupandresourcefolder). This article reviews all of them.

## Opening from file

Let's suppose that we have an HTML file with edited document content, that is saved on the disk. There is also a folder with resources (images, fonts, stylesheets), that are used by this document, and the document has correct links to these resources. Let's say the HTML document has the name "document.html". The resource folder is located near it and has the name "document_resources", and, what is most important, the HTML markup from "document.html" has proper links to files from the "document_resources" folder.

In that case creating the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance is the most simple — the [`from_file`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/fromfile) class method accepts the path to the HTML file and the path to the resource folder:

```python
from groupdocs.editor import EditableDocument

# HTML file plus the folder where its resources are stored
document = EditableDocument.from_file("document.html", "document_resources")
```

If the HTML file contains correct links, GroupDocs.Editor will scan the links and find the resources automatically. If the HTML file contains a link to a resource that is not present in the resource folder, it will be omitted.

## Opening from markup and prepared resources

In some cases the edited HTML document is not present as a file. It may be stored in a database, obtained from remote storage, or something else. Quite often the whole document content, with HTML- and CSS-markup and all the resources, is packed inside a single string (resources are packed into the HTML markup using the [data URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme) with base64 encoding). In such cases, when there are no external resources, the [`from_markup`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup) class method is the most convenient — it accepts a single string with the raw HTML markup:

```python
from groupdocs.editor import EditableDocument

input_html_markup = "<html><head><title>Edited document</title></head><body>...</body></html>"
document = EditableDocument.from_markup(input_html_markup)
```

## Opening from markup and resource folder

In some cases there is HTML markup of an edited document, but the resources are represented as a set of files located in a specific folder. For example, the original document was converted to an [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) and then saved to the disk as an HTML document with an *.html file and a folder with resources. Then it was loaded to the WYSIWYG-editor, edited by the end-user, and the edited content was obtained back from the front-end. So now there is edited HTML markup available as a string, and a resource folder. In such a case the [`from_markup_and_resource_folder`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkupandresourcefolder) class method should be used. It accepts two mandatory parameters: the HTML markup string and a valid path to an existing resource folder.

```python
from groupdocs.editor import EditableDocument

input_html_markup = "<html><head><title>Edited document</title></head><body>...</body></html>"
document = EditableDocument.from_markup_and_resource_folder(input_html_markup, "document_resources")
```

This method scans the resource folder, reads all found *.css files, and applies the parsed stylesheets to the document content. This is the main distinction between this method and the previously described [`from_file`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/fromfile) one — this one applies all stylesheets in the folder to the document, while [`from_file`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/fromfile) applies only those stylesheets which are explicitly referenced from the HTML markup. If any stylesheet references external images and/or fonts, GroupDocs.Editor will try to find these resources in the same resource folder too.

## Complete code example

The example below builds a small HTML string, wraps it into an [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance with [`from_markup`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup), and saves it as a DOCX document using an [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) opened on a sample document.

{{< tabs "code-example-create-editabledocument-from-file-or-markup">}}
{{< tab "create_editabledocument_from_file_or_markup.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.options import WordProcessingSaveOptions
from groupdocs.editor.formats import WordProcessingFormats

def create_editabledocument_from_file_or_markup():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Build an HTML markup string (for example, obtained from a WYSIWYG editor)
    html = "<html><head><title>Edited document</title></head><body><p>Hello from markup</p></body></html>"

    # Create an EditableDocument instance directly from the markup
    editable = EditableDocument.from_markup(html)

    # Save the created EditableDocument as a DOCX document
    with Editor("./sample-document.docx") as editor:
        save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
        editor.save(editable, "./output.docx", save_options)

    print("EditableDocument created from markup and saved to ./output.docx")

if __name__ == "__main__":
    create_editabledocument_from_file_or_markup()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/editabledocument/create-editabledocument-from-file-or-markup/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.docx" >}}  
```text
Binary file (DOCX, 7 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/editabledocument/create-editabledocument-from-file-or-markup/create_editabledocument_from_file_or_markup/output.docx)
{{< /tab >}}
{{< /tabs >}}
