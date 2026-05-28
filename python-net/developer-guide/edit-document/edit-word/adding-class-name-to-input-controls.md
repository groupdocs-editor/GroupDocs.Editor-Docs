---
id: adding-class-name-to-input-controls
url: editor/python-net/adding-class-name-to-input-controls
title: Adding class name to input controls
linkTitle: Adding class name to input controls
weight: 1
description: "Follow this guide and learn how to edit Word documents that contain input controls like buttons, textboxes, check-boxes, combo-boxes, input fields, dropdown lists, radio-buttons, date/time pickers etc. using GroupDocs.Editor for Python via .NET API features."
keywords: Edit Word document with input controls, Edit DOCX with input fields and text boxes, Edit Word, Edit DOCX, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
Almost all formats within WordProcessing format family, like DOC(X/M), ODT etc., support input controls of different kinds. WordProcessing documents can contain different buttons, textboxes, check-boxes, combo-boxes, input fields, dropdown lists, radio-buttons, date/time pickers and much more, which are internally present as Structured Document Tag (SDT) entities or Fields ("Insert > Quick Parts > Document Property/Field"). **[GroupDocs.Editor](https://products.groupdocs.com/editor/python-net)** supports all of these entities and preserves them while converting the document to the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance. Finally, when generating a HTML document from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) in order to edit it in the WYSIWYG HTML-editor, these input controls are translated into the most appropriate HTML structures and elements. Additionally, when input document contains not only input controls, but also user-entered data, this data is also preserved and will be present in the output HTML document.

In some specific use-cases the end-user may require not to edit the entire document content, but only edit and/or gather data, entered into the input controls. For such case it is required to identify all these input controls in some way in order to fetching them, to distinguish them from all other HTML elements, when working on client-side. For achieving this purpose the GroupDocs.Editor has an ability to set an unique user-provided CSS class name for all such input controls in HTML markup. The [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) class contains an [`input_controls_class_name`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions/) property for this purpose.

By default it has a `None` value — class names are not applied to the HTML elements. However, if user will set a valid class name, all the input control elements (like INPUT, BUTTON, SELECT etc.) will have a "class" HTML attribute with specified class name. For example:

```python
from groupdocs.editor.options import WordProcessingEditOptions

edit_options = WordProcessingEditOptions()
edit_options.input_controls_class_name = "editable-field"
```

Finally, when "class" attribute with specified class name is applied to all HTML elements, that represent input controls, client code is able to work with them by, for example, traversing the HTML DOM and gathering and/or manipulating with data.

## Complete example

The code example below demonstrates editing the sample DOCX document twice: the first time with default `WordProcessingEditOptions`, where no custom class name is specified, and the second time with a custom value in the `input_controls_class_name` property. The example then reports how many input-control elements carry the custom class name in each version — none without it, and several once it is applied.

{{< tabs "code-example-adding-class-name-to-input-controls">}}
{{< tab "add_class_name_to_input_controls.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingEditOptions

def add_class_name_to_input_controls():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Default edit options, where no custom class name is applied
    options_without_class_name = WordProcessingEditOptions()

    # Custom edit options, where a class name is applied to input controls
    options_with_class_name = WordProcessingEditOptions()
    options_with_class_name.input_controls_class_name = "editable-field"

    with Editor("./sample-document.docx") as editor:
        # Edit the document twice with both option sets
        html_without_class_name = editor.edit(options_without_class_name).get_embedded_html()
        html_with_class_name = editor.edit(options_with_class_name).get_embedded_html()

        # The custom class name is applied to every input-control element in the HTML
        print("Occurrences of 'editable-field' without custom class name:",
              html_without_class_name.count("editable-field"))
        print("Occurrences of 'editable-field' with custom class name:",
              html_with_class_name.count("editable-field"))

if __name__ == "__main__":
    add_class_name_to_input_controls()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/adding-class-name-to-input-controls/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "add-class-name-to-input-controls.txt" >}}  
```text
Occurrences of 'editable-field' without custom class name: 0
Occurrences of 'editable-field' with custom class name: 7
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/adding-class-name-to-input-controls/add_class_name_to_input_controls/add-class-name-to-input-controls.txt)
{{< /tab >}}
{{< /tabs >}}

When the `class` attribute with the specified class name is applied to all HTML elements that represent input controls, the client code is able to work with them — for example, by traversing the HTML DOM and gathering or manipulating the data.
