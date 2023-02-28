---
id: groupdocs-editor-for-java-23-2-release-notes
url: editor/java/groupdocs-editor-for-java-23-2-release-notes
title: GroupDocs.Editor for Java 23.2 Release Notes
weight: 80
description: ""
keywords: 
productName: GroupDocs.Editor for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for Java 23.2{{< /alert >}}

GroupDocs.Editor for Java version 23.2 contains several big features and a set of minor improvements and bug fixes compared to the previous 22.11 release. It introduces new supported format Markdown, new feature to preview the slides in presentation, as well as other improvements and a lot of bug fixes.


## New features

### Markdown support

In the GroupDocs.Editor for Java version 23.2 we've added a full support of the Markdown format - import, export, and auto-detection. Please read the [corresponding article](https://docs.groupdocs.com/editor/java/edit-markdown/) for details.

### Preview slides from presentation

In the version 23.2 we also added possibility to generate an SVG preview for the any slide in presentation. Please read the [corresponding article](https://docs.groupdocs.com/editor/java/generating-slides-preview-for-presentation/) for details.

## Other improvements and bug fixes

Fixed different bugs in mostly the new Markdown module, and also implemented a preserving of the MS Word build-in paragraph and character styles during roundtrip

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2343 | Add possibility to generate a preview for the selected slide | New feature |
| EDITORNET-1976 | Implement support of Markdown | New feature |
| EDITORNET-2340 | Support MS Word build-in styles during roundtrip | Improvement |
| EDITORNET-1983 | Cannot convert header element to Markdown | Bug |
| EDITORNET-2342 | External images in the Markdown document are not preserving | Bug |
| EDITORNET-1989 | Images are not correctly transfered during conversion Markdown to Docx | Bug |
| EDITORNET-1987 | Footnotes are not correctly transfered during conversion between DOCX and Markdown | Bug |
| EDITORNET-2014 | Exception for document editing | Bug |

## Public API and Backward Incompatible Changes

**New** public types:
- [`Options.MarkdownEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdowneditoptions/)
- [`Options.MarkdownSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/)
- [`Options.MarkdownImageLoadArgs`](https://reference.groupdocs.com/editor/java/groupdocs.editor.options/markdownimageloadargs/)
- [`Options.MarkdownImageLoadingAction`](https://reference.groupdocs.com/editor/java/groupdocs.editor.options/markdownimageloadingaction/)
- [`Options.IMarkdownImageLoadCallback`](https://reference.groupdocs.com/editor/java/groupdocs.editor.options/imarkdownimageloadcallback/)

**New** public members:
- [`PresentationDocumentInfo.generatePreview(int slideIndex)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/presentationdocumentinfo/generatepreview/)


**Removed** types and members:
None

