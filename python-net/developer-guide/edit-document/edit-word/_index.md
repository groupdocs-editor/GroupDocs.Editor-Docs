---
id: edit-word
url: editor/python-net/edit-word
title: Edit Word 
linkTitle: Edit Word document
weight: 12
description: "This guide demonstrates how to edit DOC, DOT, DOCX, DOCM, DOTX, ODT, RTF documents with font extraction, different pagination modes and many other powerful features of GroupDocs.Editor for Python via .NET."
keywords: Edit DOCX, Edit DOC, Edit RTF, Edit ODT, Edit Word document, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit Word document in the GroupDocs.Editor
        description: How to edit Word document using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net 
    showVideo: True
    howTo:
        name: How to edit content of the Word document in the GroupDocs.Editor in Python
        description: Learn how to edit content of the Word document in the different editing modes using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load desired Word document to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired  Word document into it.
        - name: Prepare a WordProcessingEditOptions class
          text: Create an instance of the WordProcessingEditOptions class and adjust its properties to meet your needs if necessary.
        - name: Select desired paginal mode
          text: Using the WordProcessingEditOptions you should select the desired pagination mode during editing — in the float mode all content of the Word document will be represented as one single page, while in the paginal the separation onto pages will be preserved, like in Microsoft Word.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke an editor.edit method with specifying a previously prepared WordProcessingEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable class methods of the class
        - name: Prepare a WordProcessingSaveOptions class
          text: Create an instance of the WordProcessingSaveOptions class and adjust its properties to meet your needs if necessary. You need to choose the format of the output document — this is the only mandatory parameter, that must be specified in the constructor. Also set the pagination mode value the same, as it was specified in WordProcessingEditOptions.
        - name: Save edited document with editor.save method
          text: Pass an instance of EditableDocument with content of the edited document, instance of the WordProcessingSaveOptions, and a destination byte stream or file path to the editor.save method for saving the document.
---
> This example demonstrates standard open-edit-save cycle with WordProcessing documents, using different options on every step.

## How to edit Word document?

WordProcessing is the most used and known document family format, that includes DOC, DOT, DOCX, DOCM, DOTX, ODT, RTF and much more. All these formats are supported by the [**GroupDocs.Editor**](https://products.groupdocs.com/editor/python-net).
There are two processing modes for WordProcessing documents:

* *float* (default)
* *paged* (also known as *paginal*).

In the float mode, when document is opened for editing and loaded into WYSIWYG client-side HTML-editor, it is represented without pages, like a single page text document.  
In counterpart to this, when paged mode is enabled, document content is divided onto pages. For proper usage paged mode, if enabled, should be enabled in both edit options and save options simultaneously (this is described below).

## Load Word file for edit

First of all user must open a document by loading it into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class instance. This example demonstrates how to load the password-protected document. So, let's suppose we have an encoded DOCX, and user knows its password. First of all, you need to create a load options — an instance of the [`WordProcessingLoadOptions` class](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingloadoptions).

```python
from groupdocs.editor.options import WordProcessingLoadOptions

load_options = WordProcessingLoadOptions()
load_options.password = "some_password_to_open_a_document"
```

Please note that if document has no protection, the password will be ignored. However, if document is protected, but user has not specified a password, a [PasswordRequiredException](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/passwordrequiredexception) will be thrown during document editing.

Next step is to load the document into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class. Same with load options — they should be passed.

```python
from groupdocs.editor import Editor

editor = Editor("document.docx", load_options)
```

## Edit the document

When document is loaded, it can be edited (transformed to [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class), and this process can be adjusted with edit options. Let's create them:

```python
from groupdocs.editor.options import WordProcessingEditOptions

edit_options = WordProcessingEditOptions()                 # #1
edit_options.enable_language_information = True             # #2
edit_options.enable_pagination = True                      # #3
```

Let's describe the code above line by line.
*Line #1* - every supported document family format has its own options class. So for all WordProcessing formats you need to apply the [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions). The same for other formats — [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheeteditoptions) for all spreadsheet-based formats (like XLS, ODS etc.) and so on.
*Line #2* - enables extracting language information for better subsequent spell-checking on client side. Finally, *line #3* switches document processing mode from float (default) to the paged.

After preparing options the previously loaded document can be edited:

```python
before_edit = editor.edit(edit_options)
```

Unlike previous example let's extract HTML markup and resources separately:

```python
original_content = before_edit.get_content()
all_resources = before_edit.all_resources
```

First string contains all HTML markup without resources, while second collection contains all resources (images, fonts, and stylesheets).

### Modifying document content

Let's imagine that user passed HTML markup and resources, obtained from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, to the WYSIWYG-editor, edited the document on client-side and obtained back a modified HTML markup.   
Now user needs to create new [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance from this modified markup.

```python
from groupdocs.editor import EditableDocument

edited_content = original_content.replace("document", "edited document")
after_edit = EditableDocument.from_markup(edited_content)
```

## Save Word file after edit

Before saving the document user must create saving options.

```python
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCM)   # #1, #2
save_options.password = "password"                                    # #3
save_options.enable_pagination = True                                 # #4
save_options.optimize_memory_usage = True                             # #5
```

Let's describe a piece of code above line by line:

1. Creating a format of output document, in our case it is DOCM.
2. Creating instance of [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) with previously prepared format.
3. When user specifies a password, GroupDocs.Editor encrypts the document with this password. So, when document will be saved, it can be opened only with the password.
4. Because pagination was previously enabled in [WordProcessingEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingeditoptions) (`edit_options` variable), for better output result it is highly recommended to enable it in [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions).
5. If document is really huge and causes an out-of-memory error, you can set memory optimization option.

Finally, user should save an edited document with prepared save options.

```python
editor.save(after_edit, "edited-document.docm", save_options)
```

## Conclusion

This tutorial demonstrates basic scenario — opening document, editing it and saving, — but with detailed options on every step. The subsequent articles in this section describe each of these options in detail.
