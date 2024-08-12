---
id: edit-pdf
url: editor/net/edit-pdf
title: Edit PDF 
linktitle: Edit PDF document
weight: 14
description: "This guide demonstrates how to edit content of PDF files like a common text documents using a GroupDocs.Editor for .NET."
keywords: Edit PDF, Edit PDF file, Edit PDF document, Edit PDF content, Edit PDF text, Edit text in PDF
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit PDF document in the GroupDocs.Editor
        description: How to edit PDF document using the GroupDocs.Editor in C# language
        productCode: editor
        productPlatform: net
    showVideo: True
    howTo:
        name: How to edit content of the PDF document in the GroupDocs.Editor in C#
        description: Learn how to edit content of the PDF document in the different editing modes and with different editing options using the GroupDocs.Editor in C# step by step
        steps:
        - name: Load desired PDF document to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired PDF document into it.
        - name: Prepare a PdfEditOptions class
          text: Create an instance of the PdfEditOptions class and adjust its properties to meet your needs if necessary. For example, enable or disable images, select a page range, set a pagination mode.   
        - name: Call Editor.Edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.Edit method with specifying a previously prepared PdfEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class
        - name: Prepare a PdfSaveOptions class
          text: Create an instance of the PdfSaveOptions class and adjust its properties to meet your needs if necessary. All properties are optional, so you may use a default PdfSaveOptions instance. But with options you can protect the resultant document, set a compliance level or fonts.
        - name: Save edited document with Editor.Save method
          text: Pass an instance of EditableDocument with content of the edited document, instance of the PdfSaveOptions, and a destination byte stream or file path to the Editor.Save method for saving the document.
---
> This example demonstrates the standard open-edit-save pipeline with PDF documents, using different options on every step.

## Introduction

