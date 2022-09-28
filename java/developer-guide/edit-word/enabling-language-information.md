---
id: enabling-language-information
url: editor/java/enabling-language-information
title: Enabling language information
weight: 3
description: "Following this guide you will learn how to edit Word document using locale info, apply spell-checkers to a document content written in different languages using GroupDocs.Editor for Java API."
keywords: Edit Word document with language, Edit Word document with locale, edit Word
productName: GroupDocs.Editor for Java
hideChildren: False
---
Documents of all WordProcessing formats can contain text in different languages. But, unlike the plain text documents (TXT), WordProcessing documents also contain a metadata about specific language (locale) of every piece of text. [**GroupDocs.Editor**](https://products.groupdocs.com/editor/java) allows to extract and export this language information. For achieving this the [WordProcessingEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions) class contains the [`EnableLanguageInformation`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/WordProcessingEditOptions#getEnableLanguageInformation--) public boolean property:

```java
public boolean getEnableLanguageInformation()
public void setEnableLanguageInformation(boolean)
```

By default its value is `false`, which means that language metadata will not be extracted. But when this option is manually enabled, GroupDocs.Editor extracts locale info for every piece of textual content and preserves it in the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance, when document is edited. Finally, when user have obtained the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance and is generating the HTML markup for transferring it to the WYSIWYG HTML-editor in order to make document editable in the browser, this language information is represented as the 'lang' HTML attributes with appropriate values inside the SPAN HTML elements.

Enabling language information is useful when document contains different text parts in different languages; if document has text in some single language, this option has no many sense and thus is disabled by default.

However, when document is multi-language, enabling language information may be very suitable for two scenarios:

* It eases spell checking for client-base JavaScript spell-checkers, that are working in the browser. However, this is very dependent on specific spell-checker, as not all spell-checkers are able to grab values from "lang" attributes or even use language information at all.
* It improves the quality of output WordProcessing document in roundtrip scenarios. When document with enabled [`EnableLanguageInformation`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/WordProcessingEditOptions#getEnableLanguageInformation--) option was converted to the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance, then HTML markup was generated, edited in the some HTML-editor, and then new instance of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class was created from edited markup, language metadata in "lang" attributes is still preserved. When edited [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) will be converted back to the output document of some WordProcessing format like DOCX or RTF, the textual content inside it will have connections to correct locale.
