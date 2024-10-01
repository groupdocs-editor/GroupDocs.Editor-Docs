---

id: load-document
url: editor/nodejs-java/load-document
title: Load Document
weight: 2
description: "Following this guide, you will learn how to load a document from the local disk or file stream for editing with GroupDocs.Editor for Node.js via Java API."
keywords: Load document with GroupDocs.Editor, Load and edit document, edit document, edit spreadsheet, edit presentation
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
structuredData:
    showOrganization: True
    application:
        name: Load Document into GroupDocs.Editor
        description: Load a document into GroupDocs.Editor for subsequent usage in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to load a document into GroupDocs.Editor in Node.js via Java
        description: Learn how to load a document into GroupDocs.Editor in Node.js via Java step by step
        steps:
        - name: Prepare a LoadOptions instance if needed
          text: If the input document is password-protected or requires some adjustment during loading, or if you need to specify its format family explicitly, create an appropriate inheritor of the ILoadOptions interface
        - name: Invoke the appropriate Editor class constructor
          text: Pass a file path as a string or a byte stream through a delegate into the most appropriate overload of the constructor of GroupDocs.Editor.Editor class
        - name: Make all necessary edits and savings
          text: After the document is successfully loaded, you can get its metadata, generate its editable version, and finally save it to the resultant file
---

> This article describes how to load an input document into **GroupDocs.Editor for Node.js via Java** and how to apply load options.

First, the input document, which should be accessible as a byte stream or through a valid file path, should be loaded into GroupDocs.Editor by creating an instance of the `Editor` class through one of its constructor overloads. The source code below shows two ways of loading documents: from a path and from a stream.

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Loading from path
const inputFilePath = 'C:\\input_path\\document.docx'; // Path to some document
const editor = new groupdocsEditor.Editor(inputFilePath);

// Loading from stream
const inputStream = fs.createReadStream(inputFilePath);
const editor = new groupdocsEditor.Editor(inputStream);
```

When using the two overloads from the example above, GroupDocs.Editor automatically detects the format of the input document and applies the most appropriate default loading options for it.

However, it is possible and even recommended to specify correct loading options explicitly using constructor overloads that accept two parameters. The source code below shows how to use such options.

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Loading from path with load options
const inputFilePath = 'C:\\input_path\\document.docx'; // Path to some document
const wordLoadOptions = new groupdocsEditor.WordProcessingLoadOptions();
const editor1 = new groupdocsEditor.Editor(inputFilePath, wordLoadOptions);

// Loading from stream with load options
const inputStream = fs.createReadStream(inputFilePath);
const spreadsheetLoadOptions = new groupdocsEditor.SpreadsheetLoadOptions();
const editor = new groupdocsEditor.Editor(inputStream, spreadsheetLoadOptions);
```

Please note that not all document formats have appropriate classes that represent load options. As of version 23.10, only WordProcessing, Spreadsheet, Presentation, and some other family formats have load options. For other document types, such as DSV, TXT, or XML, there are no load options.

| Format Family    | Example Formats                          | Load Options Class                                 |
|------------------|------------------------------------------|----------------------------------------------------|
| WordProcessing   | DOC, DOCX, DOCM, DOT, ODT                | `WordProcessingLoadOptions`                        |
| Spreadsheet      | XLS, XLSX, XLSM, XLSB                    | `SpreadsheetLoadOptions`                           |
| Presentation     | PPT, PPTX, PPS, POT                      | `PresentationLoadOptions`                          |
| Email            | EML, MSG, EMLX, MHT                      | `EmailLoadOptions`                                 |
| PDF              | PDF                                       | `PdfLoadOptions`                                   |

Using load options is essential when working with password-protected input documents. Any document can be loaded into the `Editor` instance, even an encrypted document without the password. However, in the next step—opening for editing—an exception will be thrown. GroupDocs.Editor handles passwords and encrypted documents in the following way:

1. **Document Not Encrypted:**
   - If the document is not encrypted, the password is ignored, whether or not it was specified.
2. **Document Encrypted, Password Not Specified:**
   - If the document is password-protected but no password is specified, a [`PasswordRequiredException`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/PasswordRequiredException) will be thrown later during editing.
3. **Incorrect Password Specified:**
   - If the document is password-protected and a password is specified but is incorrect, an [`IncorrectPasswordException`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/IncorrectPasswordException) will be thrown later during editing.

The example below shows how to specify a password for opening a password-protected WordProcessing document.

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

// Loading a password-protected document
const inputFilePath = 'C:\\input_path\\protected_document.docx';
const inputStream = fs.createReadStream(inputFilePath);
const wordLoadOptions = new groupdocsEditor.WordProcessingLoadOptions();
wordLoadOptions.setPassword('correct_password');

const editor = new groupdocsEditor.Editor(inputStream, wordLoadOptions);
```

**Note:** The same approach is applicable for Spreadsheet and Presentation documents as well.

---

**Tip:** Always handle exceptions appropriately in your applications, especially when dealing with password-protected or unsupported documents.

---

## Conclusion

This article has demonstrated how to load documents into **GroupDocs.Editor for Node.js via Java** using different methods and how to apply load options, especially when dealing with password-protected documents. By specifying the appropriate load options, you ensure that your documents are correctly processed and ready for editing.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java) repository on GitHub.

---

> **Note:** Replace `'C:\\input_path\\document.docx'` and `'C:\\input_path\\protected_document.docx'` with the actual paths to your documents.

---