---
id: editabledocument
url: editor/python-net/editabledocument
title: EditableDocument
linkTitle: EditableDocument
weight: 100
description: "This documentation section explains features of the EditableDocument class when editing a document with GroupDocs.Editor for Python via .NET API."
keywords: Edit document, Editable document, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
---
The [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class represents an input document of any supportable format, that was converted to an internal intermediate format in accordance to edit options and is ready for editing in HTML WYSIWYG editors. For doing this an [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) contains plenty of methods for emitting HTML markup, stylesheets and resources with different settings.

## [HTML markup]({{< ref "editor/python-net/developer-guide/editabledocument/get-html-markup-in-different-forms.md" >}})

The [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class is able to emit HTML markup in different forms: the whole document with header, only BODY content, with tuned external links for images, fonts and stylesheets, or even an all-embedded version, where the whole HTML document with all resources is presented as a single string.

## [Saving to disk]({{< ref "editor/python-net/developer-guide/editabledocument/save-html-to-folder.md" >}})

For some WYSIWYG editors, in order to load HTML content into them, it is necessary to specify this content via a file path. Or the user may need to save a document as HTML to disk instead of passing it to the WYSIWYG editor. For such cases the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class has the ability to save its content to disk as an ordinary HTML document, that is represented with an \*.html file and an accompanying folder with resources (images, stylesheets, fonts).

## [Obtaining resources]({{< ref "editor/python-net/developer-guide/editabledocument/working-with-resources.md" >}})

When an input document of some format is converted to an [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) and then to an HTML document, this HTML document contains not only HTML markup, but also one or more stylesheets, maybe images and sometimes even fonts. All of these are called *resources*. [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) has several methods and properties for working especially with them.

## [Creating new EditableDocument instances]({{< ref "editor/python-net/developer-guide/editabledocument/create-editabledocument-from-file-or-markup.md" >}})

Opening a document for edit implies creating instances of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class, which are returned from the [Editor.edit()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method. However, when the HTML content of the document was sent to the WYSIWYG HTML-editor and edited by the end-user, in order to save it back to an output format the user firstly needs to create a new instance of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class. For achieving this the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class has three class methods, that allow to create instances from HTML documents saved on disk (as an \*.html file with a resource folder), in memory (as HTML markup with resources), or emitted directly by a WYSIWYG HTML-editor (as inner-BODY HTML markup and a resource folder).
