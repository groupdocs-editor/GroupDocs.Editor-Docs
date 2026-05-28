---
id: edit-and-update-form-fields
url: editor/python-net/edit-and-update-form-fields
title: Edit and Update Form Fields
linkTitle: Edit and Update Form Fields
weight: 4
description: "Editing form fields with GroupDocs.Editor for Python via .NET."
keywords: Edit Form Fields, Update Form Fields, Update Form Fields with Word Document Protection, FormFieldManager, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---

This article demonstrates how to edit form fields in a Word document using GroupDocs.Editor for Python via .NET. It guides you through loading a document, inspecting its form fields, and updating them.

## Step-by-Step Guide

1. **Load the document into the Editor instance**

   Open the document as a binary stream and pass it to the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class together with [`WordProcessingLoadOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingloadoptions). If the document is password-protected, specify the password through the load options.

   ```python
   from groupdocs.editor import Editor
   from groupdocs.editor.options import WordProcessingLoadOptions

   with open("form-fields.docx", "rb") as stream:
       with Editor(stream, WordProcessingLoadOptions()) as editor:
           # Further code will be placed here
           pass
   ```

2. **Retrieve the FormFieldManager**

   Obtain the [`FormFieldManager`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/formfieldmanager/) instance from the [`form_field_manager`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/) property of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class and read the `form_field_collection`.

   ```python
   with open("form-fields.docx", "rb") as stream:
       with Editor(stream, WordProcessingLoadOptions()) as editor:
           field_manager = editor.form_field_manager
           collection = field_manager.form_field_collection
           print("Form fields count:", len(collection))
   ```

3. **Update a form field**

   A specific form field can be modified and the change applied back to the document with the `update_form_filed(...)` method. The snippet below is illustrative — retrieve the form field of interest from the collection, modify its properties, and pass the collection to `update_form_filed(...)`. Adjust the per-type access details to the exact members exposed by your build.

   ```python
   # Illustrative: retrieve, modify, and update a text form field
   text_field = collection.get_form_field("Text1")
   text_field.locale_id = 1029
   text_field.value = "new Value"
   field_manager.update_form_filed(collection)
   ```

4. **Save the updated document**

   After updating the form fields, save the document with [`editor.save(...)`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) using the desired save options.

   ```python
   from groupdocs.editor.options import WordProcessingSaveOptions
   from groupdocs.editor.formats import WordProcessingFormats

   save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
   editable = editor.edit()
   editor.save(editable, "output.docx", save_options)
   ```

## Complete code example

Below is the complete runnable example. It opens the document, retrieves the form field manager, and reports the number of form fields it contains. The mutation steps shown above are illustrative; the runnable example performs only safe, documented calls.

{{< tabs "code-example-edit-and-update-form-fields">}}
{{< tab "edit_and_update_form_fields.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingLoadOptions

def edit_and_update_form_fields():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Open the document as a stream and load it with WordProcessingLoadOptions
    with open("./form-fields.docx", "rb") as stream:
        with Editor(stream, WordProcessingLoadOptions()) as editor:
            # Read the FormFieldManager instance
            field_manager = editor.form_field_manager

            # Read the form field collection
            collection = field_manager.form_field_collection

            print("Has invalid form fields:", field_manager.has_invalid_form_fields())
            print("Form fields count:", len(collection))

if __name__ == "__main__":
    edit_and_update_form_fields()
```
{{< /tab >}}
{{< tab "form-fields.docx" >}}  
{{< tab-text >}}
`form-fields.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/form-field-management/edit-and-update-form-fields/form-fields.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edit-and-update-form-fields.txt" >}}  
```text
Has invalid form fields: True
Form fields count: 31
```
[Download full output](/editor/python-net/_output_files/developer-guide/form-field-management/edit-and-update-form-fields/edit_and_update_form_fields/edit-and-update-form-fields.txt)
{{< /tab >}}
{{< /tabs >}}
