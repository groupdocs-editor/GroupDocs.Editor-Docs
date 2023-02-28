---
id: groupdocs-editor-for-net-23-2-release-notes
url: editor/net/groupdocs-editor-for-net-23-2-release-notes
title: GroupDocs.Editor for .NET 23.2 Release Notes
weight: 100
description: ""
keywords: 
productName: GroupDocs.Editor for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for .NET 23.2{{< /alert >}}

GroupDocs.Editor for .NET version 23.2 is a new significant update that contains a new complex feature, that consists of several related sub-features, improvements and bug fixes — completely redeveloped and heavily expanded supprot of editing XML documents. 

This release notes article describes this ferature in short, for the detailed review please take a look on a [separate detailed article](https://docs.groupdocs.com/editor/net/how-to-edit-xml/).

## New features and improvements

### XML formatter

Now GroupDocs.Editor for .NET contains a XML formatter — a new XML processing mechanism, which provides better HTML-representation of the original XML with more customization and adjustments.

### New XML highlight options

[`XmlHighlightOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmlhighlightoptions/) were completely redesigned to become more powerful and flexible.

### Getting rid of System.Drawing types in public API

Tn the version 23.2 the GroupDocs.Editor has eliminated the usage of the types from the System.Drawing namespace. Instead of them a bunch of new types was introduced.

### New public types

GroupDocs.Editor version 23.2 contains a new namespace and a plenty of new public types — classes, structs and enums. Also the `QuoteType` type was completely reworked.

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2415 | Develop XML formatter | New feature |
| EDITORNET-2427 | Get rid of System.Drawing in public API | New feature |
| EDITORNET-2460 | Introduce new public types for XML module | New feature |
| EDITORNET-2461 | Rework XmlHightlightOptions with new types instead of System.Drawing types | New feature |
| EDITORNET-2459 | Redevelop QuoteType enum | Improvement |
| EDITORNET-2420 | Fix issues with bidirectional text on new .NET 6.0 | Bug |


## Public API and Backward Incompatible Changes

**New** public types:
- [`GroupDocs.Editor.HtmlCss.Css.Properties`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.properties/) namespace:
	- `FontSize`
	- `FontStyle`
	- `FontWeight`
- [`GroupDocs.Editor.HtmlCss.Css.DataTypes`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.datatypes/) namespace:
	- `ArgbColor`
- [`GroupDocs.Editor.Options`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/) namespace:
	- `WebFont`
	- `XmlHighlightOptions`
	- `XmlFormatOptions`

**Removed** types and members:
- `EnablePagination` flag in [`PdfSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfsaveoptions) class

