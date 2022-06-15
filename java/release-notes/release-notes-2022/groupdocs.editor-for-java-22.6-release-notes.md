---
id: groupdocs-editor-for-java-22-6-release-notes
url: editor/java/groupdocs-editor-for-java-22-6-release-notes
title: GroupDocs.Editor for Java 22.6 Release Notes
weight: 60
description: ""
keywords: 
productName: GroupDocs.Editor for Java
hideChildren: False
---

{{< alert style="info" >}}## NOTE: Starting from this release of the GroupDocs.Editor for Java will support version Java 8 and above.{{< /alert >}}
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for Java 22.6{{< /alert >}}

GroupDocs.Editor for Java version 22.6 is a new big release with a lot of new features, improvements and bug fixes, that brings quality of document editing on a new level! Along with a tons of bugfixes and improvements, there are two new features, which significantly change the public API: audio support and new HTML parsing mechanism. More on that below.

## New features

Version 22.6 of GroupDocs.Editor contains two new features, which are described below

#### Audio support

In version 22.6 GroupDocs.Editor is able to process the MP3 audio files, embedded in the WordProcessing documents. For supporting this a new public namespace "[com.groupdocs.editor.htmlcss.resources.audio](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.audio/package-frame)" and two new public types inside it, — [AudioType](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.audio/AudioType) and [Mp3Audio](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.audio/Mp3Audio), — were added. Now, when WordProcessing document contains an embedded audio, it will be preserved and stored in the EditableDocument instance, and will be present in editable HTML representation.

#### New HTML parser

When edited document content in a form of HTML markup, CSS, images and other resources, is passing back to the GroupDocs.Editor via [EditableDocument](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument) in order to create an output document, GroupDocs.Editor internally performs a parsing of HTML markup. Before version 22.6 the HTML parser supported only a strict and narrow range of valid HTML markup, with proper tag structure, where all opening tags are properly closed, and structure is complete and well-formed. There even was a special method "[fromBodyMarkupAndResourceFolder](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#fromBodyMarkupAndResourceFolder(java.lang.String,%20java.lang.String))" for coping with inner-BODY content.

Starting from 22.6 the completely new HTML parser has replaced the old one, and now any HTML content is supportable and correctly processable, even invalid. Method "[fromBodyMarkupAndResourceFolder](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#fromBodyMarkupAndResourceFolder(java.lang.String,%20java.lang.String))" now is depracated; instead of it a new method "[fromMarkupAndResourceFolder](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#fromMarkupAndResourceFolder(java.lang.String,%20java.lang.String))" is introduced.

## Improvements

#### Support of new HTML elements and CSS properties

Because of audio support (described above), a set of new HTML elements were added: AUDIO, SOURCE, and LABEL. All of them are supported in full size: on input (parsing), and output (serialization).

Also a new CSS property was added: text-align-last.

## Bugs

GroupDocs.Editor version 22.6 contains big amount of bugfixes and performance improvements, which address different issues in different modules of GroupDocs.Editor, including WordProcessing, Spreadsheet, Presentation, and HTML processing.

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

1. [com.groupdocs.editor.htmlcss.resources.audio.Mp3Audio](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.audio/Mp3Audio)
2. [com.groupdocs.editor.htmlcss.resources.audio.AudioType](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.audio/AudioType)

New public methods and properties:

1. [com.groupdocs.editor.htmlcss.resources.audio](https://apireference.groupdocs.com/editor/net/groupdocs.editor/editabledocument/properties/audio) - instance property
2. [com.groupdocs.editor.EditableDocument.fromMarkupAndResourceFolder(String newHtmlContent, String resourceFolderPath)](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#fromMarkupAndResourceFolder(java.lang.String,%20java.lang.String)) - static method

**Deprecated** methods:

1. [com.groupdocs.editor.EditableDocument.fromBodyMarkupAndResourceFolder(String htmlBodyContent, String resourceFolderPath)](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/EditableDocument#fromBodyMarkupAndResourceFolder(java.lang.String,%20java.lang.String)) - static method
