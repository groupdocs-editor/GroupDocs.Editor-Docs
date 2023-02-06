---
id: edit-markdown
url: editor/java/edit-markdown
title: Edit Markdown documents 
linktitle: Edit Markdown documents
weight: 95
description: "This guide demonstrates how to edit content of the Markdown documents/files like a common text documents using a GroupDocs.Editor for Java."
keywords: Edit Markdown, Edit Markdown file, Edit Markdown document, Edit MD, Edit MD file, Edit MD document, Import Markdown, Export markdown
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit Markdown document in the GroupDocs.Editor
        description: How to edit Markdown document using the GroupDocs.Editor in Java language
        productCode: editor
        productPlatform: java
    showVideo: True
    howTo:
        name: How to edit content of the Markdown document in the GroupDocs.Editor in Java
        description: Learn how to edit content of the Markdown (MD) document with different editing options using the GroupDocs.Editor in Java step by step
        steps:
        - name: Load desired Markdown document to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired Markdown document into it by a file path or a stream.          
        - name: Implement IMarkdownImageLoadCallback interface
          text: If input Markdown document has images, for preserving them it is necessary to create a IMarkdownImageLoadCallback inheritor, that will provide image data to the GroupDocs.Editor          
        - name: Prepare a MarkdownEditOptions class
          text: Create an instance of the MarkdownEditOptions class and specify a previously created inheritor of the IMarkdownImageLoadCallback.
        - name: Call Editor.Edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.Edit method with specifying a previously prepared MarkdownEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the document content in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited document content into the most suitable static methods of the class
        - name: Prepare a MarkdownSaveOptions class
          text: Create an instance of the MarkdownSaveOptions class and adjust its properties to meet your needs if necessary. Select saving images as embedded in base64 encoding (ExportImagesAsBase64 property) or as external files in specified folder (ImagesFolder property). All properties are optional.
        - name: Save edited document with Editor.Save method
          text: Pass an instance of EditableDocument with content of the edited document, instance of the MarkdownSaveOptions, and a destination byte stream or file path to the Editor.Save method for saving the document.
---
> This example demonstrates the standard open-edit-save pipeline with Markdown (MD) documents, using different options on every step.

## Introduction

