---
id: how-to-edit-ebook
url: editor/net/how-to-edit-ebook
title: How to edit e-Book file
weight: 56
description: "This article demonstrates how to edit e-Book files using C# programming language."
keywords: GroupDocs.Editor e-Book support, ebook format, editing electronic books, edit e-Book
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
## Introduction

For the [version 22.9](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-9-release-notes/), the GroupDocs.Editor for .NET supports 3 formats from the e-Book family: 

1. [MOBI](https://docs.fileformat.com/ebook/mobi/) (MobiPocket),
2. [AZW3](https://docs.fileformat.com/ebook/azw3/), also known as Kindle Format 8 (KF8),
3. [ePub](https://docs.fileformat.com/ebook/epub/) (Electronic Publication).

As for the 22.9 version, the AZW3 and ePub formats are supported on both import (load) and export (save), while MOBI is supported only on import (we plan to add support of MOBI on export in the next release).

## Load e-Book files for edit

GroupDocs.Editor for .NET doesn't contain loading options nor for the whole e-Book formats family neither for the specific e-Book formats — users should specify e-Books through file path or byte stream without any loading options at all. 

Code example below shows loading of 3 different e-Books in different formats into the 3 different istances of the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class from different sources:

```csharp
string mobiPath = "path/to/A-Room-with-a-View-morrison.mobi");
string azw3Path = "path/to/Around the World in 28 Languages.azw3";
string epubPath = "path/to/Alices Adventures in Wonderland.epub";

GroupDocs.Editor.Editor editorMobi = new Editor(mobiPath);

FileStream azw3Stream = File.OpenRead(azw3Path);
GroupDocs.Editor.Editor editorAzw3 = new Editor(delegate() { return azw3Stream; });

byte[] epubBytes = File.ReadAllBytes(epubPath);
MemoryStream epubStream = new MemoryStream(epubBytes);
GroupDocs.Editor.Editor editorEpub = new Editor(delegate () { return epubStream; });


// ...
// Don't forget to dispose Editors when work is done
editorMobi.Dispose();
editorAzw3.Dispose();
editorEpub.Dispose();
```

## Edit e-Book files

There is a common edit options for the whole e-Book formats family — a [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) class. The content of this class resembles the content of the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions) class, because [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) contains a subset of options from [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions) — [`EnablePagination`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/enablepagination) and [`EnableLanguageInformation`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/enablelanguageinformation) and, as in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions), they are disabled (`false`) by default.

- [`EnablePagination`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/enablepagination) — allows to enable or disable pagination in the resultant HTML document. By default is disabled (`false`). This option controls how exactly the content of the e-Book will be converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) representation while edited — in the _float_ (`false`) or in the _paged_ (`true`) mode. At the end, this options affects on the structure and representation of the HTML/CSS document, that the end-user edits in the WYSIWYG-editor.

- [`EnableLanguageInformation`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/enablelanguageinformation) — allows to export (`true`) or do not export (`false`) the language information to the resultant HTML markup. By default is disabled (`false`). This is useful when an e-Book contains text on different languages, and you want to preserve this language-specific metainformation while editing document in the WYSIWYG-editor.

