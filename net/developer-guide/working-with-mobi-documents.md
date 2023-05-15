---
id: how-to-edit-mobi-file
url: editor/net/how-to-edit-mobi-file
title: How to edit Mobi file
weight: 58
description: "This article demonstrates how to edit Mobi files using C# programming language."
keywords: GroupDocs.Editor Mobi support, Mobi format, editing Mobi documents
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
## Introduction

Mobi format is an E-Book format, developed by the French company MobiPocket and is based on XML. E-Books in this format can contain text with rich formatting, images, and different annotations like bookmarks, notes, highlights, corrections and so on. Mobi books can have a DRM protection.

Starting from the [version 20.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-20-7-release-notes/), the GroupDocs.Editor for .NET is able to open (load) Mobi documents for editing. However, on that moment a saving _into_ Mobi was not available, so users were forced to choose some other format (the most likely AZW3 or ePub) in order to save edited Mobi document.

Starting from the [version 22.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/) the GroupDocs.Editor is able to edit the e-books in [AZW3 format](https://docs.fileformat.com/ebook/azw3/), also known as Kindle Format 8 (KF8), whech may be considered as a successor to the Mobi. Unlike the MOBI, the AZW3 are supported on import (since version 22.7) and export (since version 22.9).

Starting from the [version 23.4](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-4-release-notes/) the GroupDocs.Editor for .NET is able to save (export) documents in the Mobi format, so from this version it can be said that Mobi is fully supported: on import, export and auto-detection.

## Loading Mobi file for edit

Despite of a distinct format, which doesn't belong to any of existing format families, Mobi has no dedicated loading options. So for loading it into the instance of the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class, users should simply specify a path to the Mobi file or a stream with its content in the constructor:

```csharp
string inputMobiPath = "C://input/book.mobi";
Editor editorFromPath = new Editor(inputMobiPath);
  
FileStream inputMobiStream = File.OpenRead("C://input/book.mobi");
Editor editorFromStream = new Editor(delegate() { return inputMobiStream; });
```

There is no loading options, because Mobi has nothing to tune-up during loading, — it cannot have a password protection, and can be processed only in one way.

## Editing Mobi file

Because Mobi belongs to the e-Book format family, it utilize common edit options for all e-Book formats — an [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions). This class may be described as a truncated version of a [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions) class, because [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) contains a subset of options from [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions) — [`EnablePagination`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/enablepagination) and [`EnableLanguageInformation`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/enablelanguageinformation) and, as in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions), they are disabled (`false`) by default:

```csharp
public bool EnablePagination { get; set; }
  
public bool EnableLanguageInformation { get; set; }
```

They have exactly the same meaning as their "siblings" from [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions). [`EnablePagination`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/enablepagination) allows to switch between float and paginal mode in the resultant HTML document. [`EnableLanguageInformation`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/enablelanguageinformation) allows to enable exporting language information in HTML. This is very useful for books, which have parts of text, written on different languages.

Example of usage is below (let's assume that [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance with loaded Mobi document is already created):

```csharp
Options.EbookEditOptions editOptions = new Options.EbookEditOptions();
editOptions.EnablePagination = true;
editOptions.EnableLanguageInformation = true;
using (EditableDocument opened = editor.Edit(editOptions))
{
    //save it or pass to the WYSIWYG-editor
}
```

{{< alert style="info" >}}The [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) class was firstly introduced in the GroupDocs.Editor for .NET [version 22.9](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-9-release-notes/). However, prior this version, there was a class `MobiEditOptions`, which was designed especially for the Mobi format. In the 22.9 version of GroupDocs.Editor for .NET this `MobiEditOptions` class was removed in favor to the more generalized [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions), so when updating from the 22.7 or older versions to the 22.9 or newer, please update the class name accordingly. All class members in these both classes are identical.{{< /alert >}}

## Save Mobi file after edit

Starting from the [version 20.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-20-7-release-notes/), and before the [version 23.4](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-4-release-notes/), the GroupDocs.Editor for .NET was able only to load and edit the Mobi documents, but not to save (export) in the Mobi format. In other words, it was able to open and edit the Mobi document, and then save the edited document in some other format, but not in the Mobi. And in version 23.4 the export in the Mobi format was finally implemented.

For all the format families the saving procedure is the same — it is required to obtain a content of the edited document on the server-side, create an instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) from it, and then pass it to the [`GroupDocs.Editor.Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) instance method. And because the Mobi format belongs to the e-Book formats family, in order to save the document in the Mobi format a [`EbookSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebooksaveoptions) class is required.

