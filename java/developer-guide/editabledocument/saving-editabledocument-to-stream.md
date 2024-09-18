---
id: saving-editabledocument-to-stream
url: editor/java/saving-editabledocument-to-stream
title: Saving EditableDocument to stream
weight: 5
description: "This article shows and explains the advances techniques and approaches while working with EditableDocument in advanced level — saving to stream with resource callback, saving resources separately from HTML markup, and saving HTML markup with adjustable resource links."
keywords: Save EditableDocument to stream, Save HTML document to stream, GroupDocs.Editor
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
> This article shows and explains the advances techniques and approaches while working with EditableDocument in advanced level — saving to stream with resource callback, saving resources separately from HTML markup, and saving HTML markup with adjustable resource links.

{{< alert style="info" >}}Information, explained in this article, is applicable for the GroupDocs.Editor for Java [version 24.4](https://releases.groupdocs.com/editor/java/release-notes/2024/groupdocs-editor-for-java-24-4-release-notes/) and higher.{{< /alert >}}

## Introduction

In the latest versions of the GroupDocs.Editor for Java, especially in the [version 24.4](https://releases.groupdocs.com/editor/java/release-notes/2024/groupdocs-editor-for-java-24-4-release-notes/), the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) class, which is one of the most important among all types in the GroupDocs.Editor, was significantly expanded and improved. These new features are focused on saving the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) to the HTML format into the stream using different ways, and allowing to specify a resource template string. This article covers all of them.

## Saving to stream the HTML markup only

Starting from the [version 23.9](https://releases.groupdocs.com/editor/java/release-notes/2023/groupdocs-editor-for-java-23-9-release-notes/), the `EditableDocument` class contains a new instane method [`getContent`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/#getContent--), which allows to save an HTML marjup of the HTML document to the specified stream with specified encoding. Its signature is below:

```java
public String getContent()
```

This method internally converts the content of the `EditableDocument` instance to the HTML markup, where CSS stylesheets and images are not stored inside the markup. These external resources (CSS and images) are referenced through the links (URIs) by specifying their filenames. So, for example, if one HTML document has one stylesheet and two images with filenames "foo.jpeg" and "bar.png", they will be referenced in the HTML markup by the next:

```html
<link href="stylesheet.css" rel="stylesheet" />
.....
<IMG src="foo.jpeg" />
.....
<IMG src="bar.png" />
```

With this method, there is no option to specify or adjust the reference to the external resources somehow. Also, this method is useful for saving the HTML markup only, but not the external resources.

For dealing with these issues the next way was introduced.

## Saving whole HTML document to stream with resource callback

Starting from the [version 24.4](https://releases.groupdocs.com/editor/java/release-notes/2024/groupdocs-editor-for-java-24-4-release-notes/) a new [`save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/#save-java.io.Writer-com.groupdocs.editor.options.HtmlSaveOptions-) method overload was added:

```java
void save(Writer htmlMarkup, HtmlSaveOptions saveOptions)
```

This method enables the most advanced, detailed and adjustable saving of the `EditableDocument` instance to the HTML format.

First parameter is an output parameter, into which the HTML markup will be written. User must specify any valid implementation of the `java.io.Writer` abstract class, usually `java.io.Writer` or `java.io.FileWriter`. It cannot be `null`.

Second parameter is the new public class [`HtmlSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/htmlsaveoptions/), which is described below. This argument also canot be `null`.

Class [`HtmlSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/htmlsaveoptions/) contains 4 properties, only one of them is mandatory and cannot be null.

- [`HtmlTagCase`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/htmlsaveoptions/#getHtmlTagCase--) property controls how the HTML tag names will be present in HTML markup: All lower case (default value), All upper case, or First letter upper case. Its value is a special enum - [`TagRenderingCase`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.serialization/tagrenderingcase/).
- [`AttributeValueDelimiter`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/htmlsaveoptions/#getAttributeValueDelimiter--) property controls which delimiter around the attribute values in HTML elements will be used: single quote (default value) or double quote. Its value is a special value type - [`QuoteType`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.serialization/quotetype/).
- [`EmbedStylesheetsIntoMarkup`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/htmlsaveoptions/#getEmbedStylesheetsIntoMarkup--) property is a boolean flag, which controls where to store the CSS stylesheet(s): as external resources (`false`), or embed them into the HTML markup, inside the `STYLE` element in the `HTML`->`HEAD` section (`true`). By default is `false` — do not embed inside the HTML markup.
- [`SavingCallback`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/htmlsaveoptions/#getSavingCallback--) is the most important and the only mandatory property, that myust be specified by user and cannot be `null`. It is a reference to the user-defined resource callback, which implements an interface [`IHtmlSavingCallback`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ihtmlsavingcallback/).

When the [`save`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/#save-java.lang.String-) method is called, the GroupDocs.Editor scans the content of `EditableDocument` instance and gathers all resources: images, stylesheets, audio, etc. For each of these resources it calls the [`saveOneResource`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ihtmlsavingcallback/#saveOneResource-com.groupdocs.editor.htmlcss.resources.IHtmlResource-) method, which is defined in the [`IHtmlSavingCallback`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/ihtmlsavingcallback/) interface and implemented by the user. When calling this method, the GroupDocs.Editor passes the resource itself as a type, which implements a [`IHtmlResource`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources/ihtmlresource/) interface — it will be [`JpegImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.raster/jpegimage/) for JPEG images, [`CssText`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.textual/csstext/) for CSS stylesheets, [`Mp3Audio`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.audio/mp3audio/) for MP3 audio files and so on.

When user obtains such a resource instance in the `saveOneResource` method, he can do with it what he wants: save to the disk, to the database, convert it somehow, or even do nothing. What is important, the user **must** return a link to this resource through the `return` value. So when the GroupDocs.Editor will start to form an HTML markup in order to write it to the `java.io.Writer htmlMarkup` argument, it will use the user-provided links to the resources in it.

The complete code sample below demonstrates the creating of `EditableDocument` instance by opening an arbitrary DOCX file for edit, then creationg the custom implementation of the `IHtmlSavingCallback`, which saves the provided resources to the database, and saving the content of the `EditableDocument` in the HTML format.

```java
public void SavingToStreamPublicExample()
{
	//Define an input document
	String inputDocx = "Path\\SampleDoc.docx";

	//Create and instance of custom SavingCallback
	SavingCallback callback = new SavingCallback();

	//Create HTML saving options
	HtmlSaveOptions saveOptions = new HtmlSaveOptions();
	saveOptions.setEmbedStylesheetsIntoMarkup(false);//Do not embed CSS into markup
	saveOptions.setAttributeValueDelimiter(QuoteType.DoubleQuote.getCode());
	saveOptions.setHtmlTagCase(TagRenderingCase.UpperCase);
	saveOptions.setSavingCallback(callback);//Specify our custom callback

	//Create an Editor class instance - load options are not necessary for DOCX and are default here
	Editor editor = new Editor(inputDocx);
	{
		//Create an EditableDocument instance - edit options are not specified and thus are default
		using (EditableDocument document = editor.edit())
		{
			//Define a StringWriter, into which the HTML markup will be written
			StringWriter writer = new StringWriter;
			document.save(writer, saveOptions);
		}
	}
}

	private final static class SavingCallback implements IHtmlSavingCallback
	{
		public final String saveOneResource(IHtmlResource resource)
		{
			//save this stream to the DB or to the disk somehow
			InputStream resourceStream = resource.getByteContent();

			//form and return a reference to the resource depending on resource type
			if (Operators.is(resource, CssText.class))
			{
				return StringExtensions.format("https://mywebsite.web/stylesheet/{0}", resource.getFilenameWithExtension());
			}
			else
			{
				return StringExtensions.format("https://mywebsite.web/GetImage/name={0}", resource.getFilenameWithExtension());
			}
		}
	}
```

After executing this code, the `markup` variable of `StringWriter` type will contain an HTML markup, where the references to the external resources will be like:

```html
<link href="https://mywebsite.web/stylesheet/stylesheet.css" rel="stylesheet"/>
.....
<IMG src="https://mywebsite.web/GetImage/name=foo.jpeg"/>
.....
<IMG src="https://mywebsite.web/GetImage/name=bar.png"/>
```

## Saving HTML markup with template strings for resource links

Sometimes it is necessary to save only markup, and do not save the resources. In such cases the approach, described above, may be treates as too redundant. The `EditableDocument` class from its beginning contains methods `getContent()` and `getBodyContent()`. These two methods have two overloads, but in general the first one returns the whole HTML markup of the document, while the second — only the inner content of the HTML->BODY element, without the BODY opening and closing tags.

Before the version 24.4 was released, these two methods allowed to specify the external resource prefixes, so when generating the HTML markup, these prefixes are added to resource names. For example, if the document has two images, let's say, "foo.jpeg" and "bar.png", then, by calling the `EditableDocument.getBodyContent("https://mywebsite.web/image?name=")`, the output markup will contain two IMG elements with the next content:

```html
<IMG src="https://mywebsite.web/image?name=foo.jpeg"/>
.....
<IMG src="https://mywebsite.web/image?name=bar.png"/>
```

However, this state of things had two main flaws:
- It was not possible to insert the resource name *inside* the request URI, only at the end.
- It was not possible to insert the resource name more than once in the single request URI.

GroupDocs.Editor version 24.4 fixes both these issues. The methods signatures are intact, but now they treat the specified strings not as the prefixes, but as the *template format strings*. Protocol is the next:

1. If specified template string is null or empty, then pure resource name will be placed in the HTML markup.
2. If specified template string contains one unique valid placeholder, which occurs one or more times, the resource name will replace the placeholder in the HTML markup.
3. Otherwise, if specified template string contains more than one unique placeholder, or contains no placeholder at all, it will be treated as a prefix string.

Due to this protocol the old-written user code will work the same as for the previous versions, because the template strings do not have the placeholders and thus are treated as prefixes.

But when using the template string, now, when calling the `EditableDocument.getBodyContent("https://mywebsite.web/image?name={0}&stamp=456789&token={0}&expireIn=123987")` method, the image named "foo.jpeg" will be presented in the output markup as the next:

```html
<IMG src="https://mywebsite.web/image?name=foo.jpeg&stamp=456789&token=foo.jpeg&expireIn=123987"/>
```

So this new mechanism allows to specify the request URIs for the external images and stylesheets very precisely.

