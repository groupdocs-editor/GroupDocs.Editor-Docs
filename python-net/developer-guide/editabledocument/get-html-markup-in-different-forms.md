---
id: get-html-markup-in-different-forms
url: editor/python-net/get-html-markup-in-different-forms
title: Get HTML markup in different forms
linkTitle: Get HTML markup in different forms
weight: 1
description: "Read this article to learn how to get edited document HTML markup - body without head tag, content in raw and base64 form and others using GroupDocs.Editor for Python via .NET API."
keywords: Get html content, get html body, get html markup, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
> This demonstration shows how to open an input document, convert it to an intermediate [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), and get HTML markup in different forms depending on client requirements.

## Preparations

When an input document is loaded into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class and opened for editing by transforming it to the intermediate [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class, it is possible to generate and get HTML markup in different forms.

First of all the user needs to load the document into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class and open it for editing, which is demonstrated in the code below.

```python
from groupdocs.editor import Editor
from groupdocs.editor.options import WordProcessingLoadOptions

load_options = WordProcessingLoadOptions()
editor = Editor("document.docx", load_options)  # passing path and load options to the constructor
document = editor.edit()  # opening the document for editing
```

The piece of code above prepares a ready-to-use instance of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class, that contains the original document in its own intermediate format and is able to generate HTML markup in different forms.

## Getting the whole HTML content

The most default and standard method for generating HTML markup is the parameterless [`get_content()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getcontent) method:

```python
html_content = document.get_content()
```

If the document has external resources (stylesheets, fonts, images), they are referenced via different HTML elements: stylesheets are specified through `LINK` elements, while images — through `IMG`. When using the [`get_content()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getcontent) method, such external resources will be referenced by external links. For example:

```html
<link href="stylesheet.css" rel="stylesheet"/>
<img src="image.png"/>
```

## Getting HTML BODY content

A lot of HTML WYSIWYG editors are not able to process the whole HTML document, with a `HEAD` section and so on. They are only able to process the inner content of the HTML->BODY element. In order to obtain such part of the HTML markup, the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class contains the [`get_body_content()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getbodycontent) method:

```python
body_content = document.get_body_content()
```

It is also possible to pass an external images template, which is added to every URL in the `src` attribute of every `IMG` tag found inside the HTML->BODY markup:

```python
external_images_template = "http://www.mywebsite.com/images/id="
prefixed_body_content = document.get_body_content(external_images_template)
```

## Getting the stylesheet content

The [`get_css_content()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getcsscontent) method returns the CSS stylesheet(s) of the document. It can be called without arguments, or with prefixes for external images and fonts referenced from the stylesheets:

```python
css_content = document.get_css_content()
```

## Getting base64-encoded content

Sometimes it is necessary to obtain all the content of the whole document with all used resources in a single string. GroupDocs.Editor allows to do this with the [`get_embedded_html()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getembeddedhtml) method:

```python
embedded_html_content = document.get_embedded_html()
```

In such a string all stylesheets will be placed into `STYLE` elements in the HTML->HEAD section, all images in `IMG` elements will be serialized with base64 encoding and placed directly in the `src` attributes. All fonts and images, which are used in the stylesheets, will also be serialized and stored in the appropriate locations. Such a string is fully autonomous and self-sufficient.

## Complete code example

The example below loads a document, opens it for editing, and prints the lengths of the HTML markup obtained in different forms.

{{< tabs "code-example-get-html-markup-in-different-forms">}}
{{< tab "get_html_markup_in_different_forms.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def get_html_markup_in_different_forms():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        document = editor.edit()

        # Generate the HTML markup in different forms and inspect their sizes
        print("Whole content length:", len(document.get_content()))
        print("Body content length:", len(document.get_body_content()))
        print("Embedded (base64) content length:", len(document.get_embedded_html()))

        # get_css_content() returns the stylesheet(s) of the document
        css = document.get_css_content()
        print("CSS stylesheets count:", len(css))

        document.dispose()

if __name__ == "__main__":
    get_html_markup_in_different_forms()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/editabledocument/get-html-markup-in-different-forms/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "get-html-markup-in-different-forms.txt" >}}  
```text
Whole content length: 31654
Body content length: 31461
Embedded (base64) content length: 95550
CSS stylesheets count: 1
```
[Download full output](/editor/python-net/_output_files/developer-guide/editabledocument/get-html-markup-in-different-forms/get_html_markup_in_different_forms/get-html-markup-in-different-forms.txt)
{{< /tab >}}
{{< /tabs >}}
