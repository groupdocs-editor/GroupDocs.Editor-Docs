---
id: edit-email
url: editor/python-net/edit-email
title: Edit Email
linkTitle: Edit Email documents
weight: 62
description: "This guide demonstrates how to edit the content of documents from the Email format family like common text documents using GroupDocs.Editor for Python via .NET."
keywords: Edit Email, Edit Email file, Edit Email document, Edit Email content, Edit Email text, Edit text in Email, Edit Email body, Edit Email message, Edit Email letter, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit Email document in the GroupDocs.Editor
        description: How to edit Email document using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to edit content of the Email document in the GroupDocs.Editor in Python
        description: Learn how to edit the content of an Email document (mail message) with different editing options using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load the desired Email document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired Email document into it.
        - name: Prepare an EmailEditOptions class
          text: Create an instance of the EmailEditOptions class and adjust its properties to meet your needs if necessary. In particular, it is possible to select which parts of the mail message will be processed.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the editor.edit method with a previously prepared EmailEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML markup and extract resources from this instance using the corresponding instance methods, and pass all this data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in the WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on the client-side (in a web-browser), and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable class methods of the class.
        - name: Prepare an EmailSaveOptions class
          text: Create an instance of the EmailSaveOptions class and adjust its properties to meet your needs if necessary. All properties are optional, so you may use a default EmailSaveOptions instance. With this class it is possible to select which parts of the mail message will be stored in the resultant Email message.
        - name: Save the edited document with the editor.save method
          text: Pass an instance of EditableDocument with the content of the edited document, an instance of EmailSaveOptions, and a destination byte stream or file path to the editor.save method for saving the document.
---
> This example demonstrates the standard open-edit-save pipeline with Email documents, using different options on every step.

## Introduction

