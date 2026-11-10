---
id: edit-pdf
url: editor/java/edit-pdf
title: Edit PDF 
linktitle: Edit PDF document
weight: 58
description: "This guide demonstrates how to edit content of PDF files like a common text documents using a GroupDocs.Editor for Java."
keywords: Edit PDF, Edit PDF file, Edit PDF document, Edit PDF content, Edit PDF text, Edit text in PDF
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit PDF document in the GroupDocs.Editor
        description: How to edit PDF document using the GroupDocs.Editor in Java language
        productCode: editor
        productPlatform: java
    showVideo: True
    howTo:
        name: How to edit content of the PDF document in the GroupDocs.Editor in Java
        description: Learn how to edit content of the PDF document in the different editing modes and with different editing options using the GroupDocs.Editor in Java step by step
        steps:
        - name: Load desired PDF document to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired PDF document into it.
        - name: Prepare a PdfEditOptions class
          text: Create an instance of the PdfEditOptions class and adjust its properties to meet your needs if necessary. For example, enable or disable images, select a page range, set a pagination mode.   
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.edit method with specifying a previously prepared PdfEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class
        - name: Prepare a PdfSaveOptions class
          text: Create an instance of the PdfSaveOptions class and adjust its properties to meet your needs if necessary. All properties are optional, so you may use a default PdfSaveOptions instance. But with options you can protect the resultant document, set a compliance level or fonts.
        - name: Save edited document with Editor.save method
          text: Pass an instance of EditableDocument with content of the edited document, instance of the PdfSaveOptions, and a destination byte stream or file path to the Editor.save method for saving the document.
---
> This example demonstrates the standard open-edit-save pipeline with PDF documents, using different options on every step.

## Introduction

