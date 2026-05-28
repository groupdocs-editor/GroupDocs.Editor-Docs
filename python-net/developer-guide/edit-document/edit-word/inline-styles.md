---
id: inline-styles
url: editor/python-net/inline-styles
title: Enabling inline CSS styles
linkTitle: Enabling inline CSS styles
weight: 12
description: "This article describes the procedure of enabling the inline styles option for the WordProcessing documents in order to store the CSS styles not in the external stylesheet, but directly inside the HTML markup."
keywords: Inline styles, inline CSS, style attribute, CSS styles in HTML markup, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
In GroupDocs.Editor, the editing operation implies that the source document is converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, and this instance can emit HTML- and CSS-markup to be placed in the client-side WYSIWYG-editor, then the end-user edits this document in the web-browser, and finally the document is saved — this implies converting document content from HTML/CSS back to the original (or some other) format.

By default the HTML-version of a document, which is obtained from the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class, has its CSS styles placed in an external stylesheet file. The HTML file in this case contains a reference to the external stylesheet.

GroupDocs.Editor provides an ability to enable the "inline styles" mode for all documents which belong to the [family of WordProcessing formats](https://docs.fileformat.com/word-processing/). In this mode only [WordProcessing styles are exported to the external stylesheet]({{< ref "editor/python-net/developer-guide/edit-document/edit-word/styles-export.md" >}}), while all other styles — text and paragraph formatting, list and table settings — are exported directly to the HTML markup. They become _inline CSS styles_, which means that in this case the styles are saved in the ["style" HTML attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/style) for every HTML element. As a result, an HTML document generated with the enabled inline styles option has larger HTML-markup and smaller CSS-markup, compared to one where this option is disabled.

The inline styles option is represented as a public boolean property in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class. By default it is disabled (`False`).

```python
from groupdocs.editor.options import WordProcessingEditOptions

edit_options = WordProcessingEditOptions()
edit_options.use_inline_styles = True
```

The example below shows opening and editing a single WordProcessing document twice: the first time in default style, and then with enabled inline styles.

{{< tabs "code-example-inline-styles">}}
{{< tab "use_inline_styles.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingEditOptions

def use_inline_styles():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Default edit options, where all styles are external
    options_external_styles = WordProcessingEditOptions()

    # Custom edit options, where most of the styles are inline
    options_inline_styles = WordProcessingEditOptions()
    options_inline_styles.use_inline_styles = True

    with Editor("./sample-document.docx") as editor:
        doc_external = editor.edit(options_external_styles)
        doc_inline = editor.edit(options_inline_styles)

        # Get only the HTML-markup for both variants
        html_external = doc_external.get_content()
        html_inline = doc_inline.get_content()

        print("External styles HTML length:", len(html_external))
        print("Inline styles HTML length:", len(html_inline))

        doc_external.dispose()
        doc_inline.dispose()

if __name__ == "__main__":
    use_inline_styles()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/inline-styles/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "use-inline-styles.txt" >}}  
```text
External styles HTML length: 31654
Inline styles HTML length: 48162
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/inline-styles/use_inline_styles/use-inline-styles.txt)
{{< /tab >}}
{{< /tabs >}}

It is worth emphasizing that no matter where the CSS styles are located — inside the HTML-markup or in the external file — the final representation of the HTML-document in the web-browser will be identical.

Also please take into account that this feature exists only for the WordProcessing formats family.
