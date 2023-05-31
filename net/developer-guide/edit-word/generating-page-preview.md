---
id: generating-page-preview
url: editor/net/generating-page-preview
title: Generating page preview for WordProcessing document
weight: 12
description: "This article describes how to generate a preview for any page for the existing WordProcessing document in SVG format using the GroupDocs.Editor"
keywords: preview page WordProcessing Word DOCX SVG
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
Starting from the [version 23.5](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-5-release-notes/) the GroupDocs.Editor for .NET allows to generate a preview for arbitrary page in the loaded WordProcessing document, like DOC, DOCX, DOCM, RTF, ODT etc. The preview is generated in the SVG format, which is a vector graphics format and is supported in the numerous image viewers and also by any modern browser.

Using this feature the users can view and inspect any document of the imported WordProcessing document without actually editing it. This preview cannot be edited by the GroupDocs.Editor itself, it can be only obtained in SVG format and saved as usual - to the byte stream or file.

This feature is working regardless of the licensing mode of the GroupDocs.Editor for .NET: it works the same for both trial and licensed mode, there are no trial limitations for this feature. While generating the pages preview, the GroupDocs.Editor doesn’t write off the consumed bytes or credits.

With this feature was introduced, the GroupDocs.Editor for .NET finally supports generating a preview for all three major office format families: WordProcessing, [Spreadsheet](https://docs.groupdocs.com/editor/net/generating-worksheets-preview-for-spreadsheet/) and [Presentation](https://docs.groupdocs.com/editor/net/generating-slides-preview-for-presentation/).

For generating the pages preview for a particular WordProcessing document the user must perform the next steps:

- Load a desired WordProcesing file to the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/) class.
- Call the [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo/) method and specify a password for the loaded document in case if this document is protected with a password.
- Because the loaded file has a format from the WordProcessing family (like DOCX, RTF, etc), then the [`GetDocumentInfo()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo/) method must return the instance of the [`GroupDocs.Editor.Metadata.WordProcessingDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/wordprocessingdocumentinfo/) struct, casted to the [`IDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo/) interface. Cast it back from [`IDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/idocumentinfo/) to the [`WordProcessingDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/wordprocessingdocumentinfo/) type.
- In the obtained struct [`WordProcessingDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/wordprocessingdocumentinfo/) invoke a new instance method [`GeneratePreview(int pageIndex)`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/wordprocessingdocumentinfo/generatepreview/) and in this method specify a zero-based _index_ (do not confuse with the page _numbers_, which are 1-based) of the desired page. If the specified index is lesser than 0 or exceeds the number of pages within a given document, then the [System.ArgumentOutOfRangeException](https://learn.microsoft.com/en-us/dotnet/api/system.ArgumentOutOfRangeException?view=net-7.0) exception will be thrown.
- The [`GeneratePreview(int pageIndex)`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/wordprocessingdocumentinfo/generatepreview/) method returns a page preview as an SVG vector image, that is encapsulated in the [`SvgImage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) public class from the [`GroupDocs.Editor.HtmlCss.Resources.Images.Vector`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/) namespace. This class has all necessary methods and properties to obtain the content of an SVG image in any desired form, save it to disk, stream and so on.

The code sample below shows opening an unprotected WordProcessing DOCX file, obtaining a number of all pages inside this DOCX file, and then generating the previews for every page in a loop. Then these previews are saved to the disk. Please note that the GroupDocs.Editor is working in the trial mode, but page preview is generated for all the pages, and no trial copyright marks are present on these previews.

```csharp
// Obtain a valid full path to the WordProcessing file
string inputPath = "SampleDoc.docx";

// Load WordProcessing file to the Editor constructor
using (GroupDocs.Editor.Editor editor = new Editor(inputPath))
{
	// Get document info for this file
	IDocumentInfo infoUncasted = editor.GetDocumentInfo(null);

	//Make sure it is really a WordProcessing of DOCX format
	Assert.AreEqual(Formats.WordProcessingFormats.Docx, infoUncasted.Format);

	// Cast this document info to the WordProcessingDocumentInfo type
	WordProcessingDocumentInfo infoWordProcessing = (WordProcessingDocumentInfo)infoUncasted;

	// Get the number of all pages
	int pagesCount = infoWordProcessing.PageCount;	

	//Iterate through all pages and generate the preview on every iteration
	for (int pageIndex = 0; pageIndex < pagesCount; pageIndex++)
	{
		// Generate one preview as SVG image by page index
		SvgImage oneSvgPreview = infoWordProcessing.GeneratePreview(pageIndex);

		// Save SVG preview to the file
		oneSvgPreview.Save(Path.Combine(outputFolder, oneSvgPreview.FilenameWithExtension));
	}
}
```

All WordProcessing formats are able to store the raster images. So when the specific page of some loaded document has one or more raster images, and for this page the SVG preview is generated, this/these raster image(s) will be embedded inside SVG in base64 format using the [data URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme).

Concluding, the new page preview feature is by its essence a new public method in the existing [`WordProcessingDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/wordprocessingdocumentinfo/) struct, that obtains a page index and returns an instance of [`SvgImage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class. If the end-user needs to obtain a preview of the page in a raster format, but not in the vector, he can use the [`SaveToPng`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/savetopng/) method in the [`SvgImage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class — this method converts the SVG content of the current [`SvgImage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) to the PNG format and saves it to the specified [`System.IO.Stream`](https://learn.microsoft.com/en-us/dotnet/api/system.IO.Stream?view=net-6.0).