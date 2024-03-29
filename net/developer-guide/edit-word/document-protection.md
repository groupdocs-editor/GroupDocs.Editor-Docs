---
id: document-protection
url: editor/net/document-protection
title: Document protection
weight: 2
description: "This article explains how to edit protected Word document, allow or restrict document editing features like adding comments or filling form fields using GroupDocs.Editor for .NET API."
keywords: Edit protected Word document, Edit and protect Word document
productName: GroupDocs.Editor for .NET
hideChildren: False
---
The [Password](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingsaveoptions/password) property, if set, enables document protection from opening by encrypting it with specified password. However, almost all WordProcessing formats support a document protection from writing, which is completely different from opening. Document protection, like document encoding, also implies a password as a form of key, but it also supports different levels of protection: some of them allow only read-only mode, other allow to edit form-fields etc.

[**GroupDocs.Editor**](https://products.groupdocs.com/editor/net) allows to apply the document protection via the [Protection](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingsaveoptions/protection) property in the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingsaveoptions) class. By default this property has a NULL value, which means that GroupDocs.Editor will not apply the protection to the document.

The [Protection](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingsaveoptions/protection) property has a [WordProcessingProtection](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection) type. This type is a class, which, it turn, consists of two properties: [Password](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/password) and a [ProtectionType](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/protectiontype).

[WordProcessingProtection](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection) class has two constructors. [First one](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/wordprocessingprotection/#constructor) is parameterless, it sets both class properties to their default values. If user will use this constructor, and then will not set values to the properties in class instance, the protection will not be applied to the document. [Second constructor](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/wordprocessingprotection/#constructor_1), in counterpart, has two parameters, which set values to the properties; so, when using this constructor, there is no need to use properties at all.

The [Password](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/password) property is responsible for setting a password for the document protection. By default it is NULL — protection is not applied. If user wants to apply the document protection, he *needs* to assign some string to this property (through [constructor](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/wordprocessingprotection/#constructor_1) or property itself); otherwise protection will not be applied regardless from the value of the [ProtectionType](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/protectiontype) property.

[ProtectionType](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/protectiontype) property has type [WordProcessingProtectionType](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotectiontype), which is a simple enum, and value of this enum determines the level of protection. By default enum has a `NoProtection` value, which means that no protection will be applied. Other values are:

1. `AllowOnlyRevisions` — User can only add revision marks to the document.
2. `AllowOnlyComments` — User can only modify comments in the document.
3. `AllowOnlyFormFields` — User can only enter data in the form fields in the document.
4. `ReadOnly` — No changes are allowed to the document.

Take a note that both the properties [Password](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/password) and a [ProtectionType](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/protectiontype) are bound to each other. If user set a valid non-NULL not-empty [Password](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/password), but the [ProtectionType](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/protectiontype) property has a [WordProcessingProtectionType](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotectiontype). `NoProtection` value, the protection will not be applied. And vice versa, if [ProtectionType](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/protectiontype) property has a `AllowOnlyFormFields` value, for example, but [Password](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingprotection/password) is NULL or empty string, then the protection will not be applied too.