The [PDF documents, or documents in a Portable Document Format](https://docs.fileformat.com/pdf/), developed by Adobe Corp, are widely used over all Internet and document management systems. PDF format has a crucial distinction from other formats such as [DOCX](https://docs.fileformat.com/word-processing/docx/), [TXT](https://docs.fileformat.com/word-processing/txt/), or [HTML](https://docs.fileformat.com/web/html/)/[CSS](https://docs.fileformat.com/web/css/) — it is a so-called fixed-layout format. The main purpose of PDF is to be platform-independent and store the exact representation of a document, — wherever and whenever this document is opened, it should provide the per-character and even a per-pixel fidelity. This means that a document, once created, is "baked" in terms of its representation and editability. While you can freely edit any DOCX document by adding, removing or moving any part of its content, and page layout is organically updated to fit newly added paragraphs or images (or pages are collapsed when text and other content is removed from the beginning or the middle of the document), the PDF documents sustain "frozen". PDF documents have pages, but every word, character, pixel is strictly bound to every page and cannot be moved. In fact, PDF format doesn't contain the definitions of paragraphs, titles, sentences, words and even letters — internally PDF document consists of pages, where every page contains a set of glyphs (visual character), where every glyph has coordinates where it is located on this page. Same for images and other visual elements. Words, text blocks, paragraphs and so on, visually identifiable by the users, who open PDF in some viewer like Adobe Reader, are nothing more than a set of symbols, displaced by coordinates over all the page to be "like a text". For example, tables are nothing more than a set of the drawn lines to form a visual grid with glyphs, drawn in its cells. From this point of view, PDF format is much closer to raster or vector images like JPEG, PNG, or SVG, then to "truly" text documents like DOCX or TXT.

Concluding:
- Editing the PDF documents like ordinary DOCX, TXT, or HTML is an extremely difficult and complex task.
- Quality of editing the PDF document may be very close to what we can do with usual text documents, but it will never be 100%, especially when input PDF has quite complex formatting and content.
- Due to the complexity of PDF format and a process of making it editable, this operation requires a lot of processing time and memory.

From its emergence the GroupDocs.Editor for Java had no support for editing the PDF documents. But starting from the [version 26.7](https://releases.groupdocs.com/editor/java/release-notes/2026/groupdocs-editor-for-java-26-7-release-notes/) we finally released this possibility! And this article explains in detail how to edit PDF document of any complexity like an ordinary text or WordProcessing document.

## In two words

Editing of the PDF documents is the same as editing any other documents:

1. Load a PDF document to the [`com.groupdocs.editor.Editor` class](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) with [`com.groupdocs.editor.options.PdfLoadOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfloadoptions), specify a password if needed.
2. Edit a document using [`Editor.edit()` method](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit-com.groupdocs.editor.options.IEditOptions-) with [`PdfEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfeditoptions) and obtain an instance of [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument).
3. Send a document content to the client-side, edit it there with WYSIWYG-editor, send modified (edited) content back to the server-side.
4. Create an instance of [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) with modified content and call an [`Editor.save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#save-com.groupdocs.editor.EditableDocument-java.lang.String-com.groupdocs.editor.options.ISaveOptions-) using [`PdfSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions).

Short code example below:

```java
import com.groupdocs.editor.Editor;
import com.groupdocs.editor.EditableDocument;
import com.groupdocs.editor.options.PdfLoadOptions;
import com.groupdocs.editor.options.PdfEditOptions;
import com.groupdocs.editor.options.PdfSaveOptions;
import com.groupdocs.editor.options.PdfCompliance;
import com.groupdocs.editor.options.PageRange;

//0. Simple preparations of input data
String filename = "NET_Framework-protected.pdf";
String password = "password";
String inputPath = "C:\\input\\" + filename;

//1. Create a load options class with password
PdfLoadOptions loadOptions = new PdfLoadOptions();
loadOptions.setPassword(password);

//2. Create edit options and tune/adjust if necessary
PdfEditOptions editOptions = new PdfEditOptions();
editOptions.setEnablePagination(true);//enable pagination for per-page processing in WYSIWYG-editor
editOptions.setPages(PageRange.fromStartPageTillEnd(3));//edit not all pages, but starting from 3rd and till the end

//3. Create Editor instance, load a document
Editor editor = new Editor(inputPath, loadOptions);

//4. Edit a document and generate EditableDocument
EditableDocument originalDoc = editor.edit(editOptions);

//5. Generate HTML/CSS, send it to WYSIWYG, edit there...and obtain edited version
String originalContent = originalDoc.getEmbeddedHtml();
String editedContent = originalContent.replace(".NET Framework", "I love Java!!!");

//6. Generate EditableDocument from edited content
EditableDocument editedDoc = EditableDocument.fromMarkup(editedContent, null);

//7. Create and adjust save options
PdfSaveOptions saveOptions = new PdfSaveOptions();
saveOptions.setCompliance(PdfCompliance.Pdf20);

//8. Save to a file or a stream
String outputPath = "C:\\output\\" + filename;
editor.save(editedDoc, outputPath, saveOptions);

//9. Don't forget to dispose all resources
originalDoc.dispose();
editedDoc.dispose();
editor.dispose();
```

## Loading

[Class `com.groupdocs.editor.options.PdfLoadOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfloadoptions) is responsible for loading the PDF files into the [`com.groupdocs.editor.Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor). It has only one property — a [`password`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfloadoptions/#getPassword--) of a [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) type. By default it is a `null` — no password is specified. This property is vital when an input document is encoded with a password. If a document is not encoded — property value is ignored whether it was specified or not.

Actually, when input PDF is not password-protected, the `PdfLoadOptions` is not necessary at all — the GroupDocs.Editor will automatically detect the PDF format and apply the default `PdfLoadOptions` by itself. However, specifying even default `PdfLoadOptions` will speed-up the document processing, because in this case the GroupDocs.Editor will not spend the processing time for the automatic format detection routine.

```java
import com.groupdocs.editor.Editor;
import com.groupdocs.editor.options.PdfLoadOptions;

//Creating the default PDF loading options
PdfLoadOptions loadOptions = new PdfLoadOptions();

//Setting a password
loadOptions.setPassword("some_password");

String inputPdfPath = "C:\\input\\NET_Framework-protected.pdf";

//Loading a PDF without PDF load options
Editor editor1 = new Editor(inputPdfPath);

//Loading a PDF with PDF load options
Editor editor2 = new Editor(inputPdfPath, loadOptions);
```

## Editing

Like for other format families in GroupDocs.Editor, there is a special [`com.groupdocs.editor.options.PdfEditOptions` class](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfeditoptions) for editing the PDF documents. This class has no its own members — instead it extends a [`FixedLayoutEditOptionsBase` abstract class](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/fixedlayouteditoptionsbase), which is common for PDF and XPS formats (because they are very close in their purpose and idea).

The next properties are inherited from the `FixedLayoutEditOptionsBase`:

1. [`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) flag [`skipImages`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/fixedlayouteditoptionsbase/#getSkipImages--). By default it has a `false` value — images are not skipped and are preserved. However, if you need only textual information from the document, you can set this flag to `true`.

2. [`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) flag [`enablePagination`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/fixedlayouteditoptionsbase/#getEnablePagination--). It has the exact meaning as [the same flag](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions/#getEnablePagination--) in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions). This flag sets the document conversion mode: the **float** (default value is `false`) or **paginal** (`true`). For the PDF and XPS documents it means the same:
> - When the float mode is selected, the document content will be converted to a pageless (float) HTML document, where there is only a single page (like any common web-document).
>
> - When the paginal mode is selected, the pages of the document will be preserved in the generated HTML document, like it can be seen in the PDF viewer like Adobe Reader.
>Actually, the relevance and necessity of this mode relies mostly on your WYSIWYG-editor.

3. [`pages` property](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/fixedlayouteditoptionsbase/#getPages--) of the [`PageRange` type](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pagerange). `PageRange` is an immutable type, that holds a page range of any document, without relation to the specific document. And the `pages` property through a `PageRange` allows setting a page range, which should be processed. By default all the pages of the input document are processed ([`PageRange.isDefault()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pagerange/#isDefault--)` == true`). You can choose the different ways to set a page range using different static methods from a `PageRange` class. Also pay attention — in `PageRange` pages are specified via page numbers, not via indexes, so they are 1-based, but not 0-based.

When the instance of a `PdfEditOptions` class is created and adjusted, you can pass it to the [`Editor.edit(IEditOptions editOptions)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit-com.groupdocs.editor.options.IEditOptions-) method and obtain an instance of [`EditableDocument` class](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument), which is ready for generating HTML/CSS markup and sending it to the client-side.

By the way, if a default `PdfEditOptions` instance is acceptable for you, i.e. you do not want to adjust its settings, you are allowed to omit `PdfEditOptions` creating at all — just call the [`Editor.edit()` parameterless overload](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) and the GroupDocs.Editor will internally generate and apply the default `PdfEditOptions` for the input PDF document.

Code sample below demonstrates all described in details:

```java
import com.groupdocs.editor.Editor;
import com.groupdocs.editor.EditableDocument;
import com.groupdocs.editor.options.PdfEditOptions;
import com.groupdocs.editor.options.PageRange;

//0. Prepare path to your input PDF file
String filename = "Comparison for .NET.pdf";
String inputPdfPath = "C:\\input\\" + filename;

//1. Create Editor instance - we do not use PdfLoadOptions here
Editor editor = new Editor(inputPdfPath);

//2. Edit PDF document with default PdfEditOptions - no need to create and pass PdfEditOptions explicitly
EditableDocument originalDefaultDoc = editor.edit();

//3. Create and adjust PdfEditOptions for advanced edit

//3.1. Create instance of PdfEditOptions class with specified pagination mode (enabled)
PdfEditOptions editOptions = new PdfEditOptions(true);

//3.2. Set to skip images - now they will be omitted during processing
editOptions.setSkipImages(true);

//3.3. Set a page range - only second and third pages (input document initially has 4 pages)
editOptions.setPages(PageRange.fromStartPageWithCount(2, 2));//don't forget that page numbers are 1-based

//4. Edit the same PDF document with tuned edit options
EditableDocument tunedDoc = editor.edit(editOptions);

// Don't forget to dispose resources when work is done
originalDefaultDoc.dispose();
tunedDoc.dispose();
editor.dispose();
```

## Saving

Like for other document formats, there is a special class that is responsible for saving the PDF documents — a [`com.groupdocs.editor.options.PdfSaveOptions` class](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions), which implements [`ISaveOptions` interface](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/isaveoptions).

This class has the next properties:

1. [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) [property `password`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions/#getPassword--) — allows to protect the output PDF document with a specified password. By default is `null` — password-protection is not applied. When specified and is not a `null` or empty string, then the output PDF will be encrypted with RC4 (key length of 128 bit).

2. [`compliance` property](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions/#getCompliance--) allows setting the PDF standards compliance level for output PDF. It has an enum type [`PdfCompliance`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfcompliance), each value represents one specific PDF compliance level. By default is a PDF 1.7 (ISO 32000-1) standard.

3. [`optimizeMemoryUsage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions/#getOptimizeMemoryUsage--) [`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) flag, that allows to modify the process of generation of an output PDF document from an `EditableDocument` instance in such a way that this process will take a lesser memory consumption at the cost of the longer processing time. By default it has a `false` value, which means that the memory optimization is disabled for the sake of better performance. In case of extremely huge documents, enabling this property is vital in order to cope with [`OutOfMemoryError`](https://docs.oracle.com/javase/8/docs/api/java/lang/OutOfMemoryError.html), especially on 32-bit processes.

4. [`fontEmbedding`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions/#getFontEmbedding--) property of [`FontEmbeddingOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/fontembeddingoptions) type is responsible for embedding the font resources into the resultant PDF document. `FontEmbeddingOptions` is an enum that has several values, which allow to control which fonts should be embedded into PDF. By default fonts are not embedded at all (`NotEmbed`). When specifying the `EmbedAll` enum value, all used fonts in the document will be embedded inside the resultant PDF. When specifying the `EmbedWithoutSystem` — only those fonts, which are absent in the current operating system (where the GroupDocs.Editor is running).

Unlike the [`PdfLoadOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfloadoptions) and [`PdfEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfeditoptions), which are optional when they are default (may be omitted during loading and editing respectively), the [`PdfSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions) is mandatory even if all its values are default.

Code sample below shows all necessary preparations (load and edit operations) and then saving in details.

```java
import com.groupdocs.editor.Editor;
import com.groupdocs.editor.EditableDocument;
import com.groupdocs.editor.options.PdfSaveOptions;
import com.groupdocs.editor.options.PdfCompliance;
import com.groupdocs.editor.options.FontEmbeddingOptions;

import java.io.ByteArrayOutputStream;

//0. Prepare path to your input PDF file
String filename = "Comparison for .NET.pdf";
String inputPdfPath = "C:\\input\\" + filename;

//1. Create Editor instance - we do not use PdfLoadOptions here
Editor editor = new Editor(inputPdfPath);

//2. Edit PDF document with default PdfEditOptions - no need to create and pass PdfEditOptions explicitly
EditableDocument originalDoc = editor.edit();

//3. Generate HTML/CSS/resources, send to WYSIWYG-editor, edit there, send back to server and create EditableDocument... omitted here
EditableDocument editedDoc = originalDoc;//just use the same in the sake of simplicity

//4. Prepare PDF save options:
//4.1. Create instance
PdfSaveOptions saveOptionsAdjusted = new PdfSaveOptions();
//4.2. Adjust properties
saveOptionsAdjusted.setPassword("some_password");//protect output PDF with password
saveOptionsAdjusted.setFontEmbedding(FontEmbeddingOptions.EmbedAll);//set font embedding - all used
saveOptionsAdjusted.setCompliance(PdfCompliance.Pdf20);//set a PDF 2.0 (ISO 32000-2) standard compliance
saveOptionsAdjusted.setOptimizeMemoryUsage(true);//set memory optimization on

//5. Create an output stream and save to it
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
try {
    editor.save(editedDoc, outputStream, saveOptionsAdjusted);
} finally {
    outputStream.close();
}

//6. Create another, default at this time, PDF save options
PdfSaveOptions saveOptionsDefault = new PdfSaveOptions();

//7. Save to the file path at this time
String outputPdfPath = "C:\\output\\" + filename;
editor.save(editedDoc, outputPdfPath, saveOptionsDefault);

//8. Don't forget to dispose all
originalDoc.dispose();
editedDoc.dispose();
editor.dispose();
```

In this example we’ve created two output PDF files with different PDF saving options from one source EditableDocument, and saved them to the two distinct storages (memory and disk).

## Obtaining PDF document info

Article [Extracting document metainfo]({{< ref "editor/java/developer-guide/extracting-document-metainfo.md" >}}) describes the [`getDocumentInfo()` method](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-), that allows to detect the document format and extract its metadata without editing it. Actually, after adding PDF support to the GroupDocs.Editor, this mechanism also works with PDF documents.

When the [`getDocumentInfo()` method](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-) is called for the [`Editor` class](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) instance that is loaded with a PDF document, the method will return a [`com.groupdocs.editor.metadata.FixedLayoutDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/fixedlayoutdocumentinfo) instance — it is a common class for all fixed-layout documents, PDF and XPS in particular.

As all [`IDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/idocumentinfo) implementations, it has four properties:

1. [`format` property](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/fixedlayoutdocumentinfo/#getFormat--) of [`Formats.FixedLayoutFormats` type](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/fixedlayoutformats). For PDF documents it always is a [`FixedLayoutFormats.Pdf`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/fixedlayoutformats/#Pdf).
2. [`pageCount` property](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/fixedlayoutdocumentinfo/#getPageCount--) of [integer (`int`)](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html) type. Returns a number of pages.
3. [`size` property](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/fixedlayoutdocumentinfo/#getSize--) of a [long integer (`long`)](https://docs.oracle.com/javase/8/docs/api/java/lang/Long.html) type. Returns a file size in bytes.
4. [`isEncrypted` property](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/fixedlayoutdocumentinfo/#isEncrypted--) of a [`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) type. For the password-encoded documents it returns `true`, and `false` otherwise. For XPS files it always returns `false`, because the XPS format doesn’t support encryption.

As usual, if the input PDF, loaded into the [`Editor` class](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor), is encoded, then its correct password should be specified in the [`getDocumentInfo()` method](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-). If PDF is not encoded, then the value of the [`getDocumentInfo()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-) is ignored.

The code example below demonstrates the extracting metadata from two PDFs: first one is unprotected, while second - protected.

```java
import com.groupdocs.editor.Editor;
import com.groupdocs.editor.formats.FixedLayoutFormats;
import com.groupdocs.editor.metadata.IDocumentInfo;
import com.groupdocs.editor.metadata.FixedLayoutDocumentInfo;

//0. Prepare path to your input unprotected and protected PDF files
String filenameUnprotected = "Comparison for .NET.pdf";
String filenameProtected = "NET_Framework-protected.pdf";
String inputPdfUnprotectedPath = "C:\\input\\" + filenameUnprotected;
String inputPdfProtectedPath = "C:\\input\\" + filenameProtected;

//1. Create two Editor instances - we do not use PdfLoadOptions here
Editor editorUnprotected = new Editor(inputPdfUnprotectedPath);
Editor editorProtected = new Editor(inputPdfProtectedPath);

//2. Extract metadata: do not specify a password (null value instead) for the 1st, and DO specify for the 2nd
IDocumentInfo unprotectedPdfInfo = editorUnprotected.getDocumentInfo(null);
IDocumentInfo protectedPdfInfo = editorProtected.getDocumentInfo("password");

//3. Cast it if necessary and check properties
FixedLayoutDocumentInfo castedUnprotected = (FixedLayoutDocumentInfo) unprotectedPdfInfo;
System.out.println(castedUnprotected.getPageCount()); // e.g. 4
System.out.println(castedUnprotected.isEncrypted()); // false
System.out.println(castedUnprotected.getSize()); // e.g. file size in bytes
System.out.println(castedUnprotected.getFormat()); // FixedLayoutFormats.Pdf

System.out.println(protectedPdfInfo.getPageCount()); // e.g. 14
System.out.println(protectedPdfInfo.isEncrypted()); // true
System.out.println(protectedPdfInfo.getSize()); // e.g. file size in bytes
System.out.println(protectedPdfInfo.getFormat()); // FixedLayoutFormats.Pdf

editorUnprotected.dispose();
editorProtected.dispose();
```

## Extended info

### Pagination when saving

The backward converter, which generates the output PDF from [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument), automatically detects the content in the obtained [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) — if it was generated with the [`enablePagination`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/fixedlayouteditoptionsbase/#getEnablePagination--) flag set to `true` in [`PdfEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfeditoptions), then pagination will be applied to the output PDF, and same for the `false` value. There is no separate pagination flag in [`PdfSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions) — the save stage follows the mode that was used during editing.

### Different output formats

Keep in mind that, when input PDF was edited and you’re going to save it, it is not necessary to save it exactly in the PDF format — you are free to choose any compatible format, like all WordProcessing formats, or text format, or eBook format.

Code example below shows editing a PDF file and then saving the edited content to three different files in different formats.

```java
import com.groupdocs.editor.Editor;
import com.groupdocs.editor.EditableDocument;
import com.groupdocs.editor.formats.WordProcessingFormats;
import com.groupdocs.editor.options.PdfSaveOptions;
import com.groupdocs.editor.options.PdfCompliance;
import com.groupdocs.editor.options.WordProcessingSaveOptions;
import com.groupdocs.editor.options.TextSaveOptions;

import java.io.ByteArrayOutputStream;

//0. Prepare path to your input PDF file
String filename = "Comparison for .NET.pdf";
String inputPdfPath = "C:\\input\\" + filename;

//1. Create Editor instance - we do not use PdfLoadOptions here
Editor editor = new Editor(inputPdfPath);

//2. Edit PDF document with default PdfEditOptions - no need to create and pass PdfEditOptions explicitly
EditableDocument originalDoc = editor.edit();

//3. Generate HTML/CSS/resources, send to WYSIWYG-editor, edit there, send back to server and create EditableDocument... omitted here
EditableDocument editedDoc = originalDoc;//just use the same in the sake of simplicity

//4. Prepare PDF save options
PdfSaveOptions pdfSaveOptions = new PdfSaveOptions();
pdfSaveOptions.setCompliance(PdfCompliance.PdfA2a);

//5. Prepare DOCX save options
WordProcessingSaveOptions docxSaveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Docx);

//6. Prepare TXT save options
TextSaveOptions txtSaveOptions = new TextSaveOptions();
txtSaveOptions.setPreserveTableLayout(true);

//7. Save to PDF format to the stream
ByteArrayOutputStream pdfStream = new ByteArrayOutputStream();
try {
    editor.save(editedDoc, pdfStream, pdfSaveOptions);
} finally {
    pdfStream.close();
}

//8. Save to DOCX format to the stream
ByteArrayOutputStream docxStream = new ByteArrayOutputStream();
try {
    editor.save(editedDoc, docxStream, docxSaveOptions);
} finally {
    docxStream.close();
}

//9. Save to TXT format to the stream
ByteArrayOutputStream txtStream = new ByteArrayOutputStream();
try {
    editor.save(editedDoc, txtStream, txtSaveOptions);
} finally {
    txtStream.close();
}

//10. Dispose all resources
editedDoc.dispose();
originalDoc.dispose();
editor.dispose();
```

## PDF editor demo App

Along with full featured Java library we provide simple, but powerful free Apps. You are welcome to edit PDF documents directly in your browser with our [PDF Editor App](https://products.groupdocs.app/editor/pdf).
