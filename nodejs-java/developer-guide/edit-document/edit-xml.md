---

id: how-to-edit-xml
url: editor/nodejs-java/how-to-edit-xml
title: How to Edit XML File
weight: 54
description: "This article demonstrates how to edit XML files and XML documents using Node.js via Java."
keywords: GroupDocs.Editor XML support, XML format, editing XML files, edit XML documents
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to Edit XML File using GroupDocs.Editor
        description: How to edit XML files using GroupDocs.Editor in Node.js via Java
        productCode: editor
        productPlatform: nodejs-java
    showVideo: True
    howTo:
        name: How to Edit XML Files using GroupDocs.Editor in Node.js via Java
        description: Learn how to edit XML files using GroupDocs.Editor in Node.js via Java step by step
        steps:
        - name: Load the desired XML file into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, passing the desired XML file into it by a file path or a stream.
        - name: Prepare an XmlEditOptions instance
          text: Create an instance of the XmlEditOptions class and adjust its properties to meet your needs if necessary. You can specify options like fixing incorrect structure, recognizing URIs, emails, etc.
        - name: Call editor.edit() and send the obtained EditableDocument to the WYSIWYG editor
          text: Invoke the editor.edit() method with the specified XmlEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML markup and extract resources from this instance using corresponding instance methods, and pass all this data to the HTML-based WYSIWYG editor.
        - name: Edit the document in the WYSIWYG editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG editor, which is running on the client-side (in a web browser), and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class.
        - name: Save the edited document
          text: Use the EditableDocument instance to save the edited content back to an XML file or another desired format.
---

> This example demonstrates how to open, edit, and save XML documents using different options and adjustments in Node.js via Java.

## Introduction

