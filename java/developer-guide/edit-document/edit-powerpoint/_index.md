---
id: edit-powerpoint
url: editor/java/edit-powerpoint
title: Edit PowerPoint Presentations
weight: 47
description: "This guide demonstrates how to edit PPT, PPTX, PPTM, PPSX, PPSM, POTX, POTM presentations with different settings and many other powerful features of GroupDocs.Editor for Java."
keywords: Edit PPT, Edit PPTX, Edit PPTM, Edit PPSX, Edit PPSM, Edit POTX, Edit POTM,  edit powerpoint
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:    
        name: How to edit PowerPoint presentation in the GroupDocs.Editor
        description: How to edit PowerPoint presentation using the GroupDocs.Editor in Java language
        productCode: editor
        productPlatform: java 
    showVideo: True
    howTo:
        name: How to edit content of the PowerPoint presentation in the GroupDocs.Editor in Java
        description: Learn how to edit content of the slides from the PowerPoint presentation using the GroupDocs.Editor in Java step by step
        steps:
        - name: Load desired PowerPoint presentation to the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired  PowerPoint presentation into it.
        - name: Prepare a PresentationEditOptions class
          text: Create an instance of the PresentationEditOptions class and adjust its properties to meet your needs if necessary.
        - name: Select desired slide for editing
          text: Using the PresentationEditOptions you should select the desired slide, that should be edited, using the "setSlideNumber()" method.
        - name: Call Editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke a Editor.edit method with specifying a previously prepared PresentationEditOptions and obtain an instance of the EditableDocument class, which is ready for editing. Then generate HTML-markup and extract resources from this instance using corresponding instance methods, and pass all these data to the HTML-based WYSIWYG-editor.
        - name: Edit the document in WYSIWYG-editor and send the edited content back to the server-side
          text: Make all necessary edits in the content of the slide in the HTML-based WYSIWYG-editor, which is running on a client-side (in a web-browser) and then submit the edited content and resources back to the server-side, where the GroupDocs.Editor is running.
        - name: Create an instance of EditableDocument
          text: Create an instance of the EditableDocument by passing the edited slide content into the most suitable static methods of the class
        - name: Prepare a PresentationSaveOptions class
          text: Create an instance of the PresentationSaveOptions class and adjust its properties to meet your needs if necessary. You need to choose the format of the output presentation — this is the only mandatory parameter, that must be specified in the constructor. Also using the "setSlideNumber()" and "setInsertAsNewSlide()" methods you can choose how to insert the edited slide into the output presentation — replace the original slide with the edited one, or inject a new edited slide to keep it along with old original simultaneously.
        - name: Save edited PowerPoint presentation with Editor.save method
          text: Pass an instance of EditableDocument with content of the edited PowerPoint presentation, instance of the PresentationSaveOptions, and a destination byte stream or file path to the Editor.save method for saving the presentation.
---
> This example demonstrates standard open-edit-save cycle with Presentation documents, using different options on every step.

## Introduction

Presentation documents are presented by many formats: PPT, PPTX, PPTM, PPS(X/M), POT(X/M) and other, which are supported by GroupDocs.Editor as a separate format family among all others. Same like for all other family formats, Presentation family has its own load, edit and save options. Presentation format has significant distinction from the WordProcessing and all textual formats, and at the same time big similarity with Spreadsheet formats, — it has no pages, but instead of pages it has slides (like Spreadsheet has tabs). Like tabs in Spreadsheets, slides in Presentations are completely separate one from each other and has no valid representation in HTML markup, so the only way to edit slides is to edit them separately, one slide per one editing procedure.

