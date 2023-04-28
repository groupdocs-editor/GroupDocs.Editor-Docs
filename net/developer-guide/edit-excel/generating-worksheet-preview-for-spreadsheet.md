---
id: generating-worksheets-preview-for-spreadsheet
url: editor/net/generating-worksheets-preview-for-spreadsheet
title: Generating worksheets (tabs) preview for spreadsheet
weight: 2
description: "This article describes how to generate a preview for any worksheet (tab) for the existing Excel spreadsheet in SVG format"
keywords: preview worksheet tab excel spreadsheet SVG
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
Starting from [version 23.4](https://docs.groupdocs.com/editor/net/groupdocs-editor-for-net-23-4-release-notes/), the GroupDocs.Editor for .NET allows to generate a preview for any worksheet (a.k.a. _tab_) in the spreadsheet (a.k.a. _workbook_ document in SVG format. With this feature the end-users are able to view and inspect the content of the spreadsheet without actually sending it for edit. This generated worksheet preview cannot be edited using the GroupDocs.Editor for .NET itself, but it can be saved and then viewed in any desktop or online image viewer as well in the browser (because any modern browser actually supports viewing of SVG format).

This feature is working regardless of the licensing mode of the GroupDocs.Editor for .NET: it works the same for both trial and licensed mode, there are no trial limitations for this feature. While generating the worksheets preview, the GroupDocs.Editor doesn’t write off the consumed bytes or credits.

Excel spreadsheets may have the so-called _hidden worksheets_, — GroupDocs.Editor for .NET generates an SVG preview for them too.

For generating the worksheets preview the new instance method [`GeneratePreview(int worksheetIndex)`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/spreadsheetdocumentinfo/generatepreview/) from the struct [`GroupDocs.Editor.Metadata.SpreadsheetDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/spreadsheetdocumentinfo/) should be used, and worksheet is specified by its zero-based _index_ (do not confuse with the worksheet _numbers_, which are 1-based). If the specified index is lesser than 0 or exceeds the number of worksheets within a given spreadsheet, then the [System.ArgumentOutOfRangeException](https://learn.microsoft.com/en-us/dotnet/api/system.ArgumentOutOfRangeException?view=net-7.0) exception will be thrown. Worksheets previews can be generated for both encoded and unencoded spreadsheets; for encoded the end-user must specify a valid password in the [`GetDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/getdocumentinfo/) method.

The [`GeneratePreview(int worksheetIndex)`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/spreadsheetdocumentinfo/generatepreview/) method returns a worksheet preview as an SVG vector image, that is encapsulated in the [`SvgImage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) public class from the [`GroupDocs.Editor.HtmlCss.Resources.Images.Vector`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/) namespace. This class has all necessary methods and properties to obtain the content of an SVG image in any desired form, save it to disk, stream and so on.

The code sample below shows opening an unprotected spreadsheet file, obtaining a number of all worksheets inside this spreadsheet, and then generating the previews for every worksheet in a loop. Then these previews are saved to the disk.

```csharp
// Obtain a valid full path to the spreadsheet file
string inputPath = "Sample.xlsx";

// Load spreadsheet file to the Editor constructor
using (GroupDocs.Editor.Editor editor = new Editor(inputPath))
{
	// Get document info for this file
	IDocumentInfo infoUncasted = editor.GetDocumentInfo(null);

	//Make sure it is really a Spreadsheet of XLSX format
	Assert.AreEqual(Formats.SpreadsheetFormats.Xlsx, infoUncasted.Format);

	// Cast this document info to the SpreadsheetDocumentInfo type
	SpreadsheetDocumentInfo infoSpreadsheet = (SpreadsheetDocumentInfo)infoUncasted;

	// Get the number of all worksheets
	int worksheetsCount = infoSpreadsheet.PageCount;

	//Iterate through all worksheets and generate the preview on every iteration
	for (int worksheetIndex = 0; worksheetIndex < worksheetsCount; worksheetIndex++)
	{
		// Generate one preview as SVG image by worksheet index
		SvgImage oneSvgPreview = infoSpreadsheet.GeneratePreview(worksheetIndex);

		// Save SVG preview to the file
		oneSvgPreview.Save(Path.Combine(outputFolder, oneSvgPreview.FilenameWithExtension));
	}
}
```

Concluding, the new worksheets preview feature is by its essence a new public method in the existing [`GroupDocs.Editor.Metadata.SpreadsheetDocumentInfo`](https://reference.groupdocs.com/editor/net/groupdocs.editor.metadata/spreadsheetdocumentinfo/) struct, that obtains a worksheet index and returns an instance of [`SvgImage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class. If the end-user needs to obtain a preview of the worksheet in a raster format, but not in the vector, he can use the [`SaveToPng`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/savetopng/) method in the [`SvgImage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class — this method converts the SVG content of the current [`SvgImage`](https://reference.groupdocs.com/editor/net/groupdocs.editor.htmlcss.resources.images.vector/svgimage/) to the PNG format and saves it to the specified [`System.IO.Stream`](https://learn.microsoft.com/en-us/dotnet/api/system.IO.Stream?view=net-6.0).