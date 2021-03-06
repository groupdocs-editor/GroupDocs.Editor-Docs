---
id: working-with-resources
url: editor/net/working-with-resources
title: Working with resources
weight: 3
description: "This article demonstrates and explains different operations with resources, including retrieving, adjusting and saving them in different scenarios when editing documents with GroupDocs.Editor for .NET."
keywords: Working with HTML resources, Edit document, GroupDocs.Editor
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
> This demonstration shows and explains different operations with resources, including retrieving, adjusting and saving them in different scenarios.

## Introduction

Almost all documents of any types have resources. It is first of all images, some document formats also hold fonts. Even for plain text document (TXT), when converting it to the HTML for editing, there will be one stylesheet, that is treated as a resource. WordProcessing documents of some format, Office Open XML usually, can also contain embedded audio files. GroupDocs.Editor allows to work with resources on editing phase, when document was loaded to the [Editor](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor) class and opened for editing by generating the [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) instance, that is produced by the [Editor](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor).[Edit](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/edit/index) method. Instance of [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument)can be treated as an input document, converted to internal intermediate format, with possibilities to generate HTML markup and resources for passing them to the client-side WYSIWYG HTML-editor. GroupDocs.Editor classifies all resources onto three groups:

* Images, including: raster (PNG, BMP, JPEG, GIF, ICON) and vector (SVG and WMF).
* Fonts, including: TTF, EOT, WOFF, WOFF2.
* Textual resources: only CSS.
* Audio files: only MP3 (appear in version 21.10).

Every resource type has its own distinct class with metadata, constructors and other methods. They all are located within [GroupDocs.Editor.HtmlCss.Resources](https://apireference.groupdocs.com/net/editor/groupdocs.editor.htmlcss.resources/index) namespace.

## Preparations

Let's prepare an [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) instance by loading and editing some input WordProcessing document, as always (using directive is omitted for the sake of simplification):

```csharp
string inputDocxPath = "C://input/document.docx";
Editor editor = new Editor(inputDocxPath, delegate { return new WordProcessingLoadOptions(); });
WordProcessingEditOptions editOptions = new WordProcessingEditOptions();
editOptions.FontExtraction = FontExtractionOptions.ExtractAll;// Enable max font extraction - ExtractAll
EditableDocument beforeEdit = editor.Edit(editOptions);// Create EditableDocument instance
```

## Obtaining resources

Now, when [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) instance is ready, it is possible to obtains resources from it, and [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) provides several ways for this.

First of all, resources can be retrieved by their type. [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) contains four properties for every resource type:

* [EditableDocument.Images](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/properties/images) — returns a List of [IImageResource](https://apireference.groupdocs.com/net/editor/groupdocs.editor.htmlcss.resources.images/iimageresource), interface, that is common for all images, raster and vector.
* [EditableDocument.Fonts](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/properties/fonts) — returns a List of [FontResourceBase](https://apireference.groupdocs.com/net/editor/groupdocs.editor.htmlcss.resources.fonts/fontresourcebase), abstract class, that is common for all fonts.
* [EditableDocument.Css](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/properties/css) — returns a List of [CssText](https://apireference.groupdocs.com/net/editor/groupdocs.editor.htmlcss.resources.textual/csstext), class, that represents one CSS stylesheet.
* [EditableDocument.Audio]((https://apireference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/properties/audio)) — returns a List of [Mp3Audio]((https://apireference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.audio/mp3audio)), class, that represents one MP3 audio file. Added in ver 21.10.

Secondly, completely all resources may be obtained with a single property [EditableDocument.AllResources](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/properties/allresources). It returns a List of [IHtmlResource](https://apireference.groupdocs.com/net/editor/groupdocs.editor.htmlcss.resources/ihtmlresource), interface, that is common for absolutely all HTML resources, including images, fonts, stylesheets, and audio. The collection, returned by the [AllResources](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/properties/allresources) property, in fact is a concatenation of previous four.

Here is a source code:

```csharp
List<IImageResource> images = beforeEdit.Images;
List<FontResourceBase> fonts = beforeEdit.Fonts;
List<CssText> stylesheets = beforeEdit.Css;
List<Mp3Audio> audiofiles = beforeEdit.Audio;
List<IHtmlResource> allTogether = beforeEdit.AllResources;
```

If user wants to manually save instance of [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) as HTML file with resources, he may use something like this:

```csharp
string outputFolder = "C://output/document_resources/";
foreach (IImageResource oneImage in images)
{
    oneImage.Save(Path.Combine(outputFolder, oneImage.FilenameWithExtension));
}
foreach (FontResourceBase oneFont in fonts)
{
    oneFont.Save(Path.Combine(outputFolder, oneFont.FilenameWithExtension));
}
foreach (CssText oneStylesheet in stylesheets)
{
    oneStylesheet.Save(Path.Combine(outputFolder, oneStylesheet.FilenameWithExtension));
}
foreach (Mp3Audio oneMp3 in audiofiles)
{
    oneMp3.Save(Path.Combine(outputFolder, oneMp3.FilenameWithExtension));
}
System.IO.File.WriteAllText("c://output/document.html", beforeEdit.GetContent());
```

## CSS resources

There also is a third way, that is designed especially for the stylesheets. The reason is that stylesheets can contain external resources too, which are presented as links with urls. For example, it can be images, fonts, and other stylesheets. In such case it is necessary to adjust such link. For coping with this [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) contains two overloads of the [GetCssContent()](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/getcsscontent/index) method.[First overload](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/getcsscontent) is parameterless and returns a `List` of strings, each one represents one stylesheet. In most cases document has only one stylesheet, so such list will have only one item. [Second overload](https://apireference.groupdocs.com/net/editor/groupdocs.editor.editabledocument/getcsscontent/methods/1) obtains two string parameters: `externalImagesPrefix` and `externalFontsPrefix`. First parameter is designated for images, while second - for fonts. Both overloads are shown in example below:

```csharp
List<string> stylesheetsWithoutPrefixes = beforeEdit.GetCssContent();
string externalImagesPrefix = "http://www.mywebsite.com/images/id=";
string externalFontsPrefix = "http://www.mywebsite.com/fonts/id=";
List<string> stylesheetsWithPrefixes = beforeEdit.GetCssContent(externalImagesPrefix, externalFontsPrefix);
```
