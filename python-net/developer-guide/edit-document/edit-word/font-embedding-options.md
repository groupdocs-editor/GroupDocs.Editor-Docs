---
id: font-embedding-options
url: editor/python-net/font-embedding-options
title: Font embedding options
linkTitle: Font embedding options
weight: 6
description: "Learn this guide to know about embedding fonts into output Word document when editing with GroupDocs.Editor for Python via .NET API."
keywords: Edit document with fonts, Font embedding, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
## Introduction

GroupDocs.Editor for Python via .NET is able, along with font extraction, to embed fonts into the output WordProcessing document. This feature can be treated as similar to the Microsoft Word feature to embed fonts into the saved document after its editing.

In counterpart to the _font extraction_ mechanism, which is responsible for extracting font resources from the input WordProcessing document into the intermediate [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), the font embedding mechanism is responsible for transferring fonts from the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) into the output WordProcessing document through embedding. When user obtains a document content, edited in the WYSIWYG-editor, and creates a new [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance with the edited document content, he then needs to invoke the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor).[`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method, which will generate an output WordProcessing document from the specified [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) and [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class. The [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class is the point where font embedding, which is disabled by default, can be enabled and tuned.

When font embedding is enabled, GroupDocs.Editor analyzes a document content of the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) and forms a list of all fonts, which are used in the document body, for example, in paragraphs, labels and so on. After this GroupDocs.Editor tries to find all of these fonts in the font resources of the input [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) (the `fonts` collection). If the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) contains all font resources, that are used in the document body, they will be embedded into the output WordProcessing document. However, if there are some fonts, used in the document content, which have no corresponding font resources in the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), then GroupDocs.Editor tries to find them in the OS (where GroupDocs.Editor is running), extract and embed into the output document.

There may be a situation, when during the document editing in the WYSIWYG-editor the end-user deleted some text, for example a paragraph, with some specific font, and after the deletion this font is no longer used, while the corresponding font resource is still left in the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument). During font embedding such dangling font resources are detected and ignored, so they will not be passed into the output document.

When working with fonts, GroupDocs.Editor must work with so-called _system fonts_. System fonts are the fonts which are the most vital for the operating system; for example, they are used in Windows Explorer, Console, and system built-in applications of the operating system. When performing font embedding, user can define whether to embed the system fonts into the output document or not. Including system fonts may be useful if the user is on an East Asian system and wants to create a document that is readable by others who do not have fonts for that language on their system. For example, a user on a Japanese system could choose to embed the fonts in a document so that the Japanese document would be readable on all systems.

## Usage

In order to support font embedding, the [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class contains a `font_embedding` property. This property is of the `FontEmbeddingOptions` enum type with three possible values. By default, when creating a new instance of the [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class, the value of the `font_embedding` property is set to `NotEmbed`, — in this case GroupDocs.Editor does not embed fonts at all.

The `FontEmbeddingOptions` enum contains two values for embedding fonts, which are almost the same, but with one slight difference:
* `EmbedAll`. As described above, when this option is chosen, GroupDocs.Editor analyzes the document content for used fonts, and then looks for these fonts in the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) and, if required, in the operating system. This option resembles the "Embed fonts in the file" option with all sub-options turned off in Microsoft Word 2007 and higher.
* `EmbedWithoutSystem`. This option is almost the same as the previous one, but with one little distinction: it does not embed system fonts. This option resembles the "Embed fonts in the file" option with the enabled "Do not embed common system fonts" sub-option in Microsoft Word 2007 and higher.

The snippet below illustrates how the `font_embedding` property is assigned on the [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) instance (the `FontEmbeddingOptions` enum comes from the `groupdocs.editor.options` module):

```python
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions, FontEmbeddingOptions

save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)

# By default fonts are not embedded
# save_options.font_embedding == FontEmbeddingOptions.NOT_EMBED

# Embed all used fonts, including system fonts
save_options.font_embedding = FontEmbeddingOptions.EMBED_ALL

# Embed all used fonts, but except system fonts
save_options.font_embedding = FontEmbeddingOptions.EMBED_WITHOUT_SYSTEM
```

## Complete example

The runnable example below loads the sample document, edits it, and saves the result to DOCX. To enable font embedding, assign the `font_embedding` property as shown in the snippet above.

{{< tabs "code-example-font-embedding-options">}}
{{< tab "embed_fonts.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

def embed_fonts():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        # Open the document for editing and produce the modified content
        original = editor.edit()
        modified = EditableDocument.from_markup(original.get_embedded_html())

        # Prepare save options (font_embedding can be assigned here)
        save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)

        editor.save(modified, "./fonts-output.docx", save_options)
        print("Saved the edited document to DOCX")

        original.dispose()
        modified.dispose()

if __name__ == "__main__":
    embed_fonts()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/font-embedding-options/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "fonts-output.docx" >}}  
```text
Binary file (DOCX, 49 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/font-embedding-options/embed_fonts/fonts-output.docx)
{{< /tab >}}
{{< /tabs >}}
