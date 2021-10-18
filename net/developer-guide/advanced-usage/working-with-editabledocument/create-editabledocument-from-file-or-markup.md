---
id: create-editabledocument-from-file-or-markup
url: editor/net/create-editabledocument-from-file-or-markup
title: Create EditableDocument from file or markup
weight: 4
description: "This article explains how to create instance of the EditableDocument class from HTML files from disk or from HTML markup with resources using GroupDocs.Editor for .NET API."
keywords: Edit HTML document, GroupDocs.Editor
productName: GroupDocs.Editor for .NET
hideChildren: False
---
> This demonstration shows how to create instance of the EditableDocument class from HTML files from disk or from HTML markup with resources

#### Introduction

When working with [**GroupDocs.Editor**](https://products.groupdocs.com/editor/net) in usual way by loading, opening, editing and saving documents, the instances of [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) class are produced by the [Editor.Edit](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/edit/index) method and accepted by the [Editor.Save](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/save/index) method. However, in some cases it is required to create a [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) instance from existing HTML markup with optional resources. For example, some document was loaded to `Editor` class, opened for editing, and then [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) was saved to the disk as a set of HTML file and connected resources. Then this HTML document was passed to the WYSIWYG-editor, edited, and save back to the disk as modified HTML. Or raw output from the WYSIWYG-editor was saved to the `string` variable. In order to save it to some final format like DOCX or XLSX, user needs to pass the document to the [Editor.Save](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editor/methods/save/index) method in a form of [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) instance. This means that user should create an instance of [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) class manually.

[EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) contains three public static methods for creating its instances: [FromFile](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/fromfile), [FromMarkup](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/frommarkup) and [FromMarkupAndResourceFolder](https://apireference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/methods/frommarkupandresourcefolder). Before version 21.10 there also was a method [FromBodyMarkupAndResourceFolder](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/frombodymarkupandresourcefolder), but then it was removed due to its uselessness. This article reviews all of them.


#### Opening from file

Let's suppose that we have an HTML file with edited document content, that is saved on the disk. There is also a folder with resources (images, fonts, stylesheets), that are used by this document, and document has correct links to this resources. Let's say, HTML document has name "document.html" and is located in the "input" folder. Resource folder is located in the same "input" folder and has name "document\_resources". And, what is most important, HTML markup from "document.html" has proper links to files from "document\_resources" folder. For example, there will be

<link rel = "stylesheet" type = "text/css" href = "document\_resources/style.css" />

in the "document.html", and "document\_resources" folder actually contains "style.css" file.

In that case creating the [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) instance is the most simple:

```csharp
string inputHtmlPath = "C://input/document.html";
EditableDocument document = EditableDocument.FromFile(inputHtmlPath, null);
```

The [FromFile](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/fromfile) method accepts two parameters, where first is a path to HTML file, while second is a path to resource folder. If HTML file contains correct links, as it is described above, second parameter is not necessary: GroupDocs.Editor will scan the links and automatically find the resource folder and correct resources. However, if path to resource folder doesn't match to the links in HTML file, it is possible to specify a resource folder forcibly, what is illustrated in code below:

```csharp
string inputHtmlPath = "C://input/document.html";
string inputResourceFolderPath = "C://input/document_resources/";
EditableDocument document = EditableDocument.FromFile(inputHtmlPath, inputResourceFolderPath);
```

If HTML file contains a link to same resource, that is not present in the resource folder, it will be omitted.

#### Opening from markup and prepared resources

In some cases edited HTML document is not present as a file. It may be stored in database, obtained from remote storage, or something else. In such cases [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) class delivers a [FromMarkup](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/frommarkup) method. This method, like previous, has two parameters:

*   `string newHtmlContent` — string, that contains raw HTML markup. This parameter is mandatory.
*   IEnumerable<[IHtmlResource](https://apireference.groupdocs.com/net/editor/groupdocs.editor.htmlcss.resources/ihtmlresource)> resources — this is a set of prepared resources (images, fonts, stylesheets, audio files), that are used or may be used in the HTML document. User needs to prepare this collection by himself. This parameter is optional, user may pass NULL or empty collection. As with previous method, if HTML markup refers to some resource, that is not found in "resources" collection, than it will be skipped (omitted).

The code example below demonstrates this approach.

```csharp
string inputHtmlMarkup = "<HTML><HEAD><TITLE>Edited document</TITLE>.....";
JpegImage image1 = new JpegImage("image1.jpg", /* stream or base64 text*/);
PngImage image2 = new PngImage("image2.png", /* stream or base64 text*/);
CssText stylesheet = new CssText("stylesheet.css", "p {color: black; text-align: left; }......", System.Text.Encoding.UTF8);

List<IHtmlResource> allResources = new List<IHtmlResource>();
allResources.Add(image1);
allResources.Add(image2);
allResources.Add(stylesheet);

EditableDocument document = EditableDocument.FromMarkup(inputHtmlMarkup, allResources);
```

#### Opening from markup and resource folder

In some cases there is an HTML markup of edited document, but resources are represented as a set of files, located in some specific folder. For example, original document was converted to [EditableDocument](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument) and then saved to the disk as an HTML document with *.html file and a folder with resources. Then it was loaded to the WYSIWYG-editor, edited by the end-user, and edited content was obtained back from the front-end to the back-end. So now there is an edited HTML markup, available as a stream, and a resource folder. In such case a new method [FromMarkupAndResourceFolder](https://apireference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/methods/frommarkupandresourcefolder) was introduced in version 21.10. Here is an example of its usage.

```csharp
string inputHtmlMarkup = "<HTML><HEAD><TITLE>Edited document</TITLE>.....";
string inputResourceFolderPath = "C://input/document_resources/";
 
EditableDocument document = EditableDocument.FromMarkupAndResourceFolder inputHtmlMarkup, inputResourceFolderPath);
```

The [FromMarkupAndResourceFolder](https://apireference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/methods/frommarkupandresourcefolder) method obtains two parameters of a [string](https://docs.microsoft.com/en-us/dotnet/api/system.string?view=net-5.0) type, both of which are mandatory. First one — `newHtmlContent` —  is a HTML markup, while second — `resourceFolderPath` — a full valid path to the resource folder, which do exist and contains the resources. If the folder doesn't exist, an exception will be thrown.

A lot of (or even most) WYSIWYG-editors modify the HTML structure of a document by removing the HTML, HEAD, BODY elements and some others, even a whole HEAD-block with metadata. In fact, most of client-side WYSIWYG HTML-editors like TinyMCE and CKEditor are working only with inner content of BODY element: they can obtain only such markup on input and produce such markup on output. External stylesheet(s) are usually included in the HTML-editor settings or somewhere else, but not through HTML-markup (excepting the inline styles in the "style" attribute). For passing a HTML->BODY markup _into_ the HTML-editor there is a [GetBodyContent()](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/getbodycontent) method, that returns an inner content of HTML-BODY element. Problem is that when edited HTML markup is obtained back from WYSIWYG-editor, it usually doesn't contain links to the external stylesheets at all.

The [FromMarkupAndResourceFolder](https://apireference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/methods/frommarkupandresourcefolder) method is very useful in especially such cases, because it scans the resource folder, specified in a `resourceFolderPath` parameter, reads all found *.css files, and applies parsed stylesheets to the document content. This is the main distinction between this method and previously described [FromFile](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/fromfile), — this one applies all stylesheets in the folder to the document, while [FromFile](https://apireference.groupdocs.com/net/editor/groupdocs.editor/editabledocument/methods/fromfile) applies only those stylesheets, which are explicitly referenced from HTML markup.

So, concluding. When the [FromMarkupAndResourceFolder()](https://apireference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/methods/frommarkupandresourcefolder) is invoked, the GroupDocs.Editor scans a folder, specified in the `resourceFolderPath` parameter, founds all *.css files, opens them, parses their content, and if this content is valid, applies these stylesheets for the document. If any stylesheet is referencing on external images and/or fonts, GroupDocs.Editor will try to find these resources in this resource folder too. Also, if HTML markup, specified in the 1st parameter, contains external images, referenced using an IMG element, GroupDocs.Editor will try to find these images in this resource folder too. In other words, the [FromMarkupAndResourceFolder()](https://apireference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/methods/frommarkupandresourcefolder) method "assumes" that the `resourceFolderPath` contains resources for that specific HTML document, which markup is specified in the 1st parameter.


## More resources
### GitHub Examples

You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Editor for .NET examples, plugins, and showcase](https://github.com/groupdocs-editor/GroupDocs.Editor-for-.NET)   
*   [GroupDocs.Editor for Java examples, plugins, and showcase](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Java)    
*   [Document Editor for .NET MVC UI Example](https://github.com/groupdocs-editor/GroupDocs.Editor-for-.NET-MVC)     
*   [Document Editor for .NET App WebForms UI Modern Example](https://github.com/groupdocs-editor/GroupDocs.Editor-for-.NET-WebForms)    
*   [Document Editor for Java App Dropwizard UI Modern Example](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Java-Dropwizard)    
*   [Document Editor for Java Spring UI Example](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Java-Spring)
    
### Free Online App
Along with full-featured .NET library we provide simple but powerful free Apps.  
You are welcome to edit your Microsoft Word (DOC, DOCX, RTF etc.), Microsoft Excel (XLS, XLSX, CSV etc.), Open Document (ODT, OTT, ODS) and other documents with free to use online **[GroupDocs Editor App](https://products.groupdocs.app/editor)**.