Like for all supported document formats and options, in order to edit the document, user should firstly load it to the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class (this step was reviewed in the section above) and then call an [`Edit(IEditOptions)`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit/#edit) method. Like for all supported document formats, the [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) are optional and user may call a parameterless [`Edit()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) method — in this case the default [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) will be implicitly applied.

Code example below demonstrates a loading of a single ePub file to the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance and then editing it twice with two different edit options (default and custom) and generating two different [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instances from an input single ePub file. Then two different HTML markup pieces are generated from these two [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instances.

```csharp
string epubPath = "path/to/Alices Adventures in Wonderland.epub";

GroupDocs.Editor.Editor editorEpub = new Editor(epubPath);

Options.EbookEditOptions defaultEditOptions = new Options.EbookEditOptions();

Options.EbookEditOptions customEditOptions = new Options.EbookEditOptions();
customEditOptions.EnablePagination = true;
customEditOptions.EnableLanguageInformation = true;

EditableDocument defaultEdited = editorEpub.Edit(defaultEditOptions);
EditableDocument customEdited = editorEpub.Edit(customEditOptions);

string embeddedHtmlDefaultEdited = defaultEdited.GetEmbeddedHtml();
string embeddedHtmlCustomEdited = customEdited.GetEmbeddedHtml();

// ...
// Don't forget to dispose Editor and EditableDocuments when work is done
defaultEdited.Dispose();
customEdited.Dispose();
editorEpub.Dispose();
```

## Save e-Book files after edit

Saving of the e-Books is performed like for all other formats. When e-Book content was edited by the client in the WYSIWYG-editor and was sent back to the server-side, it should be passed to the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument), and then this instance should be passed to the [`GroupDocs.Editor.Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method.

Unlike other format families and unlike a single [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) class, which is common for all e-Book formats, there is no such save options class. As for the version 22.9, the GroupDocs.Editor for .NET supports saving into AZW3 and ePub, and for each of these two formats the GroupDocs.Editor for .NET contains a distinct save options class:

- [`EpubSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/epubsaveoptions) - for saving into ePub format
- [`Azw3SaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/azw3saveoptions) - for saving into AZW3 format

These classes has one common property - a `SplitHeadingLevel` of the `System.Int32` type. This property controls how (if so) to split the content of AZW3 or ePub e-book onto packages in the resultant file. It doesn't affect the representation of a file, opened in any e-Book reader; rather, it is about an internal structure of the e-Book file. If you dont bother about internal structure of the ePub or AZW3 file, you may leave this property to has the default value.

[`EpubSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/epubsaveoptions) also has an `ExportDocumentProperties` boolean property — it controls whether to export built-in and custom document properties inside the resultant IDPF ePub e-Book. If you have no plans to reconvert the resultant ePub to some other format, you may leave it intact — the default `false` value disables the exporting of the document properties, so the resultant document will be a little bit smaller in size.

Code example below demonstrates a loading of a single ePub file to the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance, editing it with default options, and saving to the ePub and AZW3 with different options for each one.

```csharp
string epubPath = "path/to/Alices Adventures in Wonderland.epub";
string epubOutputPath = "Output_ePub.epub";
string azw3OutputPath = "Output_AZW3.azw3";

GroupDocs.Editor.Editor editor = new Editor(epubPath);

//edit with default EbookEditOptions
EditableDocument edited = editor.Edit();

Options.EpubSaveOptions epubSaveOptions = new Options.EpubSaveOptions();
epubSaveOptions.ExportDocumentProperties = true;
epubSaveOptions.SplitHeadingLevel = 5;

Options.Azw3SaveOptions azw3SaveOptions = new Options.Azw3SaveOptions();
azw3SaveOptions.SplitHeadingLevel = 1;

editor.Save(edited, epubOutputPath, epubSaveOptions);
editor.Save(edited, azw3OutputPath, azw3SaveOptions);

// ...
// Don't forget to dispose Editor and EditableDocument when work is done
edited.Dispose();
editor.Dispose();
```

## Extracting metainfo from e-Book files

Like for all supported formats, the GroupDocs.Editor for .NET provides an ability to detected the document metainfo for all supported e-Book formats by using a [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) method of the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class. In case when a valid e-Book was loaded into the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance, a [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) will return an instance of a [`Metadata.EbookDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo) class, which inherits from [`IDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo) interface, which, in turn, defines 4 properties: [`Format`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo/format), [`PageCount`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo/pagecount), [`Size`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo/size), and [`IsEncrypted`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo/isencrypted).

- [`Format`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/format) property returns a [`Formats.EBookFormats`](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/ebookformats) struct, which for the e-Books can be a `Mobi`, `Azw3`, or `Epub` value.
- [`PageCount`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/pagecount) property returns an **approximate** number of pages in case of MOBI or AZW3 or a number of chapters in case of ePub. For the Mobi and AZW3, it is approximate, because Mobi/AZW3 format internally is a set of HTML documents (chapters), which are not separated on pages and even have no strict page dimensions, which allows to split content on page blocks and thus calculate the number of pages. This decision was made by Mobi/AZW3 format designers intentionally to allows variable page size (and count) on different devices — from FullHD displays to smartphones. So, for returning a page count for a Mobi/AZW3 document, GroupDocs.Editor assumes standard A4 page size in a portrait orientation, splits existing document content on such "papers", and then calculates its count. So the returning number should be treated very carefully and approximately, users should not rely on it.
- [`Size`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/size) property returns a number of bytes of e-Book file.
- [`IsEncrypted`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/ebookdocumentinfo/isencrypted) property always returns a `false` value, because e-Books cannot be encrypted with password, like PDF or Office Open XML.

Code example below demonstrates a loading of 3 different e-Books in different formats (Mobi, AZW3 and ePub) into the 3 different istances of the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class and then extracting information about them and checking it with NUnit.

```csharp
string mobiPath = "Ebook.mobi");
string azw3Path = "Ebook.azw3";
string epubPath = "Ebook.epub";

GroupDocs.Editor.Editor editorMobi = new Editor(mobiPath);                        
GroupDocs.Editor.Editor editorAzw3 = new Editor(azw3Path);
GroupDocs.Editor.Editor editorEpub = new Editor(epubPath);

GroupDocs.Editor.Metadata.IDocumentInfo mobiInfo = editorMobi.GetDocumentInfo(null);
GroupDocs.Editor.Metadata.IDocumentInfo azw3Info = editorAzw3.GetDocumentInfo(null);
GroupDocs.Editor.Metadata.IDocumentInfo epubInfo = editorEpub.GetDocumentInfo(null);

Assert.AreEqual(Formats.EBookFormats.Mobi, mobiInfo.Format);
Assert.AreEqual(Formats.EBookFormats.Azw3, azw3Info.Format);
Assert.AreEqual(Formats.EBookFormats.Epub, epubInfo.Format);

// ...
// Don't forget to dispose Editors when work is done
editorMobi.Dispose();
editorAzw3.Dispose();
editorEpub.Dispose();
```









