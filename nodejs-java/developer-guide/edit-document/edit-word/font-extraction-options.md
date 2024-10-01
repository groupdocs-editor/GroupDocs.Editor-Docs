---
id: font-extraction-options
url: editor/nodejs-java/font-extraction-options
title: Font extraction options
weight: 5
description: "Learn how to extract fonts from input Word documents when editing with GroupDocs.Editor API for Node.js."
keywords: Edit document with fonts, Font extraction
productName: GroupDocs.Editor for Node.js
hideChildren: False
toc: True
---

## Introduction

WordProcessing documents, such as DOCX or ODT, typically contain text content formatted with various fonts. These fonts can either be system-installed (available on the operating system) or custom fonts embedded directly within the document. **GroupDocs.Editor** provides a way to extract fonts from WordProcessing documents, ensuring that any custom fonts used in the document are available during editing and viewing, even if they aren't installed on the system.

### Font Usage in WordProcessing Documents

When discussing font usage in WordProcessing documents, it can be approached from two perspectives:
- **Broad perspective**: All fonts referenced in the document, including those applied to styles or placeholders.
- **Narrow perspective**: Only fonts that are actively applied to visible text content.

For example, a document may reference a font through a style that is no longer applied to any text. In the narrow perspective, this font would not be considered "in use."

### Font Extraction in GroupDocs.Editor

**GroupDocs.Editor** can extract fonts embedded in WordProcessing documents and present them as resources in the `EditableDocument` instance. These extracted fonts can then be linked to the HTML content generated for editing in a WYSIWYG editor.

Font extraction can handle both embedded fonts and system-installed fonts. If a font is embedded in the document, it will be extracted; if it's not embedded, **GroupDocs.Editor** will attempt to locate the font in the system's font resources.

### Font Extraction Options

To control how fonts are extracted, the `WordProcessingEditOptions` class in **GroupDocs.Editor for Node.js** provides two main properties:

```javascript
const FontExtractionOptions = groupdocs.options.FontExtractionOptions;

let editOptions = new WordProcessingEditOptions();
editOptions.fontExtraction = FontExtractionOptions.ExtractAllEmbedded; // Example value
editOptions.extractOnlyUsedFonts = true;  // Example value
```

1. **fontExtraction**: This property controls how fonts are extracted. It accepts values from the `FontExtractionOptions` enum:
   - `NotExtract`: No fonts will be extracted from the document.
   - `ExtractAllEmbedded`: All embedded fonts will be extracted, regardless of whether they are system-installed.
   - `ExtractEmbeddedWithoutSystem`: Extract only custom embedded fonts that are not system-installed.
   - `ExtractAll`: Extract all fonts used in the document, whether embedded or system-installed.

2. **extractOnlyUsedFonts**: This boolean flag determines whether to extract only the fonts applied to visible text content (narrow perspective) or all fonts referenced in the document (broad perspective). 
   - `true`: Extract only fonts applied to text content.
   - `false`: Extract all referenced fonts, even if they are not applied to visible text.

### Example in Node.js

Here’s how to configure font extraction in **Node.js**:

```javascript
const groupdocs = require('groupdocs-editor');
const WordProcessingEditOptions = groupdocs.options.WordProcessingEditOptions;
const FontExtractionOptions = groupdocs.options.FontExtractionOptions;

let editOptions = new WordProcessingEditOptions();

// Extract all embedded fonts, ignoring system fonts
editOptions.fontExtraction = FontExtractionOptions.ExtractEmbeddedWithoutSystem;

// Extract only fonts actively used in the document
editOptions.extractOnlyUsedFonts = true;
```

### How Font Extraction Options Work Together

These two options, `fontExtraction` and `extractOnlyUsedFonts`, work in conjunction:

- If `fontExtraction` is set to `NotExtract`, the value of `extractOnlyUsedFonts` will be ignored.
- If `fontExtraction` is set to `ExtractAllEmbedded` and `extractOnlyUsedFonts` is `true`, only embedded fonts that are applied to visible text will be extracted.
- If `fontExtraction` is set to `ExtractAll`, and `extractOnlyUsedFonts` is `true`, only the fonts actively used in the document's content will be extracted, either from the embedded resources or system-installed fonts.

### Conclusion

The font extraction options in **GroupDocs.Editor** provide flexibility when editing Word documents. By choosing the appropriate extraction settings, you can ensure that the fonts used in the document are available for editing and viewing, even when the document is shared across different platforms or systems.