This class has a [mandatory constructor with a single parameter](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebooksaveoptions/ebooksaveoptions/) — a desired [e-Book format](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats/). For the Mobi it should be a [`EBookFormats.Mobi`](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats/mobi/). All other parameters in the EbookSaveOptions class are in fact optional, but nevertheless described below:

- [`SplitHeadingLevel`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebooksaveoptions/splitheadinglevel/) of the `System.Int32` type controls an internal structure of the generated Mobi file: whether its internal content is divided onto packages and if yes, then how. This parameter exists because a content of the Mobi file is stored in the packages, usually, a package per chapter. And some users may want to control the policy of distribution of the content into the packages. By default its parameter is “`2`” — the most optimal.
- [`ExportDocumentProperties`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebooksaveoptions/exportdocumentproperties/) of the `System.Boolean` type also has a relation to the internal structure of the Mobi file — it decides whether to embed the built-in and custom document properties inside the resultant Mobi file (`true`) or not (`false`). By default is `false` — do not embed.

Concluding: in order to save the edited document in the Mobi format, the user should create an instance of the [`EbookSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebooksaveoptions) class with [`EBookFormats.Mobi`](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats/mobi/) argument in the constructor parameter, and then pass this instance to the [`Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method along with other arguments.

The code example below demonstrates the full roundtrip: opening a Mobi document, editing it, and saving the edited version to the Mobi format.

```csharp
//Create an Editor instance and load an input Mobi file
Editor editor = new Editor("Input.mobi");

//Create edit options
Options.EbookEditOptions editOptions = new EbookEditOptions();

//Edit the Mobi document and obtain
EditableDocument doc = editor.Edit(editOptions);

//Send EditableDocument to the client-side, edit it there, and obtain an edited version

//Create Mobi save options
Options.EbookSaveOptions saveOptions = new EbookSaveOptions(Formats.EBookFormats.Mobi);
saveOptions.ExportDocumentProperties = true;//Tune save options

//Save the edited document in a Mobi format
editor.Save(doc, "Output.mobi", saveOptions);

//Dispose document and Editor instances
doc.Dispose();
editor.Dispose();
```

## Detecting Mobi file

As for documents of all supporting types, Mobi documents can be detected using a [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) method of the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class. In case when a valid Mobi document was loaded into the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance, a [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) will return an instance of a [`Metadata.EbookDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo) class, which inherits from [`IDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo) interface, which, in turn, defines 4 properties: `Format`, `PageCount`, `Size`, and `IsEncrypted`.

- [`Format`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/format) property returns a [`Formats.EBookFormats`](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats) struct, which for the files with a `*.mobi` extension can have a `Mobi` or `Azw3` value.
- [`PageCount`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/pagecount) property returns an **approximate** number of pages in case of MOBI or AZW3 or a number of chapters in case of ePub. For the Mobi, it is approximate, because Mobi format internally is a set of HTML documents (chapters), which are not separated on pages and even have no strict page dimensions, which allows to split content on page blocks and thus calculate the number of pages. This decision was made by Mobi format designers intentionally to allows variable page size (and count) on different devices — from FullHD displays to smartphones. So, for returning a page count for a Mobi document, GroupDocs.Editor assumes standard A4 page size in a portrait orientation, splits existing document content on such "papers", and then calculates its count. So the returning number should be treated very carefully and approximately, users should not rely on it.
- [`Size`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/size) property returns a number of bytes of a Mobi document.
- [`IsEncrypted`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/isencrypted) property always returns a `false` value, because Mobi documents cannot be encrypted with password, like PDF or Office Open XML.
