---
id: generating-slides-preview-for-presentation
url: editor/python-net/generating-slides-preview-for-presentation
title: Generating slides preview for presentation
linkTitle: Generating slides preview for presentation
weight: 4
description: "This article describes how to generate a preview for any slide for the existing PowerPoint presentation in SVG format using GroupDocs.Editor for Python via .NET"
keywords: preview slide powerpoint presentation SVG, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
GroupDocs.Editor for Python via .NET allows generating the preview for any slide in the presentation document in SVG format. This feature allows the end-user to view and inspect the content of the presentation without actually sending it for edit. This generated slide preview cannot be edited, but it can be viewed in any desktop or online image viewer as well as in the browser (because any modern browser actually supports SVG format).

This feature allows end-users to generate a preview for any slide within the presentation regardless of the licensing mode of the GroupDocs.Editor: it works the same for both trial and licensed mode, there are no trial limitations for this feature. While generating the slides preview, the GroupDocs.Editor doesn't write off the consumed bytes or credits.

For generating the slides preview the `generate_preview(slide_index)` method from the [`PresentationDocumentInfo`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.metadata/presentationdocumentinfo/) object should be used, and slide is specified by its zero-based _index_ (do not confuse with the slide _numbers_, which are 1-based). If the specified index is lesser than 0 or exceeds the number of slides within a given presentation, then an exception will be thrown. Slide previews can be generated for both encoded and unencoded presentations; for encoded the end-user must specify a valid password in the [`get_document_info`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo/) method.

The `generate_preview(slide_index)` method returns a slide preview as an SVG vector image, that is encapsulated in the `SvgImage` class. This class has all necessary methods and properties to obtain the content of an SVG image in any desired form, save it to disk and so on.

The snippet below illustrates opening an unprotected presentation file and generating the previews for every slide. Then these previews are saved to the disk.

```python
import os
from groupdocs.editor import Editor

# Obtain a valid path to the presentation file
input_path = "./sample-presentation.pptx"
output_folder = "./previews"

# Load the file to the Editor constructor
with Editor(input_path) as editor:
    # Get document info for this file
    info_slides = editor.get_document_info()

    # Get the number of all slides
    slides_count = info_slides.page_count

    # Iterate through all slides and generate the preview on every iteration
    for i in range(slides_count):
        # Generate one preview as an SVG image by slide index
        one_svg_preview = info_slides.generate_preview(i)

        # Save it to a file
        one_svg_preview.save(os.path.join(output_folder, one_svg_preview.filename_with_extension))
```

The slides preview feature is by its essence a method in the existing `PresentationDocumentInfo` object, that obtains a slide index and returns an instance of the `SvgImage` class. If the end-user needs to obtain a preview of the slide in a raster format, but not in the vector, the `SvgImage` class also provides a method to convert the SVG content to the PNG format.

The complete example below loads a presentation, opens its first slide for editing, and prints the length of the generated HTML content. This roundtrip confirms that the presentation is read correctly before any preview is generated.

{{< tabs "code-example-generating-slides-preview-for-presentation">}}
{{< tab "generating_slides_preview_for_presentation.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import PresentationEditOptions

def generating_slides_preview_for_presentation():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load the presentation file to the Editor constructor
    with Editor("./sample-presentation.pptx") as editor:
        # Prepare edit options and select the 1st slide
        edit_options = PresentationEditOptions()
        edit_options.slide_number = 0  # index is 0-based, so this is the 1st slide

        # Open the slide for editing
        slide = editor.edit(edit_options)

        # Obtain the HTML content of the slide
        content = slide.get_content()
        print("Slide HTML content length:", len(content))

        slide.dispose()

if __name__ == "__main__":
    generating_slides_preview_for_presentation()
```
{{< /tab >}}
{{< tab "sample-presentation.pptx" >}}  
{{< tab-text >}}
`sample-presentation.pptx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-powerpoint/generating-slides-preview-for-presentation/sample-presentation.pptx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "generating-slides-preview-presentation.txt" >}}  
```text
Slide HTML content length: 8422
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-powerpoint/generating-slides-preview-for-presentation/generating_slides_preview_for_presentation/generating-slides-preview-presentation.txt)
{{< /tab >}}
{{< /tabs >}}
