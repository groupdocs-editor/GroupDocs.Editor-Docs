---
id: inserting-edited-slide-into-existing-presentation
url: editor/nodejs-java/inserting-edited-slide-into-existing-presentation
title: Inserting edited slide into existing presentation in Node.js and Java
weight: 1
description: "This article describes how to insert an edited presentation slide into an existing PowerPoint presentation using GroupDocs.Editor for Node.js and Java."
keywords: edit powerpoint, insert slide, edit presentation
productName: GroupDocs.Editor for Node.js and Java
hideChildren: False
toc: True

---


## Introduction

GroupDocs.Editor for Node.js allows users to insert an edited slide into the original presentation, rather than creating a new presentation with only the edited slide. This updated functionality enhances the default editing process, which consists of the following steps:

1. Load a presentation from a file or stream into the `Editor` class.
2. Select a specific slide to edit by specifying its index in the `PresentationEditOptions`.
3. Open the presentation for editing with `Editor.edit()` and obtain an `EditableDocument` instance.
4. Extract HTML and CSS content from the `EditableDocument`.
5. Pass this content to a WYSIWYG editor on the client-side.
6. The end-user edits the content within the WYSIWYG editor.
7. The edited HTML and CSS content is sent back to the server.
8. Recreate an `EditableDocument` instance with the edited content using the `EditableDocument.fromMarkup()` method.
9. Pass the `EditableDocument` instance to the `Editor.save()` method.
10. In earlier versions, the `Editor.save()` method would generate a new presentation with just the edited slide. Starting from version 20.11, the edited slide can be inserted into the original presentation instead.

### PresentationSaveOptions

The `PresentationSaveOptions` class includes two key properties:

- `slideNumber`: Specifies the position where the edited slide should be inserted or replaced.
- `insertAsNewSlide`: A boolean flag that determines whether the edited slide should replace an existing slide or be inserted as a new one.

Here’s how these properties work in Node.js:

```javascript
let saveOptions = new groupdocs.options.PresentationSaveOptions();
saveOptions.slideNumber = 2;  // Inserts or replaces at the second slide position
saveOptions.insertAsNewSlide = true;  // Inserts a new slide, rather than replacing
```

### SlideNumber Property

The `slideNumber` property specifies where the new edited slide will be inserted. If set to `0`, a new single-slide presentation will be created. Slide numbering starts from `1`, similar to how Microsoft PowerPoint numbers slides. If the slide number exceeds the total number of slides, the new slide will be inserted at the last position.

Both positive and negative values are supported:

- Positive values count from the beginning of the presentation (1-based).
- Negative values count from the end of the presentation (`-1` for the last slide, `-2` for the second to last, etc.).

Here’s how to use the `slideNumber` property in Node.js:

```javascript
let saveOptions = new groupdocs.options.PresentationSaveOptions();
saveOptions.slideNumber = 1;  // Insert at the first slide position
saveOptions.slideNumber = -1; // Insert as the last slide
```

### InsertAsNewSlide Property

The `insertAsNewSlide` property determines whether the edited slide should be inserted as a new slide or replace the existing slide at the specified position:

- `false` (default): The existing slide at the `slideNumber` position will be replaced by the edited slide.
- `true`: The edited slide will be inserted as a new slide, pushing subsequent slides to the next positions.

#### Example in Node.js:

```javascript
saveOptions.insertAsNewSlide = true;  // Insert the slide as a new slide
```

### Example Code for Node.js

The following example demonstrates how to insert an edited slide into an existing presentation using Node.js:

```javascript
const groupdocs = require('groupdocs-editor');

// Load the presentation
const editor = new groupdocs.Editor('path/to/presentation.pptx');

// Get the editable document for the second slide
const presentationEditOptions = new groupdocs.options.PresentationEditOptions();
presentationEditOptions.slideNumber = 1;
const editableDocument = editor.edit(presentationEditOptions);

// Get the edited content from a WYSIWYG editor (mockup example)
const editedContent = '<html>...edited content...</html>';

// Recreate the editable document from the edited markup
const updatedDocument = groupdocs.EditableDocument.fromMarkup(editedContent, []);

// Save the edited slide and insert it as a new slide
const saveOptions = new groupdocs.options.PresentationSaveOptions();
saveOptions.slideNumber = 2;
saveOptions.insertAsNewSlide = true;

editor.save(updatedDocument, 'path/to/updated_presentation.pptx', saveOptions);
```
