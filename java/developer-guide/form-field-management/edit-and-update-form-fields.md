---
id: edit-and-update-form-fields
url: editor/java/edit-and-update-form-fields
title: Edit and Update Form Fields
weight: 4
description: "Editing Form Fields with GroupDocs.Editor for Java"
keywords: Edit Form Fields, Update Form Fields, Update Form Fields with Word Document Protection, FormFieldManager
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---

This article demonstrates how to edit form fields in a Word document using GroupDocs.Editor for Java. The example provided guides you through loading a document, modifying form fields, and saving the updated document.

#### Step-by-Step Guide

1. **Create a InputStream from the Path**

   Create a `InputStream` from the input file path.

   ```java
   InputStream fs = new FileInputStream(inputFilePath);
   {
       // Further code will be placed here
   }
   ```

2. **Create Load Options**

   Initialize the `WordProcessingLoadOptions` for loading the document. If your document is password-protected, specify the password. In this example, the document is unprotected, so the password will be ignored.

   ```java
   WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
   loadOptions.setPassword("some_password_to_open_a_document)";
   ```

3. **Load the Document into the Editor Instance**

   Use the `Editor` class to load the document with the specified load options.

   ```java
   Editor editor = new Editor(fs, loadOptions);
   {
       // Further code will be placed here
   }
   ```

4. **Retrieve and Update Form Fields**

   Obtain the `FormFieldManager` instance from the `Editor` and read the `FormFieldCollection`. Update a specific text form field by modifying its properties and then call the `UpdateFormFiled` method to apply the changes.

   ```java
   FormFieldManager fieldManager = editor.getFormFieldManager();
   FormFieldCollection collection = fieldManager.getFormFieldCollection();

   TextFormField textField = collection.getFormField("Text1",TextFormField.class);
    textField.setLocaleId(1029);
    textField.setValue("new Value");
    fieldManager.updateFormFields(collection);
   ```

5. **Create and Set Save Options**

   Initialize the `WordProcessingSaveOptions` with the desired format. Optimize memory usage if necessary, and protect the document from writing with a password.

   ```java
   WordProcessingFormats docFormat = WordProcessingFormats.Docx;
   WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(docFormat);
   saveOptions.setOptimizeMemoryUsage(true);
   saveOptions.setProtection(new WordProcessingProtection(WordProcessingProtectionType.AllowOnlyFormFields, "write_password"));
   ```

6. **Save the Document**

   Prepare a stream for saving and use the `Save` method of the `Editor` to save the updated document.

   ```java
   ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
   {
       editor.save(outputStream, saveOptions);
   }
   ```

7. **Completion Message**

   Print a confirmation message to indicate the successful completion of the operation.

   ```java
   System.out.println("EditFormFieldCollection routine has successfully finished");
   ```

### Complete Example Code

Below is the complete example code demonstrating the entire process:

```java
/**
 * This example demonstrates how to edit form fields in a Word document using GroupDocs.Editor for Java.
 */
public class EditFormFieldCollection {
     /**
      * Runs the example to demonstrate loading, editing, and saving form fields in a document.
      */
    public static void run() throws Exception
    {
        // 1. Get a path to the input file (or stream with file content).
        // In this case, it is a sample DOCX with form fields.
        String inputFilePath = Constants.SampleLegacyFormFields_docx;

        // 2. Create a stream from this path
        InputStream fs = new FileInputStream(inputFilePath);

        // 3. Create load options for this document
        WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
        // 3.1. If the input document is password-protected, specify the password for its opening...
        loadOptions.setPassword("some_password_to_open_a_document");
        // 3.2. ...but, because the document is unprotected, this password will be ignored

        // 4. Load the document with options into the Editor instance
        Editor editor = new Editor(fs, loadOptions);

        // 4.1. Read the FormFieldManager instance
        FormFieldManager fieldManager = editor.getFormFieldManager();
        // 4.2. Read the FormFieldCollection in the document
        FormFieldCollection collection = fieldManager.getFormFieldCollection();

        // 4.3. Update a specific text form field
        TextFormField textField = collection.getFormField("Text1",TextFormField.class);
        textField.setLocaleId(1029);
        textField.setValue("new Value");
        fieldManager.updateFormFields(collection);

        // 5. Create document save options
        WordProcessingFormats docFormat = WordProcessingFormats.Docx;
        WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(docFormat);

        // 5.1. If the document is large and causes OutOfMemoryException, set the memory optimization option
        saveOptions.setOptimizeMemoryUsage(true);

        // 5.2. Protect the document from writing (allow only form fields) with a password
        saveOptions.setProtection(new WordProcessingProtection(WordProcessingProtectionType.AllowOnlyFormFields, "write_password"));

        // 6. Save the document
        // 6.1. Prepare a stream for saving
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        {
            // 6.2. Save the document
            editor.save(outputStream, saveOptions);
        }

        System.out.println("EditFormFieldCollection routine has successfully finished");
    }
}
```
