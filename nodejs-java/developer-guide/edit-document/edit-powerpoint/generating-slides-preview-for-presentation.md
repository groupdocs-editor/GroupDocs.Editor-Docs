---
id: generating-slides-preview-for-presentation
url: editor/nodejs-java/generating-slides-preview-for-presentation
title: Generating slides preview for presentations
weight: 4
description: "This article describes how to generate a preview for any slide in an existing PowerPoint presentation in SVG format using GroupDocs.Editor for Node.js and Java."
keywords: preview slide powerpoint presentation SVG
productName: GroupDocs.Editor for Node.js and Java
hideChildren: False
toc: True

---


Starting from **[version 24.6](https://releases.groupdocs.com/editor/nodejs-java/release-notes/2024/groupdocs-editor-for-node.js-via-java-24-6-release-notes/)**, GroupDocs.Editor for Node.js and Java allows generating slide previews from any presentation document in SVG format. This feature enables users to view and inspect the content of a PowerPoint presentation without needing to send it for editing. While the generated slide preview cannot be edited, it can be viewed in any desktop or online image viewer, as well as in modern web browsers that support the SVG format.

This feature works in both trial and licensed modes, with no limitations. Furthermore, generating the slides preview does not consume credits or bytes during its use.

### How to Generate a Slide Preview

To generate a slide preview in GroupDocs.Editor for Node.js or Java, you can use the `generatePreview(slideIndex)` method of the `PresentationDocumentInfo` class. The slide to preview is specified by its zero-based index (not to be confused with one-based slide numbers). If the index is out of bounds (less than 0 or greater than the number of slides), an `IllegalArgumentException` will be thrown.

Slide previews can be generated for both encoded and unencoded presentations. For encoded presentations, a valid password must be specified in the `getDocumentInfo()` method.

The `generatePreview(slideIndex)` method returns a slide preview as an SVG image, encapsulated in the `SvgImage` class. This class provides methods to save the SVG content to disk, stream it, and more.

### Example Code for Node.js

Here’s an example showing how to load a PowerPoint presentation in Node.js, generate previews for each slide, and save the SVG images to disk:

```javascript
const groupdocs = require('groupdocs-editor');
const Editor = groupdocs.Editor;
const PresentationLoadOptions = groupdocs.options.PresentationLoadOptions;
const SvgImage = groupdocs.resources.SvgImage;

// Path to the presentation file
const inputPath = "Sample.pptx";

// Load the presentation into the Editor
const editor = new Editor(inputPath);

// Get document info for this presentation
const documentInfo = editor.getDocumentInfo();

// Cast the document info to PresentationDocumentInfo
const presentationInfo = documentInfo.asPresentationDocumentInfo();

// Get the number of slides in the presentation
const slideCount = presentationInfo.getPageCount();

// Loop through all slides and generate SVG previews
for (let i = 0; i < slideCount; i++) {
    // Generate the slide preview as an SVG image
    const svgPreview = presentationInfo.generatePreview(i);

    // Save the SVG image to a file
    svgPreview.save(`output/slide_${i}.svg`);
}
```

### Example Code for Java

The following Java example shows how to generate previews for all slides in a PowerPoint presentation and save them as SVG images:

```java
// Obtain a valid full path to the presentation file
String inputPath = "Sample.pptx";

// Load the file into the Editor constructor
Editor editor = new Editor(inputPath);

// Get document info for this file
IDocumentInfo infoUncasted = editor.getDocumentInfo(null);

// Cast the document info to the PresentationDocumentInfo type
PresentationDocumentInfo infoSlides = (PresentationDocumentInfo) infoUncasted;

// Get the number of all slides
int slidesCount = infoSlides.getPageCount();

// Iterate through all slides and generate the preview for each slide
for (int i = 0; i < slidesCount; i++) {
    // Generate one preview as an SVG image
    SvgImage oneSvgPreview = infoSlides.generatePreview(i);

    // Save the preview to a file
    oneSvgPreview.save(new File(outputFolder, oneSvgPreview.getFilenameWithExtension()).getPath());
}
```

### Concluding

The slides preview feature is a convenient method for generating non-editable SVG previews of slides from PowerPoint presentations. The `generatePreview(slideIndex)` method in the `PresentationDocumentInfo` class allows you to obtain a slide’s preview in vector format, encapsulated in the `SvgImage` class. 

If you prefer a raster format instead of vector, the `saveToPng()` method in the `SvgImage` class can be used to convert the SVG content to PNG format and save it to a specified `InputStream` or file.

