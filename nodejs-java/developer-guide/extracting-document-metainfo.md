---

id: extracting-document-metainfo
url: editor/nodejs-java/extracting-document-metainfo
title: Extracting Document Metainfo
weight: 10
description: "Following this guide, you will learn how to obtain basic document metadata like page count, size, and file type before editing it with GroupDocs.Editor for Node.js via Java API."
keywords: Extract document metadata, Get document info, obtain basic document metadata
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: Extract document metainfo in GroupDocs.Editor
        description: Extract information about document format and other properties using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to extract document metainfo using GroupDocs.Editor in Node.js via Java
        description: Learn how to extract information about document format and other properties using GroupDocs.Editor in Node.js via Java step by step
        steps:
        - name: Load the desired document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload by passing the desired document into it.
        - name: Invoke the Editor.getDocumentInfo method
          text: Once the document is loaded, call the Editor.getDocumentInfo method and specify an optional password for the document if it is password-protected.
        - name: Examine the obtained document info
          text: The Editor.getDocumentInfo method returns an inheritor of the IDocumentInfo interface, which has the type that is most appropriate for the document format. For example, for an input document in WordProcessing format, getDocumentInfo will return an instance of a WordProcessingDocumentInfo class with information about page count, protection, exact format, and some other data.
---

> This guide demonstrates how to use the [`getDocumentInfo()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#getDocumentInfo-java.lang.String-) method to extract metadata from a document using **GroupDocs.Editor for Node.js via Java**.

## Introduction

In some situations, it is necessary to retrieve metadata from a document before actually editing it. For example, a user might want to edit the last worksheet of a multi-tabbed spreadsheet but doesn't know how many worksheets the document contains. Or it might be unclear whether the document is password-protected. For such situations, **GroupDocs.Editor** provides a [`getDocumentInfo()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#getDocumentInfo-java.lang.String-) method that returns detailed metadata about the specified document.

## Using the Method

To extract metadata from a document, you first need to load it into the `Editor` class. Then, call the `getDocumentInfo()` method. This method accepts one parameter—`password` as a string. If the document is encrypted and you know the password, you can specify it here. In other cases, you can pass `null` or an empty string. The code example below demonstrates the usage:

```javascript
// Import the necessary modules
const fs = require('fs');
const groupdocsEditor = require('@groupdocs/groupdocs.editor');

// Load the document into the Editor class
const inputFilePath = 'C://input//document.docx';
const inputStream = fs.createReadStream(inputFilePath);
const editor = new groupdocsEditor.Editor(inputStream);

// Get document info without specifying a password
const infoDocxWithoutPassword = editor.getDocumentInfo(null);

// Get document info with a password
const infoDocxWithPassword = editor.getDocumentInfo('password');

// Close the editor instance
editor.dispose();
```

### Scenarios Regarding Password Protection

There can be several scenarios depending on whether the document is encrypted and whether a password is specified:

1. **Password Specified, Document Not Password-Protected:**
   - If a password is specified but the document is not password-protected, or the document format doesn't support encryption, the password will be ignored.

2. **Document Password-Protected, Password Not Specified:**
   - If the document is password-protected but no password is specified, a [`PasswordRequiredException`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/PasswordRequiredException) will be thrown when calling `getDocumentInfo()`.

3. **Incorrect Password Specified:**
   - If the document is password-protected and an incorrect password is specified, an [`IncorrectPasswordException`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/IncorrectPasswordException) will be thrown when calling `getDocumentInfo()`.

## Understanding the Resulting Type

The `getDocumentInfo()` method returns an instance of [`IDocumentInfo`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/IDocumentInfo). This interface stores metadata about the document and contains the following properties:

1. **getPageCount():**
   - Returns a positive number indicating the page count for WordProcessing documents, worksheets count for Spreadsheets, and `1` for pageless documents like XML or TXT.

2. **getSize():**
   - Returns the document size in bytes.

3. **isEncrypted():**
   - A boolean flag indicating whether the document is encrypted with a password. If the document format doesn't support encryption, this property returns `false`.

4. **getFormat():**
   - Returns information about the document format.

### Inheritors of IDocumentInfo

There are several inheritors of the `IDocumentInfo` interface, each specific to a document family:

1. **WordProcessingDocumentInfo:**
   - For all WordProcessing formats.

2. **SpreadsheetDocumentInfo:**
   - For all Spreadsheet formats.

3. **PresentationDocumentInfo:**
   - For all Presentation formats.

4. **TextualDocumentInfo:**
   - For textual types, including DSV (like CSV and TSV), XML, HTML, and plain text.

5. **FixedLayoutDocumentInfo:**
   - For documents with a fixed-layout format, such as PDF and XPS.

6. **EmailDocumentInfo:**
   - For all Email formats, like EML, MSG, VCF, PST, MBOX, and others.

7. **EbookDocumentInfo:**
   - For all eBook formats like MOBI and EPUB.

8. **MarkdownDocumentInfo:**
   - Specific to the Markdown (MD) textual format.

**Note:** If `getDocumentInfo()` returns `null` instead of an `IDocumentInfo` inheritor, it means the specified document is not supported by GroupDocs.Editor and cannot be opened for editing or saving.

## Understanding Document Format

The `IDocumentInfo` interface contains a [`getFormat()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata/IDocumentInfo#getFormat--) property of type [`IDocumentFormat`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.formats.abstraction/IDocumentFormat). This interface represents a particular document format and stores the format name, extension, and MIME type.

Each inheritor of `IDocumentFormat` provides three properties, all of type `String`:

1. **getName():**
   - Provides the name of the format.

2. **getExtension():**
   - Provides the format extension.

3. **getMime():**
   - Provides the MIME type for the particular format.

The `IDocumentFormat` interface has several inheritors, all of them are structs:

1. **WordProcessingFormats:**
   - Holds all formats from the WordProcessing family.

2. **SpreadsheetFormats:**
   - Holds all formats from the Spreadsheet family.

3. **PresentationFormats:**
   - Holds all formats from the Presentation family.

4. **TextualFormats:**
   - Holds all formats with a text-based nature.

5. **FixedLayoutFormats:**
   - Holds all formats from the fixed-layout format family (e.g., PDF and XPS).

6. **EBookFormats:**
   - Holds all eBook formats like MOBI and EPUB.

7. **EmailFormats:**
   - Holds all email formats like EML and MSG.

## Complete Example

Here is a complete example demonstrating how to extract metadata from a WordProcessing document:

```javascript
// Import the necessary modules
const fs = require('fs');
const groupdocsEditor = require('@groupdocs/groupdocs.editor');

try {
    // Load the document into the Editor class
    const inputFilePath = 'C://input//document.docx';
    const inputStream = fs.createReadStream(inputFilePath);
    const editor = new groupdocsEditor.Editor(inputStream);

    // Get document info without specifying a password
    const infoDocxWithoutPassword = editor.getDocumentInfo(null);

    console.log('Page Count:', infoDocxWithoutPassword.getPageCount());
    console.log('Size (bytes):', infoDocxWithoutPassword.getSize());
    console.log('Is Encrypted:', infoDocxWithoutPassword.isEncrypted());

    const format = infoDocxWithoutPassword.getFormat();
    console.log('Format Name:', format.getName());
    console.log('Format Extension:', format.getExtension());
    console.log('Format MIME Type:', format.getMime());

    // Close the editor instance
    editor.dispose();
} catch (error) {
    if (error instanceof groupdocsEditor.PasswordRequiredException) {
        console.error('The document is password-protected. Please provide a password.');
    } else if (error instanceof groupdocsEditor.IncorrectPasswordException) {
        console.error('The provided password is incorrect.');
    } else {
        console.error('An error occurred:', error);
    }
}
```

## Conclusion

This guide has demonstrated how to use GroupDocs.Editor for Node.js via Java to extract metadata from documents before editing them. By utilizing the `getDocumentInfo()` method, you can obtain valuable information such as page count, size, encryption status, and format details. This functionality is essential for applications that require pre-processing or validation of documents before further manipulation.

---

**Note:** Replace `'C://input//document.docx'` with the actual path to your document.

---

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java) repository on GitHub.

---

**Tip:** Always handle exceptions appropriately in your applications, especially when dealing with password-protected or unsupported documents.