---

id: edit-email
url: editor/nodejs-java/edit-email
title: Edit Email 
linktitle: Edit Email documents
weight: 62
description: "This guide demonstrates how to edit the content of documents from the Email format family like common text documents using GroupDocs.Editor for Node.js via Java."
keywords: Edit Email, Edit Email file, Edit Email document, Edit Email content, Edit Email text, Edit text in Email, Edit Email body, Edit Email message, Edit Email letter
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to Edit Email Documents using GroupDocs.Editor
        description: How to edit Email documents using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to Edit Content of Email Documents using GroupDocs.Editor in Node.js
        description: Learn how to edit the content of Email documents (mail messages) with different editing options using GroupDocs.Editor in Node.js step by step
        steps:
        - name: Load the desired Email document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload by passing the desired Email document into it.
        - name: Prepare an EmailEditOptions class
          text: Create an instance of the EmailEditOptions class and adjust its properties to meet your needs if necessary. In particular, it is possible to select which parts of the mail message will be processed.
        - name: Call editor.edit() and send the obtained EditableDocument to the WYSIWYG editor
          text: Invoke the editor.edit() method by specifying the previously prepared EmailEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG editor.
        - name: Edit the document in the WYSIWYG editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG editor, which is running on the client-side (in a web browser), and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class.
        - name: Prepare an EmailSaveOptions class
          text: Create an instance of the EmailSaveOptions class and adjust its properties to meet your needs if necessary. All properties are optional, so you may use a default EmailSaveOptions instance. With this class, it is possible to select which parts of the mail message will be stored in the resultant Email message.
        - name: Save the edited document with the editor.save() method
          text: Pass an instance of EditableDocument with the content of the edited document, an instance of EmailSaveOptions, and a destination byte stream or file path to the editor.save() method for saving the document.
---

> This example demonstrates the standard open-edit-save pipeline with Email documents, using different options at every step.

## Introduction

There are plenty of them because almost every email program uses its own set of such formats.

From its emergence, GroupDocs.Editor for Node.js via Java had no support for editing such documents in Email formats. But starting from [version 22.9](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2022/groupdocs-editor-for-node-js-via-java-22-9-release-notes/), we finally released this capability! This article explains in detail how to edit different Email files because, due to their nature, their editing differs from editing common text documents.

## Loading

Loading Email documents is straightforward and the same as for other formats. Like with text, XPS, and eBook formats, and unlike the PDF and Office formats, there are no loading options for Email documents—just specify a path to the file or a byte stream with the document content in the constructor of the [`Editor`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor) class, and that's all.

The code example below demonstrates loading a single EML file into the `Editor` instance through the file path and byte stream.

```javascript
// Import necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

// Path to the EML file
const emlFilePath = "C:\\Attachments.eml";

// Load from file path
const editorFromPath = new groupdocsEditor.Editor(emlFilePath);

// Load from stream
const emlStream = fs.createReadStream(emlFilePath);
const editorFromStream = new groupdocsEditor.Editor(emlStream);

// Now you can work with the editor instances
// ...

// Don't forget to dispose of the editors when done
editorFromPath.dispose();
editorFromStream.dispose();
```

## Editing

