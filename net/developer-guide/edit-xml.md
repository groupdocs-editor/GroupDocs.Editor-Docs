---
id: how-to-edit-xml
url: editor/net/how-to-edit-xml
title: How to edit XML file
weight: 54
description: "This article demonstrates how to edit XML files and XML documents using C# programming language."
keywords: GroupDocs.Editor XML support, XML format, editing XML files, edit XML documents
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
> This example demonstrates the opening, editing and saving the XML documents, using different options and adjustments.

## Introduction

GroupDocs.Editor has supported importing the documents in [XML (eXtensible Markup Language)](https://docs.fileformat.com/web/xml/) format for a long time. However, in [version 23.2](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-2-release-notes/) the XML processing mechanism was completely redeveloped and drastically enhanced, XML editing public options were also redesigned and significantly expanded, with new sub-option classes and new public types.

Current article describes this new XML processing mechanism and is applicable to the GroupDocs.Editor [version 23.2](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-2-release-notes/) and above.

## Loading XML documents

Loading of the XML documents to the `GroupDocs.Editor.`[`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class is usual and the same as for other formats. There are no dedicated load options for the XML format, it is enough to specify the file itself through file path or [byte stream](https://docs.microsoft.com/en-us/dotnet/api/system.IO.Stream?view=net-6.0).

If loading through file path, the file extension does not matter, so you may freely load an XML file not only with the `*.xml`, but with any other extension like `*.csproj`, `*.svg` or any other extension - only valid internal structure matters.

Also please note that you cannot treat HTML files like XML — only [XHTML](https://en.wikipedia.org/wiki/XHTML) can be treated like valid XML.

Code example below shows loading of the same document by two approaches into the two different [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instances:

```csharp
const string xmlFilename = "Sample.xml";
string xmlInputPath = System.IO.Path.Combine("full_folder_path", xmlFilename);

using (FileStream xmlStream = System.IO.File.OpenRead(xmlInputPath))
using (GroupDocs.Editor.Editor editorFromPath = new Editor(xmlInputPath))//from the path
using (GroupDocs.Editor.Editor editorFromStream = new Editor(delegate () { return xmlStream; }))//from the stream
{
	//Here two Editor instances can separately work with one file
}
```

## Editing XML documents

Like for other format families in GroupDocs.Editor, there is a special [`XmlEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/) class for editing the XML documents. As always, it is not mandatory when editing a document, so the [`Editor.Edit()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) overload without parameter may be used — GroupDocs.Editor will automatically detect the format and apply the default options. The example below shows such a case: XML document is loaded, edited, and then the edited content, represented with a [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class, may be passed to the WYSIWYG-editor or any other HTML editing software, or simply saved to the disk, as it is shown in the example.

```csharp
const string xmlFilename = "Sample.xml";
string xmlInputPath = System.IO.Path.Combine("full_folder_path", xmlFilename);

string outputPath = System.IO.Path.Combine("output_folder_path", string.Format("{0}.html", Path.GetFileNameWithoutExtension(xmlFilename)));

using (GroupDocs.Editor.Editor editor = new Editor(xmlInputPath))
{
	using (EditableDocument edited = editor.Edit())
	{
		//Send to WYSIWYG-editor or somewhere else
		edited.Save(outputPath);
	}
}
```

[`XmlEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/) class has different properties, some of them are grouped into “wrappers” — special classes [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/) and [`XmlFormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/), which are described below in detail. The most useful and important properties, however, are directly inside the [`XmlEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/) class.

#### Encoding

[`System.Text.Encoding`](https://learn.microsoft.com/en-us/dotnet/api/system.text.encoding?view=net-7.0) [`Encoding`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/encoding) 

This property allows to set the encoding, which will be applied while opening an input XML file (keep in mind that any XML is first of all a text file). By default all XML files are UTF8, so the default value of this option is also UTF8.

#### FixIncorrectStructure

[`bool`](https://learn.microsoft.com/en-us/dotnet/api/system.boolean?view=net-7.0) [`FixIncorrectStructure`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/fixincorrectstructure)

GroupDocs.Editor can handle without error or exception any XML document: corrupted, truncated, with invalid structure, it can even treat HTML like XML. However, the representation of such a document may degrade in such cases — for example, it is impossible to properly display the nested hierarchical structure, if it is broken.

In order to fix this, the [`FixIncorrectStructure`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/fixincorrectstructure) boolean flag is introduced. If enabled, the GroupDocs.Editor scans the XML document and tries to fix its structure. In particular, it escapes some prohibited characters, properly closes unclosed tags, opens unopened tags, fixes overlapping tags, and so on.

Because such document scanning and fixing requires additional computational resources and in general most of XML documents are valid, this mechanism is disabled by default: [`FixIncorrectStructure`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/fixincorrectstructure) has a `false` value. So for enabling it, you must set the `true` value manually.

#### RecognizeUris

[`bool`](https://learn.microsoft.com/en-us/dotnet/api/system.boolean?view=net-7.0) [`RecognizeUris`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/recognizeuris)

This property enables the mechanism of recognizing and preparing the URIs (web address). By default this mechanism is disabled (`false`) and URIs, if they are present in the text nodes or attribute values inside the XML documents, are represented as an ordinary text. When this property is enabled (`true`), the GroupDocs.Editor scans the XML document for any valid URIs, and if found, represents them as external links in the resultant HTML format: by using the A element. GroupDocs.Editor is searching for URIs in: text nodes, CDATA sections, XML comments, attribute values, DocType definitions.

#### RecognizeEmails

[`bool`](https://learn.microsoft.com/en-us/dotnet/api/system.boolean?view=net-7.0) [`RecognizeEmails`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/recognizeemails)

This property is very similar to the [`RecognizeUris`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/recognizeuris): it does the same, but at this time for email addresses. By default it is disabled, so if the email address is present in the input XML, the output HTML will contain it as ordinary text. However, if enabled by setting `true`, all valid email addresses will be represented with [mailto](https://en.wikipedia.org/wiki/Mailto) scheme and A element.

Unlike the [`RecognizeUris`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/recognizeuris) property, the [`RecognizeEmails`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/recognizeemails), if enabled, is working only for attribute values.

#### TrimTrailingWhitespaces

[`bool`](https://learn.microsoft.com/en-us/dotnet/api/system.boolean?view=net-7.0) [`TrimTrailingWhitespaces`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/trimtrailingwhitespaces)

This property enables the truncation of trailing whitespaces in the text nodes — the textual content, located between start and end tags (inner-tag text). By default is disabled (`false`) — trailing whitespaces will be preserved. Line breaks are also treated as whitespaces. May be useful when input XML is formed with line breaks and empty spans for the sake of readability.

#### AttributeValuesQuoteType

[`QuoteType`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.serialization/quotetype/) [`AttributeValuesQuoteType`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/attributevaluesquotetype)

[`QuoteType`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.serialization/quotetype/) is a new type, introduced in the [version 23.2](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-2-release-notes/) — it is a struct, that represents two types of quotes, permissible in the XML format in attribute values: a *single quote*, represented by the `U+0027 APOSTROPHE` character, and a *double quote*, represented by the `U+0022 QUOTATION MARK` character.

With this option users can redefine the quote type, used in the original XML document, and set the desired quote, which should be present in the resultant HTML. By default the double quotes are used.

Next and last two properties, — [`HighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/highlightoptions/) and [`FormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/formatoptions/), — have compound values, described below. What is important, that users cannot create instances of wrapping classes [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/) and [`XmlFormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/) and set the values of [`HighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/highlightoptions/) and [`FormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/formatoptions/) (including setting them to null). It is intended that only members of [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/) and [`XmlFormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/) have to be changed from the default values.

### HighlightOptions

The [`HighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/highlightoptions/) property has a type of [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/). An already created instance of `XmlHighlightOptions` is already set in the `HighlightOptions` property and a reference to it cannot be modified; only members of `XmlHighlightOptions` are allowed to modify.

XmlHighlightOptions has 6 sub-properties:

1. [`XmlTagsFontSettings`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/xmltagsfontsettings) is responsible for representing the font of XML tags, this include both start and end tags, angle brackets with tag names. By default it is a “`Calibri`” font, `12pt` size.
2. [`AttributeNamesFontSettings`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/attributenamesfontsettings) is responsible for representing the font of attribute names. By default is a “`Calibri`” font, `11pt` size, `red` color.
3. [`AttributeValuesFontSettings`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/attributevaluesfontsettings) is responsible for representing the font of attribute values. By default is a “`Calibri`” font, `11pt` size, `blue` color.
4. [`InnerTextFontSettings`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/innertextfontsettings) is responsible for representing the font of text nodes (text inside and between XML elements). By default is a "`Times New Roman`" font, `11pt` size, `black` color.
5. [`HtmlCommentsFontSettings`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/htmlcommentsfontsettings) is responsible for representing the font of HTML comments (XML comments with a syntax of HTML comments), including a pair of opening `<!--` and closing `-–>` tags. By default is a "`Consolas`" font, `11pt` size, `green` color.
6. [`CDataFontSettings`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/cdatafontsettings) is responsible for representing the font of CDATA sections, including a pair of opening `<![CDATA[` and closing `]]>` tags. By default is a "`Calibri`" font, `11pt` size, `coral` color.

All these properties are of a [`WebFont`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont/) class — a new public type, introduced in the [version 23.2](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-2-release-notes/). It has 6 public properties, one of them is of [`System.String`](https://learn.microsoft.com/en-us/dotnet/api/system.string?view=net-7.0) type, while 5 others are structs, each of which holds and represents some aspect of the font. [`WebFont`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont/) class may be treated as a container for the font properties, where by default every property has some default value, and users can change some or every of them in defined limits.

1. Font name — [`Name`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont/name/) property of a [`System.String`](https://learn.microsoft.com/en-us/dotnet/api/system.string?view=net-7.0) type. Is translated into the `font-family` CSS declaration.
2. Font size — [`Size`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont/size/) property of [`FontSize`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.properties/fontsize/) type. Is translated into the `font-size` CSS declaration.
3. Font color — [`Color`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont/color/) property of [`ArgbColor`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.datatypes/argbcolor/) type. Is translated into the `color` CSS declaration.
4. Font weight (boldness) — [`Weight`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont/weight/) property of [`FontWeight`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.properties/fontweight/) type. Is translated into the `font-weight` CSS declaration.
5. Font style — [`Style`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont/style/) property of [`FontStyle`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.properties/fontstyle/) type. Is translated into the `font-style` CSS declaration.
6. Text decoration line — [`Line`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont/line/) property of [`WebFont.TextDecorationLine`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/webfont.textdecorationline/) enum type. Is translated into the `text-decoration-line` CSS declaration.

WebFont has also some other public functions — the ability to check on equality with itself (`System.IEquatable`) and support of deep cloning (`System.ICloneable`).

[`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/) also has two useful public members:

- [`IsDefault`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/isdefault) [boolean](https://learn.microsoft.com/en-us/dotnet/api/system.boolean?view=net-7.0) property, that indicates whether the current instance has a default value, which means that all properties are in their initial state.
- [`ResetToDefault()`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/resettodefault) method, that resets all properties to their initial values

Code sample below shows a creation of the [`XmlEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/) instance and setting different (but not all) properties within the [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/) sub-property.

```csharp
Options.XmlEditOptions editOptions = new XmlEditOptions();
Assert.IsTrue(editOptions.HighlightOptions.IsDefault);
Options.XmlHighlightOptions highlightOptions = editOptions.HighlightOptions;

//Setting XML tags font settings
highlightOptions.XmlTagsFontSettings.Size = HtmlCss.Css.Properties.FontSize.Large;
highlightOptions.XmlTagsFontSettings.Color = HtmlCss.Css.DataTypes.ArgbColor.KnownColors.CssLevel1.Olive;

//Setting attribute names font settings
highlightOptions.AttributeNamesFontSettings.Name = "Arial";
highlightOptions.AttributeNamesFontSettings.Line = WebFont.TextDecorationLine.Underline;
highlightOptions.AttributeNamesFontSettings.Weight = HtmlCss.Css.Properties.FontWeight.Lighter;

//Setting attribute values font settings
highlightOptions.AttributeValuesFontSettings.Line = WebFont.TextDecorationLine.Underline | WebFont.TextDecorationLine.Overline;
highlightOptions.AttributeValuesFontSettings.Style = HtmlCss.Css.Properties.FontStyle.Italic;

//Setting CDATA sections font settings
highlightOptions.CDataFontSettings.Line = WebFont.TextDecorationLine.LineThrough;
highlightOptions.CDataFontSettings.Size = HtmlCss.Css.Properties.FontSize.Smaller;

//Setting HTML comments font settings
highlightOptions.HtmlCommentsFontSettings.Color = HtmlCss.Css.DataTypes.ArgbColor.KnownColors.CssLevel3.Lightgreen;
highlightOptions.HtmlCommentsFontSettings.Name = "Courier New";

//Setting text node font settings
highlightOptions.InnerTextFontSettings.Weight = HtmlCss.Css.Properties.FontWeight.FromNumber(300);
highlightOptions.InnerTextFontSettings.Size = HtmlCss.Css.Properties.FontSize.XSmall;

//Checking they are not default
Assert.IsFalse(editOptions.HighlightOptions.IsDefault);

//Resetting to default
highlightOptions.ResetToDefault();

//Checking they are default again now
Assert.IsTrue(editOptions.HighlightOptions.IsDefault);
```

### FormatOptions

The [`FormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/formatoptions/) property has a type of [`XmlFormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/). Like with `HighlightOptions` property, the `FormatOptions` property has already set an instance of the `XmlFormatOptions` class with all sub-properties having their default values. Similarly, a reference to this `XmlFormatOptions` instance cannot be modified, but only obtained through the getter, and no new instance of `XmlFormatOptions` can be created.

[`XmlFormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/) class has only three sub-properties:

1. [`bool`](https://learn.microsoft.com/en-us/dotnet/api/system.boolean?view=net-7.0) [`EachAttributeFromNewline`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/eachattributefromnewline/). XML documents usually have the XML elements with a set of attribute-value pairs. By default these pairs are represented in the resultant HTML in a single line next to the element name (`false` value). However, when setting this property to `true`, each and every pair of attribute-value in every XML element will be placed on a new separate line in the resultant HTML.
2. [`bool`](https://learn.microsoft.com/en-us/dotnet/api/system.boolean?view=net-7.0) [`LeafTextNodesOnNewline`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/leaftextnodesonnewline). XML documents usually have the text nodes — pieces of textual content, located inside and/or between adjacent XML elements. By default these text nodes are represented in the resultant HTML along with other XML nodes, in the same line with them (`false` value). However, when setting this property to `true`, each text node will be placed on a new line with a bigger left indent.
3. [`HtmlCss.Css.DataTypes.Length`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.datatypes/length/) [`LeftIndent`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/leftindent). XML is a hierarchical structure, and most of WYSIWYG-editors display hierarchical structures using left indentation: the deeper node is located inside the tree (bigger nesting level) — the bigger left indent is. GroupDocs.Editor does the same, and the [`LeftIndent`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/leftindent) property regulates how big is a left indent distance for a one level. By default it is 10 points (`pt`). So the root element will have no indent at all, 1st nesting level will be `10pt` shifted, 2nd level — `20pt`, 3rd level — `30pt`, and so on. This length is represented by a public type [`HtmlCss.Css.DataTypes.Length`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.datatypes/length/) and can be easily changed to desired. What is more, it can be specified not necessarily in points, but in any CSS-compatible length unit, like percent, pixel, centimeter, and so on. Left indent can be disabled at all by setting a unitless zero to this property like this: [`Length.UnitlessZero`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.datatypes/length/unitlesszero/). But keep in mind that non-zero unitless values are prohibited according to the CSS specification, and GroupDocs.Editor prohibits such values too.

Like in [`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/), the [`XmlFormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/) has a useful `boolean` property [`IsDefault`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/isdefault), that indicates whether the current instance has all properties to be set to their initial values.

Code sample below shows a creation of the [`XmlEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/) instance and setting different properties within the [`XmlFormatOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlformatoptions/) sub-property.

```csharp
Options.XmlEditOptions editOptions = new XmlEditOptions();

//Checking that options are default for now
Assert.IsTrue(editOptions.FormatOptions.IsDefault);
Options.XmlFormatOptions formatOptions = editOptions.FormatOptions;

//Each attribute-value pair must be placed on a new line
formatOptions.EachAttributeFromNewline = true;

//Text nodes (textual content between and inside XML elements) must be placed on a new line
formatOptions.LeafTextNodesOnNewline = true;

//Setting a custom text indent using 'Length' data type, which is composed from value with unit
formatOptions.LeftIndent = HtmlCss.Css.DataTypes.Length.FromValueWithUnit(20, HtmlCss.Css.DataTypes.Length.Unit.Px);

//Checking that options are not default now
Assert.IsFalse(editOptions.FormatOptions.IsDefault);

//Disabling a left indent at all
formatOptions.LeftIndent = HtmlCss.Css.DataTypes.Length.UnitlessZero;
```

## Complex example

Now, when the [`XmlEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/) class with all its properties and sub-properties is described in detail, it's time to bring it together. Code example below shows opening a XML file from file path, creating and adjusting two different [`XmlEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions/) instances, and editing a document twice with these two different option classes to obtain two different [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) representations. Then these [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) are saved to the two different HTML files.

```csharp
const string xmlFilename = "Sample.xml";
string xmlInputPath = System.IO.Path.Combine("input_folder_path", xmlFilename);

string outputPath1 = System.IO.Path.Combine("output_folder_path", string.Format("1-{0}.html", Path.GetFileNameWithoutExtension(xmlFilename)));
string outputPath2 = System.IO.Path.Combine("output_folder_path", string.Format("2-{0}.html", Path.GetFileNameWithoutExtension(xmlFilename)));

Options.XmlEditOptions editOptions1 = new XmlEditOptions();
editOptions1.RecognizeEmails = true;
editOptions1.RecognizeUris = true;
editOptions1.FixIncorrectStructure = true;
editOptions1.AttributeValuesQuoteType = HtmlCss.Serialization.QuoteType.SingleQuote;
editOptions1.FormatOptions.LeftIndent = HtmlCss.Css.DataTypes.Length.Parse("20px");
editOptions1.HighlightOptions.XmlTagsFontSettings.Line = WebFont.TextDecorationLine.Underline | WebFont.TextDecorationLine.Overline;
editOptions1.HighlightOptions.XmlTagsFontSettings.Weight = HtmlCss.Css.Properties.FontWeight.Bold;

Options.XmlEditOptions editOptions2 = new XmlEditOptions();
editOptions2.TrimTrailingWhitespaces = true;
editOptions2.AttributeValuesQuoteType = HtmlCss.Serialization.QuoteType.DoubleQuote;
editOptions2.FormatOptions.LeafTextNodesOnNewline = true;
editOptions2.HighlightOptions.XmlTagsFontSettings.Size = HtmlCss.Css.Properties.FontSize.XLarge;
editOptions2.HighlightOptions.HtmlCommentsFontSettings.Line = WebFont.TextDecorationLine.LineThrough;

using (GroupDocs.Editor.Editor editor = new Editor(xmlInputPath))
{
	using (EditableDocument edited1 = editor.Edit(editOptions1))
	using (EditableDocument edited2 = editor.Edit(editOptions2))
	{
		edited1.Save(outputPath1);
		edited2.Save(outputPath2);
	}
}
```

## Getting document metainfo

Article "[Extracting document metainfo](https://docs.groupdocs.com/editor/net/extracting-document-metainfo/)" describes the [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) method, that allows to detect the document format and extract its metadata without editing it. XML format is supported as well.

When the [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo) method is called for the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class instance, which was previously created with an XML document loaded into the constructor, this method returns a [`GroupDocs.Editor.Metadata.TextualDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/textualdocumentinfo/) instance — it is a common class for all document formats of a textual nature, like HTML, XML, TXT.

As all [`IDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo) implementations, the [`TextualDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/textualdocumentinfo/) has four inherited properties, but also one own property:

1. [`Format`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/textualdocumentinfo/format/) property of [`Formats.TextualFormats`](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/textualformats/) type. For XML documents it always is a [`TextualFormats.Xml`](https://reference.groupdocs.com/editor/net/groupdocs.editor.formats/textualformats/xml/).
2. [`PageCount`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/textualdocumentinfo/pagecount) property of [integer (Int32)](https://docs.microsoft.com/en-us/dotnet/api/system.int32?view=net-6.0) type. Always returns 1, since XML documents have no pages at all.
3. [`Size`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/textualdocumentinfo/size) property of a long integer (Int64) type. Returns a file size in bytes, not the number of characters.
4. [`IsEncrypted`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/textualdocumentinfo/isencrypted) property of a Boolean type. Always returns false, because XML documents, like any other textual documents, cannot be encrypted.
5. [`Encoding`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/textualdocumentinfo/encoding) property of System.Text.Encoding type. Returns detected presumable encoding of the XML document.

Code example below shows obtaining the metainfo for the XML file and checking its properties.

```csharp
const string xmlFilename = "Sample.xml";
string xmlInputPath = System.IO.Path.Combine("input_folder_path", xmlFilename);

using (GroupDocs.Editor.Editor editor = new Editor(xmlInputPath))
{
	GroupDocs.Editor.Metadata.IDocumentInfo info = editor.GetDocumentInfo(null);
	GroupDocs.Editor.Metadata.TextualDocumentInfo xmlInfo = (TextualDocumentInfo)info;
	Assert.AreEqual(System.Text.Encoding.UTF8, xmlInfo.Encoding);
	Assert.IsFalse(xmlInfo.IsEncrypted);
	Assert.AreEqual(1, xmlInfo.PageCount);
	Assert.AreEqual(GroupDocs.Editor.Formats.TextualFormats.Xml, xmlInfo.Format);
	Assert.Greater(xmlInfo.Size, 0);
}
```
