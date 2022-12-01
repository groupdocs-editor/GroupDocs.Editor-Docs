---
id: groupdocs-editor-for-net-22-11-release-notes
url: editor/net/groupdocs-editor-for-net-22-11-release-notes
title: GroupDocs.Editor for .NET 22.11 Release Notes
weight: 80
description: ""
keywords: 
productName: GroupDocs.Editor for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for .NET 22.11{{< /alert >}}

GroupDocs.Editor for .NET version 22.11 contains several big features and a set of minor improvements and bug fixes compared to the previous 22.9 release. It introduces new supported format Markdown, new feature to preview the slides in presentation, as well as other improvements and a lot of bug fixes. In GroupDocs.Editor for .NET we also dropped support of .NET 5.0 in favor of new .NET 6.0.

## Runtime information

### Added .NET 6.0

GroupDocs.Editor for .NET version 22.11 is the first version where we've added support of .NET 6.0 runtime.

### Removed .NET 5.0

As it was mentioned in previous [release notes](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-9-release-notes/), in this version we've dropped support of the .NET 5.0. So now a GoupDocs.Editor for .NET supports 3 runtimes:
1. .NET Framework 4.6.1
2. .NET Standard 2.0
3. .NET 6.0

If you need to use a .NET Framework version older than .NET Framework 4.6.1, please use the GroupDocs.Editor for .NET of a previous version - 22.7.

If you need to use a .NET 5.0, please use the GroupDocs.Editor for .NET version 22.9.

## New features

### Markdown support

In the GroupDocs.Editor for .NET version 22.11 we've added a full support of the Markdown format - import, export, and auto-detection. Please read the [corresponding article](https://docs.groupdocs.com/editor/net/edit-markdown/) for details.

### Preview slides from presentation

In the version 22.11 we also added possibility to generate an SVG preview for the any slide in presentation. Please read the [corresponding article](https://docs.groupdocs.com/editor/net/generating-slides-preview-for-presentation/) for details.

## Other improvements and bug fixes

Fixed different bugs in mostly the new Markdown module, and also implemented a preserving of the MS Word build-in paragraph and character styles during roundtrip

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2334 | Add support of .NET 6.0 to GD.Editor and other tasks | New feature |
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
- [`Options.MarkdownEditOptions`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.options/markdowneditoptions/)
- [`Options.MarkdownSaveOptions`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.options/markdownsaveoptions/)
- [`Options.MarkdownImageLoadArgs`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.options/markdownimageloadargs/)
- [`Options.MarkdownImageLoadingAction`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.options/markdownimageloadingaction/)
- [`Options.IMarkdownImageLoadCallback`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.options/imarkdownimageloadcallback/)

**New** public members:
- [`PresentationDocumentInfo.GeneratePreview(int slideIndex)`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.metadata/presentationdocumentinfo/generatepreview/)


**Removed** types and members:
None

