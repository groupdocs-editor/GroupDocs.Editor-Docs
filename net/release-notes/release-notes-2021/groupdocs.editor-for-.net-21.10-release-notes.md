---
id: groupdocs-editor-for-net-21-10-release-notes
url: editor/net/groupdocs-editor-for-net-21-10-release-notes
title: GroupDocs.Editor for .NET 21.10 Release Notes
weight: 60
description: ""
keywords: 
productName: GroupDocs.Editor for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for .NET 21.10{{< /alert >}}

GroupDocs.Editor for .NET version 21.10 is a new big release with a lot of new features, improvements and bug fixes, that brings quality of document editing on a new level! Along with a tons of bugfixes and improvements, there are two new features, which significantly change the public API: audio support and new HTML parsing mechanism. More on that below.

## New features

Version 21.10 of GroupDocs.Editor contains two new features, which are described below

#### Audio support

In version 21.10 GroupDocs.Editor is able to process the MP3 audio files, embedded in the WordProcessing documents. For supporting this a new public namespace "[GroupDocs.Editor.HtmlCss.Resources.Audio](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.audio)" and two new public types inside it, — [AudioType](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.audio/audiotype) and [Mp3Audio](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.audio/mp3audio), — were added. Now, when WordProcessing document contains an embedded audio, it will be preserved and stored in the EditableDocument instance, and will be present in editable HTML representation.

#### New HTML parser

When edited document content in a form of HTML markup, CSS, images and other resources, is passing back to the GroupDocs.Editor via [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) in order to create an output document, GroupDocs.Editor internally performs a parsing of HTML markup. Before version 21.10 the HTML parser supported only a strict and narrow range of valid HTML markup, with proper tag structure, where all opening tags are properly closed, and structure is complete and well-formed. There even was a special method "[FromBodyMarkupAndResourceFolder](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkupandresourcefolder/)" for coping with inner-BODY content.

Starting from 21.10 the completely new HTML parser has replaced the old one, and now any HTML content is supportable and correctly processable, even invalid. Method "[FromBodyMarkupAndResourceFolder](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkupandresourcefolder/)" now is obsolete; instead of it a new method "[FromMarkupAndResourceFolder](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkupandresourcefolder)" is introduced.

## Improvements

#### Support of new HTML elements and CSS properties

Because of audio support (described above), a set of new HTML elements were added: AUDIO, SOURCE, and LABEL. All of them are supported in full size: on input (parsing), and output (serialization).

Also a new CSS property was added: text-align-last.

## Bugs

GroupDocs.Editor version 21.10 contains big amount of bugfixes and performance improvements, which address different issues in different modules of GroupDocs.Editor, including WordProcessing, Spreadsheet, Presentation, and HTML processing.

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2137 | Add support of embedded audio | New feature |
| EDITORNET-2138 | Develop new HTML parser with new public method | New feature |
| EDITORNET-2140 | Implement text-align-last CSS declaration | Improvement |
| EDITORNET-2141 | Implement SOURCE HTML element | Improvement |
| EDITORNET-2142 | Implement AUDIO HTML element | Improvement |
| EDITORNET-2159 | Presentation: improve quality through applying a new HTML parser | Improvement |
| EDITORNET-2099 | Presentation: failed with System.NullReferenceException after updating to Aspose.Slides ver 21.5 | Bug |
| EDITORNET-2102 | Insufficient formatting while converting HTML to Spreadsheet | Bug |
| EDITORNET-2132 | Presentation: Strikethrough and Underline formatting (html to presentation processing) | Bug |

## Public API and Backward Incompatible Changes

New public types:

1. [GroupDocs.Editor.HtmlCss.Resources.Audio.Mp3Audio](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.audio/mp3audio)
2. [GroupDocs.Editor.HtmlCss.Resources.Audio.AudioType](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.audio/audiotype)

New public methods and properties:

1. [GroupDocs.Editor.EditableDocument.Audio](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/audio) - instance property
2. [GroupDocs.Editor.EditableDocument.FromMarkupAndResourceFolder(string newHtmlContent, string resourceFolderPath)](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkupandresourcefolder) - static method

**Obsolete** methods:

1. [GroupDocs.Editor.EditableDocument.FromBodyMarkupAndResourceFolder(string htmlBodyContent, string resourceFolderPath)](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/frommarkupandresourcefolder/) - static method
