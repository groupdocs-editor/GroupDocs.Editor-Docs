---

id: fixing-invalid-form-fields
url: editor/nodejs-java/fixing-invalid-form-fields
title: Fixing Invalid Form Fields
weight: 4
description: "Fixing Invalid Form Fields and Saving the Document with GroupDocs.Editor for Node.js via Java"
keywords: Fix Invalid Form Fields, FormFieldManager, Invalid Form Fields, Update Form Fields
productName: GroupDocs.Editor for Node.js via Java
productPlatform: nodejs-java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
---

This article demonstrates how to fix invalid form fields in a Word document using **GroupDocs.Editor for Node.js via Java**. The example guides you through loading a document, identifying and fixing invalid form fields, and saving the updated document.

#### Step-by-Step Guide

1. **Create a Readable Stream from the Path**

   Create a readable stream from the input file path.

   ```javascript
   const fs = require('fs');
   const inputFilePath = 'path/to/your/document.docx';
   const inputStream = fs.createReadStream(inputFilePath);
   // Further code will be placed here
   ```

2. **Create Load Options**

   Initialize the `WordProcessingLoadOptions` for loading the document. If your document is password-protected, specify the password. In this example, the document is unprotected, so the password will be ignored.

   ```javascript
   const groupdocsEditor = require('groupdocs-editor');

   const loadOptions = new groupdocsEditor.WordProcessingLoadOptions();
   loadOptions.setPassword('some_password_to_open_a_document');
   ```

3. **Load the Document into the Editor Instance**

   Use the `Editor` class to load the document with the specified load options.

   ```javascript
   const editor = new groupdocsEditor.Editor(inputStream, loadOptions);
   // Further code will be placed here
   ```

4. **Retrieve and Fix Invalid Form Fields**

   Obtain the `FormFieldManager` instance from the `Editor` and read the `FormFieldCollection`. Attempt to auto-fix invalid form fields, then check if any invalid form fields remain. If so, generate unique names for the invalid fields and fix them.

   ```javascript
   // Get the FormFieldManager instance
   const fieldManager = editor.getFormFieldManager();
   // Get the FormFieldCollection in the document
   let collection = fieldManager.getFormFieldCollection();

   // Try to auto-fix invalid form fields
   fieldManager.fixInvalidFormFieldNames([]);
   collection = fieldManager.getFormFieldCollection();

   const hasInvalidFormFields = fieldManager.hasInvalidFormFields();
   console.log('FormFieldCollection contains invalid items: ' + hasInvalidFormFields);

   // Try to fix invalid form fields
   // Get all invalid form field names
   const invalidFormFields = fieldManager.getInvalidFormFieldNames();

   // Create unique names for invalid fields
   invalidFormFields.forEach((invalidItem) => {
       invalidItem.setFixedName(`${invalidItem.getName()}_${generateUUID()}`);
   });

   // Fix invalid fields
   fieldManager.fixInvalidFormFieldNames(invalidFormFields);
   collection = fieldManager.getFormFieldCollection();
   ```

   **Note:** The `generateUUID()` function is used to create unique identifiers. You can implement it as shown below:

   ```javascript
   function generateUUID() {
       return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
           const r = Math.random() * 16 | 0, v = c === 'x' ? r : (r & 0x3 | 0x8);
           return v.toString(16);
       });
   }
   ```

5. **Create and Set Save Options**

   Initialize the `WordProcessingSaveOptions` with the desired format. Optimize memory usage if necessary, and protect the document from writing with a password.

   ```javascript
   const docFormat = groupdocsEditor.WordProcessingFormats.Docx;
   const saveOptions = new groupdocsEditor.WordProcessingSaveOptions(docFormat);
   saveOptions.setOptimizeMemoryUsage(true);
   const protectionType = groupdocsEditor.WordProcessingProtectionType.AllowOnlyFormFields;
   const protection = new groupdocsEditor.WordProcessingProtection(protectionType, 'write_password');
   saveOptions.setProtection(protection);
   ```

6. **Save the Document**

   Use the `save` method of the `Editor` to save the updated document.

   ```javascript
   const outputFilePath = 'path/to/output/document.docx';
   editor.save(outputFilePath, saveOptions);
   ```

7. **Completion Message**

   Print a confirmation message to indicate the successful completion of the operation.

   ```javascript
   console.log('Fix invalid form fields routine has successfully finished.');
   ```

### Complete Example Code

Below is the complete example code demonstrating the entire process:

```javascript
/**
 * This example demonstrates how to fix invalid form fields in a collection and save the document using GroupDocs.Editor for Node.js via Java.
 */

// Import the necessary modules
const fs = require('fs');
const groupdocsEditor = require('groupdocs-editor');

// Function to generate a unique identifier
function generateUUID() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        const r = Math.random() * 16 | 0, v = c === 'x' ? r : ((r & 0x3) | 0x8);
        return v.toString(16);
    });
}

(async () => {
    try {
        // 1. Get a path to the input file (or stream with file content).
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

        // 4.1. Get the FormFieldManager instance
        const fieldManager = editor.getFormFieldManager();
        // 4.2. Get the FormFieldCollection in the document
        let collection = fieldManager.getFormFieldCollection();

        // 4.3. Try to auto-fix invalid form fields
        fieldManager.fixInvalidFormFieldNames([]);
        collection = fieldManager.getFormFieldCollection();

        const hasInvalidFormFields = fieldManager.hasInvalidFormFields();
        console.log('FormFieldCollection contains invalid items: ' + hasInvalidFormFields);

        // 4.4. Try to fix invalid form fields
        // 4.4.1. Get all invalid form field names
        const invalidFormFields = fieldManager.getInvalidFormFieldNames();

        // 4.4.2. Create unique names for invalid fields
        invalidFormFields.forEach((invalidItem) => {
            invalidItem.setFixedName(`${invalidItem.getName()}_${generateUUID()}`);
        });

        // 4.4.3. Fix invalid fields
        fieldManager.fixInvalidFormFieldNames(invalidFormFields);
        collection = fieldManager.getFormFieldCollection();

        // 5. Create document save options
        const docFormat = groupdocsEditor.WordProcessingFormats.Docx;
        const saveOptions = new groupdocsEditor.WordProcessingSaveOptions(docFormat);

        // 5.1. If the document is large and causes memory issues, set the memory optimization option
        saveOptions.setOptimizeMemoryUsage(true);

        // 5.2. Protect the document from writing (allow only form fields) with a password
        const protectionType = groupdocsEditor.WordProcessingProtectionType.AllowOnlyFormFields;
        const protection = new groupdocsEditor.WordProcessingProtection(protectionType, 'write_password');
        saveOptions.setProtection(protection);

        // 6. Save the document
        const outputFilePath = 'path/to/output/document.docx';

        // 6.1. Save the document
        editor.save(outputFilePath, saveOptions);

        console.log('Fix invalid form fields routine has successfully finished.');
    } catch (error) {
        console.error('An error occurred:', error);
    }
})();
```

---

**Note:** Make sure to replace `'path/to/your/document.docx'` and `'path/to/output/document.docx'` with the actual file paths in your application.

---

By following this guide, you can effectively fix invalid form fields in Word documents using GroupDocs.Editor for Node.js via Java. This allows you to programmatically identify and correct issues with form fields, ensuring that your documents are correctly formatted and free of duplicate or invalid form fields.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java) repository on GitHub.
