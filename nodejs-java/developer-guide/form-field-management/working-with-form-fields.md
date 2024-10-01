---

id: working-with-form-fields
url: editor/nodejs-java/working-with-form-fields
title: Working with Form Fields
weight: 4
description: "This article demonstrates how to load, edit, and read form fields in a Word document using GroupDocs.Editor for Node.js via Java."
keywords: Edit HTML document, GroupDocs.Editor
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
---

This article demonstrates how to load, edit, and read form fields in a Word document using **GroupDocs.Editor for Node.js via Java**. We will go through the process of opening a document, retrieving form fields, and processing them based on their types.

#### Step-by-Step Guide

1. **Get the Path to the Input File**

   Specify the path to your input Word document (`.docx`) that contains form fields.

   ```javascript
   const inputFilePath = 'path/to/your/document.docx';
   ```

2. **Create a Readable Stream from the Path**

   Create a readable stream from the input file path.

   ```javascript
   const fs = require('fs');
   const inputStream = fs.createReadStream(inputFilePath);
   ```

3. **Create Load Options**

   Initialize the `WordProcessingLoadOptions` for loading the document. If your document is password-protected, specify the password. In this example, the document is unprotected, so the password will be ignored.

   ```javascript
   const groupdocsEditor = require('groupdocs-editor');

   const loadOptions = new groupdocsEditor.WordProcessingLoadOptions();
   loadOptions.setPassword('some_password_to_open_a_document');
   ```

4. **Load the Document into the Editor Instance**

   Use the `Editor` class to load the document with the specified load options.

   ```javascript
   const editor = new groupdocsEditor.Editor(inputStream, loadOptions);
   ```

5. **Retrieve and Process the Form Fields**

   Obtain the `FormFieldManager` instance from the `Editor`. Use it to read the `FormFieldCollection` and process each form field based on its type.

   ```javascript
   // Get the FormFieldManager instance
   const fieldManager = editor.getFormFieldManager();

   // Get the FormFieldCollection in the document
   const collection = fieldManager.getFormFieldCollection();

   // Iterate over each form field in the collection
   collection.forEach((formField) => {
       switch (formField.getType()) {
           case groupdocsEditor.FormFieldType.Text:
               const textFormField = collection.getFormField(formField.getName(), groupdocsEditor.TextFormField);
               console.log(`TextFormField detected, name: ${formField.getName()}, value: ${textFormField.getValue()}`);
               break;
           case groupdocsEditor.FormFieldType.CheckBox:
               const checkBoxFormField = collection.getFormField(formField.getName(), groupdocsEditor.CheckBoxForm);
               console.log(`CheckBoxForm detected, name: ${formField.getName()}, value: ${checkBoxFormField.getValue()}`);
               break;
           case groupdocsEditor.FormFieldType.Date:
               const dateFormField = collection.getFormField(formField.getName(), groupdocsEditor.DateFormField);
               console.log(`DateFormField detected, name: ${formField.getName()}, value: ${dateFormField.getValue()}`);
               break;
           case groupdocsEditor.FormFieldType.Number:
               const numberFormField = collection.getFormField(formField.getName(), groupdocsEditor.NumberFormField);
               console.log(`NumberFormField detected, name: ${formField.getName()}, value: ${numberFormField.getValue()}`);
               break;
           case groupdocsEditor.FormFieldType.DropDown:
               const dropDownFormField = collection.getFormField(formField.getName(), groupdocsEditor.DropDownFormField);
               const selectedValue = dropDownFormField.getValue()[dropDownFormField.getSelectedIndex()];
               console.log(`DropDownFormField detected, name: ${formField.getName()}, selected value: ${selectedValue}`);
               break;
           default:
               console.log(`Unknown form field type detected, name: ${formField.getName()}`);
       }
   });
   ```

   In this code:

   - We use the `getType()` method to determine the type of each form field.
   - Retrieve the specific form field instance using `getFormField()` with the appropriate class.
   - Access the properties of each form field and print them to the console.

### Complete Example Code

Below is the complete example code demonstrating the entire process:

```javascript
/**
 * This example demonstrates how to load, edit, and read form fields in a Word document using GroupDocs.Editor for Node.js via Java.
 */

// Import the necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

(async () => {
    try {
        // 1. Get the path to the input file (or stream with file content).
        // In this case, it is a sample DOCX with form fields.
        const inputFilePath = 'path/to/your/document.docx';

        // 2. Create a readable stream from this path
        const inputStream = fs.createReadStream(inputFilePath);

        // 3. Create load options for this document
        const loadOptions = new groupdocsEditor.WordProcessingLoadOptions();
        // 3.1. If the input document is password-protected, specify the password for its opening...
        loadOptions.setPassword('some_password_to_open_a_document');
        // 3.2. ...but, because the document is unprotected, this password will be ignored

        // 4. Load the document with options into the Editor instance
        const editor = new groupdocsEditor.Editor(inputStream, loadOptions);

        // 5. Retrieve and process the form fields
        // 5.1. Get the FormFieldManager instance
        const fieldManager = editor.getFormFieldManager();

        // 5.2. Get the FormFieldCollection in the document
        const collection = fieldManager.getFormFieldCollection();

        // 5.3. Iterate over each form field in the collection
        collection.forEach((formField) => {
            switch (formField.getType()) {
                case groupdocsEditor.FormFieldType.Text:
                    const textFormField = collection.getFormField(formField.getName(), groupdocsEditor.TextFormField);
                    console.log(`TextFormField detected, name: ${formField.getName()}, value: ${textFormField.getValue()}`);
                    break;
                case groupdocsEditor.FormFieldType.CheckBox:
                    const checkBoxFormField = collection.getFormField(formField.getName(), groupdocsEditor.CheckBoxForm);
                    console.log(`CheckBoxForm detected, name: ${formField.getName()}, value: ${checkBoxFormField.getValue()}`);
                    break;
                case groupdocsEditor.FormFieldType.Date:
                    const dateFormField = collection.getFormField(formField.getName(), groupdocsEditor.DateFormField);
                    console.log(`DateFormField detected, name: ${formField.getName()}, value: ${dateFormField.getValue()}`);
                    break;
                case groupdocsEditor.FormFieldType.Number:
                    const numberFormField = collection.getFormField(formField.getName(), groupdocsEditor.NumberFormField);
                    console.log(`NumberFormField detected, name: ${formField.getName()}, value: ${numberFormField.getValue()}`);
                    break;
                case groupdocsEditor.FormFieldType.DropDown:
                    const dropDownFormField = collection.getFormField(formField.getName(), groupdocsEditor.DropDownFormField);
                    const selectedValue = dropDownFormField.getValue()[dropDownFormField.getSelectedIndex()];
                    console.log(`DropDownFormField detected, name: ${formField.getName()}, selected value: ${selectedValue}`);
                    break;
                default:
                    console.log(`Unknown form field type detected, name: ${formField.getName()}`);
            }
        });

        console.log('Working with form fields routine has successfully finished.');
    } catch (error) {
        console.error('An error occurred:', error);
    }
})();
```

### Conclusion

This guide demonstrates how to work with form fields in Word documents using **GroupDocs.Editor for Node.js via Java**. By following these steps, you can load a document, retrieve form fields, and process them based on their types. This functionality is essential for applications that require manipulation and extraction of form field data from documents.

**Note:** Make sure to replace `'path/to/your/document.docx'` with the actual file path in your application.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java) repository on GitHub.

