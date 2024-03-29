---
id: edit-txt
url: editor/java/edit-txt
title: Edit TXT
linkTitle: Edit text files
weight: 60
description: "This guide demonstrates how to edit plain text files with encoding, lists recognition, pagination and other powerful features of GroupDocs.Editor for Java"
keywords: Edit TXT file, Edit plain text, Edit TXT programmatically
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit text file in the GroupDocs.Editor
        description: How to edit plain text TXT file using the GroupDocs.Editor in Java language
        productCode: editor
        productPlatform: java 
    showVideo: True
    howTo:
        name: How to edit content of the plain text (TXT) file in the GroupDocs.Editor in Java
        description: Learn how to edit content of the plain text (TXT) file using the GroupDocs.Editor in Java step by step
        steps:
        - name: Load desired text file to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired  text file into it. LoadOptions are not needed.
        - name: Prepare a TextEditOptions class
          text: Create an instance of the TextEditOptions class and adjust its properties to meet your needs if necessary. You can specify an encoding of the text document, direction of the text flow, pagination mode, and other options.
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.edit method with specifying a previously prepared TextEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the content of the text file in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited text content into the most suitable static methods of the class
        - name: Prepare a TextSaveOptions class
          text: Create an instance of the TextSaveOptions class and adjust its properties to meet your needs if necessary. You can specify an encoding of the text document, and other options. 
        - name: Save edited text file with Editor.save method
          text: Pass an instance of EditableDocument with content of the edited text file, instance of the TextSaveOptions, and a destination byte stream or file path to the Editor.save method for saving the text file.
---
> This demonstration shows how to load, open for editing and save textual documents, explains edit and save options and their destination.

## How to edit TXT file?

Textual documents are simple Plain Text flat files (TXT), that contain no images, pages, paragraphs, lists, tables and so on. However, users can create some primitive formatting like lists with leading markers, left indents with whitespaces, tables with pseudo-graphics, paragraphs with line breaks, and so on. [**GroupDocs.Editor**](https://products.groupdocs.com/editor/java) can recognize some of these structures. Other non-obvious feature, that GroupDocs.Editor provide, is ability to save edited TXT document not only back to TXT, but also to WordProcessing.

## Loading text file for edit

Unlike WordProcessing and Spreadsheet documents, DSV documents are loaded into the `Editor` class without any loading options. They are simple text files by their nature, so it is nothing to adjust:

```java
String inputTxtPath = "C://input//file.txt";
Editor editor = new Editor(inputTxtPath);
```

## Edit text file

In order to open a text document for edit by creating the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance it is required to use the [TextEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/texteditoptions) class. This class has several options (properties), that are described below:

1. [Encoding](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/texteditoptions/#getEncoding--) — allows to set character encoding of the input text document. By default, if not specified, is UTF8.
2. [RecognizeLists](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/texteditoptions/#getRecognizeLists--) — boolean flag, that indicates how exactly numbered list items are recognized. If this option is set to false (default value), lists recognition algorithm detects list paragraphs, when list numbers ends with either dot, right bracket or bullet symbols (such as "•", "\*", "-" or "o"). If this option is set to true, whitespaces are also used as list number delimiters: list recognition algorithm for Arabic style numbering (1., 1.1.2.) uses both whitespaces and dot (".") symbols.
3. [LeadingSpaces](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/texteditoptions/#getLeadingSpaces--) — enum flag, that indicates how consecutive leading spaces should be handled: convert to left indent (default value), preserve as consecutive spaces, or trim.
4. [TrailingSpaces](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/texteditoptions/#getTrailingSpaces--) — enum flag, that indicates how consecutive trailing spaces should be handled: truncated (default value), or trim.
5. [EnablePagination](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/texteditoptions/#getEnablePagination--) — boolean flag, that allows to enable paged view of the document. In their nature all text documents are pageless, however, GroupDocs.Editor allows to split them on pages, like MS Word does.
6. Direction — enum flag, which allows to specify the direction of text flow in the input plain text document. By default is Left-to-Right.

Source code below demonstrates using this option class and opening input text document for editing. Then, when [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance is ready, example below demonstrates, how to emit HTML markup from it, edit it and create new [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance from edited content:

```java
TextEditOptions editOptions = new TextEditOptions();
editOptions.setEncoding(StandardCharsets.UTF_8);
editOptions.setRecognizeLists(true);
editOptions.setLeadingSpaces(TextLeadingSpacesOptions.ConvertToIndent);
editOptions.setTrailingSpaces(TextTrailingSpacesOptions.Trim);
editOptions.setDirection(TextDirection.Auto);

EditableDocument beforeEdit = editor.edit(editOptions); // Create EditableDocument instance

String originalTextContent = beforeEdit.getContent(); // Get HTML content
String updatedTextContent = originalTextContent.replace("text", "EDITED text"); // Edit content
List<IHtmlResource> allResources = beforeEdit.getAllResources(); // Get resources (only one stylesheet actually in this case)

//Finally, create new EditableDocument
EditableDocument afterEdit = EditableDocument.fromMarkup(updatedTextContent, allResources);
```

## Save text file after edit

After being edited, text document can be saved back as TXT or as WordProcessing. For saving back to TXT format user must use a [TextSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/textsaveoptions) class, that have three properties, described below:

1. [Encoding](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/textsaveoptions/#getEncoding--) — character encoding of the text document, which will be applied for its saving. By default, if not specified, is UTF8.
2. [AddBidiMarks](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/textsaveoptions/#getAddBidiMarks--) — boolean flag, that determines whether to add bi-directional marks before each BiDi run when saving in plain text format.
3. [PreserveTableLayout](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/textsaveoptions/#getAddBidiMarks--) — boolean flag, that specifies whether the GroupDocs.Editor should try to preserve layout of tables when saving in the plain text format. The default value is false.

Source code below shows how to save  [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) to both TXt and WordProcessing formats:

```java
TextSaveOptions txtSaveOptions = new TextSaveOptions();
txtSaveOptions.setAddBidiMarks(true);
txtSaveOptions.setPreserveTableLayout(true);

WordProcessingSaveOptions wordSaveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Docm);

String outputTxtPath = "C:\\output\\document.txt";
String outputWordPath = "C:\\output\\document.docm";

editor.save(afterEdit, outputTxtPath, txtSaveOptions);
editor.save(afterEdit, outputWordPath, wordSaveOptions);
```

As a result, after running this example user will have two versions of the edited document: in TXT and DOCM formats.
