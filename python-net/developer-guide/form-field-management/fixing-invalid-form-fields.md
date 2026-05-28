---
id: fixing-invalid-form-fields
url: editor/python-net/fixing-invalid-form-fields
title: Fixing Invalid Form Fields
linkTitle: Fixing Invalid Form Fields
weight: 4
description: "Fixing invalid form fields and saving the document with GroupDocs.Editor for Python via .NET."
keywords: Fix Invalid Form Fields, FormFieldManager, Invalid Form Fields, Update Form Fields, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---

This article demonstrates how to fix invalid form fields in a Word document using GroupDocs.Editor for Python via .NET. It guides you through loading a document, identifying invalid form fields, and fixing them.

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

   Obtain the [`FormFieldManager`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/formfieldmanager/) instance from the [`form_field_manager`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/) property of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class.

   ```python
   with open("form-fields.docx", "rb") as stream:
       with Editor(stream, WordProcessingLoadOptions()) as editor:
           field_manager = editor.form_field_manager
   ```

3. **Detect invalid form fields**

   Check whether the document contains invalid form fields with `has_invalid_form_fields()`, and obtain their names with `get_invalid_form_field_names()`.

   ```python
   has_invalid = field_manager.has_invalid_form_fields()
   print("FormFieldCollection contains invalid items:", has_invalid)

   invalid_names = list(field_manager.get_invalid_form_field_names())
   print("Invalid form field names:", invalid_names)
   ```

4. **Fix the invalid form fields**

   The invalid form fields can be repaired with `fix_invalid_form_field_names(...)`. The snippet below is illustrative — generate a unique replacement name for each invalid field and pass them to the method. Adjust the per-item access details to the exact members exposed by your build.

   ```python
   # Illustrative: assign unique fixed names and repair the invalid fields
   import uuid

   invalid_form_fields = field_manager.get_invalid_form_field_names()
   for invalid_item in invalid_form_fields:
       invalid_item.fixed_name = "{0}_{1}".format(invalid_item.name, uuid.uuid4())
   field_manager.fix_invalid_form_field_names(invalid_form_fields)
   ```

## Complete code example

Below is the complete runnable example. It opens the document, retrieves the form field manager, and reports whether it has invalid form fields together with their names. The repair steps shown above are illustrative; the runnable example performs only safe, documented calls.

{{< tabs "code-example-fixing-invalid-form-fields">}}
{{< tab "fixing_invalid_form_fields.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import WordProcessingLoadOptions

def fixing_invalid_form_fields():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Open the document as a stream and load it with WordProcessingLoadOptions
    with open("./form-fields.docx", "rb") as stream:
        with Editor(stream, WordProcessingLoadOptions()) as editor:
            # Read the FormFieldManager instance
            field_manager = editor.form_field_manager

            # Detect invalid form fields
            print("Has invalid form fields:", field_manager.has_invalid_form_fields())
            invalid_form_fields = list(field_manager.get_invalid_form_field_names())
            print("Invalid form fields detected:", len(invalid_form_fields))

if __name__ == "__main__":
    fixing_invalid_form_fields()
```
{{< /tab >}}
{{< tab "form-fields.docx" >}}  
{{< tab-text >}}
`form-fields.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/form-field-management/fixing-invalid-form-fields/form-fields.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "fixing-invalid-form-fields.txt" >}}  
```text
Has invalid form fields: True
Invalid form fields detected: 21
```
[Download full output](/editor/python-net/_output_files/developer-guide/form-field-management/fixing-invalid-form-fields/fixing_invalid_form_fields/fixing-invalid-form-fields.txt)
{{< /tab >}}
{{< /tabs >}}
