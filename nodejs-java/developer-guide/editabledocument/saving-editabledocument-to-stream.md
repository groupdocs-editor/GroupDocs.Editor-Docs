---

id: saving-editabledocument-to-stream
url: editor/nodejs-java/saving-editabledocument-to-stream
title: Saving EditableDocument to Stream
weight: 5
description: "This article shows and explains advanced techniques and approaches while working with EditableDocument at an advanced level — saving to stream with resource callback, saving resources separately from HTML markup, and saving HTML markup with adjustable resource links."
keywords: Save EditableDocument to stream, Save HTML document to stream, GroupDocs.Editor
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
---

> This article shows and explains advanced techniques and approaches while working with `EditableDocument` at an advanced level — saving to stream with resource callback, saving resources separately from HTML markup, and saving HTML markup with adjustable resource links.

**Note:** The information explained in this article is applicable for GroupDocs.Editor for Node.js via Java **version 24.4** and higher.

## Introduction

In the latest versions of GroupDocs.Editor for Node.js via Java, especially in **version 24.4**, the [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/editabledocument/) class, which is one of the most important among all types in GroupDocs.Editor, was significantly expanded and improved. These new features are focused on saving the `EditableDocument` to the HTML format into a stream using different ways and allowing you to specify a resource template string. This article covers all of them.

## Saving HTML Markup to Stream Only

Starting from **version 23.9**, the `EditableDocument` class contains a new instance method `getContent()`, which allows you to get the HTML markup of the document as a string. Its signature is:

```javascript
getContent()
```

This method internally converts the content of the `EditableDocument` instance to HTML markup, where CSS stylesheets and images are not stored inside the markup. These external resources (CSS and images) are referenced through links (URIs) by specifying their filenames. So, for example, if one HTML document has one stylesheet and two images with filenames "foo.jpeg" and "bar.png", they will be referenced in the HTML markup as:

```html
<link href="stylesheet.css" rel="stylesheet" />
...
<img src="foo.jpeg" />
...
<img src="bar.png" />
```

With this method, there is no option to specify or adjust the references to the external resources. Also, this method is useful for saving the HTML markup only, but not the external resources.

To deal with these issues, the following approach was introduced.

## Saving the Whole HTML Document to Stream with Resource Callback

Starting from **version 24.4**, a new `save()` method overload was added:

```javascript
save(htmlMarkup, saveOptions)
```

This method enables the most advanced, detailed, and adjustable saving of the `EditableDocument` instance to the HTML format.

- **`htmlMarkup`**: The first parameter is an output parameter into which the HTML markup will be written. You must specify any valid writable stream, such as `fs.createWriteStream()` or any other writable stream. It cannot be `null`.

- **`saveOptions`**: The second parameter is an instance of the new public class `HtmlSaveOptions`, which is described below. This argument also cannot be `null`.

### HtmlSaveOptions Class

The `HtmlSaveOptions` class contains four properties, only one of which is mandatory and cannot be `null`.

- **`htmlTagCase`**: Controls how the HTML tag names will be presented in HTML markup: All lower case (default value), All upper case, or First letter upper case. Its value is a special enum `TagRenderingCase`.

- **`attributeValueDelimiter`**: Controls which delimiter around the attribute values in HTML elements will be used: single quote (default value) or double quote. Its value is a special value type `QuoteType`.

- **`embedStylesheetsIntoMarkup`**: A boolean flag that controls where to store the CSS stylesheet(s): as external resources (`false`), or embed them into the HTML markup, inside the `<style>` element in the `<html><head>` section (`true`). By default, it is `false` — do not embed inside the HTML markup.

- **`savingCallback`**: The most important and the only mandatory property that must be specified by the user and cannot be `null`. It is a reference to the user-defined resource callback, which implements the `IHtmlSavingCallback` interface.

### Implementing IHtmlSavingCallback Interface

When the `save()` method is called, GroupDocs.Editor scans the content of the `EditableDocument` instance and gathers all resources: images, stylesheets, audio, etc. For each of these resources, it calls the `saveOneResource(resource)` method, which is defined in the `IHtmlSavingCallback` interface and implemented by you. When calling this method, GroupDocs.Editor passes the resource itself as a type that implements the `IHtmlResource` interface — it will be `JpegImage` for JPEG images, `CssText` for CSS stylesheets, `Mp3Audio` for MP3 audio files, and so on.

When you obtain such a resource instance in the `saveOneResource` method, you can do with it what you want: save it to disk, to a database, convert it somehow, or even do nothing. Importantly, you **must** return a link to this resource through the `return` value. So when GroupDocs.Editor starts to form the HTML markup to write it to the `htmlMarkup` argument, it will use the user-provided links to the resources in it.

### Code Example

The complete code sample below demonstrates creating an `EditableDocument` instance by opening an arbitrary DOCX file for editing, then creating a custom implementation of the `IHtmlSavingCallback`, which saves the provided resources to a database or elsewhere, and saving the content of the `EditableDocument` in the HTML format.

