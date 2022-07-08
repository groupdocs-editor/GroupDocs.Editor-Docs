---
id: working-with-resources
url: editor/java/working-with-resources
title: Working with resources
weight: 3
description: "This article demonstrates and explains different operations with resources, including retrieving, adjusting and saving them in different scenarios when editing documents with GroupDocs.Editor for Java."
keywords: Working with HTML resources, Edit document, GroupDocs.Editor
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
> This demonstration shows and explains different operations with resources, including retrieving, adjusting and saving them in different scenarios.

## Introduction

Almost all documents of any types have resources. It is first of all images, some document formats also hold fonts. Even for plain text document (TXT), when converting it to the HTML for editing, there will be one stylesheet, that is treated as a resource. GroupDocs.Editor allows to work with resources on editing phase, when document was loaded to the [Editor](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class and opened for editing by generating the [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance, that is produced by the [Editor](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editor).[Edit](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editor#edit/index()) method. Instance of [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument)can be treated as an input document, converted to internal intermediate format, with possibilities to generate HTML markup and resources for passing them to the client-side WYSIWYG HTML-editor. GroupDocs.Editor classifies all resources onto three groups:

* Images, including: raster (PNG, BMP, JPEG, GIF, ICON) and vector (SVG and WMF).
* Fonts, including: TTF, EOT, WOFF, WOFF2.
* Textual resources: only CSS.
* Audio files: only MP3 (appear in version 22.6).

Every resource type has its own distinct class with metadata, constructors and other methods. They all are located within [com.groupdocs.editor.htmlcss.resources](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources/package-frame) namespace.

## Preparations

Let's prepare an [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance by loading and editing some input WordProcessing document, as always (using directive is omitted for the sake of simplification):

```java
String inputDocxPath = "C://input/document.docx";
Editor editor = new Editor(inputDocxPath, new WordProcessingLoadOptions());
WordProcessingEditOptions editOptions = new WordProcessingEditOptions();
editOptions.setFontExtraction(FontExtractionOptions.ExtractAll);// Enable max font extraction - ExtractAll
EditableDocument beforeEdit = editor.edit(editOptions);// Create EditableDocument instance
```

## Obtaining resources

Now, when [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance is ready, it is possible to obtains resources from it, and [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) provides several ways for this.

First of all, resources can be retrieved by their type. [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) contains three properties for every resource type:

* [EditableDocument.getImages()](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#getImages--) — returns a List of [IImageResource](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images/iimageresource), interface, that is common for all images, raster and vector.
* [EditableDocument.getFonts()](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#getFonts--) — returns a List of [FontResourceBase](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.fonts/fontresourcebase), abstract class, that is common for all fonts.
* [EditableDocument.getCss()](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#getCss--) — returns a List of [CssText](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.textual/csstext), class, that represents one CSS stylesheet.
* [EditableDocument.getAudio()]((https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#getAudio--)) — returns a List of [Mp3Audio]((https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.audio/Mp3Audio)), class, that represents one MP3 audio file. Added in ver 21.10.

Secondly, completely all resources may be obtained with a single property [EditableDocument.AllResources](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/properties/allresources). It returns a List of [IHtmlResource](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources/ihtmlresource), interface, that is common for absolutely all HTML resources, including images, fonts and stylesheets. The collection, returned by the [getAllResources()](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#getAllResources--) property, in fact is a concatenation of previous three.

Here is a source code:

```java
List<IImageResource> images = beforeEdit.getImages();
List<FontResourceBase> fonts = beforeEdit.getFonts();
List<CssText> stylesheets = beforeEdit.getCss();
List<IHtmlResource> allTogether = beforeEdit.getAllResources();
```

If user wants to manually save instance of [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) as HTML file with resources, he may use something like this:

```java
String outputFolder = "C://output//document_resources//";
for (IImageResource oneImage : (Iterable<IImageResource>) images) {
    oneImage.save(outputFolder + oneImage.getFilenameWithExtension());
}

for (FontResourceBase oneFont : (Iterable<FontResourceBase>) fonts) {
    oneFont.save(outputFolder + oneFont.getFilenameWithExtension());
}

for (CssText oneStylesheet : (Iterable<CssText>) stylesheets) {
    oneStylesheet.save(outputFolder + oneStylesheet.getFilenameWithExtension());
}

try (PrintWriter out = new PrintWriter("c://output//document.html")) {
    out.println(beforeEdit.getContent());
}
```

## CSS resources

There also is a third way, that is designed especially for the stylesheets. The reason is that stylesheets can contain external resources too, which are presented as links with urls. For example, it can be images, fonts, and other stylesheets. In such case it is necessary to adjust such link. For coping with this [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) contains two overloads of the [getCssContent()](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#getCssContent--) method.[ First overload](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#getCssContent--) is parameterless and returns a `List` of strings, each one represents one stylesheet. In most cases document has only one stylesheet, so such list will have only one item. [Second overload](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#getCssContent-java.lang.String-java.lang.String-) obtains two string parameters: `externalImagesPrefix` and `externalFontsPrefix`. First parameter is designated for images, while second - for fonts. Both overloads are shown in example below:

```java
List<String> stylesheetsWithoutPrefixes = beforeEdit.getCssContent();
String externalImagesPrefix = "http://www.mywebsite.com/images/id=";
String externalFontsPrefix = "http://www.mywebsite.com/fonts/id=";
List<String> stylesheetsWithPrefixes = beforeEdit.getCssContent(externalImagesPrefix, externalFontsPrefix);
```
