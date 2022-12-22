---
id: groupdocs-editor-for-net-22-12-release-notes
url: editor/net/groupdocs-editor-for-net-22-12-release-notes
title: GroupDocs.Editor for .NET 22.12 Release Notes
weight: 70
description: ""
keywords: 
productName: GroupDocs.Editor for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for .NET 22.12{{< /alert >}}

GroupDocs.Editor for .NET version 22.12 contains only several improvements and one significant bug fix, and they don't affect the public API.

## Impovements

There are two major improvements in the version 22.12: added support of the page background and border for the paginal mode in the WordProcessing module. This means that now, when editing a WordProcessing document in the paginal mode, the background and border information for the page will be preserved in HTML during forward conversion and then restored during generating output WordProcessing document from HTML.

Before this time the information about page background and border was lost during editing.

## Bug fix

There is one significant bugfix in the version 22.12 in the WordProcessing module. In order to describe it is required to explain the tricks how WordProcessing document is represented in HTML. In WordProcessing formats there can exist empty parahraphs, which may have zero characters; however, they are still displayed on the page — they occupy some empty area on the page, which can be seen visually. In HTML such paragraphs are usually represented with P element. But, unlike the MS Word, browsers collapse the empty paragraphs, even they have some content with empty text. So in order to emulate such empty paragraphs the GroupDocs.Editor adds a non-breaking whietspace into them. With non-breaking whietspaces, the browsers don't collapse these paragraphs.

However, from this solution the another problem emerges. When converting back from HTML to the WordProcessing, these whitespace characters in the empty paragraphs will be imported into the resultant document, which is not good — there were no any whitespace characters in the original document. In order to solve this new issue, the GroupDocs.Editor adds the hidden parameters for such the empty paragraphs — a special command (trick) in a form of a custom CSS property, that "tells" the backward converter in GroupDocs.Editor — do not import such whitespaces.

And this is a root of current issue — when the end-user in the WYSIWYG-editor adds some text inside such special empty paragraph, this text along with all paragraph content will be ignored during the backward conversion — the whole paragraph will be missed in the resultant WordProcessing document.

In the version 22.12 this problem is finally fixed — when some text was added in the special empty paragraph, then this paragraph will not be omitted in the resultant WordProcessing document.

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2374 | Update Nuget package tags  | Improvement |
| EDITORNET-2378 | Implement page background for forward and backward converters | Improvement |
| EDITORNET-2379 | Implement page border for forward and backward converters | Improvement |
| EDITORNET-2336 | Unable to generate the edited document | Bug |

## Public API and Backward Incompatible Changes

**New** public types:
- No new public types

**New** public members:
- No new public members


**Removed** types and members:
None

