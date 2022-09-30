---
id: groupdocs-editor-for-net-22-9-release-notes
url: editor/net/groupdocs-editor-for-net-22-9-release-notes
title: GroupDocs.Editor for .NET 22.9 Release Notes
weight: 90
description: ""
keywords: 
productName: GroupDocs.Editor for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for .NET 22.9{{< /alert >}}

GroupDocs.Editor for .NET version 22.9 contains a lot of new features, improvements and bug fixes compared to the previous 22.7 release. It introduces new supported formats, improved processing quality of the already supported formats, as well as other improvements and a lot of bug fixes. In GroupDocs.Editor for .NET we also added support of .NET 5.0 and dropped support of the .NET Framework 2.0 

## New features and improvements

### Added .NET 5.0

GroupDocs.Editor for .NET version 22.9 is the first version where we've added support of .NET 5.0 runtime. But keep in mind, that we plan to drop .NET 5.0 in favor of .NET 6.0 in the next release, so .NET 5.0 should be considered as an intermediate step to the final .NET 6.0.

### Removed .NET Framework 2.0

As it was mentioned in previous [release notes](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-7-release-notes/), in this version we've dropped support of the .NET Framework 2.0. Now a GoupDocs.Editor for .NET supports 3 runtimes:
1. .NET Framework 4.6.1
2. .NET Standard 2.0
3. .NET 5.0

If you need to use a .NET Framework version older than .NET Framework 4.6.1, please use the GroupDocs.Editor for .NET of a previous version - 22.7.

### Epub support

In the GroupDocs.Editor for .NET version 22.9 we've added a full support of the ePub format - import, export, and auto-detection. Please read the [corresponding article](https://docs.groupdocs.com/editor/net/how-to-edit-ebook/) for details.

### AZW3 and MHTML for export

In the version 22.9 we also added possibility to export documents in MHTML and AZW3 formats - now there are special save options for them.

## Runtime version information

GroupDocs.Editor for .NET version 22.9 is the latest product, which is released for the platforms/runtimes .NET Framework 4.6.1, .NET Standard 2.0, and .NET 5.0.

{{< alert style="info" >}}In the next releases the support of .NET 5.0 will be dropped and support of .NET 6.0 will be added.{{< /alert >}}


## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2305 | Add support of .NET 5.0 to GD.Editor | New feature |
| EDITORNET-2274 | Remove support of .NET Framework 2.0 from GroupDocs.Editor | New feature |
| EDITORNET-2276 | Implement support of Epub on import and export | New feature |
| EDITORNET-2296 | Add support of AZW3 format on export | New feature |
| EDITORNET-2297 | Add support of MHTML format on export | New feature |
| EDITORNET-2298 | Introduce common edit options for all E-book formats | New feature |
| EDITORNET-2293 | Add support of OST email format to format detector | Improvement |
| EDITORNET-2294 | Add support of MHTML and CHM formats to format detector | Improvement |
| EDITORNET-1890 | Exception of type 'GroupDocs.Editor.PasswordRequiredException' was thrown. | Bug |
| EDITORNET-2239 | Investigate and fix issue with API reference | Bug |
| EDITORNET-2248 | Fix NRE for specific document | Bug |
| EDITORNET-2278 | Cannot access a closed file while working with xml file | Bug |
| EDITORNET-2287 | InvalidCastException in ShapeProcessor during forward conversion of a specific file | Bug |
| EDITORNET-2288 | NullReferenceException in FieldProcessor during forward conversion of a specific file | Bug |
| EDITORNET-2290 | Several ePub saving options are not properly working | Bug |
| EDITORNET-2300 | Fix issue with HTML attribute parser on Linux | Bug |


## Public API and Backward Incompatible Changes

**New** public types:
- [`Options.EbookEditOptions`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions/)
- [`Options.Azw3SaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/azw3saveoptions/)
- [`Options.MhtmlSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/mhtmlsaveoptions/)
- [`Options.EpubSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/epubsaveoptions/)

**New** public members:
- `TextualFormats.Chm`
- `TextualFormats.Mhtml`
- `EmailFormats.Ost`

**Removed** types and members:
- `Options.MobiEditOptions` class - replaced with [`EbookEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emaileditoptions/) struct

