---
id: enabling-language-information
url: editor/python-net/enabling-language-information
title: Enabling language information
linkTitle: Enabling language information
weight: 3
description: "Following this guide you will learn how to edit Word document using locale info, apply spell-checkers to a document content written in different languages using GroupDocs.Editor for Python via .NET API."
keywords: Edit Word document with language, Edit Word document with locale, Edit Word, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
Documents of all WordProcessing formats can contain text in different languages. But, unlike the plain text documents (TXT), WordProcessing documents also contain a metadata about specific language (locale) of every piece of text. [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) allows to extract and export this language information. For achieving this the [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class contains the [`enable_language_information`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions/) public boolean property:

```python
from groupdocs.editor.options import WordProcessingEditOptions

edit_options = WordProcessingEditOptions()
edit_options.enable_language_information = True
```

By default its value is `False`, which means that language metadata will not be extracted. But when this option is manually enabled, GroupDocs.Editor extracts locale info for every piece of textual content and preserves it in the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, when document is edited. Finally, when user has obtained the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance and is generating the HTML markup for transferring it to the WYSIWYG HTML-editor in order to make document editable in the browser, this language information is represented as the 'lang' HTML attributes with appropriate values inside the SPAN HTML elements.

Enabling language information is useful when document contains different text parts in different languages; if document has text in some single language, this option does not have much sense and thus is disabled by default.

However, when document is multi-language, enabling language information may be very suitable for two scenarios:

* It eases spell checking for client-side JavaScript spell-checkers, that are working in the browser. However, this is very dependent on specific spell-checker, as not all spell-checkers are able to grab values from "lang" attributes or even use language information at all.
* It improves the quality of output WordProcessing document in roundtrip scenarios. When a document with enabled [`enable_language_information`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions/) option was converted to the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, then HTML markup was generated, edited in some HTML-editor, and then a new instance of [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class was created from the edited markup, language metadata in "lang" attributes is still preserved. When the edited [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) will be converted back to the output document of some WordProcessing format like DOCX or RTF, the textual content inside it will have connections to the correct locale.

## Complete example

The example below loads the sample document, edits it with `enable_language_information` turned on, and prints the length of the generated HTML markup that carries the language metadata.

{{< tabs "code-example-enabling-language-information">}}
{{< tab "enable_language_information.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingEditOptions

def enable_language_information():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Enable extraction of language (locale) information
    edit_options = WordProcessingEditOptions()
    edit_options.enable_language_information = True

    with Editor("./sample-document.docx") as editor:
        editable = editor.edit(edit_options)
        html = editable.get_content()
        print("HTML markup with language information, length:", len(html))
        editable.dispose()

if __name__ == "__main__":
    enable_language_information()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/enabling-language-information/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "enable-language-information.txt" >}}  
```text
HTML markup with language information, length: 35801
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/enabling-language-information/enable_language_information/enable-language-information.txt)
{{< /tab >}}
{{< /tabs >}}
