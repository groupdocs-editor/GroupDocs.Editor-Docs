---
id: locales-for-output-document
url: editor/java/locales-for-output-document
title: Locales for output document
weight: 8
description: "This guide demonstrates how to edit RTL documents and specify locale for Word documents when using GroupDocs.Editor for Java API."
keywords: Edit document with locale, GroupDocs.Editor, Edit RTL documents
productName: GroupDocs.Editor for Java
hideChildren: False
---
[WordProcessingSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions) class contains 3 very similar properties of the same type `java.util.Locale`:

```java
public java.util.Locale getLocale()
public void setLocale(java.util.Locale)

public java.util.Locale getLocaleBi()
public void setLocaleBi(java.util.Locale)

public java.util.Locale getLocaleFarEast()
public void setLocaleFarEast(java.util.Locale)
```

In most cases the output WordProcessing document, which is generated from the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance by the [`Editor.save()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/Editor#save-com.groupdocs.editor.EditableDocument-java.io.OutputStream-com.groupdocs.editor.options.ISaveOptions-) method, contains valid locales for the textual content. But in some cases it is necessary to forcibly and explicitly set a some specific locale for the output document. These three options provide such possibility. However keep in mind that they are suitable only when document should have some single locale. If document is multi-language and contains, for example, English and Spanish text, setting the locale to English ("en-GB", for example) will mark the Spanish text as English too, so spell checking in MS Word, for example, will not work properly for such text.

By default all these three locale-related properties are not specified, their values are NULL. In such case MS Word (or other program) will detect (or choose) the document locale according to its own settings or other factors. Additionally, if document is multi-language, it is strongly encouraged to enable the [`EnableLanguageInformation`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions/properties/enablelanguageinformation) property in the [WordProcessingEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions) class while editing original document, and not to use these 3 properties.

The [`Locale`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions/properties/locale) property is intended to set the locale for usual left-to-right text, which consists of letters.  
The [`LocaleBi`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions/properties/localebi) property should be used when text is right-to-left (RTL), for example, [Arabic script](https://en.wikipedia.org/wiki/Arabic_script) or [Hebrew alphabet](https://en.wikipedia.org/wiki/Hebrew_alphabet).  
The [`LocaleFarEast`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions/properties/localefareast) property should be used for East-Asian (Far-East) text, including [CJK characters](https://en.wikipedia.org/wiki/CJK_characters).