Like for all other formats, there is a special [`EmailEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.EmailEditOptions) class for adjusting the editing of Email documents. An instance of this class should be specified in the [`editor.edit(editOptions)`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#edit(com.groupdocs.editor.options.EditOptions)) method. And, like for all other formats, it is possible to use a parameterless overload of the [`editor.edit()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#edit()) method—in this case, if an Email document was loaded into the `Editor` instance, GroupDocs.Editor will automatically apply the default `EmailEditOptions`.

The `EmailEditOptions` class has one member—a `mailMessageOutput` property of the [`MailMessageOutput`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.MailMessageOutput) type. `MailMessageOutput` is a flagged enumeration that controls which parts of the mail message should be delivered to the output `EditableDocument` and then to the emitted HTML. The `MailMessageOutput` enum has both *atomic* values that are responsible for a single part of the mail message, as well as *combined* values, like `Common` and `All`. It is allowed to use not only these combined values but any user-defined combination of atomic values. For example, it is possible to create a combination of `Subject | Date | To | Cc | Bcc | From`, which processes only metadata but not the body and attachments list.

It is important to mention that currently, GroupDocs.Editor cannot process the content of attachments; it can only display a list of attachment names. So the `MailMessageOutput.Attachments` flag, when specified, emits a list of attachment names if they are present in the mail message.

The code example below shows adjusting the `MailMessageOutput` and generating two different `EditableDocument` instances for editing from a single input Email document.

```javascript
// Import necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

// Prepare a sample file
const emlFilePath = "C:\\Attachments.eml";

// Load into the Editor class
const editor = new groupdocsEditor.Editor(emlFilePath);

// Create 1st edit options with only metadata
const editOptionsMetadata = new groupdocsEditor.EmailEditOptions();
editOptionsMetadata.setMailMessageOutput(
    groupdocsEditor.MailMessageOutput.Subject |
    groupdocsEditor.MailMessageOutput.From |
    groupdocsEditor.MailMessageOutput.To |
    groupdocsEditor.MailMessageOutput.Cc |
    groupdocsEditor.MailMessageOutput.Bcc |
    groupdocsEditor.MailMessageOutput.Date
);

// Create 2nd edit options with only subject, body, and attachments list
const editOptionsBody = new groupdocsEditor.EmailEditOptions();
editOptionsBody.setMailMessageOutput(
    groupdocsEditor.MailMessageOutput.Subject |
    groupdocsEditor.MailMessageOutput.Body |
    groupdocsEditor.MailMessageOutput.Attachments
);

// Generate two different EditableDocument instances from a single input "Attachments.eml"
const onlyMetadata = editor.edit(editOptionsMetadata);
const onlyBody = editor.edit(editOptionsBody);

// Now you can work with the EditableDocument instances
// ...

// Don't forget to dispose of the editor and EditableDocuments when done
onlyMetadata.dispose();
onlyBody.dispose();
editor.dispose();
```

## Saving

Saving the edited Email documents generally follows the same principles as for other document formats but with some distinctions. As usual, after obtaining edited HTML content from the client-side WYSIWYG editor, an instance of the `EditableDocument` should be created, and then it needs to be passed to the [`editor.save()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#save(com.groupdocs.editor.EditableDocument,%20java.lang.String,%20com.groupdocs.editor.options.SaveOptions)) method.

Like for all other formats, during saving, along with the `EditableDocument` and a path or a stream, an inheritor of the [`SaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.SaveOptions) interface should be specified. For Email documents, it is the [`EmailSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.EmailSaveOptions) class. Like the previously described `EmailEditOptions`, this class has one member—a `mailMessageOutput` property of the `MailMessageOutput` type. The only distinction between this `mailMessageOutput` property in `EmailEditOptions` and `EmailSaveOptions` is that in the first case, the `mailMessageOutput` property controls which parts of the mail message will be processed while generating `EditableDocument` from the input Email file, while in the second case, the `mailMessageOutput` property controls which parts of the mail message will be processed while generating the output Email file from the `EditableDocument` obtained after editing. So it is possible, for example, to send to the WYSIWYG editor a full mail message with 100% of all content but generate an output file only with the `Subject` and `Body` content.

The Email format family consists of many different formats, but the `EmailSaveOptions` does not allow specifying the format for the output document—it will automatically be the same as the format of the original Email document loaded into the `Editor` class. So, for example, if you loaded an MSG file, then the `editor.save()` method will also generate an MSG file, no matter which file extension you specify.

The code example below shows the full load-edit-save pipeline for Email documents. It loads a document, makes it editable with `MailMessageOutput.All` combined flag, and then generates two output Email documents: the first one is saved to a file with `MailMessageOutput.Common`, while the second is saved to a stream with a custom combined flag.

```javascript
// Import necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

// Prepare a sample file
const msgInputPath = "C:\\ComplexExample.msg";

// Load into the Editor class
const msgEditor = new groupdocsEditor.Editor(msgInputPath);

// Create edit options with all content
const editOptions = new groupdocsEditor.EmailEditOptions();
editOptions.setMailMessageOutput(groupdocsEditor.MailMessageOutput.All);

// Generate an editable document
const originalDoc = msgEditor.edit(editOptions);

// Emit HTML from EditableDocument, send it to the client-side, edit it there in WYSIWYG editor (omitted here)
const savedHtmlContent = originalDoc.getEmbeddedHtml();

// Assume the edited content is obtained from the client-side and stored in 'editedHtmlContent' (omitted here)
const editedHtmlContent = savedHtmlContent; // For demonstration purposes

// Obtain edited content and generate a new EditableDocument from it
const editedDoc = groupdocsEditor.EditableDocument.fromMarkup(editedHtmlContent, null);

// Create 1st save options
const saveOptions1 = new groupdocsEditor.EmailSaveOptions();
saveOptions1.setMailMessageOutput(groupdocsEditor.MailMessageOutput.Common);

// Create 2nd save options
const saveOptions2 = new groupdocsEditor.EmailSaveOptions();
saveOptions2.setMailMessageOutput(
    groupdocsEditor.MailMessageOutput.Body |
    groupdocsEditor.MailMessageOutput.Attachments
);

// Generate and save 1st output MSG to the file
const outputMsgPath = "C:\\OutputFile.msg";
msgEditor.save(editedDoc, outputMsgPath, saveOptions1);

// Generate and save 2nd output MSG to the stream
const outputMsgStream = fs.createWriteStream("C:\\OutputStream.msg");
msgEditor.save(editedDoc, outputMsgStream, saveOptions2);

// Dispose of all resources
outputMsgStream.end();
editedDoc.dispose();
originalDoc.dispose();
msgEditor.dispose();
```

## Obtaining Email Document Info

The article [Extracting Document Metainfo](/editor/nodejs-java/extracting-document-metainfo) describes the [`getDocumentInfo()` method](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#getDocumentInfo(java.lang.String)), which allows detecting the document format and extracting its metadata without editing it. After adding Email support to GroupDocs.Editor, this mechanism also works with Email documents.

When the `getDocumentInfo()` method is called for the `Editor` instance that is loaded with an Email document, the method will return an instance of the [`EmailDocumentInfo`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.metadata.EmailDocumentInfo) class—it is a common class for all Email formats like EML, EMLX, MSG, and so on.

As all `IDocumentInfo` implementations, it has four properties:

1. **`getFormat()`**: Property of [`EmailFormats`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.formats.EmailFormats) type. It indicates the specific format of the given Email document.
2. **`getPageCount()`**: Property of `int` type. It always returns 1 because Email documents don't have a paged view.
3. **`getSize()`**: Property of `long` type. Returns the file size in bytes.
4. **`isEncrypted()`**: Property of `boolean` type. Because Email documents cannot be encrypted with a password, this property always returns `false`.

The code example below demonstrates extracting document info from three different Email documents of different formats.

```javascript
// Import necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

// Paths to the Email documents
const emlInputPath = "C:\\Attachments.eml";
const msgInputPath = "C:\\ComplexExample.msg";
const pstInputPath = "C:\\PersonalStorage.pst";

// Load into the Editor class
const emlEditor = new groupdocsEditor.Editor(emlInputPath);
const msgEditor = new groupdocsEditor.Editor(msgInputPath);
const pstEditor = new groupdocsEditor.Editor(pstInputPath);

// Get document info
const emlDocInfo = emlEditor.getDocumentInfo();
const msgDocInfo = msgEditor.getDocumentInfo();
const pstDocInfo = pstEditor.getDocumentInfo();

// Check if the returned info is of type EmailDocumentInfo
console.log(emlDocInfo instanceof groupdocsEditor.EmailDocumentInfo); // true
console.log(msgDocInfo instanceof groupdocsEditor.EmailDocumentInfo); // true
console.log(pstDocInfo instanceof groupdocsEditor.EmailDocumentInfo); // true

// Verify the formats
console.log(emlDocInfo.getFormat() === groupdocsEditor.EmailFormats.Eml); // true
console.log(msgDocInfo.getFormat() === groupdocsEditor.EmailFormats.Msg); // true
console.log(pstDocInfo.getFormat() === groupdocsEditor.EmailFormats.Pst); // true

// Dispose of the editors
emlEditor.dispose();
msgEditor.dispose();
pstEditor.dispose();
```

## Conclusion

By following the steps outlined in this guide, you can effectively edit Email documents using GroupDocs.Editor for Node.js via Java. The process involves loading the Email document, configuring editing options to specify which parts of the email to edit, editing the content using a WYSIWYG HTML editor, and finally saving the edited content back into an Email format.

This capability is particularly useful for applications that require programmatic manipulation of Email messages, such as email automation systems, archiving solutions, or custom email clients.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.

