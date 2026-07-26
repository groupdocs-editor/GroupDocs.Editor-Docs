---
id: edit-pdf
url: editor/nodejs-java/edit-pdf
title: Edit PDF
linktitle: Edit PDF document
weight: 58
description: "This guide demonstrates how to edit content of PDF files like common text documents using GroupDocs.Editor for Node.js via Java."
keywords: Edit PDF, Edit PDF file, Edit PDF document, Edit PDF content, Edit PDF text, Edit text in PDF, node.js
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit PDF document in the GroupDocs.Editor
        description: How to edit PDF document using the GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to edit content of the PDF document in the GroupDocs.Editor in Node.js
        description: Learn how to edit content of the PDF document in different editing modes and with different editing options using GroupDocs.Editor in Node.js step by step
        steps:
        - name: Load desired PDF document to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired PDF document into it.
        - name: Prepare a PdfEditOptions class
          text: Create an instance of the PdfEditOptions class and adjust its properties to meet your needs if necessary. For example, enable or disable images, select a page range, set a pagination mode.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the editor.edit method with a previously prepared PdfEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser), and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class.
        - name: Prepare a PdfSaveOptions class
          text: Create an instance of the PdfSaveOptions class and adjust its properties to meet your needs if necessary. All properties are optional, so you may use a default PdfSaveOptions instance. But with options you can protect the resultant document, set a compliance level or fonts.
        - name: Save edited document with editor.save method
          text: Pass an instance of EditableDocument with content of the edited document, instance of the PdfSaveOptions, and a destination byte stream or file path to the editor.save method for saving the document.
---
> This example demonstrates the standard open-edit-save pipeline with PDF documents, using different options on every step.

## Introduction

