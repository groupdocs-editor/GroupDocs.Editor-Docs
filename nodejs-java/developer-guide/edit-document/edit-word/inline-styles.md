---
id: inline-styles
url: editor/nodejs-java/inline-styles
title: Enabling Inline CSS Styles
weight: 12
description: "This article describes how to enable the inline styles option for WordProcessing documents to store CSS styles directly inside the HTML markup using GroupDocs.Editor for Node.js via Java."
keywords: Inline styles, inline CSS, style attribute, CSS styles in HTML markup
productName: GroupDocs.Editor for Node.js via Java
hideChildren: False
---

In GroupDocs.Editor for Node.js via Java, the editing operation involves converting the source document into an [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument) instance. This instance can emit HTML and CSS markup to be placed in a client-side WYSIWYG editor. The end-user edits the document in a web browser, and finally, the document is saved—implying a conversion of the document content from HTML/CSS back to the original (or another) format.

Prior to [version 24.4](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2024/groupdocs-editor-for-node-js-via-java-24-4-release-notes/) of GroupDocs.Editor for Node.js via Java, the HTML version of a document obtained from the `EditableDocument` class placed CSS styles in an external stylesheet file named `style.css`. The HTML file contained a reference to this external stylesheet, and there was no option to change the location of CSS styles.

Starting with [version 24.4](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2024/groupdocs-editor-for-node-js-via-java-24-4-release-notes/), GroupDocs.Editor introduces a new option—enabling the "inline styles" mode for all documents belonging to the [family of WordProcessing formats](https://docs.fileformat.com/word-processing/). In this mode, only WordProcessing styles are exported to the external stylesheet, while all other styles—text and paragraph formatting, list and table settings—are exported directly into the HTML markup. They become _inline CSS styles_, meaning the styles are saved in the [`style` HTML attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/style) for each HTML element. As a result, an HTML document generated with the inline styles option enabled has larger HTML markup and a smaller CSS file compared to when this option is disabled.

The inline styles option is represented as a public property of the `Boolean` type in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/WordProcessingEditOptions) class. By default, it is disabled (`false`), so GroupDocs.Editor will behave the same as in previous versions unless you enable it.

```javascript
// Getter and Setter for useInlineStyles
public boolean getUseInlineStyles()
public void setUseInlineStyles(boolean value)
```

The example below shows how to open and edit a WordProcessing document twice: first with the default setting (external styles) and then with the inline styles option enabled.

```javascript
// Import necessary modules
const groupdocs_editor = require('groupdocs-editor');

// Obtain a valid full path to the existing WordProcessing file
const inputPath = "SampleDoc.docx";

// Create default edit options, where all styles are external
const editOptionsExternalStyles = new groupdocs_editor.WordProcessingEditOptions();

// Create custom edit options, where most styles are inline
const editOptionsInlineStyles = new groupdocs_editor.WordProcessingEditOptions();
// Enable inline styles in this instance
editOptionsInlineStyles.setUseInlineStyles(true);

// Create an instance of the Editor class and load the document
const editor = new groupdocs_editor.Editor(inputPath);

// Get EditableDocument instance with external styles
const docWithExternalStyles = editor.edit(editOptionsExternalStyles);

// Get EditableDocument instance with inline styles
const docWithInlineStyles = editor.edit(editOptionsInlineStyles);

// Get only HTML markup with external styles
const htmlMarkupExternal = docWithExternalStyles.getContent();

// Get only HTML markup with inline styles
const htmlMarkupInline = docWithInlineStyles.getContent();

// Optionally, save the HTML content to files
const fs = require('fs');
fs.writeFileSync('ExternalStyles.html', htmlMarkupExternal);
fs.writeFileSync('InlineStyles.html', htmlMarkupInline);
```

**Note:** Replace `"SampleDoc.docx"` with the actual path to your WordProcessing document.

It's important to emphasize that no matter where the CSS styles are located—inside the HTML markup or in an external file—the final representation of the HTML document in the web browser will be identical.

Also, please note that this feature exists only for the WordProcessing formats family.

## Conclusion

The ability to enable inline CSS styles provides greater flexibility in how styles are managed within your HTML documents. This can be particularly useful in scenarios where managing external CSS files is inconvenient or when you want to simplify the deployment of HTML content by embedding styles directly into the HTML markup.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.