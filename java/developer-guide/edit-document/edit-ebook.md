---
id: how-to-edit-ebook
url: editor/java/how-to-edit-ebook
title: How to edit e-Book file
weight: 56
description: "This article demonstrates how to edit e-Book files using Java programming language."
keywords: GroupDocs.Editor e-Book support, ebook format, editing electronic books, edit e-Book
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
## Introduction

The GroupDocs.Editor for Java supports 3 formats from the e-Book family: 

1. [MOBI](https://docs.fileformat.com/ebook/mobi/) (MobiPocket),
2. [AZW3](https://docs.fileformat.com/ebook/azw3/), also known as Kindle Format 8 (KF8),
3. [ePub](https://docs.fileformat.com/ebook/epub/) (Electronic Publication).

As for the 23.9 version, the AZW3 and ePub formats are supported on both import (load) and export (save), while MOBI is supported only on import (support of MOBI on export was scheduled for the next release).

Starting from the version [23.9](https://docs.groupdocs.com/editor/java/groupdocs-editor-for-java-23-9-release-notes/), the MOBI format is fully supported on export.

## Load e-Book files for edit

GroupDocs.Editor for Java doesn't contain loading options nor for the whole e-Book formats family neither for the specific e-Book formats — users should specify e-Books through file path or byte stream without any loading options at all. 

Code example below shows loading of 3 different e-Books in different formats into the 3 different istances of the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class from different sources:

```java
String mobiPath = "path/to/A-Room-with-a-View-morrison.mobi");
String azw3Path = "path/to/Around the World in 28 Languages.azw3";
String epubPath = "path/to/Alices Adventures in Wonderland.epub";

Editor editorMobi = new Editor(mobiPath);

FileInputStream azw3Stream = new FileInputStream(azw3Path);
Editor editorAzw3 = new Editor(azw3Stream;);

byte[] epubBytes = Files.readAllBytes(Paths.get(epubPath));
ByteArrayInputStream epubStream = new ByteArrayInputStream(epubBytes);
Editor editorEpub = new Editor(epubStream);


// ...
// Don't forget to dispose Editors when work is done
editorMobi.dispose();
editorAzw3.dispose();
editorEpub.dispose();
```

## Edit e-Book files

There is a common edit options for the whole e-Book formats family — a [`EbookEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebookeditoptions/) class. The content of this class resembles the content of the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions/) class, because [`EbookEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebookeditoptions/) contains a subset of options from [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions/) — [`getEnablePagination()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebookeditoptions/#getEnablePagination--) and [`getEnableLanguageInformation()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebookeditoptions/#getEnableLanguageInformation--) and, as in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions/), they are disabled (`false`) by default.

- [`getEnablePagination()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebookeditoptions/#getEnablePagination--) — allows to enable or disable pagination in the resultant HTML document. By default is disabled (`false`). This option controls how exactly the content of the e-Book will be converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) representation while edited — in the _float_ (`false`) or in the _paged_ (`true`) mode. At the end, this options affects on the structure and representation of the HTML/CSS document, that the end-user edits in the WYSIWYG-editor.
- [`getEnableLanguageInformation()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebookeditoptions/#getEnableLanguageInformation--) — allows to export (`true`) or do not export (`false`) the language information to the resultant HTML markup. By default is disabled (`false`). This is useful when an e-Book contains text on different languages, and you want to preserve this language-specific metainformation while editing document in the WYSIWYG-editor.

Like for all supported document formats and options, in order to edit the document, user should firstly load it to the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class (this step was reviewed in the section above) and then call an [`edit(IEditOptions)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit-com.groupdocs.editor.options.IEditOptions-) method. Like for all supported document formats, the [`EbookEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebookeditoptions/) are optional and user may call a parameterless [`edit()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) method — in this case the default [`EbookEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebookeditoptions/) will be implicitly applied.

Code example below demonstrates a loading of a single ePub file to the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) instance and then editing it twice with two different edit options (default and custom) and generating two different [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instances from an input single ePub file. Then two different HTML markup pieces are generated from these two [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instances.

```java
String epubPath = "path/to/Alices Adventures in Wonderland.epub";

Editor editorEpub = new Editor(epubPath);

EbookEditOptions defaultEditOptions = new EbookEditOptions();

EbookEditOptions customEditOptions = new EbookEditOptions();
customEditOptions.setEnablePagination(true);
customEditOptions.setEnableLanguageInformation(true);

EditableDocument defaultEdited = editorEpub.edit(defaultEditOptions);
EditableDocument customEdited = editorEpub.edit(customEditOptions);

String embeddedHtmlDefaultEdited = defaultEdited.getEmbeddedHtml();
String embeddedHtmlCustomEdited = customEdited.getEmbeddedHtml();

// ...
// Don't forget to dispose Editor and EditableDocuments when work is done
defaultEdited.dispose();
customEdited.dispose();
editorEpub.dispose();
```

## Save e-Book files after edit

{{< alert style="info" >}}This section describes the saving of e-book files in the GroupDocs.Editor version 23.9 and newer. In using older versions, this description is invalid and source code is not working.{{< /alert >}}

Starting from the version 23.9 the GroupDocs.Editor for Java has obtained an ability to save e-books in all 3 supported formats: MOBI, AZW3, and ePub. Saving of the e-books is performed like for all other formats. When e-book content was edited by the client in the WYSIWYG-editor and was sent back to the server-side, it should be passed to the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/), and then this instance should be passed to the [`com.groupdocs.editor.Editor.save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/#save-java.lang.String-) method.

The [`Editor.save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/#save-java.lang.String-) method obtains an instance of the [`ISaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/isaveoptions/) interface, and for saving in some of the e-book formats the [`EbookSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebooksaveoptions/) class should be used. This class is common for all supported e-book formats within e-book family: MOBI, AZW3 and ePub. It has one constructor with a mandatory parameter — a desired output format, into which the resultant document will be stored. This specific format should be specified as one of the [`EBookFormats`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/ebookformats/) value: [`Mobi`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/ebookformats/#Mobi), [`Azw3`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/ebookformats/#Azw3), or [`Epub`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/ebookformats/#Epub). Once the instance was created, this format can be obtained and changed using the `OutputFormat` property.

The [`EbookSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebooksaveoptions/) class has also two another properties: `SplitHeadingLevel` and `ExportDocumentProperties`.

- [`getSplitHeadingLevel()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebooksaveoptions/#getSplitHeadingLevel--) of the `System.Int32` type controls how (if so) to split the content of e-book onto packages in the resultant file. It doesn't affect the representation of a file, opened in any e-Book reader; rather, it is about an internal structure of the e-Book file. If you dont bother about internal structure of the e-book file, you may leave this property to has the default value (`2`). Setting it to `0` will disable splitting, so all content of the e-Book will be incorporarted into a single package inside the resultant file.
- [`getExportDocumentProperties()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ebooksaveoptions/#getExportDocumentProperties--) of the `System.Boolean` type controls whether to export built-in and custom document properties inside the resultant e-Book file. If you have no plans to reconvert the resultant e-book to some other format, you may leave it intact — the default `false` value disables the exporting of the document properties, so the resultant document will be a little bit smaller in size.

Code example below demonstrates a loading of a single ePub file to the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) instance, editing it with default options, and saving to the ePub, AZW3, and Mobi with different options for each one.

```java
String epubPath = "path/to/Alices Adventures in Wonderland.epub";

String epubOutputPath = "Output_ePub.epub";
String azw3OutputPath = "Output_AZW3.azw3";
String mobiOutputPath = "Output_Mobi.azw3";

GroupDocs.Editor.Editor editor = new Editor(epubPath);

//edit with default EbookEditOptions
EditableDocument edited = editor.Edit();

EbookSaveOptions epubSaveOptions = new EbookSaveOptions(EBookFormats.Epub);
epubSaveOptions.setExportDocumentProperties(true);
epubSaveOptions.setSplitHeadingLevel(5);

EbookSaveOptions azw3SaveOptions = new new EbookSaveOptions(EBookFormats.Azw3);
azw3SaveOptions.setSplitHeadingLevel(1);

EbookSaveOptions mobiSaveOptions = new EbookSaveOptions(EBookFormats.Mobi);

editor.save(edited, epubOutputPath, epubSaveOptions);
editor.save(edited, azw3OutputPath, azw3SaveOptions);
editor.save(edited, mobiOutputPath, mobiSaveOptions);

// ...
// Don't forget to dispose Editor and EditableDocument when work is done
edited.dispose();
editor.dispose();
```

## Extracting metainfo from e-Book files

Like for all supported formats, the GroupDocs.Editor for Java provides an ability to detected the document metainfo for all supported e-Book formats by using a [`getDocumentInfo()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-) method of the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class. In case when a valid e-Book was loaded into the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) instance, a [`getDocumentInfo()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-) will return an instance of a [`EbookDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/ebookdocumentinfo/) class, which inherits from [`IDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/idocumentinfo/) interface, which, in turn, defines 4 properties: [`getFormat()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/idocumentinfo/#getFormat--), [`getPageCount()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/idocumentinfo/#getPageCount--), [`getSize()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/idocumentinfo/#getSize--), and [`isEncrypted()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/idocumentinfo/#isEncrypted--).

- [`getFormat()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/ebookdocumentinfo/#getFormat--) property returns a [`EBookFormats`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/ebookformats/) struct, which for the e-Books can be a `Mobi`, `Azw3`, or `Epub` value.
- [`getPageCount()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/ebookdocumentinfo/#getPageCount--) property returns an **approximate** number of pages in case of MOBI or AZW3 or a number of chapters in case of ePub. For the Mobi and AZW3, it is approximate, because Mobi/AZW3 format internally is a set of HTML documents (chapters), which are not separated on pages and even have no strict page dimensions, which allows to split content on page blocks and thus calculate the number of pages. This decision was made by Mobi/AZW3 format designers intentionally to allows variable page size (and count) on different devices — from FullHD displays to smartphones. So, for returning a page count for a Mobi/AZW3 document, GroupDocs.Editor assumes standard A4 page size in a portrait orientation, splits existing document content on such "papers", and then calculates its count. So the returning number should be treated very carefully and approximately, users should not rely on it.
- [`getSize()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/ebookdocumentinfo/#getSize--) property returns a number of bytes of e-Book file.
- [`isEncrypted()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/ebookdocumentinfo/#isEncrypted--) property always returns a `false` value, because e-Books cannot be encrypted with password, like PDF or Office Open XML.

Code example below demonstrates a loading of 3 different e-Books in different formats (Mobi, AZW3 and ePub) into the 3 different istances of the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class and then extracting information about them and checking it with NUnit.

```java
String mobiPath = "Ebook.mobi");
String azw3Path = "Ebook.azw3";
String epubPath = "Ebook.epub";

Editor editorMobi = new Editor(mobiPath);
Editor editorAzw3 = new Editor(azw3Path);
Editor editorEpub = new Editor(epubPath);

IDocumentInfo mobiInfo = editorMobi.getDocumentInfo(null);
IDocumentInfo azw3Info = editorAzw3.getDocumentInfo(null);
IDocumentInfo epubInfo = editorEpub.getDocumentInfo(null);

Assert.assertEquals(EBookFormats.Mobi, mobiInfo.getFormat());
Assert.assertEquals(EBookFormats.Azw3, azw3Info.getFormat());
Assert.assertEquals(EBookFormats.Epub, epubInfo.getFormat());

// ...
// Don't forget to dispose Editors when work is done
editorMobi.dispose();
editorAzw3.dispose();
editorEpub.dispose();
```









