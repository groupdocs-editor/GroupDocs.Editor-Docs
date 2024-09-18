---
id: generating-worksheets-preview-for-spreadsheet
url: editor/java/generating-worksheets-preview-for-spreadsheet
title: Generating worksheets (tabs) preview for spreadsheet
weight: 2
description: "This article describes how to generate a preview for any worksheet (tab) for the existing Excel spreadsheet in SVG format"
keywords: preview worksheet tab excel spreadsheet SVG
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
Starting from [version 23.9](https://releases.groupdocs.com/editor/java/release-notes/2023/groupdocs-editor-for-java-23-9-release-notes/), the GroupDocs.Editor for Java allows to generate a preview for any worksheet (a.k.a. _tab_) in the spreadsheet (a.k.a. _workbook_) document in SVG format. With this feature the end-users are able to view and inspect the content of the spreadsheet without actually sending it for edit. This generated worksheet preview cannot be edited using the GroupDocs.Editor for Java itself, but it can be saved and then viewed in any desktop or online image viewer as well in the browser (because any modern browser actually supports viewing of SVG format).

This feature is working regardless of the licensing mode of the GroupDocs.Editor for Java: it works the same for both trial and licensed mode, there are no trial limitations for this feature. While generating the worksheets preview, the GroupDocs.Editor doesn’t write off the consumed bytes or credits.

Excel spreadsheets may have the so-called _hidden worksheets_, — GroupDocs.Editor for Java generates an SVG preview for them too.

For generating the worksheets preview for a particular spreadsheet document the user must perform the next steps:

- Load a desired spreadsheet file to the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class.
- Call the [`getDocumentInfo()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-) method and specify a password of a loaded spreadsheet in case if this spreadsheet is protected with a password.
- In the obtained struct [`com.groupdocs.editor.metadata.SpreadsheetDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/spreadsheetdocumentinfo/) invoke a new instance method [`generatePreview(int worksheetIndex)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/spreadsheetdocumentinfo/#generatePreview-int-) and in this method specify a zero-based _index_ (do not confuse with the worksheet _numbers_, which are 1-based) of the desired worksheet. If the specified index is lesser than 0 or exceeds the number of worksheets within a given spreadsheet, then the [IllegalArgumentException](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html) exception will be thrown.
- The [`generatePreview(int worksheetIndex)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/spreadsheetdocumentinfo/#generatePreview-int-) method returns a worksheet preview as an SVG vector image, that is encapsulated in the [`SvgImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/) public class from the [`com.groupdocs.editor.htmlcss.resources.images.vector`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/) namespace. This class has all necessary methods and properties to obtain the content of an SVG image in any desired form, save it to disk, stream and so on.

The code sample below shows opening an unprotected spreadsheet file, obtaining a number of all worksheets inside this spreadsheet, and then generating the previews for every worksheet in a loop. Then these previews are saved to the disk.

```java
// Obtain a valid full path to the spreadsheet file
String inputPath = "Sample.xlsx";

// Load spreadsheet file to the Editor constructor
Editor editor = new Editor(inputPath);
{
	// Get document info for this file
	IDocumentInfo infoUncasted = editor.getDocumentInfo(null);

	//Make sure it is really a Spreadsheet of XLSX format
	Assert.assertEquals(SpreadsheetFormats.Xlsx, infoUncasted.getFormat());

	// Cast this document info to the SpreadsheetDocumentInfo type
	SpreadsheetDocumentInfo infoSpreadsheet = (SpreadsheetDocumentInfo)infoUncasted;

	// Get the number of all worksheets
	int worksheetsCount = infoSpreadsheet.getPageCount();

	//Iterate through all worksheets and generate the preview on every iteration
	for (int worksheetIndex = 0; worksheetIndex < worksheetsCount; worksheetIndex++)
	{
		// Generate one preview as SVG image by worksheet index
		SvgImage oneSvgPreview = infoSpreadsheet.generatePreview(worksheetIndex);

		// Save SVG preview to the file
		oneSvgPreview.save("Sample_out_image"+worksheetIndex+"."+ oneSvgPreview.getFilenameWithExtension()));
	}
}
```

Concluding, the new worksheets preview feature is by its essence a new public method in the existing [`com.groupdocs.editor.metadata.SpreadsheetDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/spreadsheetdocumentinfo/) struct, that obtains a worksheet index and returns an instance of [`SvgImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class. If the end-user needs to obtain a preview of the worksheet in a raster format, but not in the vector, he can use the [`saveToPng`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/#saveToPng-java.io.InputStream-) method in the [`SvgImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class — this method converts the SVG content of the current [`SvgImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/) to the PNG format and saves it to the specified [`OutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/OutputStream.html).