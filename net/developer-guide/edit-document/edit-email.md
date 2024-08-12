---
id: edit-email
url: editor/net/edit-email
title: Edit Email 
linktitle: Edit Email documents
weight: 62
description: "This guide demonstrates how to edit content of the documents from Email format family like a common text documents using a GroupDocs.Editor for .NET."
keywords: Edit Email, Edit Email file, Edit Email document, Edit Email content, Edit Email text, Edit text in Email, Edit Email body, Edit Email message, Edit Email letter
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit Email document in the GroupDocs.Editor
        description: How to edit Email document using the GroupDocs.Editor in C# language
        productCode: editor
        productPlatform: net
    showVideo: True
    howTo:
        name: How to edit content of the Email document in the GroupDocs.Editor in C#
        description: Learn how to edit content of the Email document (mail message) with different editing options using the GroupDocs.Editor in C# step by step
        steps:
        - name: Load desired Email document to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired Email document into it.
        - name: Prepare a EmailEditOptions class
          text: Create an instance of the EmailEditOptions class and adjust its properties to meet your needs if necessary. In particular, it is possible to select which parts of the mail message will be processed.
        - name: Call Editor.Edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.Edit method with specifying a previously prepared EmailEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class
        - name: Prepare a EmailSaveOptions class
          text: Create an instance of the EmailSaveOptions class and adjust its properties to meet your needs if necessary. All properties are optional, so you may use a default EmailSaveOptions instance. With this classs it is possible to select which parts of the mail message will be stored in the resultant Email message.
        - name: Save edited document with Editor.Save method
          text: Pass an instance of EditableDocument with content of the edited document, instance of the EmailSaveOptions, and a destination byte stream or file path to the Editor.Save method for saving the document.
---
> This example demonstrates the standard open-edit-save pipeline with Email documents, using different options on every step.

## Introduction

