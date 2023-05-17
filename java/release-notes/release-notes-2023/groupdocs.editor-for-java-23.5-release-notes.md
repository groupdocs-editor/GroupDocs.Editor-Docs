---
id: groupdocs-editor-for-java-23-5-release-notes
url: editor/java/groupdocs-editor-for-java-23-5-release-notes
title: GroupDocs.Editor for Java 23.5 Release Notes
weight: 100
description: ""
keywords: 
productName: GroupDocs.Editor for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for .Java 23.5{{< /alert >}}

GroupDocs.Editor for Java version 23.5 is a new significant update that contains a new complex feature, that consists of several related sub-features, improvements and bug fixes — completely redeveloped and heavily expanded supprot of editing XML documents. 

This release notes article describes this ferature in short, for the detailed review please take a look on a [separate detailed article](https://docs.groupdocs.com/editor/java/how-to-edit-xml/).

## New features and improvements

There are two major improvements in the version 23.5: added support of the page background and border for the paginal mode in the WordProcessing module. This means that now, when editing a WordProcessing document in the paginal mode, the background and border information for the page will be preserved in HTML during forward conversion and then restored during generating output WordProcessing document from HTML.

Before this time the information about page background and border was lost during editing.

### XML formatter

Now GroupDocs.Editor for Java contains a XML formatter — a new XML processing mechanism, which provides better HTML-representation of the original XML with more customization and adjustments.

### New XML highlight options

[`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/) were completely redesigned to become more powerful and flexible.


### New public types

GroupDocs.Editor version 23.5 contains a new namespace and a plenty of new public types — classes, structs and enums. Also the `QuoteType` type was completely reworked.

## Bug fix

There is one significant bugfix in the version 23.5 in the WordProcessing module. In order to describe it is required to explain the tricks how WordProcessing document is represented in HTML. In WordProcessing formats there can exist empty parahraphs, which may have zero characters; however, they are still displayed on the page — they occupy some empty area on the page, which can be seen visually. In HTML such paragraphs are usually represented with P element. But, unlike the MS Word, browsers collapse the empty paragraphs, even they have some content with empty text. So in order to emulate such empty paragraphs the GroupDocs.Editor adds a non-breaking whietspace into them. With non-breaking whietspaces, the browsers don't collapse these paragraphs.

However, from this solution the another problem emerges. When converting back from HTML to the WordProcessing, these whitespace characters in the empty paragraphs will be imported into the resultant document, which is not good — there were no any whitespace characters in the original document. In order to solve this new issue, the GroupDocs.Editor adds the hidden parameters for such the empty paragraphs — a special command (trick) in a form of a custom CSS property, that "tells" the backward converter in GroupDocs.Editor — do not import such whitespaces.

And this is a root of current issue — when the end-user in the WYSIWYG-editor adds some text inside such special empty paragraph, this text along with all paragraph content will be ignored during the backward conversion — the whole paragraph will be missed in the resultant WordProcessing document.

In the version 23.5 this problem is finally fixed — when some text was added in the special empty paragraph, then this paragraph will not be omitted in the resultant WordProcessing document.

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2374 | Update Nuget package tags  | Improvement |
| EDITORNET-2378 | Implement page background for forward and backward converters | Improvement |
| EDITORNET-2379 | Implement page border for forward and backward converters | Improvement |
| EDITORNET-2336 | Unable to generate the edited document | Bug |
| EDITORNET-2415 | Develop XML formatter | New feature |
| EDITORNET-2427 | Get rid of System.Drawing in public API | New feature |
| EDITORNET-2460 | Introduce new public types for XML module | New feature |
| EDITORNET-2461 | Rework XmlHightlightOptions with new types instead of System.Drawing types | New feature |
| EDITORNET-2459 | Redevelop QuoteType enum | Improvement |
| EDITORNET-2420 | Fix issues with bidirectional text on new .NET 6.0 | Bug |


## Public API and Backward Incompatible Changes

**New** public types:
- [`com.groupdocs.editor.htmlcss.css.properties`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.properties/) namespace:
	- `FontSize`
	- `FontStyle`
	- `FontWeight`
- [`groupdocs.editor.htmlcss.css.datatypes`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.css.datatypes/) namespace:
	- `ArgbColor`
- [`groupdocs.editor.options`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/) namespace:
	- `WebFont`
	- `XmlHighlightOptions`
	- `XmlFormatOptions`

**Removed** types and members:
- `EnablePagination` flag in [`PdfSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/pdfsaveoptions) class

