---
id: edit-word
url: editor/java/edit-word
title: Edit Word 
linktitle: Edit Word document
weight: 45
description: "This guide demonstrates how to edit DOC, DOT, DOCX, DOCM, DOTX, ODT, RTF documents with font extraction, different pagination modes and many other powerful features of GroupDocs.Editor for Java."
keywords: Edit DOCX, Edit DOC, Edit RTF, Edit ODT, Edit Word document
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit Word document in the GroupDocs.Editor
        description: How to edit Word document using the GroupDocs.Editor in Java language
        productCode: editor
        productPlatform: java 
    showVideo: True
    howTo:
        name: How to edit content of the Word document in the GroupDocs.Editor in Java
        description: Learn how to edit content of the Word document in the different editing modes using the GroupDocs.Editor in Java step by step
        steps:
        - name: Load desired Word document to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired  Word document into it.
        - name: Prepare a WordProcessingEditOptions class
          text: Create an instance of the WordProcessingEditOptions class and adjust its properties to meet your needs if necessary.
        - name: Select desired paginal mode
          text: Using the WordProcessingEditOptions you should select the desired pagination mode during editing — in the float mode all content of the Word document will be represented as one single page, while in the paginal the separation onto pages will be preserved, like in Microsoft Word.
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.edit method with specifying a previously prepared WordProcessingEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class
        - name: Prepare a WordProcessingSaveOptions class
          text: Create an instance of the WordProcessingSaveOptions class and adjust its properties to meet your needs if necessary. You need to choose the format of the output document — this is the only mandatory parameter, that must be specified in the constructor. Also set the pagination mode value the same, as it was specified in WordProcessingEditOptions.
        - name: Save edited document with Editor.save method
          text: Pass an instance of EditableDocument with content of the edited document, instance of the WordProcessingSaveOptions, and a destination byte stream or file path to the Editor.save method for saving the document.
---
> This example demonstrates standard open-edit-save cycle with WordProcessing documents, using different options on every step.

## How to edit Word document?

WordProcessing is the most used and known document family format, that includes DOC, DOT, DOCX, DOCM, DOTX, ODT, RTF and much more. All these formats are supported by the [**GroupDocs.Editor**](https://products.groupdocs.com/editor/java).
There are two processing modes for WordProcessing documents:

* *float* (default)
* *paged* (also known as *paginal*).

In the float mode, when document is opened for editing and loaded into WYSIWYG client-side HTML-editor, it is represented without pages, like a single page text document.  
In counterpart to this, when paged mode is enabled, document content is divided onto pages. For proper usage paged mode, if enabled, should be enabled in both edit options and save options simultaneously (this is described below).

## Loading WordProcessing documents

First of all user must open a document by loading it into the [Editor](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class instance. This example demonstrates how to load the password-protected document from the stream. So, let's suppose we have an encoded DOCX, and user knows its password. First of all, you need to create a load options.

```java
WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
loadOptions.setPassword("some_password_to_open_a_document");
```

Please note that if document has no protection, the password will be ignored. However, if document is protected, but user has not specified a password, a [PasswordRequiredException](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/passwordrequiredexception) will be thrown during document editing.

Next step is to load the document from stream into the [Editor](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class. For loading documents from streams [Editor](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editor) uses delegates. In other words, you need to pass the delegate instance, that points to the method, that returns a stream.
Same with load options — they are passed via delegate.

```java
InputStream inputStream = new FileInputStream("C:\\input_path\\document.docx");
Editor editor = new Editor(inputStream, loadOptions);
```

## Editing WordProcessing documents

When document is loaded, it can be edited (transformed to [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class), and this process can be adjusted with edit options. Let's create them:

```java
WordProcessingEditOptions editOptions = new WordProcessingEditOptions(); //#1
editOptions.setFontExtraction(FontExtractionOptions.ExtractEmbeddedWithoutSystem); //#2
editOptions.setEnableLanguageInformation(true); //#3
//5.3. Switch to pagination mode instead of default float mode
editOptions.setEnablePagination(true); //#4
```

Let's describe the code above line by line.   
*Line #1* - every supported document family format has its own options class. So for all WordProcessing formats you need to apply the [WordProcessingEditOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions) . The same for other formats — [SpreadsheetEditOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/spreadsheeteditoptions) for all spreadsheet-based formats (like XLS, ODS etc.) and so on.  
*Line #2* - specifies font extraction options. By default no fonts are extracted from the document, but with this option user can specify, which fonts categories should be extracted and saved in the [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance.   
*Line #3* - enables extracting language information for better subsequent spell-checking on client side. Finally, *line #4* switches document processing mode from float (default) to the paged.

After preparing options the previously loaded document can be edited:

```java
EditableDocument beforeEdit = editor.edit(editOptions);
```

Unlike previous example let's extract HTML markup and resources separately:

```java
String originalContent = beforeEdit.getContent();
List<IHtmlResource> allResources = beforeEdit.getAllResources();
```

First string contains all HTML markup without resources, while second `List` contains all resources (images, fonts, and stylesheets).

### Modifying document content

Let's imagine that user passed HTML markup and resources, obtained from [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance, to the WYSIWYG-editor, edited the document on client-side and obtained back a modified HTML markup.  
Now user needs to create new [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance from this modified markup.

```java
String editedContent = originalContent.replace("document", "edited document");
EditableDocument afterEdit = EditableDocument.fromMarkup(editedContent, allResources);
```

We passed the same `List` with resources to the edited [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument), however it can be a completely new `List` with other resources.

## Save Word file after edit

Before saving the document user must create saving options.

```java
WordProcessingFormats docmFormat = WordProcessingFormats.Docm; //#1
WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(docmFormat); //#2
saveOptions.setPassword("password"); //#3
saveOptions.setEnablePagination(true); //#4
saveOptions.setLocale(java.util.Locale.US); //#5
saveOptions.setOptimizeMemoryUsage(true); //#6
saveOptions.setProtection(new WordProcessingProtection(WordProcessingProtectionType.ReadOnly, "write_password")); //#7
```

Let's describe a piece of code above line by line:

1. Creating a format of output document, in our case it is DOCM.
2. Creating instance of [WordProcessingSaveOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions) with previously prepared format.
3. When user specifies a password, GroupDocs.Editor encrypts the document with this password. So, when document will be saved, it can be opened only with the password.
4. Because pagination was previously enabled in [WordProcessingEditOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingeditoptions) (`editOptions` variable), for better output result it is highly recommended to enable it in [WordProcessingSaveOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions).
5. You can set locale for output WordProcessing document manually.
6. If document is really huge and causes OutOfMemoryException, you can set memory optimization option
7. You can protect document from writing (make it read-only) with password. Please note that this is write protection, and it is separate from opening protection, that was installed above.

Finally, user should save an edited document into the stream with prepared save options.

```java
OutputStream outputStream = new ByteArrayOutputStream();
editor.save(afterEdit, outputStream, saveOptions);
```

And don't forget to dispose all resources:

```java
beforeEdit.dispose();
afterEdit.dispose();
editor.dispose();
```

### Conclusion

This tutorial demonstrates basic scenario — opening document, editing it and saving, — but with detailed options on every step.
