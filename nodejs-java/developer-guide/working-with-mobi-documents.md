---

id: how-to-edit-mobi-file
url: editor/nodejs-java/how-to-edit-mobi-file
title: How to Edit MOBI File
weight: 58
description: "This article demonstrates how to edit MOBI files using Node.js via Java."
keywords: GroupDocs.Editor MOBI support, MOBI format, editing MOBI documents
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
---

## Introduction

The MOBI format is an e-book format developed by the French company MobiPocket and is based on XML. E-books in this format can contain text with rich formatting, images, and different annotations like bookmarks, notes, highlights, corrections, and more. MOBI books can have DRM protection.

Starting from [version 24.6](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2024/groupdocs-editor-for-node.js-via-java-24-6-release-notes/), **GroupDocs.Editor for Node.js via Java** is able to save (export) documents in the MOBI format. Therefore, MOBI is fully supported: on import, export, and auto-detection.

## Loading MOBI File for Edit

Despite being a distinct format that doesn't belong to any existing format families, MOBI has no dedicated loading options. So, to load it into an instance of the [`Editor`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor) class, you should simply specify a path to the MOBI file or a stream with its content in the constructor:

```javascript
// Import necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Loading from path
const inputMobiPath = 'C://input/book.mobi';
const editorFromPath = new groupdocsEditor.Editor(inputMobiPath);

// Loading from stream
const inputMobiStream = fs.createReadStream('C://input/book.mobi');
const editorFromStream = new groupdocsEditor.Editor(inputMobiStream);
```

There are no loading options because MOBI has nothing to tune during loading—it cannot have password protection and can be processed only in one way.

## Editing MOBI File