The [PDF documents, or documents in a Portable Document Format](https://docs.fileformat.com/pdf/), developed by Adobe Corp, are widely used over all Internet and document management systems. PDF format has a crucial distinction from other formats such as [DOCX](https://docs.fileformat.com/word-processing/docx/), [TXT](https://docs.fileformat.com/word-processing/txt/), or [HTML](https://docs.fileformat.com/web/html/)/[CSS](https://docs.fileformat.com/web/css/) — it is a so-called fixed-layout format. The main purpose of PDF is to be platform-independent and store the exact representation of a document, — wherever and whenever this document is opened, it should provide the per-character and even a per-pixel fidelity. This means that a document, once created, is "baked" in terms of its representation and editability. While you can freely edit any DOCX document by adding, removing or moving any part of its content, and page layout is organically updated to fit newly added paragraphs or images (or pages are collapsed when text and other content is removed from the beginning or the middle of the document), the PDF documents sustain "frozen". PDF documents have pages, but every word, character, pixel is strictly bound to every page and cannot be moved. In fact, PDF format doesn't contain the definitions of paragraphs, titles, sentences, words and even letters — internally PDF document consists of pages, where every page contains a set of glyphs (visual character), where every glyph has coordinated where it is located on this page. Same for images and other visual elements. Words, text blocks, paragraphs and so on, visually identifiable by the users, who open PDF in some viewer like Adobe Reader, are nothing more than a set of symbols, displaced by coordinates over all the page to be "like a text". For example, tables are nothing more than a set of the drawn lines to form a visual grid with glyphs, drawn in its cells. From this point of view, PDF format is much closer to raster or vector images like JPEG, PNG, or SVG, then to "truly" text documents like DOCX or TXT.

Concluding:
- Editing the PDF documents like ordinary DOCX, TXT, or HTML is an extremely difficult and complex task.
- Quality of editing the PDF document may be very close to what we can do with usual text documents, but it will never be 100%, especially when input PDF has quite complex formatting and content.
- Due to the complexity of PDF format and a process of making it editable, this operation requires a lot of processing time and memory.

From its emergence the GroupDocs.Editor for .NET had no support for editing the PDF documents. But starting from the [version 22.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/) we finally released this possibility! And this article explains in detail how to edit PDF document of any complexity like an ordinary text or WordProcessing document.

**Important note!**

{{< alert style="info" >}}GroupDocs.Editor for .NET [version 22.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/) is distributed for three platforms/runtimes: .NET Framework 2.0, .NET Framework 4.6.1 and .NET Standard 2.0. Ability to edit PDF documents is available only for the .NET Framework 4.6.1 and higher, or .NET Standard 2.0 and higher.{{< /alert >}}

## In two words

Editing of the PDF documents is the same as editing any other documents:

1. Load a PDF document to the [`GroupDocs.Editor.Editor` class](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) with  [`GroupDocs.Editor.Options.PdfLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfloadoptions), specify a password if needed.
2. Edit a document using [`GroupDocs.Editor.Editor.Edit()` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit/#edit) with [`PdfEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfeditoptions) and obtain an instance of [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument).
3. Send a document content to the client-side, edit it there with WYSIWYG-editor, send modified (edited) content back to the server-side.
4. Create an instance of [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) with modified content and call an [`GroupDocs.Editor.Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) using [`PdfSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions).

Short code example below:

```csharp
//0. Simple preparations of input data
const string filename = "NET_Framework-protected.pdf";
const string password = "password";
string inputPath = System.IO.Path.Combine(Common.TestHelper.PdfFolder, filename);

//1. Create a load options class with password
GroupDocs.Editor.Options.PdfLoadOptions loadOptions = new PdfLoadOptions();
loadOptions.Password = password;

//2. Create edit options and tune/adjust if necessary
GroupDocs.Editor.Options.PdfEditOptions editOptions = new PdfEditOptions();
editOptions.EnablePagination = true;//enable pagination for per-page processing in WYSIWYG-editor
editOptions.Pages = PageRange.FromStartPageTillEnd(3);//edit not all pages, but starting from 3rd and till the end

//3. Create Editor instance, load a document
GroupDocs.Editor.Editor editor = new Editor(inputPath, loadOptions);

//4. Edit a document and generate EditableDocument
GroupDocs.Editor.EditableDocument originalDoc = editor.Edit(editOptions);

//5. Generate HTML/CSS, send it to WYSIWYG, edit there...and obtain edited version
string originalContent = originalDoc.GetEmbeddedHtml();
string editedContent = originalContent.Replace(".NET Framework", "I love Java!!!");

//6. Generate EditableDocument from edited content
EditableDocument editedDoc = EditableDocument.FromMarkup(editedContent, null);

//7. Create and adjust save options
GroupDocs.Editor.Options.PdfSaveOptions saveOptions = new PdfSaveOptions();
saveOptions.Compliance = PdfCompliance.Pdf20;

//8. Save to a file or a stream
string outputPath = System.IO.Path.Combine(Common.TestHelper.OutputFolder, filename);
editor.Save(editedDoc, outputPath, saveOptions);

//9. Don't forget to dispose all resources
originalDoc.Dispose();
editedDoc.Dispose();
editor.Dispose();
```

## Loading

[Class `GroupDocs.Editor.Options.PdfLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfloadoptions) is responsible for loading the PDF files into the [`GroupDocs.Editor.Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor). It has only one property — a [`Password`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfloadoptions/password) of a [`string`](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0) type. By default it is a `null` — no password is specified. This property is vital when an input document is encoded with a password. If a document is not encoded — property value is ignored whether it was specified or not.

Actually, when input PDF is not password-protected, the `PdfLoadOptions` is not necessary at all — the GroupDocs.Editor will automatically detect the PDF format and apply the default `PdfLoadOptions` by itself. However, specifying even default `PdfLoadOptions` will speed-up the document processing, because in this case the GroupDocs.Editor will not spend the processing time for the automatic format detection routine.

```csharp
//Creating the default PDF loading options
PdfLoadOptions loadOptions = new PdfLoadOptions();

//Setting a password
loadOptions.Password = "some_password";

string inputPdfPath = System.IO.Path.Combine(Common.TestHelper.PdfFolder, "NET_Framework-protected.pdf");

//Loading a PDF without PDF load options
Editor editor1 = new Editor(inputPdfPath);

//Loading a PDF with PDF load options
Editor editor2 = new Editor(inputPdfPath, loadOptions);
```

## Editing

Like for other format families in GroupDocs.Editor, there is a special [`GroupDocs.Editor.Options.PdfEditOptions` class](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfeditoptions) for editing the PDF documents. This class has no its own members — instead it implements a [`FixedLayoutEditOptionsBase` abstract class](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/fixedlayouteditoptionsbase), which is common for PDF and XPS formats (because they are very close in their purpose and idea).

The next properties are inherited from the `FixedLayoutEditOptionsBase`:

1. [`Boolean`](https://docs.microsoft.com/en-us/dotnet/api/system.boolean?view=net-6.0) flag [`SkipImages`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/fixedlayouteditoptionsbase/skipimages). By default if has a `false` value — images are not skipped and are preserved. However, if you need only textual information from the document, you can set this flag to true.

2. [`Boolean`](https://docs.microsoft.com/en-us/dotnet/api/system.boolean?view=net-6.0) flag [`EnablePagination`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/fixedlayouteditoptionsbase/enablepagination). It has the exact meaning as [the same flag](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions/enablepagination) in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions). This flag sets the document conversion mode: the **float** (default value is `false`) or **paginal** (`true`). For the PDF and XPS documents it means the same:
> - When the float mode is selected, the document content will be converted to a pageless (float) HTML document, where there is only a single page (like any common web-document).
>
> - When the paginal mode is selected, the pages of the document will be preserved in the generated HTML document, like it can be seen in the PDF viewer like Adobe Reader.
>Actually, the relevance and necessity of this mode relies mostly on your WYSIWYG-editor.

3. [`Pages` property](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/fixedlayouteditoptionsbase/pages) of the [`PageRange` type](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pagerange). `PageRange` is an immutable struct, that holds a page range of any document, without relation to the specific document. And the `Pages` property through a `PageRange` struct allows setting a page range, which should be processed. By default all the pages of the input document are processed ([`PageRange.IsDefault`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pagerange/isdefault)` == true`). You can choose the different ways to set a page range using different static methods from a `PageRange` struct. Also pay attention — in `PageRange` pages are specified via page numbers, not via indexes, so they are 1-based, but not 0-based.

When the instance of a `PdfEditOptions` class is created and adjusted, you can pass it to the [`Editor.Edit(PdfEditOptions editOptions)`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit/#edit) method and obtain an instance of [`EditableDocument` class](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument), which is ready for generating HTML/CSS markup and sending it to the client-side.

By the way, if a default `PdfEditOptions` instance is acceptable for you, i.e. you do not want to adjust its settings, you allowed to omit `PdfEditOptions` creating at all — just call the [`Editor.Edit()` parameterless overload](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) and the GroupDocs.Editor will internally generate and apply the default `PdfEditOptions` for the input PDF document.

Code sample below demonstrates all described in details:

```csharp
//0. Prepare path to your input PDF file
const string filename = "Comparison for .NET.pdf";
string inputPdfPath = System.IO.Path.Combine(Common.TestHelper.PdfFolder, filename);

//1. Create Editor instance - we do not use PdfLoadOptions here
Editor editor = new Editor(inputPdfPath);

//2. Edit PDF document with default PdfEditOptions - no need to create and pass PdfEditOptions explicitly
EditableDocument originalDefaultDoc = editor.Edit();

//3. Create and adjust PdfEditOptions for advanced edit

//3.1. Create instance of PdfEditOptions class with specified pagination mode (enabled)
PdfEditOptions editOptions = new PdfEditOptions(true);

//3.2. Set to skip images - now they will be omitted during processing
editOptions.SkipImages = true;

//3.3. Set a page range - only second and third pages (input document initially has 4 pages)
editOptions.Pages = PageRange.FromStartPageWithCount(2, 2);//don't forget that page numbers are 1-based

//4. Edit the same PDF document with tuned edit options
EditableDocument tunedDoc = editor.Edit(editOptions);
```

## Saving

Like for other document formats, there is a special class that is responsible for saving the PDF documents — a [`GroupDocs.Editor.Options.PdfSaveOptions` class](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions), which implements [`ISaveOptions` interface](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/isaveoptions).

This class has the next properties:

1. [`String`](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-6.0) [property `Password`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions/password) — allows to protect the output PDF document with a specified password. By default is `null` — password-protection is not applied. When specified and is not a `null` or empty string, then the output PDF will be encrypted with RC4 (key length of 128 bit).

2. [`Compliance` property](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions/compliance) allows setting the PDF standards compliance level for output PDF. It has an enum type [`PdfCompliance`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfcompliance), each value represents one specific PDF compliance level. By default is a PDF 1.7 (ISO 32000-1) standard.

3. [`OptimizeMemoryUsage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions/optimizememoryusage) [`boolean`](https://docs.microsoft.com/en-us/dotnet/api/system.boolean?view=net-6.0) flag, that allows to modify the process of generation of an output PDF document from an `EditableDocument` instance in such a way that this process will take a lesser memory consumption at the cost of the longer processing time. By default it has a `false` value, which means that the memory optimization is disabled for the sake of better performance. In case of extremely huge documents, enabling this property is vital in order to cope with [`OutOfMemoryException`](https://docs.microsoft.com/en-us/dotnet/api/system.OutOfMemoryException?view=net-6.0), especially on 32-bit processes.

4. [`FontEmbedding`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions/fontembedding/) property of [`FontEmbeddingOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/fontembeddingoptions) type is responsible for embedding the font resources into the resultant PDF document. `FontEmbeddingOptions` is an enum that has several values, which allow to control which fonts should be embedded into PDF. By default fonts are not embedded at all (`NotEmbed`). When specifying the `EmbedAll` enum value, all used fonts in the document will be embedded inside the resultant PDF. When specifying the `EmbedWithoutSystem` — only those fonts, which are absent in the current operating system (where the GroupDocs.Editor is running).

Unlike the [`PdfLoadOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfloadoptions) and [`PdfEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfeditoptions), which are optional when they are default (may be omitted during loading and editing respectively), the [`PdfSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions) is mandatory even if all its values are default.

Code sample below shows all necessary preparations (load and edit operations) and then saving in details.

```csharp
//0. Prepare path to your input PDF file
const string filename = "Comparison for .NET.pdf";
string inputPdfPath = System.IO.Path.Combine(Common.TestHelper.PdfFolder, filename);

//1. Create Editor instance - we do not use PdfLoadOptions here
Editor editor = new Editor(inputPdfPath);

//2. Edit PDF document with default PdfEditOptions - no need to create and pass PdfEditOptions explicitly
EditableDocument originalDoc = editor.Edit();

//3. Generate HTML/CSS/resources, send to WYSIWYG-editor, edit there, send back to server and create EditableDocument... omitted here
EditableDocument editedDoc = originalDoc;//just use the same in the sake of simplicity

//4. Prepare PDF save options:
//4.1. Create instance
Options.PdfSaveOptions saveOptionsAdjusted = new PdfSaveOptions();
//4.2. Adjust properties
saveOptionsAdjusted.Password = "some_password";//protect output PDF with password
saveOptionsAdjusted.FontEmbedding = FontEmbeddingOptions.EmbedAll;//set font embedding - all used
saveOptionsAdjusted.Compliance = PdfCompliance.Pdf20;//set a PDF 2.0 (ISO 32000-2) standard compliance
saveOptionsAdjusted.OptimizeMemoryUsage = true;//set memory optimization on

//5. Create an output stream and save to it
using (System.IO.MemoryStream outputStream = new MemoryStream())
{
	editor.Save(editedDoc, outputStream, saveOptionsAdjusted);
}

//6. Create another, default at this time, PDF save options
Options.PdfSaveOptions saveOptionsDefault = new PdfSaveOptions();

//7. Save to the file path at this time
string outputPdfPath = System.IO.Path.Combine(Common.TestHelper.OutputFolder, filename);
editor.Save(editedDoc, outputPdfPath, saveOptionsDefault);

//8. Don't forget to dispose all
originalDoc.Dispose();
editedDoc.Dispose();
editor.Dispose();
```

In this example we’ve created two output PDF files with different PDF saving options from one source EditableDocument, and saved them to the two distinct storages (memory and disk).

## Obtaining PDF document info

Article [Extracting document metainfo](https://docs.groupdocs.com/editor/net/extracting-document-metainfo/) describes the [`GetDocumentInfo()` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo), that allows to detect the document format and extract its metadata without editing it. Actually, after adding PDF support to the GroupDocs.Editor, this mechanism also works with PDF documents.

When the [`GetDocumentInfo()` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) is called for the [`Editor` class](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance that is loaded with a PDF document, the method will return a [`GroupDocs.Editor.Metadata.FixedLayoutDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/fixedlayoutdocumentinfo) instance — it is a common class for all fixed-layout documents, PDF and XPS in particular.

As all [`IDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo) implementations, it has four properties:

1. [`Format` property](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/fixedlayoutdocumentinfo/format) of [`Formats.FixedLayoutFormats` type](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/fixedlayoutformats). For PDF documents it always is a [`FixedLayoutFormats.Pdf`](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/fixedlayoutformats/fields/pdf).
2. [`PageCount` property](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/fixedlayoutdocumentinfo/pagecount) of [integer (`Int32`)](https://docs.microsoft.com/en-us/dotnet/api/system.int32?view=net-6.0) type. Returns a number of pages.
3. [`Size` property](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/fixedlayoutdocumentinfo/size) of a [long integer (`Int64`)](https://docs.microsoft.com/en-us/dotnet/api/system.int64?view=net-6.0) type. Returns a file size in bytes.
4. [`IsEncrypted` property](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/fixedlayoutdocumentinfo/isencrypted) of a [`Boolean`](https://docs.microsoft.com/en-us/dotnet/api/system.boolean?view=net-6.0) type. For the password-encoded documents it returns `true`, and `false` otherwise. For XPS files it always returns `false`, because the XPS format doesn’t support encryption.

As usual, if the input PDF, loaded into the [`Editor` class](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor), is encoded, then its correct password should be specified in the [`GetDocumentInfo()` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo). If PDF is not encoded, then the value of the [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) is ignored.

**Important note!**

{{< alert style="info" >}}Unlike the editing process, the extraction of metadata from PDF doesn’t require the .NET Framework 4.6.1 and higher, or .NET Standard 2.0 and higher — it is supportable from the lowest .NET Framework 2.0.{{< /alert >}}

The code example below demonstrates the extracting metadata from two PDFs: first one is unprotected, while second - protected.

```csharp
//0. Prepare path to your input unprotected and protected PDF files
const string filenameUnprotected = "Comparison for .NET.pdf";
const string filenameProtected = "NET_Framework-protected.pdf";
string inputPdfUnprotectedPath = System.IO.Path.Combine(Common.TestHelper.PdfFolder, filenameUnprotected);
string inputPdfProtectedPath = System.IO.Path.Combine(Common.TestHelper.PdfFolder, filenameProtected);

//1. Create two Editor instances - we do not use PdfLoadOptions here
Editor editorUnprotected = new Editor(inputPdfUnprotectedPath);
Editor editorProtected = new Editor(inputPdfProtectedPath);

//2. Extract metadata: do not specify a password (null value instead) for the 1st, and DO specify for the 2nd
GroupDocs.Editor.Metadata.IDocumentInfo unprotectedPdfInfo = editorUnprotected.GetDocumentInfo(null);
GroupDocs.Editor.Metadata.IDocumentInfo protectedPdfInfo = editorProtected.GetDocumentInfo("password");

//3. Cast it if necessary and check properties
GroupDocs.Editor.Metadata.FixedLayoutDocumentInfo castedUnprotected = (FixedLayoutDocumentInfo)unprotectedPdfInfo;
Assert.AreEqual(4, castedUnprotected.PageCount);
Assert.IsFalse(castedUnprotected.IsEncrypted);
Assert.AreEqual(246298, castedUnprotected.Size);
Assert.AreEqual(Formats.FixedLayoutFormats.Pdf, castedUnprotected.Format);

Assert.AreEqual(14, protectedPdfInfo.PageCount);
Assert.IsTrue(protectedPdfInfo.IsEncrypted);
Assert.AreEqual(1016198, protectedPdfInfo.Size);
Assert.AreEqual(Formats.FixedLayoutFormats.Pdf, protectedPdfInfo.Format);
```

## Extended info

### EnablePagination in save options

In the previous versions of the GroupDocs.Editor the [`PdfSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions) contained an [`EnablePagination`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions/enablepagination) boolean flag. Starting from the [version 22.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/) this flag was marked as [`Obsolete`](https://docs.microsoft.com/en-us/dotnet/api/system.obsoleteattribute?view=net-6.0) and its value is ignored whether it was specified by the user or was left intact. This was done by the improvements of the backward converter, which generates the output PDF from [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument). Now the backward converter automatically detects the content in the obtained [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) — if it was generated with the [`EnablePagination`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/fixedlayouteditoptionsbase/enablepagination) flag set to `true` in [`PdfEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfeditoptions), then pagination will be applied to the output PDF, and same for the `false` value.

If you’re using the GroupDocs.Editor version older then 22.7, you should specify the [`EnablePagination`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions/enablepagination) flag manually and in accordance with the value of the same flag in the [`PdfEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfeditoptions): if `EnablePagination` in [`PdfEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfeditoptions) is set to `true`, then in the [`PdfSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions) it should also be `true`; same for `false`.

Starting from the [version 23.2](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-2-release-notes/) the `EnablePagination` flag was completely removed from the [`PdfSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions) class, so please update your code while migrating from the older versions to the newer.

### Different output formats

Keep in mind that, when input PDF was edited and you’re going to save it, it is not necessary to save it exactly in the PDF format — you are free to choose any compatible format, like all WordProcessing formats, or text format, or eBook format.

Code example below shows editing a PDF file and then saving the edited content to three different files in different formats.

```csharp
//0. Prepare path to your input PDF file
const string filename = "Comparison for .NET.pdf";
string inputPdfPath = System.IO.Path.Combine(Common.TestHelper.PdfFolder, filename);

//1. Create Editor instance - we do not use PdfLoadOptions here
Editor editor = new Editor(inputPdfPath);

//2. Edit PDF document with default PdfEditOptions - no need to create and pass PdfEditOptions explicitly
EditableDocument originalDoc = editor.Edit();

//3. Generate HTML/CSS/resources, send to WYSIWYG-editor, edit there, send back to server and create EditableDocument... omitted here
EditableDocument editedDoc = originalDoc;//just use the same in the sake of simplicity

//4. Prepare PDF save options
Options.PdfSaveOptions pdfSaveOptions = new PdfSaveOptions();
pdfSaveOptions.Compliance = PdfCompliance.PdfA2a;

//5. Prepare DOCX save options
Options.WordProcessingSaveOptions docxSaveOptions = new WordProcessingSaveOptions(Formats.WordProcessingFormats.Docx);

//6. Prepare TXT save options
Options.TextSaveOptions txtSaveOptions = new TextSaveOptions();
txtSaveOptions.PreserveTableLayout = true;

//7. Save to PDF format to the stream
using (MemoryStream pdfStream = new MemoryStream())
{
	editor.Save(editedDoc, pdfStream, pdfSaveOptions);
}

//8. Save to DOCX format to the stream
using (MemoryStream docxStream = new MemoryStream())
{
	editor.Save(editedDoc, docxStream, docxSaveOptions);
}

//9. Save to TXT format to the stream
using (MemoryStream txtStream = new MemoryStream())
{
	editor.Save(editedDoc, txtStream, txtSaveOptions);
}

//10. Dispose all resources
editedDoc.Dispose();
originalDoc.Dispose();
editor.Dispose();
```
