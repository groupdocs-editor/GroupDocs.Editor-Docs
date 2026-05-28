---
id: locales-for-output-document
url: editor/python-net/locales-for-output-document
title: Locales for output document
linkTitle: Locales for output document
weight: 8
description: "This guide demonstrates how to edit RTL documents and specify locale for Word documents when using GroupDocs.Editor for Python via .NET API."
keywords: Edit document with locale, GroupDocs.Editor, Edit RTL documents, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
The [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class contains 3 very similar properties that represent a culture (locale):

```python
locale          # left-to-right text
locale_bi       # right-to-left (bidirectional) text
locale_far_east # East-Asian (Far-East) text
```

In most cases the output WordProcessing document, which is generated from the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance by the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method, contains valid locales for the textual content. But in some cases it is necessary to forcibly and explicitly set some specific locale for the output document. These three options provide such possibility. However, keep in mind that they are suitable only when the document should have a single locale. If the document is multi-language and contains, for example, English and Spanish text, setting the locale to English ("en-GB", for example) will mark the Spanish text as English too, so spell checking in MS Word, for example, will not work properly for such text.

By default all these three locale-related properties are not specified, their values are `None`. In such case MS Word (or another program) will detect (or choose) the document locale according to its own settings or other factors. Additionally, if the document is multi-language, it is strongly encouraged to enable the [`enable_language_information`]({{< ref "editor/python-net/developer-guide/edit-document/edit-word/enabling-language-information.md" >}}) property in the [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class while editing the original document, and not to use these 3 properties.

The [`locale`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions/) property is intended to set the locale for usual left-to-right text, which consists of letters.  
The [`locale_bi`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions/) property should be used when text is right-to-left (RTL), for example, [Arabic script](https://en.wikipedia.org/wiki/Arabic_script) or [Hebrew alphabet](https://en.wikipedia.org/wiki/Hebrew_alphabet).  
The [`locale_far_east`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions/) property should be used for East-Asian (Far-East) text, including [CJK characters](https://en.wikipedia.org/wiki/CJK_characters).

The snippet below illustrates how the locale properties are assigned on the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) instance before saving. The locale value is a culture object, obtained from the corresponding type in the underlying .NET globalization layer:

```python
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)

# Set the locale for the output document (culture object for "en-US")
save_options.locale = en_us_culture
# Set the locale for right-to-left text
save_options.locale_bi = arabic_culture
# Set the locale for East-Asian text
save_options.locale_far_east = japanese_culture
```

## Complete example

The runnable example below performs the safe core workflow — loading the sample document, editing it, and saving the result to DOCX. The locale properties can additionally be assigned on the save options as shown in the snippet above.

{{< tabs "code-example-locales-for-output-document">}}
{{< tab "set_output_locale.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

def set_output_locale():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        original = editor.edit()
        modified = EditableDocument.from_markup(original.get_embedded_html())

        # Prepare save options (locale properties can be assigned here)
        save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)

        editor.save(modified, "./localized-document.docx", save_options)
        print("Saved the edited document to DOCX")

        original.dispose()
        modified.dispose()

if __name__ == "__main__":
    set_output_locale()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/locales-for-output-document/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "localized-document.docx" >}}  
```text
Binary file (DOCX, 49 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/locales-for-output-document/set_output_locale/localized-document.docx)
{{< /tab >}}
{{< /tabs >}}