There is a group of [Email file formats](https://docs.fileformat.com/email/), which usually are intended for storing individual mail letters, or contact data, personal information, calendar and so on. There are plenty of them, because almost every email program uses its own set of such formats. 

From its emergence the GroupDocs.Editor for .NET had no support for editing the such documents in Email formats. But starting from the [version 22.7](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/) we finally released this possibility! And this article explains in detail how to edit different Email files, because due to their nature their editing differs from editing common text documents.

## Loading

Loding of the Email documents is usual and the same as for other formats. Like with text, XPS and e-Book formats, and unlike the PDF and Office formats, there is no loading options for the Email documents — just specify a path to the file or a [byte stream](https://docs.microsoft.com/en-us/dotnet/api/system.IO.Stream?view=net-6.0) with the document content in the constructor of the [`GroupDocs.Editor.Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class and that's all.

Code example below demonstrates loading a single EML file to the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance through the file path and byte stream.

```csharp
const string emlFilename = "Attachments.eml";
string emlInputPath = System.IO.Path.Combine(Common.TestHelper.EmailFolder, emlFilename);

using (FileStream emlStream = File.OpenRead(emlInputPath))
using (Editor editorFromPath = new Editor(emlInputPath))//from the path
using (Editor editorFromStream = new Editor(emlStream))//from the stream
{
	//Here two Editor instances can separately work with one file
}
```

## Editing

Like for all other formats, there is a special [`EmailEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emaileditoptions) class for adjusting editing of the Email documents. Instance of this class should be specified in the [`Editor.Edit(IEditOptions editOptions)`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit/#edit) method. And, like for all other formats, it is possible to use a parameterless overload of the [`Editor.Edit()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) — in this case, if Email document was loaded to the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance, the GroupDocs.Editor will automatically apply the default [`EmailEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emaileditoptions).

[`EmailEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emaileditoptions) class has only one member — a [`MailMessageOutput`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emaileditoptions/mailmessageoutput) property of a [`MailMessageOutput`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/mailmessageoutput) type. [`MailMessageOutput`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/mailmessageoutput) is a flagged enum that controls, which parts of the mail message should be delivered to the output [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) and then to the emitted HTML. The [`MailMessageOutput`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/mailmessageoutput) enum has both *atomic* values that are responsible for a single part of the mail message, as well as *combined* values, like `Common` and `All`. It is allowed to use not only these combined values, but any user-defined combination of atomic values is acceptable. For example, it is possible to create a combination `Subject | Date | To | Cc | Bcc | From`, which processes only a metadata, but not the body and attachments list.

It is important to mention that currently the GroupDocs.Editor cannot process the content of the attachments, it can only display a list of attachment names. So the `MailMessageOutput.Attachments` flag, when specified, emits a list of attachment names, if they are present in the mail message.

Code example below shows adjusting of the [`MailMessageOutput`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/mailmessageoutput) and generating two different [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) for editing from a single input Email document.

```csharp
//0. Prepare a sample file
const string emlFilename = "Attachments.eml";
string emlInputPath = System.IO.Path.Combine(Common.TestHelper.EmailFolder, emlFilename);

//1. Load to the Editor class
Editor editor = new Editor(emlInputPath);

//2. Create 1st edit options with only metadata
Options.EmailEditOptions editOptionsMetadata = new EmailEditOptions(MailMessageOutput.Subject | MailMessageOutput.From | MailMessageOutput.To | MailMessageOutput.Cc | MailMessageOutput.Bcc | MailMessageOutput.Date);

//2. Create 2nd edit options with only subject, body and attachments list
Options.EmailEditOptions editOptionsBody = new EmailEditOptions(MailMessageOutput.Subject | MailMessageOutput.Body | MailMessageOutput.Attachments);

//3. Generate two different EditableDocument with these options from single input "Attachments.eml"
EditableDocument onlyMetadata = editor.Edit(editOptionsMetadata);
EditableDocument onlyBody = editor.Edit(editOptionsBody);

//4. Then emit HTML/CSS and send to the WYSIWYG-editor on the client-side
```

## Saving

Saving the edited Email documents in general follows the same principles, like for other document formats, but with several distinctions. As usual, after obtaining an edited HTML-content from the client-side WYSIWYG-editor an instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) should be created, and then need to pass it to the [`GroupDocs.Editor.Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method.

Like for all other formats, during saving, along with the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) and a path or a stream, an inheritor of the [`ISaveOptions` interface](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/isaveoptions) should be specified, and for email documents it is a [`EmailSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emailsaveoptions) class. Like the previously described [`EmailEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emaileditoptions), this class has only one member — a [`MailMessageOutput`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emailsaveoptions/mailmessageoutput) property of a [`MailMessageOutput`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/mailmessageoutput) type. The only distinction between this `MailMessageOutput` property in `EmailEditOptions` and `EmailSaveOptions` is that in the first case the `MailMessageOutput` property controls, which parts of the mail message will be processed while generating [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) from the input Email file, while in the second case the `MailMessageOutput` property controls, which parts of the mail message will be processed while generating output Email file from [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument), obtained after editing. So it is possible, for example, send to the WYSIWYG-editor a full mail message with 100% all content, but generate an output file only with a `Subject` and `Body` content, for example.

Email format family consists of many different formats, but the [`EmailSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emailsaveoptions) does not allow to specify the format for the output document — it automatically be the same, as the format of the original Email document, loaded into the [`GroupDocs.Editor.Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class. So, for example, if you loaded the MSG file, then the [`GroupDocs.Editor.Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method will also generate the MSG file, and no matter which file extension you specify.

Code example below shows full load-edit-save pipeline for the Email documents. It loads a document, makes it editable with `MailMessageOutput.All` combined flag, and then generates to output Email documents: first one is saved to the file with `MailMessageOutput.Common`, while second is saved to the stream with customly created combined flag.

```csharp
//0. Prepare a sample file
const string msgFilename = "ComplexExample.msg";
string msgInputPath = System.IO.Path.Combine(Common.TestHelper.EmailFolder, msgFilename);

//1. Load to the Editor class
Editor msgEditor = new Editor(msgInputPath);

//2. Create edit options with all content
Options.EmailEditOptions editOptions = new EmailEditOptions(MailMessageOutput.All);

//3. Generate an editable document
EditableDocument originalDoc = msgEditor.Edit(editOptions);

//4. Emit HTML from EditableDocument, send it to the client-side, edit it there in WYSIWYG-editor (omitted here)
string savedHtmlContent = originalDoc.GetEmbeddedHtml();

//5. Obtain edited content from the client-side and generate a new EditableDocument from it (omitted here)
EditableDocument editedDoc = EditableDocument.FromMarkup(savedHtmlContent, null);

//6. Create 1st save options
EmailSaveOptions saveOptions1 = new EmailSaveOptions(MailMessageOutput.Common);

//7. Create 2nd save options
EmailSaveOptions saveOptions2 = new EmailSaveOptions(MailMessageOutput.Body | MailMessageOutput.Attachments);

//8. Generate and save 1st output MSG to the file
string outputMsgPath = System.IO.Path.Combine(Common.TestHelper.OutputFolder, "OutputFile.msg");
msgEditor.Save(editedDoc, outputMsgPath, saveOptions1);

//9. Generate and save 2nd output MSG to the stream
Stream outputMsgStream = File.Create(Path.Combine(Common.TestHelper.OutputFolder, "OutputStream.msg"));
msgEditor.Save(editedDoc, outputMsgStream, saveOptions2);

//10. Dispose all resources
outputMsgStream.Dispose();
editedDoc.Dispose();
originalDoc.Dispose();
msgEditor.Dispose();
```

## Obtaining Email document info

Article [Extracting document metainfo](https://docs.groupdocs.com/editor/net/extracting-document-metainfo/) describes the [`GetDocumentInfo()` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo), that allows to detect the document format and extract its metadata without editing it. Actually, after adding Email support to the GroupDocs.Editor, this mechanism also works with Email documents.

When the [`GetDocumentInfo()` method](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) is called for the [`Editor` class](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance that is loaded with an Email document, the method will return a [`GroupDocs.Editor.Metadata.EmailDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/emaildocumentinfo) instance — it is a common class for all Email formats like EML, EMLX, MSG and so on.

As all [`IDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo) implementations, it has four properties:

1. [`Format`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/emaildocumentinfo/format) property of [`Formats.EmailFormats`](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/emailformats) type. It indicates the specific format of the given Email document.
2. [`PageCount`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/emaildocumentinfo/pagecount) property of [integer (`Int32`)](https://docs.microsoft.com/en-us/dotnet/api/system.int32?view=net-6.0) type. It always return 1, because email documents don't have paged view
3. [`Size`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/emaildocumentinfo/size) property of a [long integer (`Int64`)](https://docs.microsoft.com/en-us/dotnet/api/system.int64?view=net-6.0) type. Returns a file size in bytes.
4. [`IsEncrypted` property](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/emaildocumentinfo/isencrypted) of a [`Boolean`](https://docs.microsoft.com/en-us/dotnet/api/system.boolean?view=net-6.0) type. Because email documents cannot be encrypted with password, this property always returns `false`.

Code example below displays extracting document info from three different Email documents of different formats. Source code is represented as a unit-test, written with NUnit framework.

```csharp
const string emlFilename = "Attachments.eml";
string emlInputPath = System.IO.Path.Combine(Common.TestHelper.EmailFolder, emlFilename);

const string msgFilename = "ComplexExample.msg";
string msgInputPath = System.IO.Path.Combine(Common.TestHelper.EmailFolder, msgFilename);

const string pstFilename = "PersonalStorage.pst";
string pstInputPath = System.IO.Path.Combine(Common.TestHelper.EmailFolder, pstFilename);

Editor emlEditor = new Editor(emlInputPath);
Editor msgEditor = new Editor(msgInputPath);
Editor pstEditor = new Editor(pstInputPath);

IDocumentInfo emlDocInfo = emlEditor.GetDocumentInfo(null);
IDocumentInfo msgDocInfo = msgEditor.GetDocumentInfo(null);
IDocumentInfo pstDocInfo = pstEditor.GetDocumentInfo(null);

Assert.IsTrue(emlDocInfo is EmailDocumentInfo);
Assert.IsTrue(msgDocInfo is EmailDocumentInfo);
Assert.IsTrue(pstDocInfo is EmailDocumentInfo);

Assert.AreEqual(GroupDocs.Editor.Formats.EmailFormats.Eml, emlDocInfo.Format);
Assert.AreEqual(GroupDocs.Editor.Formats.EmailFormats.Msg, msgDocInfo.Format);
Assert.AreEqual(GroupDocs.Editor.Formats.EmailFormats.Pst, pstDocInfo.Format);

emlEditor.Dispose();
msgEditor.Dispose();
pstEditor.Dispose();
```






