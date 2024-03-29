---
id: adding-class-name-to-input-controls
url: editor/net/adding-class-name-to-input-controls
title: Adding class name to input controls
weight: 1
description: "Follow this guide and learn how to edit Word documents that contain input controls like buttons, textboxes, check-boxes, combo-boxes, input fields, dropdown lists, radio-buttons, date/time pickers etc. using GroupDocs.Editor for .NET API features."
keywords: Edit Word document with input controls, Edit DOCX with input fields and text boxes, Edit Word, Edit DOCX
productName: GroupDocs.Editor for .NET
hideChildren: False
---
Almost all formats within WordProcessing format family, like DOC(X/M), ODT etc., support input controls of different kinds. WordProcessing documents can contain different buttons, textboxes, check-boxes, combo-boxes, input fields, dropdown lists, radio-buttons, date/time pickers and much more, which are internally present as Structured Document Tag (SDT) entities or Fields ("Insert > Quick Parts > Document Property/Field"). **[GroupDocs.Editor](https://products.groupdocs.com/editor/net)** supports all of these entities and preserves them while converting the document to the [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance. Finally, when generating a HTML document from [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) in order to edit it in the WYSIWYG HTML-editor on client-side, these input controls are translated into the most appropriate HTML structures and elements. Additionally, when input document contains not only input controls, but also user-entered data, this data is also preserved and will be present in the output HTML document.

In some specific use-cases end-user may require not to edit the entire document content, but only edit and/or gather data, entered into input controls, on a client-side. For such case it is required to identify all these input controls in some way in order to fetching them, to distinguish them from all other HTML elements, when working on client-side. For achieving this purpose the GroupDocs.Editor has an ability to set an unique user-provided CSS class name for all such input controls in HTML markup. Starting from version 20.2 there is an option `InputControlsClassName` in the [WordProcessingEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions) class. It has the next appearance:

```csharp
public string InputControlsClassName {get; set;}
```

By default it has a NULL value — class names are not applied to the HTML elements. However, if user will set a valid class name, all the input control elements (like INPUT, BUTTON, SELECT etc.) will have a "class" HTML attribute with specified class name. For example:

```csharp
WordProcessingEditOptions editOptions = new WordProcessingEditOptions();
editOptions.InputControlsClassName = "myClass1";
```

Finally, when "class" attribute with specified class name is applied to all HTML elements, that represent input controls, client code is able to work with them by, for example, traversing the HTML DOM and gathering and/or manipulating with data.