[Markdown](https://docs.fileformat.com/word-processing/md/) is a lightweight markup language, which became popular last time. Markdown files with an `*.md` extension are actually plain text files, contain special syntax, support text formatting, tables, lists, images and so on. There are actually several dialects of markdown, including GFM, CommonMark, Multi-Markdown and so on.

Starting from the [version 23.2](https://docs.groupdocs.com/editor/java/groupdocs-editor-for-java-23-2-release-notes/), GroupDocs.Editor for Java fully supports the Markdown format on both import, export, and also its auto-detection.

As for the [version 23.2](https://docs.groupdocs.com/editor/java/groupdocs-editor-for-java-23-2-release-notes/), GroupDocs.Editor for Java supports the following Markdown features, which mostly follow the CommonMark specification and are represented as appropriate styles or direct formatting:

- Headings
- Blockquotes
- Code blocks
- Horizontal rules
- Bold emphasis
- Italic emphasis
- StrikeThrough formatting
- Numbered and bulleted lists
- Tables
- Internal images, stored with base64 encoding
- External images

## Loading

Loading of the Markdown documents to the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class is usual and the same as for other formats. There are no dedicated load options for the Markdown format, it is enough to specify the file itself through file path or [`InputStream`](https://docs.oracle.com/javase/7/docs/api/java/io/InputStream.html).

Code example below shows loading the same Markdown file to the two [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) instances through file path and a byte stream, and then checking how the GroupDocs.Editor will detect the format of specified file.

```java
String filename = "sample.md";
String inputPath = System.IO.Path.Combine("markdown folder", filename);

Editor fromStream = new Editor(new FileInputStream(inputPath));
Editor fromPath = new Editor(inputPath);

com.groupdocs.editor.metadata.IDocumentInfo info = fromPath.getDocumentInfo(null);
Assert.assertEquals(com.groupdocs.editor.formats.TextualFormats.Md, info.getFormat());


```

## Editing

There is a special class [`MarkdownEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdowneditoptions/) for editing the Markdown files. As always, it is not mandatory when editing a document, so the [`Editor.edit()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) overload without parameter may be used — GroupDocs.Editor will automatically detect the format and apply the default options.

However, specifying the custom [`MarkdownEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdowneditoptions/) may be vital when an input Markdown file has external images; “external” means that images are stored somewhere else, and in the Markdown code there are _links_ to these images. When [`MarkdownEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdowneditoptions/) are not specified and correctly tuned (explained below), the GroupDocs.Editor will not be able to locate such images. In order to point all external images, the instance of [`MarkdownEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdowneditoptions/) should be created, and an [`getImageLoadCallback()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdowneditoptions/#getImageLoadCallback--) property should be correctly specified.

For doing this the user must create its own type, that implements a [`IMarkdownImageLoadCallback`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/imarkdownimageloadcallback/) interface. This interface defines a single method [`processImage()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/imarkdownimageloadcallback/#processImage-com.groupdocs.editor.options.MarkdownImageLoadArgs-), which obtains a [`MarkdownImageLoadArgs`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadargs/) type and must return a value of [`MarkdownImageLoadingAction`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadingaction/) enumeration.

Working mechanism is the next. GroupDocs.Editor parses an input Markdown file line by line, character by character. When it encounters the link to the external image, it creates an instance of [`MarkdownImageLoadArgs`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadargs/) with all data about this image — its filename (or relative path, or URL) and a boolean flag, that indicates whether it is a local link (in the filesystem) or a web link. Then, when [`MarkdownEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdowneditoptions/) have the [`setImageLoadCallback`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdowneditoptions/#setImageLoadCallback-com.groupdocs.editor.options.IMarkdownImageLoadCallback-) property specified with the user implementation of [`IMarkdownImageLoadCallback`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/imarkdownimageloadcallback/), then GroupDocs.Editor invokes the [`processImage()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/imarkdownimageloadcallback/#processImage-com.groupdocs.editor.options.MarkdownImageLoadArgs-) method by passing a prepared [`MarkdownImageLoadArgs`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadargs/) into it. Then GroupDocs.Editor “waits” until the user code makes a decision — a return value of the [`MarkdownImageLoadingAction`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadingaction/) enumeration.

If user implementation returns a [`MarkdownImageLoadingAction`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadingaction/#Skip)`.Skip` value, then the image will be skipped — the resultant document will have an “empty area”, where the image should be.

If user implementation returns a [`MarkdownImageLoadingAction`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadingaction/#Default)`.Default` value, then the GroupDocs.Editor will try to load an image by itself — this is possible, if, for example, a link to image is an absolute path or URL.

If user implementation returns a [`MarkdownImageLoadingAction`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadingaction/#UserProvided)`.UserProvided`, then this means that user code must provide the image data by itself specifying it in the [`setData(byte[] data)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadargs/#setData-byte---). In that case GroupDocs.Editor will process the specified binary data for this image.

Code example below shows exactly the last scenario, where the end-user creates his own implementation of the [`IMarkdownImageLoadCallback`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/imarkdownimageloadcallback/) interface.

Let’s say we have the next file and folder structure:
- root/`input.md`
- root/images/`image1.png`
- root/images/`image2.jpeg`

The `input.md` is a Markdown file we want to open and edit, that is located in the “root” folder, and it uses (references) two raster images `image1.png` and `image2.jpeg`, located in the “images” subfolder.


```java
public void EditingMarkdown()
{
	String inputMdPath = Path.Combine("root", "input.md");
	String imagesFolder = Path.Combine("root", "images");

	// Creating the edit options
	MarkdownEditOptions editOptions = new MarkdownEditOptions();
	editOptions.setImageLoadCallback(new MdImageLoader(imagesFolder));

	Editor editor = new Editor(inputMdPath);

	EditableDocument beforeEdit = editor.edit(editOptions);

	// Make sure there are 2 images here
	Assert.assertEquals(2, beforeEdit.getImages().size());
	Assert.assertEquals("png", beforeEdit.getImages().get(0).getType().getFileExtension());
	Assert.assertEquals("jpeg", beforeEdit.getImages().get(1).getType().getFileExtension());

	String originalHtmlContent = beforeEdit.getEmbeddedHtml();

	// Send the 'originalHtmlContent' to the client-side WYSIWYG-editor,
	// obtain the edited version and create a new EditableDocument from it

	EditableDocument afterEdit = EditableDocument.fromMarkup(originalHtmlContent, null);

	// Make sure 2 images are still here
	Assert.assertEquals(2, afterEdit.getImages().size());
	Assert.assertEquals("png", afterEdit.getImages().get(0).getType().getFileExtension());
	Assert.assertEquals("jpeg", afterEdit.getImages().get(1).getType().getFileExtension());

	// Save to the DOCX, for example
	WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Docx);
	String outputDocxPath = Path.Combine("root", "Output."+ saveOptions.getOutputFormat().getExtension());

	editor.save(afterEdit, outputDocxPath, saveOptions);

}

static class MdImageLoader implements IMarkdownImageLoadCallback
{
	private final String _imagesFolder;

	public MdImageLoader(String imagesFolder)
	{
		this._imagesFolder = imagesFolder;
	}

	public final byte processImage(MarkdownImageLoadArgs args)
	{
		String filePath = new java.io.File(this._imagesFolder, Paths.get(args.getImageFileName()).getFileName().toString()).getPath();

		byte[] data = Files.readAllBytes(filePath);
		args.setData(data);

		return MarkdownImageLoadingAction.UserProvided;
	}
}
```

In this example there is a user-created `MdImageLoader` class. It is initialized with the images folder path, and in the `processImage` method file content is read and pushed to the [`MarkdownImageLoadArgs`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadargs/) class through the [`setData(byte[] data)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownimageloadargs/#setData-byte---) method.

## Saving

GroupDocs.Editor also supports saving into the Markdown format. Like for any other format, for saving into markdown the user must create an instance of [`MarkdownSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/) class and specify it in the [`com.groupDocs.editor.Editor.save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#save-com.groupdocs.editor.EditableDocument-java.io.OutputStream-com.groupdocs.editor.options.ISaveOptions-) method.

If the document, destined for the saving in markdown format, has images, they should be “resolved” in a way similar to described above. But there is no callback for saving the images here. Instead of it, user has 3 choices:
1. Ignore images - they will be absent.
2. Save images inside the Markdown code, when they will be stored in base64 encoding.
3. Save images as files separately in the specified folder, and in the markdown code there will be references to these image files.

For doing this the [`MarkdownSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/) class has several properties. [`getExportImagesAsBase64()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/#getExportImagesAsBase64--) is a boolean flag, by default set to `false`. If setted to `true`, the content of the images will be injected inside output Markdown as base64. Also, if this flag is set to `true`, it has the highest priority, and the [`getImagesFolder()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/#getImagesFolder--) property is ignored.

[`getImagesFolder()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/#getImagesFolder--) property, in turn, works when [`setExportImagesAsBase64(boolean)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/#setExportImagesAsBase64-boolean-) is set to `false`. This property, if specified, should contain a valid full path to the existing folder, where GroupDocs.Editor should save images.

The example below shows opening an input DOCX file for editing and saving it into 3 different Markdown output files, each of one has its own saving properties.

```java
String filename = "SampleDoc.docx";
String inputPath = Path.Combine("some-path", filename);

String outputFolder = "Some-full-path-to-output-folder";

String outputMdEmbeddedFilePath = Path.Combine(outputFolder, "Output_Markdown_Embedded.md");
String outputMdExternalFilePath = Path.Combine(outputFolder, "Output_Markdown_External.md");
String outputMdAbsentFilePath = Path.Combine(outputFolder, "Output_Markdown_Absent.md");

Editor editor = new Editor(inputPath);
EditableDocument beforeEdit = editor.edit();
// Send content to client-side WYSIWYG-editor, edit it there, send back to the server-side
EditableDocument afterEdit = EditableDocument.fromMarkup(beforeEdit.getEmbeddedHtml(), null);

{// Saving to "embedded" version, where all images are injected inside MD with base64 encoding
	MarkdownSaveOptions mdSaveOptionsEmbedded = new MarkdownSaveOptions();
	mdSaveOptionsEmbedded.setExportImagesAsBase64(true);
	editor.save(afterEdit, outputMdEmbeddedFilePath, mdSaveOptionsEmbedded);
}

{// Saving to "external" version, where all images are stored in distinct files, while MD contains links to these files as paths in file system
	MarkdownSaveOptions mdSaveOptionsExternal = new MarkdownSaveOptions();
	mdSaveOptionsExternal.setImagesFolder(outputFolder);
	editor.save(afterEdit, outputMdExternalFilePath, mdSaveOptionsExternal);
}

{// Saving to "absent" version, where all images are skipped and are not present in MD
	MarkdownSaveOptions mdSaveOptionsAbsent = new MarkdownSaveOptions();
	ByteArrayOutputStream outputMdAbsentTemp = new ByteArrayOutputStream();
	editor.save(afterEdit, outputMdAbsentTemp, mdSaveOptionsAbsent);
	OutputStream outputMdAbsentStream = new FileOutputStream(outputMdAbsentFilePath);
	outputMdAbsentTemp.writeTo(outputMdAbsentStream);
	outputMdAbsentStream.close();
	outputMdAbsentTemp.close();
}
	
// Disposing all resources
beforeEdit.dispose();
afterEdit.dispose();
editor.dispose();
```

In this example you can see a little trick — for the “absent” version the document content is saved to the [`ByteArrayOutputStream`](https://docs.oracle.com/javase/7/docs/api/java/io/ByteArrayOutputStream.html), and then copied to the [`FileOutputStream`](https://docs.oracle.com/javase/7/docs/api/java/io/FileOutputStream.html). This is done, because GroupDocs.Editor has a trick: if both [`getExportImagesAsBase64()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/#getExportImagesAsBase64--) is set to `false` and [`setImagesFolder(String)`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/markdownsaveoptions/#setImagesFolder-java.lang.String-) is not set, then GroupDocs.Editor tries to analyze the specified stream. If this stream is a [`FileOutputStream`](https://docs.oracle.com/javase/7/docs/api/java/io/FileOutputStream.html), the GroupDocs.Editor obtains a path to the folder and saves images in it. So, in order to “deceive” the GroupDocs.Editor in this example, the [`ByteArrayOutputStream`](https://docs.oracle.com/javase/7/docs/api/java/io/ByteArrayOutputStream.html) was used.

## Roundtrip

Because Markdown format is supported on import and export, it is possible to perform a roundtrip scenario with it — open a markdown file for editing, edit it and then save the edited version to the Markdown format too. The example below demonstrates such a scenario.

```java
public void MarkdownRoundtrip()
{
	String inputFolderPath = "Some-full-path-to-input-folder";
	String outputFolder = "Some-full-path-to-output-folder";
	String outputMdPath = Path.Combine(outputFolder, "Output.md");

	String filename = "ComplexImage.md";
	String inputPath = Path.Combine(inputFolderPath, filename);	

	MarkdownEditOptions editOptions = new MarkdownEditOptions();
	editOptions.setImageLoadCallback(new MdImageLoader(inputFolderPath);

	MarkdownSaveOptions saveOptions = new MarkdownSaveOptions();
	saveOptions.setTableContentAlignment(MarkdownTableContentAlignment.Center);
	saveOptions.setImagesFolder(outputFolder);

	Editor editor = new Editor(inputPath);
	{
		EditableDocument doc = editor.edit(editOptions);
		{
			Assert.assertEquals(3, doc.getImages().size());
			// edit "doc" in WYSIWYG-editor and obtain its edited version

			editor.save(doc, outputMdPath, saveOptions);
		}
	}
}

static class MdImageLoader implements IMarkdownImageLoadCallback
{
	private final String _imagesFolder;

	public MdImageLoader(String imagesFolder)
	{
		this._imagesFolder = imagesFolder;
	}

	public final byte processImage(MarkdownImageLoadArgs args)
	{
		String filePath = new java.io.File(this._imagesFolder, Paths.get(args.getImageFileName()).getFileName().toString()).getPath();
		byte[] data = Files.readAllBytes(filePath);
		args.setData(data);

		return MarkdownImageLoadingAction.UserProvided;
	}
}
```