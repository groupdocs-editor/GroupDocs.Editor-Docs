---
id: edit-powerpoint
url: editor/nodejs-java/edit-powerpoint
title: Edit PowerPoint Presentations in Node.js
weight: 47
description: "This guide demonstrates how to edit PPT, PPTX, PPTM, PPSX, PPSM, POTX, POTM presentations with different settings and many other powerful features of GroupDocs.Editor for Node.js."
keywords: Edit PPT, Edit PPTX, Edit PPTM, Edit PPSX, Edit PPSM, Edit POTX, Edit POTM,  edit PowerPoint
productName: GroupDocs.Editor for Node.js
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit PowerPoint presentations in GroupDocs.Editor for Node.js
        description: How to edit PowerPoint presentations using GroupDocs.Editor in Node.js
        productCode: editor
        productPlatform: nodejs-java 
    showVideo: True
    howTo:
        name: How to edit the content of a PowerPoint presentation using GroupDocs.Editor in Node.js
        description: Learn how to edit content of the slides from the PowerPoint presentation using GroupDocs.Editor in Node.js step by step
        steps:
        - name: Load the desired PowerPoint presentation into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor, passing the desired PowerPoint presentation into it.
        - name: Prepare a PresentationEditOptions class
          text: Create an instance of the PresentationEditOptions class and adjust its properties to meet your needs if necessary.
        - name: Select the desired slide for editing
          text: Using the PresentationEditOptions, select the desired slide for editing by specifying the slide number using the "setSlideNumber()" method.
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the Editor.edit method, specifying the prepared PresentationEditOptions, and obtain an EditableDocument instance that is ready for editing. Generate HTML markup and extract resources from this instance and pass them to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Edit the content of the slide in the HTML-based WYSIWYG-editor on the client-side (browser) and then submit the edited content and resources back to the server-side, where GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument class by passing the edited slide content into the appropriate static methods of the class.
        - name: Prepare a PresentationSaveOptions class
          text: Create an instance of the PresentationSaveOptions class and adjust its properties as needed. Choose the format of the output presentation — the only mandatory parameter in the constructor. Use "setSlideNumber()" and "setInsertAsNewSlide()" methods to choose whether to replace the original slide with the edited one or insert a new slide while keeping the original.
        - name: Save the edited PowerPoint presentation with Editor.save method
          text: Pass the EditableDocument containing the edited PowerPoint presentation, an instance of PresentationSaveOptions, and a destination file path or stream to the Editor.save method to save the presentation.
---

## Introduction

PowerPoint presentations are supported by GroupDocs.Editor in various formats such as PPT, PPTX, PPTM, PPS(X/M), POT(X/M), and others. The GroupDocs.Editor for Node.js provides separate load, edit, and save options for these formats.

Unlike word processing formats, PowerPoint presentations don't have pages but instead consist of slides, which are edited one at a time. A presentation with multiple slides is loaded into the `Editor` class, and to edit a specific slide, the user selects its index using `PresentationEditOptions`. The user will receive an `EditableDocument` representing that particular slide. To edit another slide, repeat the process by calling `Editor.edit()` again with a different slide index.

GroupDocs.Editor also supports password-protected presentations. When opening a password-protected document, the password must be specified in the `PresentationLoadOptions`. Similarly, password protection can be applied when saving the document by using `PresentationSaveOptions`.

## Loading a Presentation for Editing

To load a presentation in GroupDocs.Editor for Node.js, use the `PresentationLoadOptions` class. This is particularly important if the presentation is password-protected.

Here’s an example of loading a presentation with a password:

```javascript
const groupdocs = require('groupdocs-editor');
const Editor = groupdocs.Editor;
const PresentationLoadOptions = groupdocs.options.PresentationLoadOptions;

const inputPath = "C://input//presentation.pptx";
const loadOptions = new PresentationLoadOptions();
loadOptions.password = "password";

const editor = new Editor(inputPath, loadOptions);
```

## Editing PowerPoint Presentations

To edit a PowerPoint presentation, use the `PresentationEditOptions` class. This class allows you to select a slide and specify whether hidden slides should be shown.

- `slideNumber`: A zero-based index to specify which slide to edit. If not set, the first slide is selected by default.
- `showHiddenSlides`: A boolean flag that determines whether hidden slides should be shown.

Here’s an example that demonstrates how to use these options:

```javascript
const PresentationEditOptions = groupdocs.options.PresentationEditOptions;

// Edit the first slide
const firstSlide = editor.edit(); // Default behavior, edits the first slide

// Edit the second slide
const editOptions2 = new PresentationEditOptions();
editOptions2.slideNumber = 1;  // Index is zero-based
const secondSlide = editor.edit(editOptions2);

// Edit the third slide and show hidden slides if they exist
const editOptions3 = new PresentationEditOptions();
editOptions3.slideNumber = 2;  // Third slide
editOptions3.showHiddenSlides = true;
const thirdSlide = editor.edit(editOptions3);
```

## Saving a PowerPoint Presentation After Editing

To save a PowerPoint presentation after editing, use the `PresentationSaveOptions` class. You must specify the output format using the `PresentationFormats` structure and can optionally set a password for the output file.

Here’s an example of saving an edited slide in the PPTM format with password protection:

```javascript
const PresentationSaveOptions = groupdocs.options.PresentationSaveOptions;
const PresentationFormats = groupdocs.formats.PresentationFormats;

// Create save options with output format and password
const saveOptions = new PresentationSaveOptions(PresentationFormats.Pptm);
saveOptions.password = "new password";

// Assume we have an EditableDocument instance (after editing)
const outputStream = /* obtain output stream */;

// Save the edited presentation
editor.save(editedDocument, outputStream, saveOptions);
```

### Conclusion

In **GroupDocs.Editor for Node.js**, PowerPoint presentations are edited slide by slide. By using the `Editor` class in conjunction with `PresentationEditOptions` and `PresentationSaveOptions`, users can load, edit, and save PowerPoint presentations efficiently while applying features like password protection.

---

This adapted guide reflects the Node.js version of **GroupDocs.Editor**, translating the steps from Java to Node.js while preserving the same functionality. It provides a clear workflow for loading, editing, and saving PowerPoint presentations in Node.js.