Because MOBI belongs to the e-book format family, it utilizes common edit options for all e-book formats—the [`EbookEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/EbookEditOptions) class. This class can be described as a truncated version of the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/WordProcessingEditOptions) class because `EbookEditOptions` contains a subset of options from `WordProcessingEditOptions`—[`getEnablePagination()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/EbookEditOptions#getEnablePagination--) and [`getEnableLanguageInformation()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/EbookEditOptions#getEnableLanguageInformation--). As in `WordProcessingEditOptions`, they are disabled (`false`) by default:

```javascript
// Properties of EbookEditOptions
getEnablePagination();
setEnablePagination(value);

getEnableLanguageInformation();
setEnableLanguageInformation(value);
```

They have exactly the same meaning as their counterparts from `WordProcessingEditOptions`:

- `getEnablePagination()` allows switching between flow and paginal mode in the resultant HTML document.
- `getEnableLanguageInformation()` enables exporting language information in HTML, which is useful for books that have parts of text written in different languages.

Example of usage (assuming that an `Editor` instance with a loaded MOBI document is already created):

```javascript
// Create edit options
const editOptions = new groupdocsEditor.EbookEditOptions();
editOptions.setEnablePagination(true);
editOptions.setEnableLanguageInformation(true);

// Edit the MOBI document
const opened = editor.edit(editOptions);

// Now, you can save it or pass it to the WYSIWYG editor
```

## Saving MOBI File After Edit

Before [version 24.6](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2024/groupdocs-editor-for-node.js-via-java-24-6-release-notes/), **GroupDocs.Editor for Node.js via Java** was only able to load and edit MOBI documents but not save (export) them in the MOBI format. In other words, it was able to open and edit the MOBI document and then save the edited document in some other format but not in MOBI. In version 23.9, export in the MOBI format was finally implemented.

For all the format families, the saving procedure is the same—you need to obtain the content of the edited document on the server-side, create an instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument) from it, and then pass it to the `editor.save()` method. Because the MOBI format belongs to the e-book formats family, to save the document in the MOBI format, an instance of the [`EbookSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/EbookSaveOptions) class is required.

This class has a [mandatory constructor with a single parameter](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/EbookSaveOptions#EbookSaveOptions-com.groupdocs.editor.formats.EBookFormats-)—a desired [e-book format](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.formats/EBookFormats). For MOBI, it should be `EBookFormats.Mobi`. All other parameters in the `EbookSaveOptions` class are optional but are described below:

- [`getSplitHeadingLevel()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/EbookSaveOptions#getSplitHeadingLevel--) of type `Number` controls the internal structure of the generated MOBI file—whether its internal content is divided into packages and, if yes, how. This parameter exists because the content of the MOBI file is stored in packages, usually one package per chapter. Some users may want to control the policy of distributing content into packages. By default, this parameter is `2`, which is optimal.
- [`getExportDocumentProperties()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/EbookSaveOptions#getExportDocumentProperties--) of type `Boolean` decides whether to embed the built-in and custom document properties inside the resultant MOBI file (`true`) or not (`false`). By default, it is `false`—do not embed.

In summary, to save the edited document in the MOBI format, you should create an instance of the `EbookSaveOptions` class with `EBookFormats.Mobi` as the constructor parameter and then pass this instance to the `editor.save()` method along with other arguments.

The code example below demonstrates the full round-trip: opening a MOBI document, editing it, and saving the edited version to the MOBI format.

```javascript
// Import necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Create an Editor instance and load an input MOBI file
const editor = new groupdocsEditor.Editor('Input.mobi');

// Create edit options
const editOptions = new groupdocsEditor.EbookEditOptions();

// Edit the MOBI document and obtain EditableDocument
const doc = editor.edit(editOptions);

// Send EditableDocument to the client-side, edit it there, and obtain an edited version

// For demonstration, let's assume the edited HTML content is obtained
const editedHtmlContent = doc.getContent(); // This should be the edited content from the client-side

// Create an EditableDocument from the edited HTML content
const editedDocument = groupdocsEditor.EditableDocument.fromMarkup(editedHtmlContent, null);

// Create MOBI save options
const saveOptions = new groupdocsEditor.EbookSaveOptions(groupdocsEditor.EBookFormats.Mobi);
saveOptions.setExportDocumentProperties(true); // Tune save options

// Save the edited document in MOBI format
editor.save(editedDocument, 'Output.mobi', saveOptions);

// Dispose of document and Editor instances
doc.dispose();
editedDocument.dispose();
editor.dispose();
```

## Detecting MOBI File

As with documents of all supported types, MOBI documents can be detected using the [`getDocumentInfo()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#getDocumentInfo--) method of the [`Editor`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor) class. When a valid MOBI document is loaded into the `Editor` instance, `getDocumentInfo()` will return an instance of the [`EbookDocumentInfo`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/EbookDocumentInfo) class, which inherits from the [`IDocumentInfo`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/IDocumentInfo) interface. This interface defines four properties: `getFormat()`, `getPageCount()`, `getSize()`, and `isEncrypted()`.

- **[`getFormat()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/EbookDocumentInfo#getFormat--):** Returns an `EBookFormats` struct, which for files with a `.mobi` extension can have a `Mobi` or `Azw3` value.
- **[`getPageCount()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/EbookDocumentInfo#getPageCount--):** Returns an **approximate** number of pages in the case of MOBI or AZW3, or a number of chapters in the case of EPUB. For MOBI, it is approximate because the MOBI format internally is a set of HTML documents (chapters), which are not separated into pages and even have no strict page dimensions. This design allows variable page size (and count) on different devices—from Full HD displays to smartphones. Therefore, to return a page count for a MOBI document, GroupDocs.Editor assumes a standard A4 page size in portrait orientation, splits existing document content into such "pages," and then calculates the count. The returned number should be treated carefully and approximately; users should not rely on it.
- **[`getSize()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/EbookDocumentInfo#getSize--):** Returns the number of bytes of a MOBI document.
- **[`isEncrypted()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/EbookDocumentInfo#isEncrypted--):** Always returns `false` because MOBI documents cannot be encrypted with a password like PDF or Office Open XML.

Example of detecting a MOBI file:

```javascript
// Import necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Load the MOBI file into the Editor
const editor = new groupdocsEditor.Editor('Input.mobi');

// Get document info
const documentInfo = editor.getDocumentInfo();

console.log('Format:', documentInfo.getFormat().getName());
console.log('Page Count:', documentInfo.getPageCount());
console.log('Size (bytes):', documentInfo.getSize());
console.log('Is Encrypted:', documentInfo.isEncrypted());

// Dispose of the editor instance
editor.dispose();
```

## Conclusion

In this article, we've demonstrated how to edit MOBI files using **GroupDocs.Editor for Node.js via Java**. We've covered loading the MOBI file, editing it using `EbookEditOptions`, saving the edited document back to MOBI format, and detecting MOBI files using `getDocumentInfo()`. With the support for MOBI format starting from version 23.9, you can fully integrate MOBI file editing into your Node.js applications.

---

**Note:** Replace `'Input.mobi'` and `'Output.mobi'` with the actual paths to your MOBI files.

---

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java) repository on GitHub.
