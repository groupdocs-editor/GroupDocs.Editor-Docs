---

id: introduction
url: editor/nodejs-java/introduction
title: Introduction
weight: 1
description: "This is an introduction to editing documents, explaining the main stages of document opening, editing, and saving results within Node.js via Java applications."
keywords: Edit document programmatically, Edit document Node.js, Edit document principles
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: Introduction to GroupDocs.Editor usage
        description: Description of the purposes and the most basic principles of GroupDocs.Editor
        productCode: editor
        productPlatform: nodejs-java 
    showVideo: True 
---

> This article explains the most common and fundamental principles of GroupDocs.Editor for Node.js via Java, how it works, what its purpose is, and how it should be used.

**GroupDocs.Editor for Node.js via Java** is a Node.js library that allows you to edit documents programmatically without a graphical user interface (GUI). It provides an API that enables you to manipulate documents within your Node.js applications. To edit a document, you need to use GroupDocs.Editor in conjunction with a third-party HTML editor application, through which the end-user can edit the document content via a GUI. GroupDocs.Editor is not concerned with which specific editor software is used, but since it is aimed at web development, the only requirement is that the third-party editor should be compatible with HTML documents.

To edit a document with GroupDocs.Editor, you need to perform several sequential steps:

1. **Load the document into GroupDocs.Editor** (using optional load options).
2. **Open the document for editing** (with optional edit options).
3. **Generate HTML markup with resources** (using different options and settings).
4. **Pass the markup to the third-party WYSIWYG HTML editor**.
5. **Edit the document content in the HTML editor**.
6. **Transfer the modified markup back to GroupDocs.Editor**.
7. **Convert the edited markup to the output document of the desired format**.

From the perspective of GroupDocs.Editor, this pipeline can be divided into three main stages, which are described below.

## Loading Document into GroupDocs.Editor

In the **[loading document](/editor/nodejs-java/developer-guide/load-document/)** stage, you should create an instance of the `Editor` class and pass an input document (through a file path or byte stream) along with document load options. Loading options are not required, and GroupDocs.Editor can automatically detect the document format and select the most appropriate default options for the given format. However, it is recommended to specify them explicitly, especially when dealing with password-protected documents.

```javascript
// Import the necessary modules
const groupdocsEditor = require('@groupdocs/groupdocs.editor');
const fs = require('fs');

const inputFilePath = 'C:\\input_path\\document.docx'; // Path to some document
const inputStream = fs.createReadStream(inputFilePath);
const editor = new groupdocsEditor.Editor(inputStream); // Passing the stream to the constructor; default WordProcessingLoadOptions will be applied automatically
```

After this stage, the document is ready to be opened and edited.

## Opening a Document for Editing

Since GroupDocs.Editor is a GUI-less library, the document cannot be edited directly within it. To edit the document in a WYSIWYG HTML editor, GroupDocs.Editor needs to generate an HTML version of the document, because any WYSIWYG editor works with HTML/CSS markup. After creating an instance of the `Editor` class in the first stage, you should **[open the document for editing](/editor/nodejs-java/developer-guide/edit-document/)** by calling the `edit()` method of the `Editor` class. This method returns an instance of the `EditableDocument` class. This class represents a converted version of the input document stored in an internal intermediate format compatible with all formats that GroupDocs.Editor supports. With `EditableDocument`, you can obtain the HTML markup of the input document with different options, stylesheets, images, fonts, save the HTML document to disk, and other operations. It is implied that the HTML markup emitted by `EditableDocument` is then passed to the client-side WYSIWYG HTML editor, where the end-user can actually edit the document.

Like with loading, the `edit()` method accepts optional `IEditOptions` inheritors that control how exactly the document will be opened for editing.

```javascript
const editOptions = new groupdocsEditor.WordProcessingEditOptions();
editOptions.setEnableLanguageInformation(true);

const readyToEdit = editor.edit(editOptions);
```

After this stage, the document is ready to be passed to the WYSIWYG HTML editor, and its content can be edited by the end-user.

## Saving a Document