```javascript
// Import necessary modules
const groupdocsEditor = require('groupdocs-editor');
const fs = require('fs');

// Define an input document
const inputDocx = 'Path/SampleDoc.docx';

// Create an instance of the custom SavingCallback
class SavingCallback {
    saveOneResource(resource) {
        // Save this stream to the DB or to the disk somehow
        const resourceStream = resource.getByteContent();

        // Form and return a reference to the resource depending on resource type
        if (resource instanceof groupdocsEditor.CssText) {
            return `https://mywebsite.web/stylesheet/${resource.getFilenameWithExtension()}`;
        } else {
            return `https://mywebsite.web/GetImage/name=${resource.getFilenameWithExtension()}`;
        }
    }
}

const callback = new SavingCallback();

// Create HTML saving options
const saveOptions = new groupdocsEditor.HtmlSaveOptions();
saveOptions.setEmbedStylesheetsIntoMarkup(false); // Do not embed CSS into markup
saveOptions.setAttributeValueDelimiter(groupdocsEditor.QuoteType.DoubleQuote.getCode());
saveOptions.setHtmlTagCase(groupdocsEditor.TagRenderingCase.UpperCase);
saveOptions.setSavingCallback(callback); // Specify our custom callback

// Create an Editor class instance - load options are not necessary for DOCX and are default here
const editor = new groupdocsEditor.Editor(inputDocx);

// Create an EditableDocument instance - edit options are not specified and thus are default
const editableDocument = editor.edit();

// Define a writable stream into which the HTML markup will be written
const outputHtmlPath = 'Path/output.html';
const htmlStream = fs.createWriteStream(outputHtmlPath);

// Save the EditableDocument to the stream with the specified save options
editableDocument.save(htmlStream, saveOptions);

// Close the stream
htmlStream.end();

// Dispose of resources
editableDocument.dispose();
editor.dispose();
```

After executing this code, the `output.html` file will contain HTML markup where the references to the external resources will be like:

```html
<link href="https://mywebsite.web/stylesheet/stylesheet.css" rel="stylesheet"/>
...
<img src="https://mywebsite.web/GetImage/name=foo.jpeg"/>
...
<img src="https://mywebsite.web/GetImage/name=bar.png"/>
```

## Saving HTML Markup with Template Strings for Resource Links

Sometimes it is necessary to save only the markup and not save the resources. In such cases, the approach described above may be too redundant. The `EditableDocument` class from its beginning contains methods `getContent()` and `getBodyContent()`. These two methods have two overloads, but in general, the first one returns the whole HTML markup of the document, while the second — only the inner content of the `<body>` element, without the `<body>` opening and closing tags.

Before version 24.4 was released, these two methods allowed you to specify the external resource prefixes, so when generating the HTML markup, these prefixes are added to resource names. For example, if the document has two images, let's say, "foo.jpeg" and "bar.png", then, by calling:

```javascript
const bodyContent = editableDocument.getBodyContent("https://mywebsite.web/image?name=");
```

The output markup will contain two `<img>` elements with the following content:

```html
<img src="https://mywebsite.web/image?name=foo.jpeg"/>
...
<img src="https://mywebsite.web/image?name=bar.png"/>
```

However, this approach had two main flaws:

- It was not possible to insert the resource name *inside* the request URI, only at the end.
- It was not possible to insert the resource name more than once in a single request URI.

GroupDocs.Editor version 24.4 fixes both these issues. The method signatures are intact, but now they treat the specified strings not as prefixes but as *template format strings*. The protocol is as follows:

1. If the specified template string is `null` or empty, then the pure resource name will be placed in the HTML markup.
2. If the specified template string contains one unique valid placeholder `{0}`, which occurs one or more times, the resource name will replace the placeholder in the HTML markup.
3. Otherwise, if the specified template string contains more than one unique placeholder, or contains no placeholder at all, it will be treated as a prefix string.

Due to this protocol, old user code will work the same as in previous versions because the template strings do not have placeholders and thus are treated as prefixes.

But when using the template string, now, when calling:

```javascript
const bodyContent = editableDocument.getBodyContent("https://mywebsite.web/image?name={0}&stamp=456789&token={0}&expireIn=123987");
```

The image named "foo.jpeg" will be presented in the output markup as:

```html
<img src="https://mywebsite.web/image?name=foo.jpeg&stamp=456789&token=foo.jpeg&expireIn=123987"/>
```

So this new mechanism allows you to specify the request URIs for the external images and stylesheets very precisely.

---

By following this guide, you can effectively save the `EditableDocument` to a stream with advanced options, control how resources are handled, and adjust resource links using templates in GroupDocs.Editor for Node.js via Java.

**Note:** Be sure to replace the paths in the code examples with the actual paths used in your application.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.