GroupDocs.Editor for Node.js via Java has supported importing documents in the [XML (eXtensible Markup Language)](https://docs.fileformat.com/web/xml/) format for a long time. However, in [version 24.6](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2024/groupdocs-editor-for-node-js-via-java-24-6-release-notes/), the XML processing mechanism was completely redeveloped and drastically enhanced. XML editing public options were also redesigned and significantly expanded, with new sub-option classes and new public types.

This article describes this new XML processing mechanism and is applicable to GroupDocs.Editor for Node.js via Java [version 24.6](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2024/groupdocs-editor-for-node-js-via-java-24-6-release-notes/) and above.

## Loading XML Documents

Loading XML documents into the `Editor` class is straightforward and similar to other formats. There are no dedicated load options for the XML format; you just need to specify the file itself through a file path or an input stream.

If loading through a file path, the file extension does not matter, so you may freely load an XML file not only with the `*.xml` extension but with any other extension like `*.csproj`, `*.svg`, or any other extension—as long as it has a valid internal structure.

Also, please note that you cannot treat HTML files like XML—only [XHTML](https://en.wikipedia.org/wiki/XHTML) can be treated as valid XML.

The code example below shows loading the same document by two approaches into two different `Editor` instances:

```javascript
// Import necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

// Specify the input file path
const inputFilePath = 'path/to/sample.xml';

// Load from file path
const editorFromPath = new groupdocsEditor.Editor(inputFilePath);

// Load from stream
const xmlStream = fs.createReadStream(inputFilePath);
const editorFromStream = new groupdocsEditor.Editor(xmlStream);

// Now you can work with both editor instances
// ...

// Don't forget to dispose of the editors when done
editorFromPath.dispose();
editorFromStream.dispose();
```

## Editing XML Documents

Like for other format families in GroupDocs.Editor, there is a special `XmlEditOptions` class for editing XML documents. As always, it is not mandatory when editing a document; you can use the `editor.edit()` method without parameters—GroupDocs.Editor will automatically detect the format and apply the default options. The example below shows such a case: an XML document is loaded, edited, and then the edited content, represented with an `EditableDocument` class, may be passed to the WYSIWYG editor or any other HTML editing software, or simply saved to disk, as shown in the example.

```javascript
// Import necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

// Specify the input and output file paths
const xmlInputPath = 'path/to/sample.xml';
const outputPath = 'path/to/output.html';

// Create an Editor instance
const editor = new groupdocsEditor.Editor(xmlInputPath);

// Open the document for editing
const editableDocument = editor.edit();

// Get the HTML content (you can send this to a WYSIWYG editor)
const htmlContent = editableDocument.getContent();

// Optionally, save the HTML content to a file
fs.writeFileSync(outputPath, htmlContent);

// Dispose of resources
editableDocument.dispose();
editor.dispose();
```

The `XmlEditOptions` class has various properties, some of which are grouped into "wrappers"—special classes `XmlHighlightOptions` and `XmlFormatOptions`, which are described in detail below. The most useful and important properties, however, are directly inside the `XmlEditOptions` class.

### Encoding

`encoding`

This property allows you to set the encoding that will be applied while opening an input XML file (keep in mind that any XML is first of all a text file). By default, all XML files are UTF-8, so the default value of this option is also UTF-8.

### FixIncorrectStructure

`fixIncorrectStructure`

GroupDocs.Editor can handle any XML document without error or exception: corrupted, truncated, with invalid structure—it can even treat HTML like XML. However, the representation of such a document may degrade in such cases—for example, it is impossible to properly display the nested hierarchical structure if it is broken.

To fix this, the `fixIncorrectStructure` boolean flag is introduced. If enabled, GroupDocs.Editor scans the XML document and tries to fix its structure. In particular, it escapes some prohibited characters, properly closes unclosed tags, opens unopened tags, fixes overlapping tags, and so on.

Because such document scanning and fixing requires additional computational resources and, in general, most XML documents are valid, this mechanism is disabled by default (`false`). So, to enable it, you must set it to `true` manually.

### RecognizeUris

`recognizeUris`

This property enables the mechanism of recognizing and preparing URIs (web addresses). By default, this mechanism is disabled (`false`), and URIs, if they are present in text nodes or attribute values inside the XML documents, are represented as ordinary text. When this property is enabled (`true`), GroupDocs.Editor scans the XML document for any valid URIs, and if found, represents them as external links in the resultant HTML format using the `<a>` element. GroupDocs.Editor searches for URIs in text nodes, CDATA sections, XML comments, attribute values, and DocType definitions.

### RecognizeEmails

`recognizeEmails`

This property is very similar to `recognizeUris`; it does the same but for email addresses. By default, it is disabled, so if an email address is present in the input XML, the output HTML will contain it as ordinary text. However, if enabled by setting it to `true`, all valid email addresses will be represented with the [mailto](https://en.wikipedia.org/wiki/Mailto) scheme and the `<a>` element.

Unlike the `recognizeUris` property, the `recognizeEmails` property, if enabled, works only for attribute values.

### TrimTrailingWhitespaces

`trimTrailingWhitespaces`

This property enables the truncation of trailing whitespaces in text nodes—the textual content located between start and end tags (inner-tag text). By default, it is disabled (`false`), so trailing whitespaces will be preserved. Line breaks are also treated as whitespaces. This may be useful when the input XML is formed with line breaks and empty spans for the sake of readability.

### AttributeValuesQuoteType

`attributeValuesQuoteType`

`QuoteType` is a new type introduced in version 23.5—it represents two types of quotes permissible in the XML format in attribute values: a single quote (`'`) and a double quote (`"`).

With this option, users can redefine the quote type used in the original XML document and set the desired quote, which should be present in the resultant HTML. By default, double quotes are used.

The next two properties—`highlightOptions` and `formatOptions`—have compound values, described below. Users cannot create instances of the wrapping classes `XmlHighlightOptions` and `XmlFormatOptions` and set the values of `highlightOptions` and `formatOptions` (including setting them to null). It is intended that only members of `XmlHighlightOptions` and `XmlFormatOptions` are to be changed from their default values.

#### HighlightOptions

The `highlightOptions` property has a type of `XmlHighlightOptions`. An already created instance of `XmlHighlightOptions` is set in the `highlightOptions` property, and a reference to it cannot be modified; only members of `XmlHighlightOptions` are allowed to be modified.

`XmlHighlightOptions` has six sub-properties:

1. **xmlTagsFontSettings**: Responsible for representing the font of XML tags, including both start and end tags, angle brackets with tag names. By default, it is a "Calibri" font, 12pt size.
2. **attributeNamesFontSettings**: Responsible for representing the font of attribute names. By default, it is a "Calibri" font, 11pt size, red color.
3. **attributeValuesFontSettings**: Responsible for representing the font of attribute values. By default, it is a "Calibri" font, 11pt size, blue color.
4. **innerTextFontSettings**: Responsible for representing the font of text nodes (text inside and between XML elements). By default, it is a "Times New Roman" font, 11pt size, black color.
5. **htmlCommentsFontSettings**: Responsible for representing the font of HTML comments (XML comments with the syntax of HTML comments), including a pair of opening `<!--` and closing `-->` tags. By default, it is a "Consolas" font, 11pt size, green color.
6. **cDataFontSettings**: Responsible for representing the font of CDATA sections, including a pair of opening `<![CDATA[` and closing `]]>` tags. By default, it is a "Calibri" font, 11pt size, coral color.

All these properties are of the `WebFont` class—a new public type introduced in version 23.5. It has several public properties, each of which holds and represents some aspect of the font.

`XmlHighlightOptions` also has two useful public members:

- `isDefault`: Boolean property that indicates whether the current instance has default values, which means that all properties are in their initial state.
- `resetToDefault()`: Method that resets all properties to their initial values.

The code sample below shows the creation of an `XmlEditOptions` instance and setting different (but not all) properties within the `XmlHighlightOptions` sub-property.

```javascript
// Import necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Create XmlEditOptions instance
const editOptions = new groupdocsEditor.XmlEditOptions();

// Check if options are default
console.log('Are highlight options default?', editOptions.getHighlightOptions().isDefault());

// Get highlight options
const highlightOptions = editOptions.getHighlightOptions();

// Setting XML tags font settings
highlightOptions.getXmlTagsFontSettings().setSize(groupdocsEditor.FontSize.Large);
highlightOptions.getXmlTagsFontSettings().setColor(groupdocsEditor.ArgbColor.fromKnownColor('Olive'));

// Setting attribute names font settings
highlightOptions.getAttributeNamesFontSettings().setName('Arial');
highlightOptions.getAttributeNamesFontSettings().setLine(groupdocsEditor.WebFont.TextDecorationLine.Underline);
highlightOptions.getAttributeNamesFontSettings().setWeight(groupdocsEditor.FontWeight.Lighter);

// Setting attribute values font settings
highlightOptions.getAttributeValuesFontSettings().setLine(
    groupdocsEditor.WebFont.TextDecorationLine.Underline | groupdocsEditor.WebFont.TextDecorationLine.Overline
);
highlightOptions.getAttributeValuesFontSettings().setStyle(groupdocsEditor.FontStyle.Italic);

// Setting CDATA sections font settings
highlightOptions.getCDataFontSettings().setLine(groupdocsEditor.WebFont.TextDecorationLine.LineThrough);
highlightOptions.getCDataFontSettings().setSize(groupdocsEditor.FontSize.Smaller);

// Setting HTML comments font settings
highlightOptions.getHtmlCommentsFontSettings().setColor(groupdocsEditor.ArgbColor.fromKnownColor('LightGreen'));
highlightOptions.getHtmlCommentsFontSettings().setName('Courier New');

// Setting text node font settings
highlightOptions.getInnerTextFontSettings().setWeight(groupdocsEditor.FontWeight.fromNumber(300));
highlightOptions.getInnerTextFontSettings().setSize(groupdocsEditor.FontSize.XSmall);

// Checking they are not default
console.log('Are highlight options default now?', highlightOptions.isDefault());

// Resetting to default
highlightOptions.resetToDefault();

// Checking they are default again
console.log('Are highlight options default after reset?', highlightOptions.isDefault());
```

#### FormatOptions

The `formatOptions` property has a type of `XmlFormatOptions`. Like with `highlightOptions`, the `formatOptions` property already has an instance of `XmlFormatOptions` with all sub-properties having their default values. A reference to this instance cannot be modified, but only obtained through the getter, and no new instance of `XmlFormatOptions` can be created.

`XmlFormatOptions` has three sub-properties:

1. **eachAttributeFromNewline**: Boolean flag. By default, attribute-value pairs are represented in the resultant HTML in a single line next to the element name (`false` value). When set to `true`, each pair of attribute-value in every XML element will be placed on a new separate line in the resultant HTML.
2. **leafTextNodesOnNewline**: Boolean flag. By default, text nodes are represented in the resultant HTML along with other XML nodes, in the same line (`false` value). When set to `true`, each text node will be placed on a new line with a bigger left indent.
3. **leftIndent**: Represents the left indent distance for one level of nesting. By default, it is 10 points (`pt`). This property uses the `Length` data type, and you can specify the value in any CSS-compatible length unit.

Like `XmlHighlightOptions`, `XmlFormatOptions` has a useful boolean property `isDefault()` that indicates whether the current instance has all properties set to their initial values.

The code sample below shows the creation of an `XmlEditOptions` instance and setting different properties within the `XmlFormatOptions` sub-property.

```javascript
// Import necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Create XmlEditOptions instance
const editOptions = new groupdocsEditor.XmlEditOptions();

// Check if options are default
console.log('Are format options default?', editOptions.getFormatOptions().isDefault());

// Get format options
const formatOptions = editOptions.getFormatOptions();

// Each attribute-value pair must be placed on a new line
formatOptions.setEachAttributeFromNewline(true);

// Text nodes must be placed on a new line
formatOptions.setLeafTextNodesOnNewline(true);

// Setting a custom left indent using 'Length' data type
formatOptions.setLeftIndent(groupdocsEditor.Length.fromValueWithUnit(20, groupdocsEditor.Length.Unit.Px));

// Checking that options are not default now
console.log('Are format options default now?', formatOptions.isDefault());

// Disabling left indent
formatOptions.setLeftIndent(groupdocsEditor.Length.UnitlessZero);
```

## Complex Example

Now that the `XmlEditOptions` class with all its properties and sub-properties is described in detail, it's time to bring it together. The code example below shows opening an XML file from a file path, creating and adjusting two different `XmlEditOptions` instances, and editing a document twice with these two different option classes to obtain two different `EditableDocument` representations. Then these `EditableDocument` instances are saved to two different HTML files.

```javascript
// Import necessary modules
const fs = require('fs');
const path = require('path');
const groupdocsEditor = require('groupdocs-editor');

// Specify input and output paths
const xmlInputPath = 'path/to/sample.xml';

const outputPath1 = 'path/to/output1.html';
const outputPath2 = 'path/to/output2.html';

// Create and adjust the first XmlEditOptions instance
const editOptions1 = new groupdocsEditor.XmlEditOptions();
editOptions1.setRecognizeEmails(true);
editOptions1.setRecognizeUris(true);
editOptions1.setFixIncorrectStructure(true);
editOptions1.setAttributeValuesQuoteType(groupdocsEditor.QuoteType.SingleQuote);
editOptions1.getFormatOptions().setLeftIndent(groupdocsEditor.Length.parse('20px'));
editOptions1.getHighlightOptions().getXmlTagsFontSettings().setLine(
    groupdocsEditor.WebFont.TextDecorationLine.Underline | groupdocsEditor.WebFont.TextDecorationLine.Overline
);
editOptions1.getHighlightOptions().getXmlTagsFontSettings().setWeight(groupdocsEditor.FontWeight.Bold);

// Create and adjust the second XmlEditOptions instance
const editOptions2 = new groupdocsEditor.XmlEditOptions();
editOptions2.setTrimTrailingWhitespaces(true);
editOptions2.setAttributeValuesQuoteType(groupdocsEditor.QuoteType.DoubleQuote);
editOptions2.getFormatOptions().setLeafTextNodesOnNewline(true);
editOptions2.getHighlightOptions().getXmlTagsFontSettings().setSize(groupdocsEditor.FontSize.XLarge);
editOptions2.getHighlightOptions().getHtmlCommentsFontSettings().setLine(groupdocsEditor.WebFont.TextDecorationLine.LineThrough);

// Create an Editor instance
const editor = new groupdocsEditor.Editor(xmlInputPath);

// Edit the document with the first set of options
const edited1 = editor.edit(editOptions1);

// Edit the document with the second set of options
const edited2 = editor.edit(editOptions2);

// Save the first edited document to an HTML file
fs.writeFileSync(outputPath1, edited1.getContent());

// Save the second edited document to another HTML file
fs.writeFileSync(outputPath2, edited2.getContent());

// Dispose of resources
edited1.dispose();
edited2.dispose();
editor.dispose();
```

## Getting Document Metainfo

The article "[Extracting Document Metainfo](/editor/nodejs-java/extracting-document-metainfo)" describes the `getDocumentInfo()` method, which allows detecting the document format and extracting its metadata without editing it. The XML format is supported as well.

When the `getDocumentInfo()` method is called for the `Editor` class instance, which was previously created with an XML document loaded into the constructor, this method returns a `TextualDocumentInfo` instance—it is a common class for all document formats of a textual nature, like HTML, XML, TXT.

As with all `IDocumentInfo` implementations, the `TextualDocumentInfo` has four inherited properties, but also one own property:

1. **format**: Property of `TextualFormats` type. For XML documents, it is always `TextualFormats.Xml`.
2. **pageCount**: Integer type. Always returns 1 since XML documents have no pages.
3. **size**: Long integer type. Returns the file size in bytes, not the number of characters.
4. **isEncrypted**: Boolean type. Always returns `false` because XML documents, like any other textual documents, cannot be encrypted.
5. **encoding**: Returns the detected presumable encoding of the XML document.

The code example below shows obtaining the metainfo for the XML file and checking its properties.

```javascript
// Import necessary modules
const groupdocsEditor = require('groupdocs-editor');

// Specify the input XML file path
const xmlInputPath = 'path/to/sample.xml';

// Create an Editor instance
const editor = new groupdocsEditor.Editor(xmlInputPath);

// Get document info
const info = editor.getDocumentInfo();
const xmlInfo = info;

// Output the properties
console.log('Encoding:', xmlInfo.getEncoding());
console.log('Is Encrypted:', xmlInfo.isEncrypted());
console.log('Page Count:', xmlInfo.getPageCount());
console.log('Format:', xmlInfo.getFormat());
console.log('Size:', xmlInfo.getSize());

// Dispose of the editor
editor.dispose();
```

## Conclusion

By following this guide, you can effectively edit XML files using GroupDocs.Editor for Node.js via Java. The `XmlEditOptions` class provides extensive customization for processing XML documents, including fixing incorrect structures, recognizing URIs and emails, customizing font settings, and formatting options.

This capability is particularly useful for applications that require programmatic manipulation of XML files, such as configuration files, project files, or any XML-based data.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.