As a result, a Presentation document with multiple slides is loaded into the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class. Then, in order to open document for editing by calling an [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor).[Edit](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) method, user should select desired slide for editing by specifying its index in the [PresentationEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions). As a result, user will obtain an instance of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class, that holds that edited slide. For editing another slide, user should perform another call of the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor).[edit()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) method with another slide index, and obtain a new instance of [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) class. Finally, when saving edited slides back to Presentation format, user should save every [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance separately.

Like all Office OOXML formats, Presentation documents can be encrypted with password. GroupDocs.Editor supports opening password-protected Presentation documents (password should be specified in the [PresentationLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationloadoptions)) and creating password-protected documents (password should be specified in the [PresentationSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions)).

## Loading Presentation for editing

First step for edit a document is to load it. In order to load presentation to the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class, user should use the [PresentationLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationloadoptions) class. It is not necessary in some general cases — even without [PresentationLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationloadoptions) the GroupDocs.Editor is able to recognize presentation format and apply appropriate default load options automatically. But when presentation is encoded, the [PresentationLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationloadoptions) is the only way to set a password and load the document properly.

If document is encoded, but password is not set in the [PresentationLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationloadoptions), or [PresentationLoadOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationloadoptions) is not specified at all, then [PasswordRequiredException](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/passwordrequiredexception) exception will be thrown. If password is set, but is incorrect, then [IncorrectPasswordException](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/incorrectpasswordexception) will be thrown. If password is set, but the document is not encoded, then password will be ignored.

Below is an example of loading a presentation document from file path with load options and password.

```java
String inputPptxPath = "C://input//presentation.pptx";
PresentationLoadOptions loadOptions = new PresentationLoadOptions();
loadOptions.setPassword("password");
Editor editor = new Editor(inputPptxPath, loadOptions);
```

## Edit PowerPoint presentation

For opening Presentation document for edit a [PresentationEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions) class should be used. It has two properties: [getSlideNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions#getSlideNumber--) of Integer type, and a [getShowHiddenSlides()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions#setShowHiddenSlides-boolean-), that is a boolean flag.

[getSlideNumber()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions#getSlideNumber--) is a zero-based index of a slide, that allows to specify and select one particular slide from a presentation to edit. If lesser then 0, the first slide will be selected (same as *SlideNumber = 0*). If greater then amount of all slides in presentation, the last slide will be selected. If input presentation contains only single slide, this option will be ignored, and this single slide will be edited. By default is 0, that implies first slide for edit.

[getShowHiddenSlides()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions#getShowHiddenSlides--) is a boolean flag, which specifies whether the hidden slides should be included or not. Default is *false* — hidden slides are not shown and exception will be thrown while trying to edit them. So, if input Presentation has 3 slides, where 2nd is hidden, user has specified *SlideNumber = 1* (2nd slide) and simultaneously *ShowHiddenSlides  = false*, the `InvalidOperationException` will be thrown.

If parameterless overload of the [Editor.edit()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#edit--) method is used, the default [PresentationEditOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions) instance will be used: first slide and disabled [getShowHiddenSlides()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions#getShowHiddenSlides--).

Code example below demonstrates all options, described above. Let's assume, that Presentation document with at least 3 slides is already loaded into the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class.

```java
//parameterless overload is used => default PresentationEditOptions is applied, which means 1st slide
EditableDocument firstSlide = editor.edit();

PresentationEditOptions editOptions2 = new PresentationEditOptions();
editOptions2.setSlideNumber(1); //index is 0-based, so this is 2nd slide
EditableDocument secondSlide = editor.edit(editOptions2);

PresentationEditOptions editOptions3 = new PresentationEditOptions();
editOptions3.setSlideNumber(2); //index is 0-based, so this is 3rd slide
editOptions3.setShowHiddenSlides(true); //if 3rd slide is hidden, it will be opened anyway
EditableDocument thirdSlide = editor.edit(editOptions3);
```

## Save Presentation after edit

[PresentationSaveOptions](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions) class is designed for saving the edited Presentation documents. This class has one [constructor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions#PresentationSaveOptions-com.groupdocs.editor.formats.PresentationFormats-), which has one parameter — a [Presentation format](https://reference.groupdocs.com/editor/java/groupdocs.editor.formats/presentationformats), in which the output document should be saved. This output format is represented by the [PresentationFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/presentationformats) struct. After creating an instance, output format can be obtained or modified later, through the [getOutputFormat()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions#getOutputFormat--) property. This is the only parameter, which is necessary for saving the document: all others are optional and may be omitted.

Along with format, user is able to specify a password through the [getPassword()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions#getPassword--) string property — in this case output document will be encoded and protected with this password. By default password is not set, which implies that output document will be unprotected. This is shown in example below, where it is supposed that a user has an [EditableDocument](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance with edited slide.

```java
PresentationSaveOptions saveOptions = new PresentationSaveOptions(PresentationFormats.Pptm);
saveOptions.setPassword("new password");

EditableDocument afterEdit = /* obtain it from somewhere */;
FileStream outputStream = /* obtain it from somewhere */;

//saving edited slide to specified stream in PPTM format and with password encoding
editor.save(afterEdit, outputStream, saveOptions);
```