The [PDF documents, or documents in a Portable Document Format](https://docs.fileformat.com/pdf/), developed by Adobe Corp, are widely used over all Internet and document management systems. PDF format has a crucial distinction from other formats such as [DOCX](https://docs.fileformat.com/word-processing/docx/), [TXT](https://docs.fileformat.com/word-processing/txt/), or [HTML](https://docs.fileformat.com/web/html/)/[CSS](https://docs.fileformat.com/web/css/) — it is a so-called fixed-layout format. The main purpose of PDF is to be platform-independent and store the exact representation of a document — wherever and whenever this document is opened, it should provide per-character and even per-pixel fidelity. This means that a document, once created, is "baked" in terms of its representation and editability. While you can freely edit any DOCX document by adding, removing or moving any part of its content, PDF documents stay "frozen". Internally, a PDF document consists of pages, where every page contains a set of glyphs (visual characters), each having coordinates of where it is located on the page.

Concluding:
- Editing the PDF documents like ordinary DOCX, TXT, or HTML is an extremely difficult and complex task.
- Quality of editing the PDF document may be very close to what we can do with usual text documents, but it will never be 100%, especially when input PDF has quite complex formatting and content.
- Due to the complexity of PDF format and a process of making it editable, this operation requires a lot of processing time and memory.

From its emergence GroupDocs.Editor for Node.js via Java had no support for editing the PDF documents. But starting from the [version 26.7](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2026/groupdocs-editor-for-node.js-via-java-26-7-release-notes/) we finally released this possibility! And this article explains in detail how to edit PDF documents like ordinary text or WordProcessing documents.

## In two words

Editing of the PDF documents is the same as editing any other documents:

1. Load a PDF document to the [`Editor`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor) class with [`PdfLoadOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PdfLoadOptions), specify a password if needed.
2. Edit a document using [`editor.edit()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#edit-com.groupdocs.editor.options.IEditOptions-) with [`PdfEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PdfEditOptions) and obtain an instance of [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument).
3. Send a document content to the client-side, edit it there with WYSIWYG-editor, send modified (edited) content back to the server-side.
4. Create an instance of [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument) with modified content and call [`editor.save()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#save-com.groupdocs.editor.EditableDocument-java.lang.String-com.groupdocs.editor.options.ISaveOptions-) using [`PdfSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PdfSaveOptions).

Short code example below:

```javascript
const {
    Editor,
    EditableDocument,
    PdfLoadOptions,
    PdfEditOptions,
    PdfSaveOptions,
    PdfCompliance,
    PageRange
} = require('@groupdocs/groupdocs.editor');

// 0. Simple preparations of input data
const filename = 'NET_Framework-protected.pdf';
const password = 'password';
const inputPath = 'C:\\input\\' + filename;

// 1. Create a load options class with password
const loadOptions = new PdfLoadOptions();
loadOptions.setPassword(password);

// 2. Create edit options and tune/adjust if necessary
const editOptions = new PdfEditOptions();
editOptions.setEnablePagination(true); // enable pagination for per-page processing in WYSIWYG-editor
editOptions.setPages(PageRange.fromStartPageTillEnd(3)); // from 3rd page till the end

// 3. Create Editor instance, load a document
const editor = new Editor(inputPath, loadOptions);

// 4. Edit a document and generate EditableDocument
const originalDoc = editor.edit(editOptions);

// 5. Generate HTML/CSS, send it to WYSIWYG, edit there...and obtain edited version
const originalContent = originalDoc.getEmbeddedHtml();
const editedContent = originalContent.replace('.NET Framework', 'I love Node.js!!!');

// 6. Generate EditableDocument from edited content
const editedDoc = EditableDocument.fromMarkup(editedContent, null);

// 7. Create and adjust save options
const saveOptions = new PdfSaveOptions();
saveOptions.setCompliance(PdfCompliance.Pdf20);

// 8. Save to a file or a stream
const outputPath = 'C:\\output\\' + filename;
editor.save(editedDoc, outputPath, saveOptions);

// 9. Don't forget to dispose all resources
originalDoc.dispose();
editedDoc.dispose();
editor.dispose();
```

## Loading

[`PdfLoadOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PdfLoadOptions) is responsible for loading the PDF files into the [`Editor`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor). It has only one property — a password (`getPassword` / `setPassword`). By default it is `null` — no password is specified. This property is vital when an input document is encoded with a password. If a document is not encoded — property value is ignored whether it was specified or not.

Actually, when input PDF is not password-protected, the `PdfLoadOptions` is not necessary at all — GroupDocs.Editor will automatically detect the PDF format and apply the default `PdfLoadOptions` by itself. However, specifying even default `PdfLoadOptions` will speed-up the document processing, because in this case GroupDocs.Editor will not spend the processing time for the automatic format detection routine.

```javascript
const { Editor, PdfLoadOptions } = require('@groupdocs/groupdocs.editor');

const loadOptions = new PdfLoadOptions();
loadOptions.setPassword('some_password');

const inputPdfPath = 'C:\\input\\NET_Framework-protected.pdf';

// Loading a PDF without PDF load options
const editor1 = new Editor(inputPdfPath);

// Loading a PDF with PDF load options
const editor2 = new Editor(inputPdfPath, loadOptions);
```

## Editing

Like for other format families in GroupDocs.Editor, there is a special [`PdfEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PdfEditOptions) class for editing the PDF documents. This class has no its own members — instead it extends [`FixedLayoutEditOptionsBase`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/FixedLayoutEditOptionsBase), which is common for PDF and XPS formats.

The next options are inherited from `FixedLayoutEditOptionsBase`:

1. `skipImages` (`getSkipImages` / `setSkipImages`) — by default `false` (images are preserved). Set to `true` if you need only textual information.
2. `enablePagination` (`getEnablePagination` / `setEnablePagination`) — **float** mode (`false`, default) produces a pageless HTML document; **paginal** mode (`true`) preserves pages like in a PDF viewer.
3. `pages` (`getPages` / `setPages`) of [`PageRange`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PageRange) type — page range to process. By default all pages are processed. Page numbers are **1-based**.

When the instance of a `PdfEditOptions` class is created and adjusted, you can pass it to [`editor.edit(editOptions)`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#edit-com.groupdocs.editor.options.IEditOptions-) and obtain an [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument) ready for generating HTML/CSS markup.

If a default `PdfEditOptions` instance is acceptable, you may omit it and call the parameterless [`editor.edit()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#edit--) — GroupDocs.Editor will apply default `PdfEditOptions` internally.

```javascript
const { Editor, PdfEditOptions, PageRange } = require('@groupdocs/groupdocs.editor');

const inputPdfPath = 'C:\\input\\Comparison for .NET.pdf';
const editor = new Editor(inputPdfPath);

// Edit with default PdfEditOptions
const originalDefaultDoc = editor.edit();

// Advanced edit options
const editOptions = new PdfEditOptions(true); // pagination enabled
editOptions.setSkipImages(true);
editOptions.setPages(PageRange.fromStartPageWithCount(2, 2)); // pages 2 and 3

const tunedDoc = editor.edit(editOptions);

originalDefaultDoc.dispose();
tunedDoc.dispose();
editor.dispose();
```

## Saving

[`PdfSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PdfSaveOptions) implements `ISaveOptions` and is responsible for saving PDF documents. Main options:

1. `password` — protect the output PDF (RC4, 128-bit). Default is `null` (no protection).
2. `compliance` — PDF standards compliance ([`PdfCompliance`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PdfCompliance)); default is PDF 1.7.
3. `optimizeMemoryUsage` — lower memory usage at the cost of longer processing time. Default is `false`.
4. `fontEmbedding` — ([`FontEmbeddingOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/FontEmbeddingOptions)) control which fonts are embedded (`NotEmbed` by default, `EmbedAll`, `EmbedWithoutSystem`).

Unlike `PdfLoadOptions` and `PdfEditOptions`, which may be omitted when default, `PdfSaveOptions` is mandatory even if all its values are default.

```javascript
const {
    Editor,
    PdfSaveOptions,
    PdfCompliance,
    FontEmbeddingOptions,
    StreamBuffer
} = require('@groupdocs/groupdocs.editor');

const inputPdfPath = 'C:\\input\\Comparison for .NET.pdf';
const editor = new Editor(inputPdfPath);
const originalDoc = editor.edit();
const editedDoc = originalDoc; // for simplicity

const saveOptionsAdjusted = new PdfSaveOptions();
saveOptionsAdjusted.setPassword('some_password');
saveOptionsAdjusted.setFontEmbedding(FontEmbeddingOptions.EmbedAll);
saveOptionsAdjusted.setCompliance(PdfCompliance.Pdf20);
saveOptionsAdjusted.setOptimizeMemoryUsage(true);

const outputStream = new StreamBuffer();
editor.save(editedDoc, outputStream, saveOptionsAdjusted);

const saveOptionsDefault = new PdfSaveOptions();
const outputPdfPath = 'C:\\output\\Comparison for .NET.pdf';
editor.save(editedDoc, outputPdfPath, saveOptionsDefault);

originalDoc.dispose();
editedDoc.dispose();
editor.dispose();
```

## Obtaining PDF document info

Article [Extracting document metainfo]({{< ref "editor/nodejs-java/developer-guide/extracting-document-metainfo.md" >}}) describes the [`getDocumentInfo()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#getDocumentInfo-java.lang.String-) method. For a PDF loaded into `Editor`, it returns [`FixedLayoutDocumentInfo`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/FixedLayoutDocumentInfo) with `format`, `pageCount`, `size`, and `isEncrypted`.

If the input PDF is encrypted, pass the correct password to `getDocumentInfo()`.

```javascript
const { Editor } = require('@groupdocs/groupdocs.editor');

const editorUnprotected = new Editor('C:\\input\\Comparison for .NET.pdf');
const editorProtected = new Editor('C:\\input\\NET_Framework-protected.pdf');

const unprotectedPdfInfo = editorUnprotected.getDocumentInfo(null);
const protectedPdfInfo = editorProtected.getDocumentInfo('password');

console.log(unprotectedPdfInfo.getPageCount());
console.log(unprotectedPdfInfo.isEncrypted()); // false
console.log(protectedPdfInfo.isEncrypted());   // true

editorUnprotected.dispose();
editorProtected.dispose();
```

## Extended info

### Pagination when saving

The backward converter automatically detects pagination from the obtained `EditableDocument` — if it was generated with `enablePagination` set to `true` in `PdfEditOptions`, pagination is applied to the output PDF (and the same for `false`). There is no separate pagination flag in `PdfSaveOptions`.

### Different output formats

When an input PDF was edited, you are not required to save it only as PDF — you may choose any compatible format (WordProcessing, text, eBook, and so on).

```javascript
const {
    Editor,
    PdfSaveOptions,
    PdfCompliance,
    WordProcessingSaveOptions,
    WordProcessingFormats,
    TextSaveOptions,
    StreamBuffer
} = require('@groupdocs/groupdocs.editor');

const editor = new Editor('C:\\input\\Comparison for .NET.pdf');
const originalDoc = editor.edit();
const editedDoc = originalDoc;

const pdfSaveOptions = new PdfSaveOptions();
pdfSaveOptions.setCompliance(PdfCompliance.PdfA2a);

const docxSaveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Docx);

const txtSaveOptions = new TextSaveOptions();
txtSaveOptions.setPreserveTableLayout(true);

const pdfStream = new StreamBuffer();
editor.save(editedDoc, pdfStream, pdfSaveOptions);

const docxStream = new StreamBuffer();
editor.save(editedDoc, docxStream, docxSaveOptions);

const txtStream = new StreamBuffer();
editor.save(editedDoc, txtStream, txtSaveOptions);

editedDoc.dispose();
originalDoc.dispose();
editor.dispose();
```
