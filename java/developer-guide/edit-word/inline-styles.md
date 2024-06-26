---
id: inline-styles
url: editor/java/inline-styles
title: Enabling inline CSS styles
weight: 12
description: "This article describes the procedure of enabling the inline styles option for the WordProcessing documents in order to store the CSS styles not in the external stylesheet, but directly inside the HTML markup."
keywords: Inline styles, inline CSS, style attribute, CSS styles in HTML markup
productName: GroupDocs.Editor for Java
hideChildren: False
---
In the GroupDocs.Editor, the editing operation implies that the source document is converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instance, and this instance can emit HTML- and CSS-markup to be placed in the client-side WYSIWYG-editor, then end-user edits this document in the web-browser, and finally document is saved — this implies converting document content from HTML/CSS back to the original (or some other) format.

Prior to the [version 24.4](https://releases.groupdocs.com/editor/java/release-notes/2024/groupdocs-editor-for-java-24-4-release-notes/) of the GroupDocs.Editor the HTML-version of a document, which is obtained from [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) class, the CSS styles were placed in the external stylesheet file “style.css”. HTML file in this case contains a reference to the external stylesheet. And there was no possibility to change the location of CSS styles.

[Version 24.4](https://releases.groupdocs.com/editor/java/release-notes/2024/groupdocs-editor-for-java-24-4-release-notes/) of GroupDocs.Editor introduces a new option — an ability to enable the “inline styles” mode for all of the documents, which belong to the [family of WordProcessing formats](https://docs.fileformat.com/word-processing/). In this mode only [WordProcessing styles are exported to the external stylesheet](https://docs.groupdocs.com/editor/java/styles-export/), while all other styles — text and paragraph formatting, list and table settings, — are exported directly to the HTML markup. They became _inline CSS styles_, which means that in this case the styles are saved in the [“style” HTML attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/style) for every HTML element. As a result, an HTML document, generated with enabled inline styles option, has larger HTML-markup and lesser CSS-markup, compared to the one, where this option is disabled.

The inline styles option is represented as a public property of the [`Boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) type in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions/) class. By default it is disabled (`false`), so the GroupDocs.Editor will act in the same behavior, as its previous versions.

```java
public final boolean getUseInlineStyles()
public final void setUseInlineStyles(boolean value)
```
The example below shows opening and editing a single WordProcessing document twice: first one in default style, and then with enabled inline styles.

```java
// Obtain a valid full path to the existing WordProcessing file
String inputPath = "SampleDoc.docx";

// Create default edit options, where all styles are external
WordProcessingEditOptions editOptionsExternalStyles = new WordProcessingEditOptions();

//Create custom edit options, where most of styles are inline
WordProcessingEditOptions editOptionsInlineStyles = new WordProcessingEditOptions();
// Enable inline styles in this instance
editOptionsInlineStyles.setUseInlineStyles(true);

// Create instance of the Editor class and load a document
Editor editor = new Editor(inputPath);
{
	// Get editable document instance, where all styles are external 
	EditableDocument docWithExternalStyles = editor.edit(editOptionsExternalStyles);

	// Get editable document instance, where most of styles are inline
	EditableDocument docWithInlineStyles = editor.edit(editOptionsInlineStyles);

	// Get only HTML-markup, where all styles are external
	String htmlMarkupExternal = docWithExternalStyles.getContent();

	// Get only HTML-markup, where most of styles are inline
	String htmlMarkupInline = docWithInlineStyles.getContent();

}

```

Need to emphasize that no matter where the CSS styles are located — inside the HTML-markup or in the external file, — the final representation of the HTML-document in the web-browser will be identical.

Also please take into account that this feature exists only for the WordProcessing formats family.