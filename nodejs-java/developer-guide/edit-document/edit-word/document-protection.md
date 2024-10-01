---
id: document-protection
url: editor/nodejs-java/document-protection
title: Document protection
weight: 2
description: "This article explains how to edit protected Word documents and allow or restrict document editing features like adding comments or filling form fields using GroupDocs.Editor for Node.js and Java."
keywords: Edit protected Word document, Edit and protect Word document
productName: GroupDocs.Editor for Node.js and Java
hideChildren: False
---

The [Password](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingsaveoptions/#getPassword--) property, if set, enables document protection from opening by encrypting it with a specified password. However, almost all WordProcessing formats support document protection from writing, which is different from opening protection. Document protection, like document encoding, involves a password but also supports different levels of protection: some allow only read-only mode, while others allow editing form fields, adding comments, etc.

**[GroupDocs.Editor](https://products.groupdocs.com/editor/nodejs-java)** allows applying document protection through the [Protection](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingsaveoptions/#getProtection--) property in the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingsaveoptions) class. By default, this property is set to NULL, meaning no protection is applied to the document.

The [Protection](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingsaveoptions/#getProtection--) property is of the type [WordProcessingProtection](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection). This class consists of two properties: [Password](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getPassword--) and [ProtectionType](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getProtectionType--).

The [WordProcessingProtection](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection) class has two constructors. The [first constructor](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#WordProcessingProtection--) is parameterless and sets both class properties to their default values. If this constructor is used without modifying the properties, the protection will not be applied. The [second constructor](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#WordProcessingProtection-byte-java.lang.String-) takes two parameters to set both properties directly.

The [Password](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getPassword--) property sets the password for document protection. If the password is not provided, protection will not be applied, regardless of the [ProtectionType](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getProtectionType--) value.

The [ProtectionType](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getProtectionType--) property is of type [WordProcessingProtectionType](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotectiontype), which is an enum. This enum determines the level of protection. The default value is `NoProtection`, meaning no protection is applied. Other values include:

1. `AllowOnlyRevisions` — User can only add revision marks to the document.
2. `AllowOnlyComments` — User can only modify comments in the document.
3. `AllowOnlyFormFields` — User can only enter data into the form fields in the document.
4. `ReadOnly` — No changes are allowed to the document.

### Important Notes

Both the [Password](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getPassword--) and [ProtectionType](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getProtectionType--) properties are interdependent. If a non-NULL and non-empty [Password](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getPassword--) is provided, but the [ProtectionType](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getProtectionType--) is set to `NoProtection`, then no protection will be applied. Conversely, if a [ProtectionType](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getProtectionType--) like `AllowOnlyFormFields` is set but the [Password](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/wordprocessingprotection/#getPassword--) is NULL or an empty string, the protection will not be applied either.
