---

id: enabling-language-information
url: editor/nodejs-java/enabling-language-information
title: Enabling language information
weight: 3
description: "Follow this guide to learn how to edit Word documents using locale information and apply spell-checkers to document content written in different languages using GroupDocs.Editor for Node.js and Java."
keywords: Edit Word document with language, Edit Word document with locale, edit Word
productName: GroupDocs.Editor for Node.js and Java
hideChildren: False
---

Documents in WordProcessing formats like DOC(X/M), ODT, and others can contain text in multiple languages. Unlike plain text documents (TXT), WordProcessing documents also carry metadata about the language (locale) of every piece of text. **[GroupDocs.Editor](https://products.groupdocs.com/editor/nodejs-java)** allows extracting and exporting this language information. 

To achieve this, the `WordProcessingEditOptions` class contains the `enableLanguageInformation` boolean property:

```javascript
getEnableLanguageInformation(): boolean;
setEnableLanguageInformation(enable: boolean): void;
```

By default, this property is set to `false`, meaning that language metadata will not be extracted. However, when this option is manually enabled, **GroupDocs.Editor** extracts locale information for every piece of text and preserves it in the [EditableDocument](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/editabledocument) instance. When the user generates HTML markup from the [EditableDocument](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/editabledocument) to edit it in a WYSIWYG HTML-editor in the browser, this language information is added as the `lang` HTML attributes with appropriate values inside `SPAN` elements.

### Benefits of Enabling Language Information

Enabling language information is useful in documents that contain text in multiple languages. If a document contains text in a single language, enabling this option has limited benefits and is disabled by default.

However, for multi-language documents, enabling this feature can be highly beneficial in the following scenarios:

1. **Spell-checking in browsers**: Enabling language information improves spell-checking for client-side JavaScript spell-checkers in browsers. This depends on the specific spell-checker, as not all can utilize the `lang` attributes or language metadata.
2. **Improved round-trip document editing**: When a document with enabled `enableLanguageInformation` is converted to an [EditableDocument](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/editabledocument), and HTML markup is generated and edited in an HTML editor, the language information is preserved. If the edited HTML is then converted back to a WordProcessing format (e.g., DOCX or RTF), the language metadata is still maintained, ensuring the text retains its correct locale information.

### Example

Here is how to enable language information in **Node.js**:

```javascript
const WordProcessingEditOptions = groupdocs.options.WordProcessingEditOptions;

let editOptions = new WordProcessingEditOptions();
editOptions.enableLanguageInformation = true;  // Enable language information
```

In **Java**, the same can be done as follows:

```java
WordProcessingEditOptions editOptions = new WordProcessingEditOptions();
editOptions.setEnableLanguageInformation(true);  // Enable language information
```

With this setting enabled, when the document is converted to HTML, language information for each text portion will be retained, easing spell-checking and improving document conversion quality.
