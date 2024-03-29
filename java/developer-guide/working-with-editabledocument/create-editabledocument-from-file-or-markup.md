---
id: create-editabledocument-from-file-or-markup
url: editor/java/create-editabledocument-from-file-or-markup
title: Create EditableDocument from file or markup
weight: 4
description: "This article explains how to create instance of the EditableDocument class from HTML files from disk or from HTML markup with resources using GroupDocs.Editor for Java API."
keywords: Edit HTML document, GroupDocs.Editor
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
> This demonstration shows how to create instance of the EditableDocument class from HTML files from disk or from HTML markup with resources

## Introduction

When working with [**GroupDocs.Editor**](https://products.groupdocs.com/editor/java) in usual way by loading, opening, editing and saving documents, the instances of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class are produced by the [Editor.edit()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor#edit--) method and accepted by the [Editor.save()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor#save-com.groupdocs.editor.EditableDocument-java.io.OutputStream-com.groupdocs.editor.options.ISaveOptions-) method. However, in some cases it is required to create a [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance from existing HTML markup with optional resources. For example, some document was loaded to `Editor` class, opened for editing, and then [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) was saved to the disk as a set of HTML file and connected resources. Then this HTML document was passed to the WYSIWYG-editor, edited, and save back to the disk as modified HTML. Or raw output from the WYSIWYG-editor was saved to the `String` variable. In order to save it to some final format like DOCX or XLSX, user needs to pass the document to the [Editor.save()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor#save-com.groupdocs.editor.EditableDocument-java.io.OutputStream-com.groupdocs.editor.options.ISaveOptions-) method in a form of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance. This means that user should create an instance of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class manually.

[EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) contains three public static methods for creating its instances: [fromFile()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromFile-java.lang.String-java.lang.String-), [fromMarkup()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromMarkup-java.lang.String-java.util.List-) and [fromBodyMarkupAndResourceFolder()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromMarkupAndResourceFolder-java.lang.String-java.lang.String-). This article reviews all of them.

## Opening from file

Let's suppose that we have an HTML file with edited document content, that is saved on the disk. There is also a folder with resources (images, fonts, stylesheets), that are used by this document, and document has correct links to this resources. Let's say, HTML document has name "document.html" and is located in the "input" folder. Resource folder is located in the same "input" folder and has name "document\_resources". And, what is most important, HTML markup from "document.html" has proper links to files from "document\_resources" folder. For example, there will be

<link rel = "stylesheet" type = "text/css" href = "document\_resources/style.css" />

in the "document.html", and "document\_resources" folder actually contains "style.css" file.

In that case creating the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance is the most simple:

```java
String inputHtmlPath = "C://input//document.html";
EditableDocument document = EditableDocument.fromFile(inputHtmlPath, null);
```

The [fromFile()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromFile-java.lang.String-java.lang.String-) method accepts two parameters, where first is a path to HTML file, while second is a path to resource folder. If HTML file contains correct links, as it is described above, second parameter is not necessary: GroupDocs.Editor will scan the links and automatically find the resource folder and correct resources. However, if path to resource folder doesn't match to the links in HTML file, it is possible to specify a resource folder forcibly, what is illustrated in code below:

```java
String inputHtmlPath = "C://input//document.html";
String inputResourceFolderPath = "C://input//document_resources/";
EditableDocument document = EditableDocument.fromFile(inputHtmlPath, inputResourceFolderPath);
```

If HTML file contains a link to same resource, that is not present in the resource folder, it will be omitted.

## Opening from markup

In some cases edited HTML document is not present as a file. It may be stored in database, obtained from remote storage, or something else. In such cases [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class delivers a [fromMarkup()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromMarkup-java.lang.String-java.util.List-) method. This method, like previous, has two parameters:

* `String newHtmlContent` — string, that contains raw HTML markup. This parameter is mandatory.
* IEnumerable<[IHtmlResource](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources/ihtmlresource)> resources — this is a set of prepared resources (images, fonts, stylesheets), that are used or may be used in the HTML document. User needs to prepare this collection by himself. This parameter is optional, user may pass NULL or empty collection. As with previous method, if HTML markup refers to some resource, that is not found in "resources" collection, that it will be skipped (omitted).

The code example below demonstrates this approach.

```java
String inputHtmlMarkup = "<HTML><HEAD><TITLE>Edited document</TITLE>.....";
JpegImage image1 = new JpegImage("image1.jpg", /* stream or base64 text*/);
PngImage image2 = new PngImage("image2.png", /* stream or base64 text*/);
CssText stylesheet = new CssText("stylesheet.css", "p {color: black; text-align: left; }......", StandardCharsets.UTF_8);

List<IHtmlResource> allResources = new ArrayList<IHtmlResource>();
allResources.add(image1);
allResources.add(image2);
allResources.add(stylesheet);

EditableDocument document = EditableDocument.fromMarkup(inputHtmlMarkup, allResources);
```

## Opening from inner-BODY HTML content and folder with resources

All previously described methods of creating instances of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class assume that both HTML file or string with HTML markup contains a valid, well-formed HTML document in all according to all W3C requirements. Such document should contain an HTML Document Definition (DOCTYPE) and a root HTML element, that, in turn, has two and only two children: HEAD (with document meta-information) and a BODY (with document content). All stylesheets are included and/or embedded in the HEAD element (LINK and/or STYLE elements respectively), and are 'used by' content markup (by using 'class' and 'id' attributes, in most cases).

However, most of client-side WYSIWYG HTML-editors like TinyMCE and CKEditor are working only with inner content of BODY element: they can obtain only such markup on input and produce such markup on output. External stylesheet(s) are usually included in the HTML-editor settings or somewhere else, but not through HTML-markup (excepting the inline styles in the "style" attribute). For passing a HTML->BODY markup *into* the HTML-editor there is a [getBodyContent()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#getBodyContent--) method, that returns an inner content of HTML-BODY element. And, in counterpart, for obtaining a HTML markup *from* HTML-editor and creating a new [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance with this markup the new method was introduced in the 19.12 version: [fromBodyMarkupAndResourceFolder()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromMarkupAndResourceFolder-java.lang.String-java.lang.String-). Like [fromFile()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromFile-java.lang.String-java.lang.String-) and [fromMarkup()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromMarkup-java.lang.String-java.util.List-), this is also a static method, that serves as a factory for creating the [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instances. It obtains two mandatory parameters:

* `String htmlBodyContent`— string, that contains raw HTML markup, that is located inside BODY element (between <BODY> and </BODY> tags, without this pair of opening/closing tags itself). Of course, this should be a valid markup.
* `String resourceFolderPath` — string, that contains a full path to the folder with all external resources for the opening document. If the folder doesn't exist, an exception will be thrown.

Because inner-BODY markup doesn't contain stylesheets (neither included not embedded), these stylesheets should be referenced by some another way. For obtaining them a second parameter exists; user should specify a full path to the folder, that contains an images, fonts and stylesheets, used by the HTML document, specified in the 1st parameter. GroupDocs.Editor scans this folder, founds \*.css files, opens them, parses their content, and if this content is valid, applies these stylesheets for the document. If stylesheet references on external images and/or fonts, GroupDocs.Editor will try to find these resources in this resource folder too. Also, if HTML markup, specified in the 1st parameter, contains external images, referenced using an IMG element, GroupDocs.Editor will try to find these images in this resource folder too. In other words, the [fromBodyMarkupAndResourceFolder()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#fromMarkupAndResourceFolder-java.lang.String-java.lang.String-)  method "assumes" that contains resources for that specific HTML document, which inner-BODY content was specified in the 1st parameter.