**[Saving a document](/editor/nodejs-java/developer-guide/save-document/)** is the final stage, which occurs when the document content has been edited in the WYSIWYG HTML editor (or any other software—it makes no difference to GroupDocs.Editor) and should be saved back as a document in some format (like DOCX, PDF, or XLSX). At this stage, you should create a new instance of the `EditableDocument` class with the HTML markup and resources of the edited version of the original document obtained from the end-user. The `EditableDocument` class contains several static methods that allow you to create its instances from HTML documents that may be presented in different forms. Once the `EditableDocument` instance is ready, you can save it as an ordinary document using the `save()` method of the `Editor` class.

```javascript
const editedContent = '<body>HTML content of the document...</body>';
const afterEdit = groupdocsEditor.EditableDocument.fromMarkup(editedContent, null);
const outputFilePath = 'C:\\output_path\\document.rtf';
const saveOptions = new groupdocsEditor.WordProcessingSaveOptions(groupdocsEditor.WordProcessingFormats.Rtf);
editor.save(afterEdit, outputFilePath, saveOptions);
```

Unlike previous load options and edit options, save options are mandatory because GroupDocs.Editor needs to know the exact document format for saving.

## Detecting Document Type

Sometimes it is necessary to **[detect a document type and extract its metadata](/editor/nodejs-java/developer-guide/extracting-document-metainfo/)** before sending it for editing. For such scenarios, GroupDocs.Editor allows you to detect the document type and extract its most necessary metadata depending on the document type:

1. Is the document encoded or not?
2. Exact document format.
3. Document size.
4. Number of pages (or tabs).
5. Text encoding, if the document is textual.

To detect the document type and gather its metadata, you should load the desired document into the `Editor` class and then call the `getDocumentInfo()` method.

```javascript
const editor = new groupdocsEditor.Editor(inputStream);
const documentInfo = editor.getDocumentInfo();
```

## Describing Options

At every stage, you can adjust (tune) the processing by using different options:

1. `ILoadOptions` for **loading** the document.
2. `IEditOptions` for opening the document for **editing**.
3. `ISaveOptions` for **saving** the edited document.

Some of these options may be optional in specific cases, while others are mandatory. For example, it is possible to load a document into the `Editor` class without loading options—in such a case, GroupDocs.Editor will try to detect the document format automatically and apply the most appropriate default options for the detected document format.

### Describing Family Formats

All document formats supported by GroupDocs.Editor are grouped into family formats. Each family format has many common features, so there are no options for each format—only for the family format. The relation between formats, family formats, import/export formats, and options is illustrated in the table below.

