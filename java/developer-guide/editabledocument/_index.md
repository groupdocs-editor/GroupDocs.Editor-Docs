---
id: editabledocument
url: editor/java/editabledocument
title: EditableDocument
weight: 100
description: "This documentation section explains features of EditableDocument class when editing document with GroupDocs.Editor for Java API."
keywords: Edit document, Editable document
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
---
[EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class represents an input document of any supportable format, that was converted to internal intermediate format in accordance to edit options and is ready for editing in HTML WYSIWYG editors. For doing this an [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) contains a plenty of methods for emitting HTML markup, stylesheets and resources with different settings.

## [HTML markup]({{< ref "editor/java/developer-guide/editabledocument/get-html-markup-in-different-forms.md" >}})

[EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class is able to emit HTML markup in different forms: whole document with header, only BODY content, with tuned external links for images, fonts and stylesheets, or even all-embedded version, where whole HTML document with all resources is presented as a single string.

## [Saving to disk]({{< ref "editor/java/developer-guide/editabledocument/save-html-to-folder.md" >}})

For some WYSIWYG editors for loading HTML content into them, it is necessary to specify this content via file path. Or user may need to save document as HTML to disk instead of passing it to the WYSIWYG editor. For such cases [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class has ability to save its content to disk as ordinary HTML document, that is represented with \*.html file and accompanying folder with resources (images, stylesheets, fonts).

## [Obtaining resources]({{< ref "editor/java/developer-guide/editabledocument/working-with-resources.md" >}})

When input document of some format is converted to [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) and then to HTML document, this HTML document contains not only HTML markup, but also one or more stylesheets, maybe images and sometimes even fonts. All of these are called *resources*. [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) has several methods and properties for working especially with them.

## [Creating new EditableDocument instances]({{< ref "editor/java/developer-guide/editabledocument/create-editabledocument-from-file-or-markup.md" >}})

Opening document for edit implies creating instances of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class, which are returned from the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor).[Edit()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) method. However, when HTML content of the document was sent to the WYSIWYG HTML-editor and edited by the end-user, in order to save it back to output format user firstly need to create new instance of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class. For achieving this [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class has three static methods, that allows to create instances from HTML documents, saved on disk (as \*.html file with resource folder), in memory (as HTML markup with resource classes), or emitted directly by WYSIWYG HTML-editor (as inner-BODY HTML markup an resource folder).
