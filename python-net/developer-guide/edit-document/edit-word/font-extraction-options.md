---
id: font-extraction-options
url: editor/python-net/font-extraction-options
title: Font extraction options
linkTitle: Font extraction options
weight: 5
description: "Learn this guide to know about extracting fonts from input Word document when editing with GroupDocs.Editor for Python via .NET API."
keywords: Edit document with fonts, Font extraction, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
## Introduction

WordProcessing documents usually contain text content, while every piece of text should be represented with some font. There may be used system fonts (installed in the operating system), but also custom fonts, which are not installed in the system. On the other hand, lots of WordProcessing formats like DOCX and ODT have an ability to store fonts inside the document itself; such fonts, stored as binary resources inside WordProcessing files, are called *embedded*. Embedded fonts are the only choice for the end-user to use some specific fonts, which are not installed in the system. But any font can be embedded into the document. This means that, for example, there can be a situation when the same font is installed in the system, and at the same time embedded in the WordProcessing document.

In the MS Windows operating system before Windows 10 (and MS Windows Server 2016) there is only one location, where fonts installed in the system are stored: the system folder "`%windir%\fonts`", fonts from which are available for all users. However, starting from MS Windows 10 every user has its own local fonts storage: "`%userprofile%\AppData\Local\Microsoft\Windows\Fonts`" (along with the common folder, which still exists, of course).

When we're saying "the WordProcessing document uses some font", this statement can be treated differently, because the WordProcessing document can use fonts differently. From the "broad" point of view, every font, which is referenced in the WordProcessing somehow, applied to text content or is a part of some style, is used by the document. In counterpart, from the "narrow" point of view, only the font which is applied to some text content is used by the document. For example, there can be a scenario when a user created a WordProcessing document, writes some text, creates some style named "*Style1*" with a specific font "*Font1*", and applies this style "*Style1*" to the text. In this situation that specific "*Font1*" is used by the document from all points of view. However, after this, the user edits the document and removes all the textual content, which uses the "*Style1*" style, but, what is important, the "*Style1*" is not removed from the document, — it still exists and holds a reference to the "*Font1*". In this situation, from the narrow point of view, the document doesn't use "*Font1*", because there is no piece of text which uses this font, even while "*Font1*" is still a part of the "*Style1*" style, which is a part of the document.

## How to extract fonts

GroupDocs.Editor has an ability to extract fonts from a WordProcessing document and represent them as resources in the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance; when this instance will be used for generating HTML markup, all font resources will be present and correctly linked to the HTML content. GroupDocs.Editor is able to extract embedded fonts from the WordProcessing document, as well as extract installed fonts from the system.

There are two public properties responsible for working with fonts, both of them are located in the [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class:

```python
font_extraction       # FontExtractionOptions enum
extract_only_used_font  # bool
```

`FontExtractionOptions` is a public enum, located in the `groupdocs.editor.options` module. By default, when an instance of the [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class is created, this enum has a default value `NotExtract`, which means do not extract any fonts: neither from the document nor from the system. Other values are the following:

* `ExtractAllEmbedded`. In this case GroupDocs.Editor extracts all the fonts embedded in the input WordProcessing document regardless of the fact whether some of them are installed in the system or not. In other words, the converter finds and extracts all 100% of font resources which are embedded into the input WordProcessing document, but it doesn't determine whether they are system or custom. Because of this fact the converter doesn't touch the Windows Registry or system folders at all.
* `ExtractEmbeddedWithoutSystem`. Unlike the previous option, in this case GroupDocs.Editor not only simply extracts all embedded fonts, but also checks every font whether it is system or not. If some embedded font is system-installed, it will be ignored and will not be present in the font resources inside the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument). In order to detect which font is system or not, the converter tries to obtain a list of all system fonts by using the Windows Registry and system folders, and then compares this list with a set of embedded fonts. As a result, only a subset of those embedded fonts which were not found in the system will be returned.
* `ExtractAll`. When this option is selected, GroupDocs.Editor tries to extract absolutely all fonts which are used in the input document, regardless of their nature: embedded or system. In order to do this, GroupDocs.Editor analyzes an input WordProcessing document and finds all fonts which are used there (from the "broad" point of view). If some of these used fonts are embedded in the document, GroupDocs.Editor extracts and uses them. If the document contains no embedded fonts at all, or a collection of embedded fonts is present but doesn't cover all used fonts in the document, GroupDocs.Editor tries to extract these font resources from the system by using the Windows Registry and system folders. This option is perfectly useful in case clients want to make sure that the input document will have a perfect representation for every end-user regardless of what fonts are installed on the client machine or not.

The second property, a boolean flag named `extract_only_used_font`, by default has a `False` value, which means that all fonts used in the WordProcessing document from the "broad" point of view will be processed. But when it is enabled by setting a `True` value, GroupDocs.Editor processes only those fonts which are used in the WordProcessing document from the "narrow" point of view, i.e. only those fonts which are applied to some textual content in the document.

These two public options work in conjunction. In particular:

* If the `font_extraction` enum has a `NotExtract` value, then the value of the `extract_only_used_font` property will be ignored.
* If the `font_extraction` enum has an `ExtractAllEmbedded` value, and the `extract_only_used_font` flag is set to `True`, GroupDocs.Editor will not extract 100% of all embedded fonts, but only a subset of those embedded which are used in the WordProcessing document from the "narrow" point of view.
* If the `font_extraction` enum has an `ExtractEmbeddedWithoutSystem` value, and the `extract_only_used_font` flag is set to `True`, GroupDocs.Editor returns a subset of the fonts returned by this option when `extract_only_used_font` is `False` — only those fonts will be returned which are simultaneously embedded in the WordProcessing document, not installed in the system, and used in the document from the "narrow" point of view.
* If the `font_extraction` enum has an `ExtractAll` value, and the `extract_only_used_font` flag is set to `True`, GroupDocs.Editor first of all forms a list of fonts which are used in the document from the "narrow" point of view, and then only these fonts are returned from the embedded or (if not found in embedded) from system-installed font storages.

The `font_extraction` enum value is assigned as illustrated below (the `FontExtractionOptions` enum comes from the `groupdocs.editor.options` module):

```python
from groupdocs.editor.options import WordProcessingEditOptions, FontExtractionOptions

edit_options = WordProcessingEditOptions()
edit_options.font_extraction = FontExtractionOptions.EXTRACT_ALL
```

## Complete example

The runnable example below loads the sample document and edits it with `font_extraction` set to `ExtractAll` and the `extract_only_used_font` flag enabled, then prints the number of extracted font resources.

{{< tabs "code-example-font-extraction-options">}}
{{< tab "extract_used_fonts.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingEditOptions, FontExtractionOptions

def extract_used_fonts():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Extract the fonts used in the document; extract_only_used_font keeps only
    # those applied to textual content (the "narrow" point of view)
    edit_options = WordProcessingEditOptions()
    edit_options.font_extraction = FontExtractionOptions.EXTRACT_ALL
    edit_options.extract_only_used_font = True

    with Editor("./sample-document.docx") as editor:
        editable = editor.edit(edit_options)
        fonts_count = len(list(editable.fonts))
        print("Extracted font resources:", fonts_count)
        editable.dispose()

if __name__ == "__main__":
    extract_used_fonts()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/font-extraction-options/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-used-fonts.txt" >}}  
```text
Extracted font resources: 6
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/font-extraction-options/extract_used_fonts/extract-used-fonts.txt)
{{< /tab >}}
{{< /tabs >}}
