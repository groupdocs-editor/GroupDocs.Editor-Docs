---
id: generating-slides-preview-for-presentation
url: editor/java/generating-slides-preview-for-presentation
title: Generating slides preview for presentation
weight: 4
description: "This article describes how to generate a preview for any slide for the existing PowerPoint presentation in SVG format"
keywords: preview slide powerpoint presentation SVG
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
Starting from [version 23.2](https://docs.groupdocs.com/editor/java/groupdocs-editor-for-java-23-2-release-notes/), the GroupDocs.Editor for Java allows generating the preview for any slide in the presentation document in SVG format. This feature allows the end-user to view and inspect the content of the presentation without actually sending it for edit. This generated slide preview cannot be edited, but it can be viewed in any desktop or online image viewer as well in the browser (because any modern browser actually supports SVG format).

This feature allows end-users to generate a preview for any slide within the presentation regardless of the licensing mode of the GroupDcos.Editor for Java: it works the same for both trial and licensed mode, there are no trial limitations for this feature. While generating the slides preview, the GroupDocs.Editor doesn’t write off the consumed bytes or credits.

For generating the slides preview the new instance method [`generatePreview(int slideIndex)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/presentationdocumentinfo/#generatePreview-int-) from the struct [`com.groupdocs.editor.metadata.PresentationDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/presentationdocumentinfo/) should be used, and slide is specified by its zero-based _index_ (do not confuse with the slide _numbers_, which are 1-based). If the specified index is lesser than 0 or exceeds the number of slides within a given presentation, then the [IllegalArgumentException](https://docs.oracle.com/javase/7/docs/api/java/lang/IllegalArgumentException.html) exception will be thrown. Slide previews can be generated for both encoded and unencoded presentations; for encoded the end-user must specify a valid password in the [`getDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#getDocumentInfo-java.lang.String-) method.

The [`generatePreview(int slideIndex)`](https://reference.groupdocs.com/editor/java/groupdocs.editor.metadata/presentationdocumentinfo/#generatePreview-int-) method returns a slide preview as an SVG vector image, that is encapsulated in the [`SvgImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/) public class from the [`com.groupdocs.editor.htmlcss.resources.images.vector`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/) namespace. This class has all necessary methods and properties to obtain the content of an SVG image in any desired form, save it to disk and so on.

The code sample below shows opening an unprotected presentation file and generating the previews for every slide. Then these previews are saved to the disk.

```java
// Obtain a valid full path to the presentation file
String inputPath = "Sample.pptx";

// Load file to the Editor constructor
Editor editor = new Editor(inputPath);
{
	// Get document info for this file
	IDocumentInfo infoUncasted = editor.getDocumentInfo(null);

	// Cast this document info to the PresentationDocumentInfo type
	PresentationDocumentInfo infoSlides = (PresentationDocumentInfo)infoUncasted;

	// Get the number of all slides
	int slidesCount = infoSlides.getPageCount();

	//Iterate through all slides and generate the preview on every iteration
	for (int i = 0; i < slidesCount; i++)
	{
		// Generate one preview as SVG image by slide index
		SvgImage oneSvgPreview = infoSlides.generatePreview(i);

		// Save it to the file
		oneSvgPreview.save(new File(outputFolder, oneSvgPreview.getFilenameWithExtension()).getPath());
	}
}
```

Concluding, the new slides preview feature is by its essence a new public method in the existing [`com.groupdocs.editor.metadata.PresentationDocumentInfo`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.metadata/presentationdocumentinfo/) struct, that obtains a slide index and returns an instance of [`SvgImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class. If the end-user needs to obtain a preview of the slide in a raster format, but not in the vector, he can use the [`saveToPng`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/#saveToPng-java.io.InputStream-) method in the [`SvgImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/) class — this method converts the SVG content of the current [`SvgImage`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/svgimage/) to the PNG format and saves it to the specified [`InputStream`](https://docs.oracle.com/javase/7/docs/api/java/io/InputStream.html).




