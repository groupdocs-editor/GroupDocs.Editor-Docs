---
id: float-and-paginal-modes
url: editor/nodejs-java/float-and-paginal-modes
title: Float and paginal modes
weight: 4
description: "This article explains the pros and cons of float and paginal document editing modes when editing Word documents with GroupDocs.Editor API."
keywords: Edit Word document in float mode, Edit Word document page by page, edit Word
productName: GroupDocs.Editor for Node.js
hideChildren: False
toc: True
---

## How to enable different document edit modes?

The WordProcessing module of [**GroupDocs.Editor**](https://products.groupdocs.com/editor/nodejs-java), which converts WordProcessing formats to an [EditableDocument](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/editabledocument) instance (and vice versa), supports two modes: *float* and *paginal* (also known as *paged*). By default, the *float* mode is enabled. These modes are controlled by a property in the `WordProcessingEditOptions` and `WordProcessingSaveOptions` classes:

```javascript
getEnablePagination(): boolean;
setEnablePagination(enable: boolean): void;
```

### Float Mode vs. Paginal Mode

The primary distinction between these two modes lies in how the WordProcessing document is represented:

- **Float mode** (default): The document is presented in a pageless form. Content is continuous, without page breaks, and entities like headers, footers, watermarks, and page numbers are not present. This mode is most compatible with client-side HTML editors like TinyMCE or CKEditor, which do not support page separation or real-time page recalculations.

- **Paginal mode**: The document is divided into pages, similar to how it appears in desktop applications like Microsoft Word. Page-related elements like headers, footers, watermarks, and page numbers are included. This mode must be manually enabled and is best suited for scenarios where the end-user can process page-divided content, such as in custom HTML editing software.

### When to Use Each Mode?

1. **Float mode** is ideal when using HTML editors that cannot handle page structures, allowing for smooth, continuous editing of document content.
2. **Paginal mode** is useful when page-based editing is required, or when you need to preserve the document's original formatting with headers, footers, and page numbers.

### Example

Here’s how to enable paginal mode in **Node.js** during the conversion of a WordProcessing document:

```javascript
const WordProcessingEditOptions = groupdocs.options.WordProcessingEditOptions;

let editOptions = new WordProcessingEditOptions();
editOptions.enablePagination = true;  // Enable paginal mode
```

Similarly, you can enable paginal mode when saving the edited document:

```javascript
const WordProcessingSaveOptions = groupdocs.options.WordProcessingSaveOptions;

let saveOptions = new WordProcessingSaveOptions();
saveOptions.enablePagination = true;  // Ensure paginal mode is preserved during save
```

### Best Practice

When performing a complete roundtrip (editing and saving a document), it is recommended to use the same mode for both conversion to and from the `EditableDocument` format. If you start with *float* mode during the conversion to `EditableDocument`, continue using *float* mode when saving. Similarly, if *paginal* mode is used, it should be maintained throughout the entire process.
