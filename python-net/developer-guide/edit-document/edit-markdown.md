---
id: edit-markdown
url: editor/python-net/edit-markdown
title: Edit Markdown documents
linkTitle: Edit Markdown documents
weight: 95
description: "This guide demonstrates how to edit the content of Markdown documents/files like common text documents using GroupDocs.Editor for Python via .NET."
keywords: Edit Markdown, Edit Markdown file, Edit Markdown document, Edit MD, Edit MD file, Edit MD document, Import Markdown, Export markdown, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit Markdown document in the GroupDocs.Editor
        description: How to edit Markdown document using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to edit content of the Markdown document in the GroupDocs.Editor in Python
        description: Learn how to edit the content of a Markdown (MD) document with different editing options using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load the desired Markdown document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired Markdown document into it by a file path or a stream.
        - name: Prepare a MarkdownEditOptions class
          text: Create an instance of the MarkdownEditOptions class. If the input Markdown document has external images, specify an image load callback so that GroupDocs.Editor can load them.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the editor.edit method with a previously prepared MarkdownEditOptions and obtain an instance of the EditableDocument class, which is ready for editing.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable class methods of the class.
        - name: Prepare a MarkdownSaveOptions class
          text: Create an instance of the MarkdownSaveOptions class and adjust its properties to meet your needs if necessary. Select saving images as embedded in base64 encoding (export_images_as_base64 property) or as external files in a specified folder (images_folder property). All properties are optional.
        - name: Save the edited document with the editor.save method
          text: Pass an instance of EditableDocument with the content of the edited document, an instance of MarkdownSaveOptions, and a destination byte stream or file path to the editor.save method for saving the document.
---
> This example demonstrates the standard open-edit-save pipeline with Markdown (MD) documents, using different options on every step.

## Introduction

[Markdown](https://docs.fileformat.com/word-processing/md/) is a lightweight markup language, which has become popular lately. Markdown files with an `*.md` extension are actually plain text files that contain special syntax, and support text formatting, tables, lists, images, and so on. There are several dialects of Markdown, including GFM, CommonMark, Multi-Markdown, and so on.

GroupDocs.Editor for Python via .NET fully supports the Markdown format on both import and export, as well as its auto-detection. GroupDocs.Editor supports the following Markdown features, which mostly follow the CommonMark specification: headings, blockquotes, code blocks, horizontal rules, bold emphasis, italic emphasis, strikethrough formatting, numbered and bulleted lists, tables, internal images (stored with base64 encoding), and external images.

## Loading

Loading Markdown documents into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class is usual and the same as for other formats. There are no dedicated load options for the Markdown format; it is enough to specify the file itself through a file path or a byte stream.

```python
from groupdocs.editor import Editor

# Load from a file path
editor_from_path = Editor("article.md")

# Load from a binary stream
with open("article.md", "rb") as stream:
    editor_from_stream = Editor(stream)
```

## Editing

There is a special class [`MarkdownEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/markdowneditoptions) for editing Markdown files. As always, it is not mandatory when editing a document, so the parameterless [`editor.edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) overload may be used — GroupDocs.Editor will automatically detect the format and apply the default options.

However, specifying the custom [`MarkdownEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/markdowneditoptions) may be vital when an input Markdown file has external images. "External" means that images are stored somewhere else, and in the Markdown code there are _links_ to these images. In order to point GroupDocs.Editor to all external images, the `image_load_callback` property of [`MarkdownEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/markdowneditoptions) should be specified with an implementation of the image load callback interface, which provides the image data to GroupDocs.Editor when it encounters a link to an external image. This is an advanced scenario that requires a callback object; a schematic, non-runnable illustration is shown below.

```python
from groupdocs.editor.options import MarkdownEditOptions

# Assume image_loader is an object that implements the Markdown image load callback,
# returning binary data for each external image referenced from the Markdown file
edit_options = MarkdownEditOptions()
edit_options.image_load_callback = image_loader
opened = editor.edit(edit_options)
```

## Saving

GroupDocs.Editor also supports saving into the Markdown format. Like for any other format, for saving into Markdown the user must create an instance of the [`MarkdownSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/markdownsaveoptions) class and specify it in the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.

If the document destined for saving in the Markdown format has images, they can be resolved in one of three ways:
1. Ignore images — they will be absent.
2. Save images inside the Markdown code, where they will be stored in base64 encoding.
3. Save images as separate files in a specified folder, and in the Markdown code there will be references to these image files.

For this the [`MarkdownSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/markdownsaveoptions) class has several properties. `export_images_as_base64` is a boolean flag, by default set to `False`. If set to `True`, the content of the images will be injected inside the output Markdown as base64. This flag has the highest priority, and when it is `True` the `images_folder` property is ignored. The `images_folder` property, in turn, works when `export_images_as_base64` is set to `False`; if specified, it should contain a valid path to an existing folder where GroupDocs.Editor should save the images.

## Roundtrip

Because the Markdown format is supported on both import and export, it is possible to perform a roundtrip scenario — open a Markdown file for editing, edit it, and then save the edited version back to the Markdown format. The runnable example below demonstrates such a scenario with a Markdown file that has no external images, so no image load callback is needed.

{{< tabs "code-example-edit-markdown">}}
{{< tab "edit_markdown.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.options import MarkdownEditOptions, MarkdownSaveOptions

def edit_markdown():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load an input Markdown file into the Editor
    with Editor("./sample-article.markdown") as editor:
        # Create the Markdown edit options
        edit_options = MarkdownEditOptions()

        # Edit the Markdown document and obtain an EditableDocument
        editable = editor.edit(edit_options)

        # Edit the content programmatically (in practice this is done in a WYSIWYG-editor)
        html = editable.get_embedded_html()
        edited = EditableDocument.from_markup(html.replace("</p>", " (edited)</p>", 1))

        # Create Markdown save options with images embedded as base64
        save_options = MarkdownSaveOptions()
        save_options.export_images_as_base64 = True

        # Save the edited document back to the Markdown format
        editor.save(edited, "./edited-article.md", save_options)

        editable.dispose()
        edited.dispose()

if __name__ == "__main__":
    edit_markdown()
```
{{< /tab >}}
{{< tab "sample-article.markdown" >}}  
{{< tab-text >}}
`sample-article.markdown` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-markdown/sample-article.markdown) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-article.md" >}}  
```text
﻿# Sample Article
This is a **sample Markdown document** used by the GroupDocs.Editor for Python via .NET examples.
## Introduction
GroupDocs.Editor converts Markdown to editable HTML and back, so you can edit content programmatically or in any WYSIWYG editor.
## Features
- Headings, paragraphs, and emphasis
- Ordered and unordered lists
- Links such as [GroupDocs](https://www.groupdocs.com)
- Inline code and fenced code blocks
## Example List
[TRUNCATED]
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-markdown/edit_markdown/edited-article.md)
{{< /tab >}}
{{< /tabs >}}

In this example the same Markdown file is opened for editing and then saved back into the Markdown format with all images embedded as base64. The `export_images_as_base64` flag could be replaced with the `images_folder` property to store images as separate files instead.
