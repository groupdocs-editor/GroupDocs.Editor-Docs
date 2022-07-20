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
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired  PDF document into it.
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
> This example demonstrates the standard open-edit-save cycle with PDF documents, using different options on every step.

## Introduction

The [PDF documents, or documents in a Portable Document Format](https://docs.fileformat.com/pdf/), developed by Adobe Corp, are widely used over all Internet and document management systems. PDF format has a crucial distinction from other formats such as [DOCX](https://docs.fileformat.com/word-processing/docx/), [TXT](https://docs.fileformat.com/word-processing/txt/), or [HTML](https://docs.fileformat.com/web/html/)/[CSS](https://docs.fileformat.com/web/css/) — it is a so-called fixed-layout format. The main purpose of PDF is to be platform-independent and store the exact representation of a document, — wherever and whenever this document is opened, it should provide the per-character and even a per-pixel fidelity. This means that a document, once created, is "baked" in terms of its representation and editability. While you can freely edit any DOCX document by adding, removing or moving any part of its content, and page layout is organically updated to fit newly added paragraphs or images (or pages are collapsed when text and other content is removed from the beginning or the middle of the document), the PDF documents sustain "frozen". PDF documents have pages, but every word, character, pixel is strictly bound to every page and cannot be moved. In fact, PDF format doesn't contain the definitions of paragraphs, titles, sentences, words and even letters — internally PDF document consists of pages, where every page contains a set of glyphs (visual character), where every glyph has coordinated where it is located on this page. Same for images and other visual elements. Words, text blocks, paragraphs and so on, visually identifiable by the users, who open PDF in some viewer like Adobe Reader, are nothing more than a set of symbols, displaced by coordinates over all the page to be "like a text". For example, tables are nothing more than a set of the drawn lines to form a visual grid with glyphs, drawn in its cells. From this point of view, PDF format is much closer to raster or vector images like JPEG, PNG, or SVG, then to "truly" text documents like DOCX or TXT.

Concluding:
* Editing the PDF documents like ordinary DOCX, TXT, or HTML is an extremely difficult and complex task.
* Quality of editing the PDF document may be very close to what we can do with usual text documents, but it will never be 100%, especially when input PDF has quite complex formatting and content.
* Due to the complexity of PDF format and a process of making it editable, this operation requires a lot of processing time and memory.

From its emergence the GroupDocs.Editor for .NET had no support for editing the PDF documents. But starting from the [version 22.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/) we finally released this possibility! And this article explains in detail how to edit PDF document of any complexity like an ordinary text or WordProcessing document.

__Important note!__

GroupDocs.Editor for .NET [version 22.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/) is distributed for three platforms/runtimes: .NET Framework 2.0, .NET Framework 4.6.1 and .NET Standard 2.0. Ability to edit PDF documents is available only for the .NET Framework 4.6.1 and higher, or .NET Standard 2.0 and higher.

## In two words

Editing of the PDF documents is the same as editing any other documents:

1. Load a PDF document to the [`GroupDocs.Editor.Editor` class](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor) with `PdfLoadOptions`, specify a password if needed.
2. Edit a document using `GroupDocs.Editor.Editor.Edit()` and `PdfEditOptions` and obtain an instance of `EditableDocument`.
3. Send a document content to the client-side, edit it there with WYSIWYG-editor, send modified (edited) content back to the server-side.
4. Create an instance of `EditableDocument` with modified content and call an `GroupDocs.Editor.Editor.Save()` using `PdfSaveOptions`.

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
GroupDocs.Editor.Editor editor = new Editor(inputPath, delegate () { return loadOptions; });

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

[Class `GroupDocs.Editor.Options.PdfLoadOptions`](https://apireference.groupdocs.com/editor/net/groupdocs.editor.options/pdfloadoptions) is responsible for loading the PDF files into the [`GroupDocs.Editor.Editor`](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor). It has only one property — a `[Password](https://apireference-qa.groupdocs.com/editor/net/groupdocs.editor.options/pdfloadoptions/properties/password)` of a `string` type. By default it is a `null` — no password is specified. This property is vital when an input document is encoded with a password. If a document is not encoded — property value is ignored whether it was specified or not.

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
Editor editor2 = new Editor(inputPdfPath, delegate () { return loadOptions; });
```

