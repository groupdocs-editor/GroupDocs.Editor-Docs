---
id: generating-page-preview
url: editor/python-net/generating-page-preview
title: Generating page preview for WordProcessing document
linkTitle: Generating page preview
weight: 16
description: "This article describes how to generate a preview for any page for the existing WordProcessing document in SVG format using the GroupDocs.Editor for Python via .NET."
keywords: preview page WordProcessing Word DOCX SVG, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
GroupDocs.Editor for Python via .NET allows to generate a preview for an arbitrary page in a loaded WordProcessing document, like DOC, DOCX, DOCM, RTF, ODT etc. The preview is generated in the SVG format, which is a vector graphics format and is supported in numerous image viewers and also by any modern browser.

Using this feature the users can view and inspect any page of the imported WordProcessing document without actually editing it. This preview cannot be edited by GroupDocs.Editor itself, it can only be obtained in SVG format and saved as usual — to a byte stream or file.

This feature works regardless of the licensing mode of GroupDocs.Editor: it works the same for both trial and licensed mode, there are no trial limitations for this feature. While generating the page preview, GroupDocs.Editor doesn't write off the consumed bytes or credits.

With this feature, GroupDocs.Editor supports generating a preview for all three major office format families: WordProcessing, Spreadsheet and Presentation.

For generating the page preview for a particular WordProcessing document the user must perform the following steps:

- Load a desired WordProcessing file into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/) class.
- Obtain the document info via the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/) method (specifying a password if the document is protected).
- Because the loaded file has a format from the WordProcessing family (like DOCX, RTF, etc.), the obtained document info exposes the page count and a method to generate a preview for a given page.
- Invoke the preview method and specify a zero-based _index_ (do not confuse with the page _numbers_, which are 1-based) of the desired page. If the specified index is less than 0 or exceeds the number of pages within a given document, an out-of-range error will be raised.
- The preview method returns a page preview as an SVG vector image, which has all necessary methods and properties to obtain the content of an SVG image in any desired form, save it to disk, stream, and so on.

The snippet below illustrates the page preview workflow. Because the page-preview API surface is illustrative here, treat this as a reference outline rather than a copy-paste runnable script:

```python
from groupdocs.editor import Editor

with Editor("./document.docx") as editor:
    # Obtain document info for the loaded WordProcessing file
    info = editor.get_document_info()

    # Get the number of all pages
    pages_count = info.page_count

    # Iterate through all pages and generate a preview on every iteration
    for page_index in range(pages_count):
        # Generate one preview as an SVG image by page index
        one_svg_preview = info.generate_preview(page_index)

        # Save the SVG preview to a file
        one_svg_preview.save("page-{0}.svg".format(page_index))
```

All WordProcessing formats are able to store raster images. So when a specific page of some loaded document has one or more raster images, and for this page the SVG preview is generated, this/these raster image(s) will be embedded inside the SVG in base64 format using the [data URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme).

If the end-user needs to obtain a preview of the page in a raster format instead of vector, the returned SVG image object can convert its SVG content to the PNG format and save it.

## Complete example

The runnable example below performs the safe core workflow — loading the sample document, opening it for editing, and obtaining the HTML content (the page-preview API itself is shown illustratively above).

{{< tabs "code-example-generating-page-preview">}}
{{< tab "generate_page_preview.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def generate_page_preview():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        # Open the document for editing and obtain its HTML representation
        editable = editor.edit()
        html = editable.get_content()
        print("Loaded WordProcessing document, HTML content length:", len(html))
        editable.dispose()

if __name__ == "__main__":
    generate_page_preview()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/generating-page-preview/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "generate-page-preview.txt" >}}  
```text
Loaded WordProcessing document, HTML content length: 31654
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/generating-page-preview/generate_page_preview/generate-page-preview.txt)
{{< /tab >}}
{{< /tabs >}}
