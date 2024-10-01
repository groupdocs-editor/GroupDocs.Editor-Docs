---

id: save-document
url: editor/nodejs-java/save-document
title: Save Document
weight: 8
description: "This article demonstrates how to save edited text documents, spreadsheets, and presentations with GroupDocs.Editor for Node.js via Java API."
keywords: Save edited document, edit document, GroupDocs.Editor
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
structuredData:
    showOrganization: True
    application:
        name: Save Document in GroupDocs.Editor
        description: Save documents using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to save a document using GroupDocs.Editor in Node.js via Java
        description: Learn how to save a document using GroupDocs.Editor in Node.js via Java step by step
        steps:
        - name: Submit the edited HTML document to the server-side
          text: After the document was edited on the client-side with a WYSIWYG HTML editor, its edited HTML markup and resources must be submitted to the server-side, because GroupDocs.Editor is a server-based middleware.
        - name: Create an EditableDocument instance from the edited content
          text: Once the content of the edited document is transferred to the server-side, you should create an instance of the EditableDocument class from this content by using the most appropriate static method.
        - name: Create and adjust the appropriate SaveOptions
          text: Depending on what format the input document had and which output format you want to obtain, you need to create an appropriate inheritor of the ISaveOptions interface and adjust it by selecting the desired format, protection, and other options.
        - name: Invoke the Editor.save method
          text: Finally, when the edited document is stored within the EditableDocument instance and SaveOptions are prepared, you should invoke the desired overload of the Editor.save method (the first one saves the document to a file, while the second saves to a byte stream).
---

This article describes how to obtain edited document content from the client, process it, and save it to the resultant document of a specified format using **GroupDocs.Editor for Node.js via Java**.

When the end-user has finished editing the document in the WYSIWYG HTML editor (usually a pure client-side application written in JavaScript), they submit the editing operation, and the HTML markup with stylesheets, images, and possibly other resources are passed to the server-side. To generate a document of some output format, these resources should be passed to the [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument).

As shown in previous articles, instances of [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument) are generated and returned by the [`Editor.edit()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#edit--) method and then are used for emitting HTML markup and resources to pass them *to* the WYSIWYG editor.

However, `EditableDocument` also has a second purpose—to obtain edited content *from* the WYSIWYG editor. The `EditableDocument` class has no public constructors; instead, it has two static factory methods that obtain the HTML document in different forms:

1. **[`fromFile()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument#fromFile--):** Designed for opening HTML documents from disk—it accepts the path to the `.html` file and the path to the corresponding resource folder.
2. **[`fromMarkup()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/EditableDocument#fromMarkup--):** Designed for opening HTML documents from memory—it accepts HTML markup as a `String` and a list of [`IHtmlResource`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.htmlcss.resources/IHtmlResource) items.

More information about creating `EditableDocument` instances from files or markup with resources can be found in the corresponding article: [Create EditableDocument from File or Markup](/editor/nodejs-java/developer-guide/editabledocument/create-editabledocument-from-file-or-markup/).

## Saving the Edited Document

When the `EditableDocument` is created, it can be converted to the output document. To do this, you must use the [`Editor.save()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#save--) method, which has two overloads. These overloads differ only in the way the output document is specified: as a path where the file should be created, or as a byte stream into which the document content should be written. All other parameters are the same. They are:

1. **EditableDocument Instance:**
   - An instance of the `EditableDocument` class that holds the content of the edited document.
2. **Output Document:**
   - Specified as a file path (`String`) or byte stream (`OutputStream`).
3. **Save Options:**
   - Mandatory save options, represented by one of the inheritors of the [`ISaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/ISaveOptions) interface.

### Save Options Classes

Like with load and edit options, every format family has its own class that implements the `ISaveOptions` interface. These classes are listed below:

| Format Family                    | Example Formats                 | Save Options Class                                                                                 | Format Class                                                                                 |
|----------------------------------|---------------------------------|----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| WordProcessing                   | DOC, DOCX, DOCM, DOT, ODT       | [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/WordProcessingSaveOptions) | [`WordProcessingFormats`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.formats/WordProcessingFormats) |
| Spreadsheet                      | XLS, XLSX, XLSM, XLSB           | [`SpreadsheetSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/SpreadsheetSaveOptions)       | [`SpreadsheetFormats`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.formats/SpreadsheetFormats) |
| Delimiter-Separated Values (DSV) | CSV, TSV                        | [`DelimitedTextSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/DelimitedTextSaveOptions)   | [`TextualFormats`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.formats/TextualFormats) |
| Presentation                     | PPT, PPTX, PPS, POT             | [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PresentationSaveOptions)     | [`PresentationFormats`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.formats/PresentationFormats) |
| Plain Text Documents             | TXT                             | [`TextSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/TextSaveOptions)                     | [`TextualFormats`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.formats/TextualFormats) |
| PDF                              | PDF                             | [`PdfSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options/PdfSaveOptions)                       | N/A                                                                                          |

## Code Example

The source code below shows how to create an instance of the `EditableDocument` class and subsequently save two versions of the document: one to a file and the second to another file in a different format.

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');

// Load the edited HTML document from a file
const inputHtmlPath = 'C:\\input\\document.html';
const document = groupdocsEditor.EditableDocument.fromFile(inputHtmlPath, null);

// Create an Editor instance (original document path can be provided if needed)
const editor = new groupdocsEditor.Editor('C:\\path\\original.docx');

// Save the first version to a file
const outputPath1 = 'C:\\output_path\\document.rtf';
const saveOptions1 = new groupdocsEditor.WordProcessingSaveOptions(groupdocsEditor.WordProcessingFormats.Rtf);
editor.save(document, outputPath1, saveOptions1);

// Save the second version to another file
const outputPath2 = 'C:\\output_path\\document.docm';
const saveOptions2 = new groupdocsEditor.WordProcessingSaveOptions(groupdocsEditor.WordProcessingFormats.Docm);
editor.save(document, outputPath2, saveOptions2);

// Dispose of resources
document.dispose();
editor.dispose();
```

In the example above:

- **Loading the Edited HTML Document:**
  - We use `EditableDocument.fromFile()` to load the edited HTML document from the specified path.
- **Creating an Editor Instance:**
  - We create an `Editor` instance by specifying the original document path.
- **Saving the First Version:**
  - We specify the output path and create `WordProcessingSaveOptions` with the desired format (`Rtf`).
  - We call `editor.save()` to save the document.
- **Saving the Second Version:**
  - We repeat the process with a different output path and format (`Docm`).

As you can see from the example above, it is possible to create multiple output documents from a single `EditableDocument` with different save options and different formats. These output formats do not need to be the same as the format of the input document.

Furthermore, in some cases, the format family can also be different. For example, the original document can be one of the WordProcessing formats, while the output document can be TXT or PDF. Similar transitions are allowed between Spreadsheets and DSV (two-way). However, transitions are not allowed where formats are theoretically incompatible in their essence, such as between WordProcessing and Spreadsheet formats.

## Conclusion

This article has demonstrated how to save edited documents using **GroupDocs.Editor for Node.js via Java**. By obtaining the edited content from the client, creating an `EditableDocument` instance, and using the appropriate save options, you can generate output documents in various formats. This flexibility allows you to integrate document editing and conversion capabilities into your Node.js applications effectively.

---

**Note:** Be sure to replace `'C:\\input\\document.html'`, `'C:\\path\\original.docx'`, and output paths with the actual file paths in your application.

---

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java) repository on GitHub.


