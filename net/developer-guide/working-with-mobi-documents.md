---
id: how-to-edit-mobi-file
url: editor/net/how-to-edit-mobi-file
title: How to edit Mobi file
weight: 52
description: "This article demonstrates how to edit Mobi files using C# programming language."
keywords: GroupDocs.Editor Mobi support, Mobi format, editing Mobi documents
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
## Introduction

Mobi format is an E-Book format, developed by the French company MobiPocket and is based on XML. E-Books in this format can contain text with rich formatting, images, and different annotations like bookmarks, notes, highlights, corrections and so on. Mobi books can have a DRM protection.

Starting from the version 20.7, the GroupDocs.Editor for .NET is able to open (load) Mobi documents for editing. However, on this moment saving _into_ Mobi is not available, so users should choose some other format (the most likely, AZW3, or some of WordProcessing) in order to save edited Mobi document.

Starting from the [version 22.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/) the GroupDocs.Editor is able to edit the e-books in [AZW3 format](https://docs.fileformat.com/ebook/azw3/), also known as Kindle Format 8 (KF8), whech may be considered as a successor to the Mobi. Unlike the MOBI, the AZW3 are supported on inport (since version 22.7) and export (since version 22.9).

## Loading Mobi file for edit

Despite of a distinct format, which doesn't belong to any of existing format families, Mobi has no dedicated loading options. So for loading it into the instance of the [`Editor`](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor) class, users should simply specify a path to the Mobi file or a stream with its content in the constructor:

```csharp
string inputMobiPath = "C://input/book.mobi";
Editor editorFromPath = new Editor(inputMobiPath);
  
FileStream inputMobiStream = File.OpenRead("C://input/book.mobi");
Editor editorFromStream = new Editor(delegate() { return inputMobiStream; });
```

There is no loading options, because Mobi has nothing to tune-up during loading, — it cannot have a password protection, and can be processed only by one way.

## Editing Mobi file

Because Mobi belongs to the e-Book format family, it utilize common edit options for all e-Book formats — an [`EbookEditOptions`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions). This class may be described as a truncated version of a [`WordProcessingEditOptions`](https://apireference.groupdocs.com/net/editor/groupdocs.editor.options/wordprocessingeditoptions) class, because [`EbookEditOptions`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) contains a subset of options from [`WordProcessingEditOptions`](https://apireference.groupdocs.com/net/editor/groupdocs.editor.options/wordprocessingeditoptions) — [`EnablePagination`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/properties/enablepagination) and [`EnableLanguageInformation`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/properties/enablelanguageinformation) and, as in the [`WordProcessingEditOptions`](https://apireference.groupdocs.com/net/editor/groupdocs.editor.options/wordprocessingeditoptions), they are disabled (`false`) by default:

```csharp
public bool EnablePagination { get; set; }
  
public bool EnableLanguageInformation { get; set; }
```

They have exactly the same meaning as their "siblings" from [`WordProcessingEditOptions`](https://apireference.groupdocs.com/net/editor/groupdocs.editor.options/wordprocessingeditoptions). [`EnablePagination`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/properties/enablepagination) allows to switch between float and paginal mode in the resultant HTML document. [`EnableLanguageInformation`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/properties/enablelanguageinformation) allows to enable exporting language information in HTML. This is very useful for books, which have parts of text, written on different languages.

Example of usage is below (let's assume that [`Editor`](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor) instance with loaded Mobi document is already created):

```csharp
Options.EbookEditOptions editOptions = new Options.EbookEditOptions();
editOptions.EnablePagination = true;
editOptions.EnableLanguageInformation = true;
using (EditableDocument opened = editor.Edit(editOptions))
{
    //save it or pass to the WYSIWYG-editor
}
```

{{< alert style="info" >}}The [`EbookEditOptions`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) class was firstly introduced in the GroupDocs.Editor for .NET [version 22.9](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-9-release-notes/). However, prior this version, there was a class `MobiEditOptions`, which was designed especially for the Mobi format. In the 22.9 version of GroupDocs.Editor for .NET this `MobiEditOptions` class was removed in favor of the more generalized [`EbookEditOptions`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions), so when updating from the 22.7 or older versions to the 22.9 or newer, please update the class name accordingly. All class members in these both classes are identical.{{< /alert >}}

## Save Mobi file after edit

As it was stated in the introduction, for now saving edited document in Mobi format is not supported.

## Detecting Mobi file

As for documents of all supporting types, Mobi documents can be detected using a [`GetDocumentInfo()`](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo) method of the [`Editor`](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor) class. In case when a valid Mobi document was loaded into the [`Editor`](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor) instance, a [`GetDocumentInfo()`](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/getdocumentinfo) will return an instance of a [`Metadata.EbookDocumentInfo`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo) class, which inherits from [`IDocumentInfo`](https://apireference.groupdocs.com/net/editor/groupdocs.editor.metadata/idocumentinfo) interface, which, in turn, defines 4 properties: `Format`, `PageCount`, `Size`, and `IsEncrypted`.

- [`Format`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/properties/format) property returns a [`Formats.EBookFormats`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats) struct, which for the files with a `\*.mobi` extension can have a `Mobi` or `Azw3` value.
- [`PageCount`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/properties/pagecount) property returns an **approximate** number of pages in case of MOBI or AZW3 or a number of chapters in case of ePub. For the Mobi, it is approximate, because Mobi format internally is a set of HTML documents (chapters), which are not separated on pages and even have no strict page dimensions, which allows to split content on page blocks and thus calculate the number of pages. This decision was made by Mobi format designers intentionally to allows variable page size (and count) on different devices — from FullHD displays to smartphones. So, for returning a page count for a Mobi document, GroupDocs.Editor assumes standard A4 page size in a portrait orientation, splits existing document content on such "papers", and then calculates its count. So the returning number should be treated very carefully and approximately, users should not rely on it.
- [`Size`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/properties/size) property returns a number of bytes of a Mobi document.
- [`IsEncrypted`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/properties/isencrypted) property always returns a `false` value, because Mobi documents cannot be encrypted with password, like PDF or Office Open XML.
