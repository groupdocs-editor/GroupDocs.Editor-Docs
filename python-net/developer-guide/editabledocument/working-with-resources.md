---
id: working-with-resources
url: editor/python-net/working-with-resources
title: Working with resources
linkTitle: Working with resources
weight: 3
description: "This article demonstrates and explains different operations with resources, including retrieving them in different scenarios when editing documents with GroupDocs.Editor for Python via .NET."
keywords: Working with HTML resources, Edit document, GroupDocs.Editor, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
> This demonstration shows and explains different operations with resources, including retrieving them in different scenarios.

## Introduction

Almost all documents of any type have resources. These are first of all images; some document formats also hold fonts. Even for a plain text document (TXT), when converting it to HTML for editing, there will be one stylesheet, that is treated as a resource. WordProcessing documents of some formats, Office Open XML usually, can also contain embedded audio files. GroupDocs.Editor allows to work with resources on the editing phase, when the document was loaded into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class and opened for editing by generating the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, that is produced by the [Editor.edit()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method. GroupDocs.Editor classifies all resources into several groups:

* Images, including: raster (PNG, BMP, JPEG, GIF, ICON) and vector (SVG and WMF).
* Fonts, including: TTF, EOT, WOFF, WOFF2.
* Textual resources: CSS stylesheets.
* Audio files: MP3.

## Preparations

Let's prepare an [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance by loading and editing some input WordProcessing document, as always:

```python
from groupdocs.editor import Editor
from groupdocs.editor.options import WordProcessingLoadOptions

editor = Editor("document.docx", WordProcessingLoadOptions())
before_edit = editor.edit()  # create an EditableDocument instance
```

## Obtaining resources

Now, when the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance is ready, it is possible to obtain resources from it, and [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) provides several ways for this.

First of all, resources can be retrieved by their type. [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) exposes an iterable collection for every resource type:

* `images` — all images, raster and vector.
* `fonts` — all fonts.
* `css` — the CSS stylesheets, where each item represents one stylesheet.
* `audio` — the MP3 audio files.

Secondly, completely all resources may be obtained with a single property — `all_resources`. It returns everything above, combined, and in fact is a concatenation of the previous collections.

All these collections can be iterated with `for` loops and measured with `len()`:

```python
images = before_edit.images
fonts = before_edit.fonts
stylesheets = before_edit.css
audio_files = before_edit.audio
all_together = before_edit.all_resources

for one_image in images:
    print("image resource:", one_image)
```

## CSS resources

There is also a dedicated way for the stylesheets. The reason is that stylesheets can contain external resources too, presented as links with URLs — for example images, fonts, and other stylesheets. In such a case it may be necessary to adjust such a link. For coping with this, [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) contains the [`get_css_content()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getcsscontent) method. Without arguments it returns the stylesheets as-is; it can also accept prefixes for external images and fonts referenced from the stylesheets:

```python
stylesheets_without_prefixes = before_edit.get_css_content()
external_images_prefix = "http://www.mywebsite.com/images/id="
external_fonts_prefix = "http://www.mywebsite.com/fonts/id="
stylesheets_with_prefixes = before_edit.get_css_content(external_images_prefix, external_fonts_prefix)
```

## Complete code example

The example below loads a document, opens it for editing, and reports how many resources of each kind were extracted.

{{< tabs "code-example-working-with-resources">}}
{{< tab "working_with_resources.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def working_with_resources():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        before_edit = editor.edit()

        # Inspect the extracted resources by their type
        print("Images:", len(before_edit.images))
        print("Stylesheets:", len(before_edit.css))
        print("Fonts:", len(before_edit.fonts))
        print("All resources:", len(before_edit.all_resources))

        # Enumerate the stylesheets of the document
        for one_stylesheet in before_edit.css:
            print("stylesheet resource:", one_stylesheet)

        before_edit.dispose()

if __name__ == "__main__":
    working_with_resources()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/editabledocument/working-with-resources/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "working-with-resources.txt" >}}  
```text
Images: 4
Stylesheets: 1
Fonts: 0
All resources: 5
stylesheet resource: GroupDocs.Editor.HtmlCss.Resources.Textual.CssText
```
[Download full output](/editor/python-net/_output_files/developer-guide/editabledocument/working-with-resources/working_with_resources/working-with-resources.txt)
{{< /tab >}}
{{< /tabs >}}
