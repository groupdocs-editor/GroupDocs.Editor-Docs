---
id: font-embedding-options
url: editor/nodejs-java/font-embedding-options
title: Font embedding options
weight: 6
description: "Learn how to embed fonts into an output Word document when editing with GroupDocs.Editor API for Node.js."
keywords: Edit document with fonts, Font embedding
productName: GroupDocs.Editor for Node.js
hideChildren: False
toc: True
---

## Introduction

**GroupDocs.Editor** supports font embedding for output WordProcessing documents. This is similar to the feature in Microsoft Word that allows embedding fonts into a saved document after editing.

Font embedding differs from font extraction. While font extraction transfers font resources from the input document to the intermediate `EditableDocument`, font embedding works by embedding fonts into the output document from the `EditableDocument` after it has been edited. When saving the edited document, fonts used in the content can be embedded into the final output document, ensuring consistency across different environments.

### How it Works

When font embedding is enabled, **GroupDocs.Editor** analyzes the content of the `EditableDocument` for all the fonts used (e.g., in paragraphs, labels, etc.). It then checks if these fonts are available in the `EditableDocument`'s font resources (`EditableDocument.getFonts()`). If the fonts are available, they will be embedded in the output document. 

If the fonts are not found in the document, **GroupDocs.Editor** attempts to locate them in the operating system where the editor is running, and they will be embedded if found. Unused font resources (e.g., fonts associated with deleted content) are automatically ignored.

### Font Embedding Options

To support font embedding, the `WordProcessingSaveOptions` class has a new property: `getFontEmbedding()`. This property is of type `FontEmbeddingOptions`, an enum with three values:

1. **NotEmbed**: (Default) Fonts are not embedded in the output document, preserving behavior from older versions of **GroupDocs.Editor**.
2. **EmbedAll**: All fonts used in the document are embedded, including system fonts.
3. **EmbedWithoutSystem**: All fonts except system fonts are embedded.

System fonts are essential for operating systems (e.g., for UI elements like Windows Explorer, Console, etc.). Excluding system fonts may be desirable in certain cases, such as when creating documents on an East Asian system for distribution to users without those specific fonts.

### Example in Node.js

Here’s how to use the font embedding options in **Node.js**:

```javascript
const groupdocs = require('groupdocs-editor');
const WordProcessingSaveOptions = groupdocs.options.WordProcessingSaveOptions;
const FontEmbeddingOptions = groupdocs.options.FontEmbeddingOptions;
const WordProcessingFormats = groupdocs.formats.WordProcessingFormats;

// Create save options for a DOCX file
let saveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Docx);

// By default, fonts are not embedded
console.log(saveOptions.fontEmbedding === FontEmbeddingOptions.NotEmbed); // true

// Embed all used fonts, including system fonts
saveOptions.fontEmbedding = FontEmbeddingOptions.EmbedAll;

// Embed all used fonts, excluding system fonts
saveOptions.fontEmbedding = FontEmbeddingOptions.EmbedWithoutSystem;
```

### When to Use Font Embedding

Embedding fonts ensures that the output document maintains its original appearance, even when opened on systems that do not have the required fonts installed. This is particularly important when sharing documents across different platforms or geographic regions where font availability may vary.

By using the appropriate font embedding option, you can control whether to include system fonts, which may be unnecessary for most documents.
