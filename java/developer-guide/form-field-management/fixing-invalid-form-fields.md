---
id: fixing-invalid-form-fields
url: editor/java/fixing-invalid-form-fields
title: Fixing Invalid Form Fields
weight: 4
description: "Fixing Invalid Form Fields and Saving the Document with GroupDocs.Editor for Java"
keywords: Fix Invalid Form Fields, FormFieldManager, Invalid Form Fields, Update Form Fields
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---

This article demonstrates how to fix invalid form fields in a Word document using GroupDocs.Editor for Java. The example guides you through loading a document, identifying and fixing invalid form fields, and saving the updated document.

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

4. **Retrieve and Fix Invalid Form Fields**

   Obtain the `FormFieldManager` instance from the `Editor` and read the `FormFieldCollection`. Attempt to auto-fix invalid form fields, then check if any invalid form fields remain. If so, generate unique names for the invalid fields and fix them.

   ```java
   FormFieldManager fieldManager = editor.getFormFieldManager();
   FormFieldCollection collection = fieldManager.getFormFieldCollection();

   fieldManager.fixInvalidFormFieldNames(new ArrayList<InvalidFormField>());
   collection = fieldManager.getFormFieldCollection();

   boolean hasInvalidFormFields = fieldManager.hasInvalidFormFields();
   System.out.println("FormFieldCollection contains invalid items: "+ hasInvalidFormFields);

   Collection<InvalidFormField> invalidFormFields = fieldManager.getInvalidFormFieldNames();
   for (InvalidFormField invalidItem : invalidFormFields)
   {
       invalidItem.setFixedName(String.format("%s_%s", invalidItem.getName(), java.util.UUID.randomUUID()));
   }
   fieldManager.fixInvalidFormFieldNames(new ArrayList(invalidFormFields));
   collection = fieldManager.getFormFieldCollection();
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
   System.out.println("FixInvalidFormFieldCollectionAndSave routine has successfully finished");
   ```

### Complete Example Code

Below is the complete example code demonstrating the entire process:

```java
/**
  * This example demonstrates how to fix invalid form fields in a collection and save the document using GroupDocs.Editor for Java.
  */
public class FixInvalidFormFieldCollectionAndSave {
  /**
    * Runs the example to demonstrate loading, fixing invalid form fields, and saving a document.
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
        Editor editor = new Editor(fs,loadOptions);

        // 4.1. Read the FormFieldManager instance
        FormFieldManager fieldManager = editor.getFormFieldManager();
        // 4.2. Read the FormFieldCollection in the document
        FormFieldCollection collection = fieldManager.getFormFieldCollection();

        // 4.3. Try to auto-fix invalid form fields
        fieldManager.fixInvalidFormFieldNames(new ArrayList<InvalidFormField>());
        collection = fieldManager.getFormFieldCollection();

        boolean hasInvalidFormFields = fieldManager.hasInvalidFormFields();
        System.out.println("FormFieldCollection contains invalid items: "+ hasInvalidFormFields);

        // 4.4. Try to fix invalid form fields
        // 4.4.1. Get all invalid form field names
        Collection<InvalidFormField> invalidFormFields = fieldManager.getInvalidFormFieldNames();

        // 4.4.2. Create unique names for invalid fields
        for (InvalidFormField invalidItem : invalidFormFields)
        {
            invalidItem.setFixedName(String.format("%s_%s", invalidItem.getName(), java.util.UUID.randomUUID()));
        }

        // 4.4.3. Fix invalid fields
        fieldManager.fixInvalidFormFieldNames(new ArrayList(invalidFormFields));
        collection = fieldManager.getFormFieldCollection();

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


        System.out.println("FixInvalidFormFieldCollectionAndSave routine has successfully finished");
    }
}

```

