---
id: adding-class-name-to-input-controls
url: editor/nodejs-java/adding-class-name-to-input-controls
title: Adding class name to input controls
weight: 1
description: "Follow this guide and learn how to edit Word documents that contain input controls like buttons, textboxes, checkboxes, combo-boxes, input fields, dropdown lists, radio-buttons, date/time pickers etc. using GroupDocs.Editor for Node.js and Java."
keywords: Edit Word document with input controls, Edit DOCX with input fields and text boxes, Edit Word, Edit DOCX
productName: GroupDocs.Editor for Node.js and Java
hideChildren: False
---


Almost all formats within the WordProcessing format family, such as DOC(X/M), ODT, etc., support input controls of various kinds. These documents can contain buttons, textboxes, checkboxes, combo-boxes, input fields, dropdown lists, radio-buttons, date/time pickers, and more, which are internally represented as Structured Document Tag (SDT) entities or Fields. **[GroupDocs.Editor](https://products.groupdocs.com/editor/nodejs-java)** supports all of these entities and preserves them when converting the document to an [EditableDocument](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/editabledocument) instance. When generating an HTML document from an [EditableDocument](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/editabledocument) to edit in a WYSIWYG HTML-editor on the client-side, these input controls are translated into the most appropriate HTML elements. Additionally, any user-entered data in these controls is preserved in the output HTML document.

### Why apply class names to input controls?

In specific scenarios, users may need to work only with input controls to edit or gather data entered on the client-side. For such cases, identifying input controls by assigning a unique CSS class name is essential. This helps distinguish these controls from other HTML elements, making them easier to manipulate or gather data from using client-side JavaScript.

Starting from version 20.8, the `inputControlsClassName` option is available in the `WordProcessingEditOptions` class, allowing users to assign a CSS class to all input controls in the HTML markup. The following methods are available:

```javascript
getInputControlsClassName(): string;
setInputControlsClassName(className: string): void;
```

In **Node.js**, you can assign a class name to input controls as follows:

```javascript
const WordProcessingEditOptions = groupdocs.options.WordProcessingEditOptions;

let editOptions = new WordProcessingEditOptions();
editOptions.inputControlsClassName = "myClass1";  // Assign a class name
```

In **Java**, this can be done similarly:

```java
WordProcessingEditOptions editOptions = new WordProcessingEditOptions();
editOptions.setInputControlsClassName("myClass1");  // Assign a class name
```

### Example

After assigning the class name to input controls, all HTML elements representing input controls (e.g., `<input>`, `<button>`, `<select>`) will include the class attribute. For example:

```html
<input type="text" class="myClass1" />
<select class="myClass1">
  <option>Option 1</option>
</select>
```

This allows client-side JavaScript to easily target these input elements, for instance, to gather data or apply further manipulations:

```javascript
const inputs = document.querySelectorAll('.myClass1');
// Now you can loop through or manipulate these inputs
inputs.forEach(input => {
  console.log(input.value);
});
```