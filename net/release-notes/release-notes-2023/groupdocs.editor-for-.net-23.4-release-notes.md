---
id: groupdocs-editor-for-net-23-4-release-notes
url: editor/net/groupdocs-editor-for-net-23-4-release-notes
title: GroupDocs.Editor for .NET 23.4 Release Notes
weight: 90
description: ""
keywords: 
productName: GroupDocs.Editor for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for .NET 23.4{{< /alert >}}

GroupDocs.Editor for .NET version 23.2 is a new significant update that contains a new complex feature, that consists of several related sub-features, improvements and bug fixes — completely redeveloped and heavily expanded supprot of editing XML documents. 

This release notes article describes this ferature in short, for the detailed review please take a look on a [separate detailed article](https://docs.groupdocs.com/editor/net/how-to-edit-xml/).

## New features and improvements

### Ability to generate a preview of selected worksheet of a loaded spreadsheet in SVG format

Starting from the version 23.4 the GroupDocs.Editor for .NET is able to generate an SVG preview of arbitrary worksheet within spreadsheet. In order to do this need to load a desired spreadsheet document inside the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/) class, invoke the [`GetDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo/) method, and then in the obtained [`GroupDocs.Editor.Metadata.SpreadsheetDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/spreadsheetdocumentinfo/) struct call the [`GeneratePreview(int worksheetIndex)`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/spreadsheetdocumentinfo/generatepreview/) method.

A separate article ["Generating worksheets (tabs) preview for spreadsheet"](https://docs.groupdocs.com/editor/net/generating-worksheets-preview-for-spreadsheet) explains this new feature in detail.

### New e-Book save options

In previous versions there were two different classes for save options for the e-Book formats family: `EpubSaveOptions` and `Azw3SaveOptions`. In version 23.4 these classes are deleted and replaced with single common `EbookSaveOptions` class. Article ["How to edit e-Book file"](https://docs.groupdocs.com/editor/net/how-to-edit-ebook/) explains this change in detail.

### Support of export into Mobi format

Before the version 23.4 was released, the Mobi format was supported only on import, so it was possible to load and edit a Mobi document, but not to save the edited document in Mobi format. Starting from the version 23.4 this is possible. Article ["How to edit Mobi file "](https://docs.groupdocs.com/editor/net/how-to-edit-mobi-file/) explains this in detail.

### Greatly improved HTML parser

In the version 23.4 the HTML parser was significantly improved. Now it is able to correctly process invalid, incorrect and distorted markup. In particular, the next distortions are supported:
- Flow elements nested inside phrasing elements, for example, `P` inside `P` or `DIV` inside `SPAN`.
- Prohibited elements and nodes inside TABLE element, for example, prohibited `P` inside `TABLE` or `DIV` inside `TR` will be pushed upward.
- `SELECT` is generated and applied as a parent for the orphan `OPTION`/`OPTGROUP` elements, because `OPTION`/`OPTGROUP` must be direct children of the `SELECT`.
- `COLGROUP` is generated and applied as a parent for the `COL`.
- Incorrectly nested lists are properly processed.
- Better parsing of HTML attributes
- And many more

### New method for obtaining an HTML markup

Now the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/) class contains a new public method [`EditableDocument.GetContent`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/getcontent/#getcontent_2), which allows to obtains the HTML markup of the opened document and write it to the specified stream with specified text encoding.

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2423 | Add ability to generate a worksheet (tab) preview for the Spreadsheet module | New feature |
| EDITORNET-2475 | Implement Mobi export support | New feature |
| EDITORNET-2480 | Intoduce new universal eBook save options | New feature |
| EDITORNET-2468 | Add full support of text-decoration-thickness | Improvement |
| EDITORNET-2476 | Improve and fix text decorations in forward WordProcessing converter | Improvement |
| EDITORNET-2478 | Develop new parser for all text-decoration properties | Improvement |
| EDITORNET-2482 | Improve parsing of HTML attributes to properly process invalid quote chars in style attribute value | Improvement |
| EDITORNET-2483 | Improve parsing of font-family property in CSS to correctly process invalid CSS markup | Improvement |
| EDITORNET-2486 | Develop new public method for obtaining HTML markup in stream | Improvement |
| EDITORNET-2489 | Enhance HTML parser to properly cope with invalid nesting in HTML elements | Improvement |
| EDITORNET-2491 | Implement a valid parsing of incorrectly nested lists | Improvement |
| EDITORNET-2494 | Implement new parser for invalid OPTION/OPTGROUP elements | Improvement |
| EDITORNET-2479 | Fix Word-to-HTML converter, which generates invalid HTML markup | Bug |
| EDITORNET-2485 | GroupDocs.Editor memory leak while converting WordProcessing document | Bug |
| EDITORNET-2490 | Fix bug with skipped HTML markup during parsing | Bug |
| EDITORNET-2492 | Fix bug with incorrect child HTML element getter in the new HTML DOM | Bug |
| EDITORNET-2495 | Fix issue with collapsed anchors | Bug |


## Public API and Backward Incompatible Changes

**New** public types:
- [`GroupDocs.Editor.HtmlCss.Css.Properties.TextDecorationLineType`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.css.properties/textdecorationlinetype/)
- [`GroupDocs.Editor.Options.EbookSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebooksaveoptions/)

**New** public members:
- [`EditableDocument.GetContent`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/getcontent/#getcontent_2)

**Removed** types:
- `GroupDocs.Editor.Options.EpubSaveOptions`
- `GroupDocs.Editor.Options.Azw3SaveOptions`
