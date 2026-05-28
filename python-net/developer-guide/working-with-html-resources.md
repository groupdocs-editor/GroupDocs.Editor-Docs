---
id: working-with-html-resources
url: editor/python-net/working-with-html-resources
title: Working with HTML resources
linkTitle: Working with HTML resources
weight: 90
description: "This article describes the capabilities of GroupDocs.Editor for Python via .NET while working with HTML resources, which are an integral part of an HTML document."
keywords: GroupDocs.Editor HTML resources, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
> This article describes the capabilities of GroupDocs.Editor while working with HTML resources, which are an integral part of an HTML document.

## What HTML resources are

Almost all existing document formats (except plain TXT and some others) contain a concept of _resources_. This usually includes images, specific fonts (which are not installed in the operating system) and so on — depending on the specific document format. In order to edit documents in a browser, GroupDocs.Editor must convert them to HTML and only then send them to the client-side WYSIWYG-editor. In this case GroupDocs.Editor must work with HTML resources, which may be divided into several groups: _images_, _stylesheets (CSS)_, _fonts_, and _audio_.

When a document is opened for editing with [`editor.edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit), the resulting [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) exposes its extracted assets as collections that you can iterate (with `for` loops) and measure (with `len()`):

| Property | Contents |
|---|---|
| `images` | embedded raster (JPEG, PNG, GIF) and vector (SVG, WMF/EMF) images |
| `fonts` | extracted fonts (WOFF, WOFF2, TTF, OTF, EOT) |
| `css` | stylesheets |
| `audio` | audio resources (for example, from presentations) |
| `all_resources` | everything above, combined |

## Iterating over resources

You can inspect, count, and enumerate the resources of an opened document directly from the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance:

```python
from groupdocs.editor import Editor

with Editor("document.docx") as editor:
    editable = editor.edit()
    print("images:", len(editable.images))
    print("css:", len(editable.css))
    print("fonts:", len(editable.fonts))

    for image in editable.images:
        print("image resource:", image)
```

## Saving HTML together with its resources

The [`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/save) method of [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) writes the HTML markup and every extracted resource (images, fonts, css) into a folder, so that the markup references its assets on disk:

```python
with Editor("document.docx") as editor:
    editable = editor.edit()
    # Write HTML plus all resources into a folder
    editable.save("page.html", "page_resources")
```

## Feeding modified markup with resources back

When a customer edits a document in a WYSIWYG-editor and obtains the edited HTML markup along with its resources, the markup must be wrapped back into an [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) before it can be saved. Depending on how the resources are stored, you can use one of the class methods:

1. [`from_markup(html)`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup) — for HTML markup held in memory as a string, with resources baked into it.
2. [`from_markup_and_resource_folder(html, folder)`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkupandresourcefolder) — for HTML markup as a string whose resources live in an existing folder on disk.
3. [`from_file(html_path, folder)`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/fromfile) — for an HTML file on disk plus its corresponding resource folder.

```python
from groupdocs.editor import EditableDocument

# Markup with baked-in resources
edited = EditableDocument.from_markup(edited_html)

# Markup string + on-disk resource folder
edited = EditableDocument.from_markup_and_resource_folder(edited_html, "page_resources")

# HTML file + resource folder
edited = EditableDocument.from_file("page.html", "page_resources")
```

## Complete code example

The example below loads a document, opens it for editing, reports how many resources of each kind were extracted, and saves the HTML together with all of its resources into a folder.

{{< tabs "code-example-working-with-html-resources">}}
{{< tab "working_with_html_resources.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def working_with_html_resources():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        editable = editor.edit()

        # Inspect the extracted resources
        print("Images:", len(editable.images))
        print("Stylesheets:", len(editable.css))
        print("Fonts:", len(editable.fonts))
        print("All resources:", len(editable.all_resources))

        # Persist the HTML together with every resource into a folder
        editable.save("output.html", "output_resources")

        editable.dispose()

if __name__ == "__main__":
    working_with_html_resources()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/working-with-html-resources/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.html" >}}  
```text
<HTML><HEAD><META charset="utf-8" /><TITLE>Sample Document ver.1</TITLE><META name="author" content="neD" /><LINK href="output_resources/style.css" rel="stylesheet" type="text/css" /></HEAD><BODY><DIV class="documentMainContent"><DIV class="Section1"><H1 class="Heading_1"><A id="_Toc465352669" ><SPAN class="Default_Paragraph_Font text001">Title of the document</SPAN></A></H1><H2 class="Heading_2"><A id="_Toc465352671" ><SPAN class="Default_Paragraph_Font text002">Subtitle #1</SPAN></A></H2><P cl
[TRUNCATED]
```
[Download full output](/editor/python-net/_output_files/developer-guide/working-with-html-resources/working_with_html_resources/output.html)
{{< /tab >}}
{{< /tabs >}}
