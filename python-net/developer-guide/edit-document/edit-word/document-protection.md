---
id: document-protection
url: editor/python-net/document-protection
title: Document protection
linkTitle: Document protection
weight: 2
description: "This article explains how to edit protected Word document, allow or restrict document editing features like adding comments or filling form fields using GroupDocs.Editor for Python via .NET API."
keywords: Edit protected Word document, Edit and protect Word document, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
The [`password`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions/) property, if set, enables document protection from opening by encrypting it with the specified password. However, almost all WordProcessing formats support a document protection from writing, which is completely different from opening. Document protection, like document encoding, also implies a password as a form of key, but it also supports different levels of protection: some of them allow only read-only mode, others allow to edit form-fields etc.

[**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net) allows to apply the document protection via the `protection` property in the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class. By default this property has a `None` value, which means that GroupDocs.Editor will not apply the protection to the document.

The `protection` property has a `WordProcessingProtection` type. This type is a class, which, in turn, consists of two properties: a password and a protection type.

The password is responsible for setting a password for the document protection. By default it is `None` — protection is not applied. If user wants to apply the document protection, he *needs* to assign some string to this property; otherwise protection will not be applied regardless from the value of the protection type property.

The protection type property determines the level of protection. By default it has a `NoProtection` value, which means that no protection will be applied. Other values are:

1. `AllowOnlyRevisions` — User can only add revision marks to the document.
2. `AllowOnlyComments` — User can only modify comments in the document.
3. `AllowOnlyFormFields` — User can only enter data in the form fields in the document.
4. `ReadOnly` — No changes are allowed to the document.

Take a note that both the password and the protection type are bound to each other. If user sets a valid non-`None`, not-empty password, but the protection type property has a `NoProtection` value, the protection will not be applied. And vice versa, if the protection type property has an `AllowOnlyFormFields` value, for example, but the password is `None` or an empty string, then the protection will not be applied either.

The snippet below illustrates how the `protection` property is assigned on the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) instance before saving (the `WordProcessingProtection` and `WordProcessingProtectionType` types come from the `groupdocs.editor.options` module):

```python
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import (
    WordProcessingSaveOptions,
    WordProcessingProtection,
    WordProcessingProtectionType,
)

save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)

# Make the resultant document read-only and protect this restriction with a password
save_options.protection = WordProcessingProtection(
    WordProcessingProtectionType.READ_ONLY, "write_password")
```

## Complete example

Document protection (write protection) is separate from document encryption (protection from opening). The runnable example below shows the safe and most common scenario — loading the sample document, editing it, and saving the result encrypted with the [`password`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions/) property so that the output document can be opened only with this password.

{{< tabs "code-example-document-protection">}}
{{< tab "protect_document.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

def protect_document():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        # Open the document for editing
        original = editor.edit()

        # Edit the content (here done programmatically)
        modified_content = original.get_embedded_html().replace("Title of the document", "Title of the protected document")
        modified = EditableDocument.from_markup(modified_content)

        # Encrypt the output document with a password (protection from opening)
        save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
        save_options.password = "p@ss"

        editor.save(modified, "./protected-document.docx", save_options)
        print("Saved a password-protected document")

        original.dispose()
        modified.dispose()

if __name__ == "__main__":
    protect_document()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/document-protection/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "protected-document.docx" >}}  
```text
Binary file (DOCX, 53 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/document-protection/protect_document/protected-document.docx)
{{< /tab >}}
{{< /tabs >}}