There is a group of [Email file formats](https://docs.fileformat.com/email/), which are usually intended for storing individual mail letters, contact data, personal information, calendars, and so on. There are plenty of them, because almost every email program uses its own set of such formats. This article explains how to edit different Email files, because due to their nature their editing differs from editing common text documents.

## Loading

Loading Email documents is usual and the same as for other formats. Like with text, XPS, and e-Book formats, and unlike PDF and Office formats, there are no loading options for Email documents — just specify a path to the file or a byte stream with the document content in the constructor of the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class, and that's all.

```python
from groupdocs.editor import Editor

# Load from a file path
editor_from_path = Editor("message.eml")

# Load from a binary stream
with open("message.eml", "rb") as stream:
    editor_from_stream = Editor(stream)
```

## Editing

Like for all other formats, there is a special [`EmailEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emaileditoptions) class for adjusting the editing of Email documents. An instance of this class can be specified in the [`editor.edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method. It is also possible to use the parameterless overload of [`editor.edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) — in this case GroupDocs.Editor automatically applies the default [`EmailEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emaileditoptions).

The [`EmailEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emaileditoptions) class has only one member — a `mail_message_output` property of the [`MailMessageOutput`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/mailmessageoutput) type. `MailMessageOutput` is a flagged enum that controls which parts of the mail message should be delivered to the output [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) and then to the emitted HTML. The `MailMessageOutput` enum has both *atomic* values that are responsible for a single part of the mail message (`SUBJECT`, `FROM`, `TO`, `CC`, `BCC`, `DATE`, `BODY`, `ATTACHMENTS`), as well as *combined* values, like `COMMON` and `ALL`.

It is important to mention that currently GroupDocs.Editor cannot process the content of attachments — it can only display a list of attachment names. So the `MailMessageOutput.ATTACHMENTS` flag, when specified, emits a list of attachment names, if they are present in the mail message.

The runnable example below loads an EML file, edits it with the `MailMessageOutput.ALL` value, and obtains the resulting HTML content.

{{< tabs "code-example-edit-email">}}
{{< tab "edit_email.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import EmailEditOptions, MailMessageOutput

def edit_email():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load an input Email document into the Editor
    with Editor("./sample-email.eml") as editor:
        # Create edit options that process all parts of the mail message
        edit_options = EmailEditOptions(MailMessageOutput.ALL)

        # Edit the Email document and obtain an EditableDocument
        editable = editor.edit(edit_options)

        # Obtain the HTML content (in practice it is sent to the WYSIWYG-editor)
        content = editable.get_content()
        print("Generated HTML content length:", len(content))

        editable.dispose()

if __name__ == "__main__":
    edit_email()
```
{{< /tab >}}
{{< tab "sample-email.eml" >}}  
{{< tab-text >}}
`sample-email.eml` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-email/sample-email.eml) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edit-email.txt" >}}  
```text
Generated HTML content length: 1255
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-email/edit_email/edit-email.txt)
{{< /tab >}}
{{< /tabs >}}

It is also possible to use any user-defined combination of atomic values. For example, you can create a combination that processes only metadata, but not the body and attachments list:

```python
from groupdocs.editor.options import EmailEditOptions, MailMessageOutput

metadata_only = (MailMessageOutput.SUBJECT | MailMessageOutput.FROM | MailMessageOutput.TO |
                 MailMessageOutput.CC | MailMessageOutput.BCC | MailMessageOutput.DATE)
edit_options = EmailEditOptions(metadata_only)
```

## Saving

Saving edited Email documents in general follows the same principles as for other document formats, but with several distinctions. As usual, after obtaining edited HTML content from the client-side WYSIWYG-editor, an instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) should be created, and then it should be passed to the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.

For Email documents the save options class is [`EmailSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emailsaveoptions). Like [`EmailEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emaileditoptions), this class has only one member — a `mail_message_output` property of the [`MailMessageOutput`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/mailmessageoutput) type. The only distinction is that in `EmailEditOptions` the `mail_message_output` property controls which parts of the mail message are processed while generating the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) from the input file, while in `EmailSaveOptions` it controls which parts are processed while generating the output Email file from the edited [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument).

The [`EmailSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emailsaveoptions) class does not allow specifying the format for the output document — it is automatically the same as the format of the original Email document loaded into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class. For example, if you loaded an MSG file, then [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) will also generate an MSG file, no matter which file extension you specify.

```python
from groupdocs.editor import Editor, EditableDocument
from groupdocs.editor.options import EmailEditOptions, EmailSaveOptions, MailMessageOutput

with Editor("./sample-email.eml") as editor:
    # Edit with all content
    original = editor.edit(EmailEditOptions(MailMessageOutput.ALL))

    # Send the content to the WYSIWYG-editor and obtain the edited content (omitted here)
    edited = EditableDocument.from_markup(original.get_embedded_html())

    # Save only the common parts of the mail message
    save_options = EmailSaveOptions(MailMessageOutput.COMMON)
    editor.save(edited, "./edited-email.eml", save_options)

    original.dispose()
    edited.dispose()
```

## Obtaining Email document info

The article [Extracting document metainfo]({{< ref "editor/python-net/developer-guide/extracting-document-metainfo.md" >}}) describes the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method, which allows you to detect the document format and extract its metadata without editing it. This mechanism also works with Email documents.

When [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) is called for an [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance that is loaded with an Email document, the method returns a metadata view corresponding to the `EmailDocumentInfo` type — a common type for all Email formats like EML, EMLX, MSG, and so on. It exposes the `format`, `page_count`, `size`, and `is_encrypted` properties. For Email documents `page_count` always returns 1 (email documents have no paged view), and `is_encrypted` always returns `False` (email documents cannot be encrypted with a password).

```python
from groupdocs.editor import Editor

with Editor("./sample-email.eml") as editor:
    info = editor.get_document_info()
    print("Format:", info.format.name)
    print("Size:", info.size)
    print("Is encrypted:", info.is_encrypted)
```
