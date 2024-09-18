---
id: how-to-edit-xml
url: editor/java/how-to-edit-xml
title: How to edit XML file
weight: 54
description: "This article demonstrates how to edit XML files and XML documents using Java programming language."
keywords: GroupDocs.Editor XML support, XML format, editing XML files, edit XML documents
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
> This example demonstrates the opening, editing and saving the XML documents, using different options and adjustments.

## Introduction

com.groupdocs.editor has supported importing the documents in [XML (eXtensible Markup Language)](https://docs.fileformat.com/web/xml/) format for a long time. However, in [version 23.5](https://docs.groupdocs.com/editor/java/groupdocs-editor-for-java-23-5-release-notes/) the XML processing mechanism was completely redeveloped and drastically enhanced, XML editing public options were also redesigned and significantly expanded, with new sub-option classes and new public types.

Current article describes this new XML processing mechanism and is applicable to the GroupDocs.Editor [version 23.5](https://docs.groupdocs.com/editor/java/groupdocs-editor-for-java-23-5-release-notes/) and above.

## Loading XML documents

Loading of the XML documents to the `com.groupdocs.`[`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/) class is usual and the same as for other formats. There are no dedicated load options for the XML format, it is enough to specify the file itself through file path or [ByteArrayInputStream](https://docs.oracle.com/javase/8/docs/api/java/io/ByteArrayInputStream.html).

If loading through file path, the file extension does not matter, so you may freely load an XML file not only with the `*.xml`, but with any other extension like `*.csproj`, `*.svg` or any other extension - only valid internal structure matters.

Also please note that you cannot treat HTML files like XML — only [XHTML](https://en.wikipedia.org/wiki/XHTML) can be treated like valid XML.

Code example below shows loading of the same document by two approaches into the two different [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/) instances:

```java
	String inputFilePath = Constants.SAMPLE_XML;
	final FileInputStream xmlStream = new FileInputStream(inputFilePath) ;
	Editor editorFromPath = new Editor(inputFilePath);
	try 
	{//from the path
		Editor editorFromStream = new Editor(new FileInputStream(inputFilePath));
		try /*JAVA: was using*///from the stream
		{
			//Here two Editor instances can separately work with one file
		}
		finally {
			(editorFromStream).dispose();
		}
	}
	finally {
		(editorFromPath).dispose();
	}
```

## Editing XML documents

Like for other format families in GroupDocs.Editor, there is a special [`XmlEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/) class for editing the XML documents. As always, it is not mandatory when editing a document, so the [`Editor.edit()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) overload without parameter may be used — GroupDocs.Editor will automatically detect the format and apply the default options. The example below shows such a case: XML document is loaded, edited, and then the edited content, represented with a [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) class, may be passed to the WYSIWYG-editor or any other HTML editing software, or simply saved to the disk, as it is shown in the example.

```java
	String xmlInputPath = Constants.SAMPLE_XML;
	String outputPath = Constants.getOutputFilePath(Constants.removeExtension(Path.getFileName(xmlInputPath)), "html");

	Editor editor = new Editor(xmlInputPath);
	try /*JAVA: was using*/
	{
		final EditableDocument edited = editor.edit();
		try /*JAVA: was using*/
		{
			//Send to WYSIWYG-editor or somewhere else
			edited.save(outputPath);
		}
		finally {
			(edited).dispose();
		}
	}
	finally {
		(editor).dispose();
	}
```

[`XmlEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/) class has different properties, some of them are grouped into “wrappers” — special classes [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/) and [`XmlFormatOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/), which are described below in detail. The most useful and important properties, however, are directly inside the [`XmlEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/) class.

#### Encoding

[`java.nio.charset.Charset`](https://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html) [`getEncoding()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getEncoding--) 

This property allows to set the encoding, which will be applied while opening an input XML file (keep in mind that any XML is first of all a text file). By default all XML files are UTF8, so the default value of this option is also UTF8.

#### FixIncorrectStructure

[`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) [`getFixIncorrectStructure()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getFixIncorrectStructure--)

GroupDocs.Editor can handle without error or exception any XML document: corrupted, truncated, with invalid structure, it can even treat HTML like XML. However, the representation of such a document may degrade in such cases — for example, it is impossible to properly display the nested hierarchical structure, if it is broken.

In order to fix this, the [`getFixIncorrectStructure()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getFixIncorrectStructure--) boolean flag is introduced. If enabled, the GroupDocs.Editor scans the XML document and tries to fix its structure. In particular, it escapes some prohibited characters, properly closes unclosed tags, opens unopened tags, fixes overlapping tags, and so on.

Because such document scanning and fixing requires additional computational resources and in general most of XML documents are valid, this mechanism is disabled by default: [`getFixIncorrectStructure()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getFixIncorrectStructure--) has a `false` value. So for enabling it, you must set the `true` value manually.

#### RecognizeUris

[`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) [`getRecognizeUris()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getRecognizeUris--)

This property enables the mechanism of recognizing and preparing the URIs (web address). By default this mechanism is disabled (`false`) and URIs, if they are present in the text nodes or attribute values inside the XML documents, are represented as an ordinary text. When this property is enabled (`true`), the GroupDocs.Editor scans the XML document for any valid URIs, and if found, represents them as external links in the resultant HTML format: by using the A element. GroupDocs.Editor is searching for URIs in: text nodes, CDATA sections, XML comments, attribute values, DocType definitions.

#### RecognizeEmails

[`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) [`getRecognizeEmails()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getRecognizeEmails--)

This property is very similar to the [`getRecognizeUris()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getRecognizeUris--): it does the same, but at this time for email addresses. By default it is disabled, so if the email address is present in the input XML, the output HTML will contain it as ordinary text. However, if enabled by setting `true`, all valid email addresses will be represented with [mailto](https://en.wikipedia.org/wiki/Mailto) scheme and A element.

Unlike the [`getRecognizeUris()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getRecognizeUris--) property, the [`getRecognizeEmails()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getRecognizeEmails--), if enabled, is working only for attribute values.

#### TrimTrailingWhitespaces

[`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) [`getTrimTrailingWhitespaces()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getTrimTrailingWhitespaces--)

This property enables the truncation of trailing whitespaces in the text nodes — the textual content, located between start and end tags (inner-tag text). By default is disabled (`false`) — trailing whitespaces will be preserved. Line breaks are also treated as whitespaces. May be useful when input XML is formed with line breaks and empty spans for the sake of readability.

#### AttributeValuesQuoteType

[`QuoteType`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.serialization/quotetype/) [`getAttributeValuesQuoteType()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getAttributeValuesQuoteType--)

[`QuoteType`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.serialization/quotetype/) is a new type, introduced in the [version 23.5](https://docs.groupdocs.com/editor/java/groupdocs-editor-for-java-23-5-release-notes/) — it is a struct, that represents two types of quotes, permissible in the XML format in attribute values: a *single quote*, represented by the `U+0027 APOSTROPHE` character, and a *double quote*, represented by the `U+0022 QUOTATION MARK` character.

With this option users can redefine the quote type, used in the original XML document, and set the desired quote, which should be present in the resultant HTML. By default the double quotes are used.

Next and last two properties, — [`getHighlightOptions()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getHighlightOptions--) and [`getFormatOptions()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getFormatOptions--), — have compound values, described below. What is important, that users cannot create instances of wrapping classes [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/) and [`XmlFormatOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/) and set the values of [`getHighlightOptions()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getHighlightOptions--) and [`getFormatOptions()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getFormatOptions--) (including setting them to null). It is intended that only members of [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/) and [`XmlFormatOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/) have to be changed from the default values.

### HighlightOptions

The [`getHighlightOptions()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getHighlightOptions--) property has a type of [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/). An already created instance of `XmlHighlightOptions` is already set in the `setHighlightOptions()` property and a reference to it cannot be modified; only members of `XmlHighlightOptions` are allowed to modify.

XmlHighlightOptions has 6 sub-properties:

1. [`getXmlTagsFontSettings()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/#getXmlTagsFontSettings--) is responsible for representing the font of XML tags, this include both start and end tags, angle brackets with tag names. By default it is a “`Calibri`” font, `12pt` size.
2. [`getAttributeNamesFontSettings()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/#getAttributeNamesFontSettings--) is responsible for representing the font of attribute names. By default is a “`Calibri`” font, `11pt` size, `red` color.
3. [`getAttributeValuesFontSettings()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/#getAttributeValuesFontSettings--) is responsible for representing the font of attribute values. By default is a “`Calibri`” font, `11pt` size, `blue` color.
4. [`getInnerTextFontSettings()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/#getInnerTextFontSettings--) is responsible for representing the font of text nodes (text inside and between XML elements). By default is a "`Times New Roman`" font, `11pt` size, `black` color.
5. [`getHtmlCommentsFontSettings()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/#getHtmlCommentsFontSettings--) is responsible for representing the font of HTML comments (XML comments with a syntax of HTML comments), including a pair of opening `<!--` and closing `-–>` tags. By default is a "`Consolas`" font, `11pt` size, `green` color.
6. [`getCDataFontSettings()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/#getCDataFontSettings--) is responsible for representing the font of CDATA sections, including a pair of opening `<![CDATA[` and closing `]]>` tags. By default is a "`Calibri`" font, `11pt` size, `coral` color.

All these properties are of a [`WebFont`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/webfont/) class — a new public type, introduced in the [version 23.5](https://docs.groupdocs.com/editor/java/groupdocs-editor-for-java-23-5-release-notes/). It has 6 public properties, one of them is of [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) type, while 5 others are structs, each of which holds and represents some aspect of the font. [`WebFont`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/webfont/) class may be treated as a container for the font properties, where by default every property has some default value, and users can change some or every of them in defined limits.

1. Font name — [`getName()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/webfont/#getName--) property of a [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) type. Is translated into the `font-family` CSS declaration.
2. Font size — [`getSize()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/webfont/#getSize--) property of [`FontSize`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.properties/fontsize/) type. Is translated into the `font-size` CSS declaration.
3. Font color — [`getColor()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/webfont/#getColor--) property of [`ArgbColor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.datatypes/argbcolor/) type. Is translated into the `color` CSS declaration.
4. Font weight (boldness) — [`getWeight()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/webfont/#getWeight--) property of [`FontWeight`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.properties/fontweight/) type. Is translated into the `font-weight` CSS declaration.
5. Font style — [`getStyle()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/#getStyle--) property of [`FontStyle`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.properties/fontstyle/) type. Is translated into the `font-style` CSS declaration.
6. Text decoration line — [`getLine()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/#getLine--) property of [`WebFont.TextDecorationLine`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/webfont.textdecorationline/) enum type. Is translated into the `text-decoration-line` CSS declaration.


[`XmlHighlightOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/) also has two useful public members:

- [`isDefault()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/#isdefault--) [boolean](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) property, that indicates whether the current instance has a default value, which means that all properties are in their initial state.
- [`resetToDefault()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/#resettodefault--) method, that resets all properties to their initial values

Code sample below shows a creation of the [`XmlEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/) instance and setting different (but not all) properties within the [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/) sub-property.

```java
	XmlEditOptions editOptions = new XmlEditOptions();
	Assert.assertTrue(editOptions.getHighlightOptions().isDefault());
	XmlHighlightOptions highlightOptions = editOptions.getHighlightOptions();

	//Setting XML tags font settings
	highlightOptions.getXmlTagsFontSettings().setSize(FontSize.Large);
	highlightOptions.getXmlTagsFontSettings().setColor(ArgbColor.KnownColors.CssLevel1.Olive);

	//Setting attribute names font settings
	highlightOptions.getAttributeNamesFontSettings().setName("Arial");
	highlightOptions.getAttributeNamesFontSettings().setLine(WebFont.TextDecorationLine.Underline);
	highlightOptions.getAttributeNamesFontSettings().setWeight(FontWeight.Lighter);

	//Setting attribute values font settings
	highlightOptions.getAttributeValuesFontSettings().setLine((byte) (WebFont.TextDecorationLine.Underline + WebFont.TextDecorationLine.Overline));
	highlightOptions.getAttributeValuesFontSettings().setStyle(FontStyle.Italic);

	//Setting CDATA sections font settings
	highlightOptions.getCDataFontSettings().setLine(WebFont.TextDecorationLine.LineThrough);
	highlightOptions.getCDataFontSettings().setSize(FontSize.Smaller);

	//Setting HTML comments font settings
	highlightOptions.getHtmlCommentsFontSettings().setColor(ArgbColor.KnownColors.CssLevel3.Lightgreen);
	highlightOptions.getHtmlCommentsFontSettings().setName("Courier New");

	//Setting text node font settings
	highlightOptions.getInnerTextFontSettings().setWeight(FontWeight.fromNumber(300));
	highlightOptions.getInnerTextFontSettings().setSize(FontSize.XSmall);

	//Checking they are not default
	Assert.assertFalse(editOptions.getHighlightOptions().isDefault());

	//Resetting to default
	highlightOptions.resetToDefault();

	//Checking they are default again now
	Assert.assertTrue(editOptions.getHighlightOptions().isDefault());
```

### FormatOptions

The [`getFormatOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/#getFormatOptions--) property has a type of [`XmlFormatOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/). Like with `getHighlightOptions()` property, the `getFormatOptions()` property has already set an instance of the `XmlFormatOptions` class with all sub-properties having their default values. Similarly, a reference to this `XmlFormatOptions` instance cannot be modified, but only obtained through the getter, and no new instance of `XmlFormatOptions` can be created.

[`XmlFormatOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/) class has only three sub-properties:

1. [`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) [`getEachAttributeFromNewline()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/#getEachAttributeFromNewline--). XML documents usually have the XML elements with a set of attribute-value pairs. By default these pairs are represented in the resultant HTML in a single line next to the element name (`false` value). However, when setting this property to `true`, each and every pair of attribute-value in every XML element will be placed on a new separate line in the resultant HTML.
2. [`boolean`](https://docs.oracle.com/javase/8/docs/api/java/lang/Boolean.html) [`getLeafTextNodesOnNewline()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/#getLeafTextNodesOnNewline--). XML documents usually have the text nodes — pieces of textual content, located inside and/or between adjacent XML elements. By default these text nodes are represented in the resultant HTML along with other XML nodes, in the same line with them (`false` value). However, when setting this property to `true`, each text node will be placed on a new line with a bigger left indent.
3. [`com.groupdocs.editor.htmlcss.css.datatypes.Length`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.datatypes/length/) [`getLeftIndent()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/#getLeftIndent--). XML is a hierarchical structure, and most of WYSIWYG-editors display hierarchical structures using left indentation: the deeper node is located inside the tree (bigger nesting level) — the bigger left indent is. GroupDocs.Editor does the same, and the [`getLeftIndent()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/#getLeftIndent--) property regulates how big is a left indent distance for a one level. By default it is 10 points (`pt`). So the root element will have no indent at all, 1st nesting level will be `10pt` shifted, 2nd level — `20pt`, 3rd level — `30pt`, and so on. This length is represented by a public type [`com.groupdocs.editor.htmlcss.css.datatypes.Length`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.datatypes/length/) and can be easily changed to desired. What is more, it can be specified not necessarily in points, but in any CSS-compatible length unit, like percent, pixel, centimeter, and so on. Left indent can be disabled at all by setting a unitless zero to this property like this: [`Length.UnitlessZero`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.datatypes/length/#UnitlessZero). But keep in mind that non-zero unitless values are prohibited according to the CSS specification, and GroupDocs.Editor prohibits such values too.

Like in [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlhighlightoptions/), the [`XmlFormatOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/) has a useful `boolean` property [`isDefault()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/isdefault), that indicates whether the current instance has all properties to be set to their initial values.

Code sample below shows a creation of the [`XmlEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/) instance and setting different properties within the [`XmlFormatOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmlformatoptions/) sub-property.

```java
	XmlEditOptions editOptions = new XmlEditOptions();

	//Checking that options are default for now
	Assert.assertTrue(editOptions.getFormatOptions().isDefault());
	XmlFormatOptions formatOptions = editOptions.getFormatOptions();

	//Each attribute-value pair must be placed on a new line
	formatOptions.setEachAttributeFromNewline(true);

	//Text nodes (textual content between and inside XML elements) must be placed on a new line
	formatOptions.setLeafTextNodesOnNewline(true);

	//Setting a custom text indent using 'Length' data type, which is composed from value with unit
	formatOptions.setLeftIndent(Length.fromValueWithUnit(20, Length.Unit.Px));

	//Checking that options are not default now
	Assert.assertFalse(editOptions.getFormatOptions().isDefault());

	//Disabling a left indent at all
	formatOptions.setLeftIndent(Length.UnitlessZero);
```

## Complex example

Now, when the [`XmlEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/) class with all its properties and sub-properties is described in detail, it's time to bring it together. Code example below shows opening a XML file from file path, creating and adjusting two different [`XmlEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/xmleditoptions/) instances, and editing a document twice with these two different option classes to obtain two different [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) representations. Then these [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) are saved to the two different HTML files.

```java
	String xmlInputPath = Constants.SAMPLE_XML;

	String outputPath1 = Constants.getOutputFilePath("1--"+Constants.removeExtension(Path.getFileName(xmlInputPath)), "html");
	String outputPath2 = Constants.getOutputFilePath("2--"+Constants.removeExtension(Path.getFileName(xmlInputPath)), "html");

	XmlEditOptions editOptions1 = new XmlEditOptions();
	editOptions1.setRecognizeEmails(true);
	editOptions1.setRecognizeUris(true);
	editOptions1.setFixIncorrectStructure(true);
	editOptions1.setAttributeValuesQuoteType(QuoteType.SingleQuote);
	editOptions1.getFormatOptions().setLeftIndent(Length.parse("20px"));
	editOptions1.getHighlightOptions().getXmlTagsFontSettings().setLine((byte) (WebFont.TextDecorationLine.Underline + WebFont.TextDecorationLine.Overline));
	editOptions1.getHighlightOptions().getXmlTagsFontSettings().setWeight(FontWeight.Bold);

	XmlEditOptions editOptions2 = new XmlEditOptions();
	editOptions2.setTrimTrailingWhitespaces(true);
	editOptions2.setAttributeValuesQuoteType(QuoteType.DoubleQuote);
	editOptions2.getFormatOptions().setLeafTextNodesOnNewline(true);
	editOptions2.getHighlightOptions().getXmlTagsFontSettings().setSize(FontSize.XLarge);
	editOptions2.getHighlightOptions().getHtmlCommentsFontSettings().setLine(WebFont.TextDecorationLine.LineThrough);

	Editor editor = new Editor(xmlInputPath);
	try /*JAVA: was using*/
	{
		final EditableDocument edited1 = editor.edit(editOptions1);
		try /*JAVA: was using*/
		{
			final EditableDocument edited2 = editor.edit(editOptions2);
			try /*JAVA: was using*/
			{
				edited1.save(outputPath1);
				edited2.save(outputPath2);
			}
			finally {
				(edited2).dispose();
			}
		}
		finally {
			(edited1).dispose();
		}
	}
	finally {
		(editor).dispose();
	}
```

## Getting document metainfo

Article "[Extracting document metainfo](https://docs.groupdocs.com/editor/java/extracting-document-metainfo/)" describes the [`getDocumentInfo()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-) method, that allows to detect the document format and extract its metadata without editing it. XML format is supported as well.

When the [`getDocumentInfo()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-) method is called for the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class instance, which was previously created with an XML document loaded into the constructor, this method returns a [`com.groupdocs.editor.metadata.TextualDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo/) instance — it is a common class for all document formats of a textual nature, like HTML, XML, TXT.

As all [`IDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/idocumentinfo/) implementations, the [`TextualDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo/) has four inherited properties, but also one own property:

1. [`getFormat()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo/#getFormat--) property of [`com.groupdocs.editor.formats.TextualFormats`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/textualformats/) type. For XML documents it always is a [`TextualFormats.Xml`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/textualformats/#Xml).
2. [`getPageCount()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo/#getPageCount--) property of [integer](https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html) type. Always returns 1, since XML documents have no pages at all.
3. [`getSize()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo/#getSize--) property of a long integer (Int64) type. Returns a file size in bytes, not the number of characters.
4. [`isEncrypted()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo/#isEncrypted--) property of a Boolean type. Always returns false, because XML documents, like any other textual documents, cannot be encrypted.
5. [`getEncoding()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/textualdocumentinfo/#getEncoding--) property of System.Text.Encoding type. Returns detected presumable encoding of the XML document.

Code example below shows obtaining the metainfo for the XML file and checking its properties.

```java
	String xmlInputPath = Constants.SAMPLE_XML;

	Editor editor = new Editor(xmlInputPath);
	try /*JAVA: was using*/
	{
		IDocumentInfo info = editor.getDocumentInfo(null);
		TextualDocumentInfo xmlInfo = (TextualDocumentInfo)info;
		Assert.assertEquals(java.nio.charset.Charset.defaultCharset(), xmlInfo.getEncoding());
		Assert.assertFalse(xmlInfo.isEncrypted());
		Assert.assertEquals(1, xmlInfo.getPageCount());
		Assert.assertEquals(TextualFormats.Xml, xmlInfo.getFormat());
		//msAssert.Greater(xmlInfo.Size, 0);
	}
	finally {
		(editor).dispose();
	}
```
