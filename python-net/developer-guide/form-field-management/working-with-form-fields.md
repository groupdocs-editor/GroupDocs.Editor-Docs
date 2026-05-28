---
id: working-with-form-fields
url: editor/python-net/working-with-form-fields
title: Working with Form Fields
linkTitle: Working with Form Fields
weight: 4
description: "This article demonstrates how to load, edit, and read form fields in a Word document using GroupDocs.Editor for Python via .NET."
keywords: Edit HTML document, GroupDocs.Editor, form fields, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---

This article demonstrates how to load and read form fields in a Word document using GroupDocs.Editor for Python via .NET. We will go through the process of opening a document, retrieving the form field manager, and inspecting the form fields it contains.

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

   Obtain the [`FormFieldManager`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/formfieldmanager/) instance from the [`form_field_manager`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/) property of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class. Use it to read the `form_field_collection` and to check whether the document contains invalid form fields.

   ```python
   with open("form-fields.docx", "rb") as stream:
       with Editor(stream, WordProcessingLoadOptions()) as editor:
           field_manager = editor.form_field_manager
           collection = field_manager.form_field_collection

           print("Has invalid form fields:", field_manager.has_invalid_form_fields())
           print("Form fields count:", len(collection))
   ```

3. **Process the form fields**

   The `form_field_collection` can be iterated to inspect each form field. Every field exposes common properties such as `name` and `type`. The snippet below illustrates how to enumerate the collection and react to a field's type. Treat the per-type access details as illustrative — adjust them to the exact members exposed by your build.

   ```python
   # Illustrative: iterate the collection and inspect each form field
   for form_field in collection:
       print("name:", form_field.name, "type:", form_field.type)
   ```

## Complete code example

Below is the complete runnable example. It opens the document, retrieves the form field manager, and reports whether the document has invalid form fields together with the number of form fields it contains.

{{< tabs "code-example-working-with-form-fields">}}
{{< tab "working_with_form_fields.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingLoadOptions

def working_with_form_fields():
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

            # Iterate the collection and inspect each form field
            for form_field in collection:
                print("name:", form_field.name, "type:", form_field.type)

if __name__ == "__main__":
    working_with_form_fields()
```
{{< /tab >}}
{{< tab "form-fields.docx" >}}  
{{< tab-text >}}
`form-fields.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/form-field-management/working-with-form-fields/form-fields.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "working-with-form-fields.txt" >}}  
```text
Has invalid form fields: True
Form fields count: 31
name: Text1 type: 0
name: Check1 type: 5
name: Check2 type: 5
name: Check3 type: 5
name: Check4 type: 5
name: Check5 type: 5
name: Check6 type: 5
name: Text7 type: 0
[TRUNCATED]
```
[Download full output](/editor/python-net/_output_files/developer-guide/form-field-management/working-with-form-fields/working_with_form_fields/working-with-form-fields.txt)
{{< /tab >}}
{{< /tabs >}}

## Conclusion

This guide demonstrates how to work with form fields in Word documents using GroupDocs.Editor for Python via .NET. By following these steps, you can load a document, retrieve the form field manager, and inspect the form fields it contains. This functionality is essential for applications that require manipulation and extraction of form field data from documents.