| Family Format         | Supported Formats                                                                                   | Load                                   | Save                                   | Load Options                                                | Edit Options                                                 | Save Options                                                  | Metadata                                                                 |
|-----------------------|-----------------------------------------------------------------------------------------------------|----------------------------------------|----------------------------------------|-------------------------------------------------------------|--------------------------------------------------------------|---------------------------------------------------------------|--------------------------------------------------------------------------|
| WordProcessing        | DOC, DOCX, DOCM, DOT, DOTX, DOTM, RTF, WordprocessingML Flat XML, ODT, OTT, Word 2003 XML           | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | `WordProcessingLoadOptions`                                  | `WordProcessingEditOptions`                                   | `WordProcessingSaveOptions`                                    | `WordProcessingDocumentInfo`                                              |
| Spreadsheet           | XLS, XLT, XLSX, XLSM, XLSB, XLTX, XLTM, XLAM, SpreadsheetML XML, ODS, FODS, SXC, DIF                | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | `SpreadsheetLoadOptions`                                     | `SpreadsheetEditOptions`                                      | `SpreadsheetSaveOptions`                                       | `SpreadsheetDocumentInfo`                                                 |
| DSV                   | CSV, TSV, semicolon-separated, whitespace-separated, arbitrary separator                             | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | N/A                                                         | `DelimitedTextEditOptions`                                    | `DelimitedTextSaveOptions`                                     | N/A                                                                      |
| Presentation          | PPT, PPTX, PPTM, PPS, PPSX, PPSM, POT, POTX, POTM, ODP, OTP                                         | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | `PresentationLoadOptions`                                    | `PresentationEditOptions`                                     | `PresentationSaveOptions`                                      | `PresentationDocumentInfo`                                                |
| XML                   | Any XML document                                                                                    | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | ![error](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAhElEQVQ4T2NkoBAwUqifAEY/BiBaAVDC6ATITJoNwjGCKMkD8TaioShgsgagBqgDoHLEP4R0rJQDxH5DHASIRWB6wgYUg7iATCizAUxoADNVxigC0QK0V6I1g1EDpD6QlQSgHkWFAFcIlQEGakEATKhcAiHQA0RqlAzUAsYDZBrAFk4ikpW0QBpK9BJAKoQiIfT2EAJxBYAAAAAElFTkSuQmCC) | N/A                                                         | `XmlEditOptions`                                             | N/A                                                          | `TextualDocumentInfo`                                                     |
| TXT                   | Any text document                                                                                   | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | N/A                                                         | `TextEditOptions`                                            | `TextSaveOptions`                                             | `TextualDocumentInfo`                                                     |
| Fixed-layout format   | PDF                                                                                                 | ![error](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAhElEQVQ4T2NkoBAwUqifAEY/BiBaAVDC6ATITJoNwjGCKMkD8TaioShgsgagBqgDoHLEP4R0rJQDxH5DHASIRWB6wgYUg7iATCizAUxoADNVxigC0QK0V6I1g1EDpD6QlQSgHkWFAFcIlQEGakEATKhcAiHQA0RqlAzUAsYDZBrAFk4ikpW0QBpK9BJAKoQiIfT2EAJxBYAAAAAElFTkSuQmCC) | ![tick](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAAt0lEQVQ4T6WU0Q3CMBBF+5ZcAHZAK3YAm0Aj0Ar0AtIAtwAtwAtAKvSCpJcJtyddxhuHuZJJIwvqOaZ6V7ivAlUFc/xB85BUCSPuAmTAER4CimxCfAFctXr8FQCXX+jhV2AFVwzIrgVUgE0+uQ7JtwMgG8xH0cAHkEVDZgFSr4RAE6cUHgP0JtMDcKoCwLoY4+qwHIJ/AuhJeBWq1JkIBczXz/D32AyppAIpAHmV5nCXe/sdc3ibN43qAAAAAElFTkSuQmCC) | N/A                                                         | N/A                                                          | `PdfSaveOptions`                                              | N/A                                                                      |

### Additional Materials

Detailed information about every stage of document processing, along with source code examples and options explanations, can be found in the following articles:

1. [Load Document](/editor/nodejs-java/developer-guide/load-document/)
2. [Edit Document](/editor/nodejs-java/developer-guide/edit-document/)
3. [Save Document](/editor/nodejs-java/developer-guide/save-document/)

A complete description of the `EditableDocument` class, all its possibilities, members, and purpose, along with source code examples, is located in the following articles:

- [Working with EditableDocument](/editor/nodejs-java/developer-guide/editabledocument/)
- [Get HTML Markup in Different Forms](/editor/nodejs-java/developer-guide/editabledocument/get-html-markup-in-different-forms/)
- [Save HTML to Folder](/editor/nodejs-java/developer-guide/editabledocument/save-html-to-folder/)
- [Working with Resources](/editor/nodejs-java/developer-guide/editabledocument/working-with-resources/)
- [Create EditableDocument from File or Markup](/editor/nodejs-java/developer-guide/editabledocument/create-editabledocument-from-file-or-markup/)

A detailed review of all supported family formats, along with explanations of their load/edit/save options, illustrated with source code, can be found in the following articles:

- [Working with WordProcessing Documents](/editor/nodejs-java/developer-guide/edit-document/edit-word/)
- [Working with Spreadsheets](/editor/nodejs-java/developer-guide/edit-document/edit-excel/)
- [Working with DSV](/editor/nodejs-java/developer-guide/edit-document/how-to-edit-csv-file/)
- [Working with Presentations](/editor/nodejs-java/developer-guide/edit-document/edit-powerpoint/)
- [Working with Text Documents](/editor/nodejs-java/developer-guide/edit-document/edit-txt/)

---