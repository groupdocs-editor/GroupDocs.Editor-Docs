---
id: generating-slides-preview-for-presentation
url: editor/net/generating-slides-preview-for-presentation
title: Generating slides preview for presentation
weight: 4
description: "This article describes how to generate a preview for any slide for the existing PowerPoint presentation in SVG format"
keywords: preview slide powerpoint presentation SVG
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
Starting from [version 22.11](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-22-11-release-notes/), the GroupDocs.Editor for .NET allows generating the preview for any slide in the presentation document in SVG format. This feature allows the end-user to view and inspect the content of the presentation without actually sending it for edit. This generated slide preview cannot be edited, but it can be viewed in any desktop or online image viewer as well in the browser (because any modern browser actually supports SVG format).

This feature allows end-users to generate a preview for any slide within the presentation regardless of the licensing mode of the GroupDcos.Editor for .NET: it works the same for both trial and licensed mode, there are no trial limitations for this feature. While generating the slides preview, the GroupDocs.Editor doesn’t write off the consumed bytes or credits.

For generating the slides preview the new instance method [`GeneratePreview(int slideIndex)`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.metadata/presentationdocumentinfo/generatepreview/) from the struct [`GroupDocs.Editor.Metadata.PresentationDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/presentationdocumentinfo/) should be used, and slide is specified by its zero-based _index_ (do not confuse with the slide _numbers_, which are 1-based). If the specified index is lesser than 0 or exceeds the number of slides within a given presentation, then the [System.ArgumentOutOfRangeException](https://learn.microsoft.com/en-us/dotnet/api/system.ArgumentOutOfRangeException?view=net-7.0) exception will be thrown. Slide previews can be generated for both encoded and unencoded presentations; for encoded the end-user must specify a valid password in the [`GetDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo/) method.

The [`GeneratePreview(int slideIndex)`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.metadata/presentationdocumentinfo/generatepreview/) method returns a slide preview as an SVG vector image, that is encapsulated in the [`SvgImage`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) public class from the [`GroupDocs.Editor.HtmlCss.Resources.Images.Vector`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/) namespace. This class has all necessary methods and properties to obtain the content of an SVG image in any desired form, save it to disk and so on.

The code sample below shows opening an unprotected presentation file and generating the previews for every slide. Then these previews are saved to the disk.

```csharp
// Obtain a valid full path to the presentation file
string inputPath = "Sample.pptx";

// Load file to the Editor constructor
using (GroupDocs.Editor.Editor editor = new Editor(inputPath))
{
	// Get document info for this file
	IDocumentInfo infoUncasted = editor.GetDocumentInfo(null);

	// Cast this document info to the PresentationDocumentInfo type
	PresentationDocumentInfo infoSlides = (PresentationDocumentInfo)infoUncasted;

	// Get the number of all slides
	int slidesCount = infoSlides.PageCount;

	//Iterate through all slides and generate the preview on every iteration
	for (int i = 0; i < slidesCount; i++)
	{
		// Generate one preview as SVG image by slide index
		SvgImage oneSvgPreview = infoSlides.GeneratePreview(i);

		// Save it to the file
		oneSvgPreview.Save(Path.Combine(outputFolder, oneSvgPreview.FilenameWithExtension));
	}
}
```

Concluding, the new slides preview feature is by its essence a new public method in the existing [`GroupDocs.Editor.Metadata.PresentationDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/presentationdocumentinfo/) struct, that obtains a slide index and returns an instance of [`SvgImage`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class. If the end-user needs to obtain a preview of the slide in a raster format, but not in the vector, he can use the [`SaveToPng`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/savetopng/) method in the [`SvgImage`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class — this method converts the SVG content of the current [`SvgImage`](https://reference2.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) to the PNG format and saves it to the specified [`System.IO.Stream`](https://learn.microsoft.com/en-us/dotnet/api/system.IO.Stream?view=net-6.0).




