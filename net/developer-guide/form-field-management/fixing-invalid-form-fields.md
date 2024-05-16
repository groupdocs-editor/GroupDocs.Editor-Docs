---
id: fixing-invalid-form-fields
url: editor/net/fixing-invalid-form-fields
title: Fixing Invalid Form Fields
weight: 4
description: "Fixing Invalid Form Fields and Saving the Document with GroupDocs.Editor for .NET"
keywords: Fix Invalid Form Fields, FormFieldManager, Invalid Form Fields, Update Form Fields
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---

This article demonstrates how to fix invalid form fields in a Word document using GroupDocs.Editor for .NET. The example guides you through loading a document, identifying and fixing invalid form fields, and saving the updated document.

#### Step-by-Step Guide

1. **Create a Stream from the Path**

   Create a `FileStream` from the input file path.

   ```csharp
   using (FileStream fs = File.OpenRead(inputFilePath))
   {
       // Further code will be placed here
   }
   ```

2. **Create Load Options**

   Initialize the `WordProcessingLoadOptions` for loading the document. If your document is password-protected, specify the password. In this example, the document is unprotected, so the password will be ignored.

   ```csharp
   WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
   loadOptions.Password = "some_password_to_open_a_document";
   ```

3. **Load the Document into the Editor Instance**

   Use the `Editor` class to load the document with the specified load options.

   ```csharp
   using (Editor editor = new Editor(delegate { return fs; }, delegate { return loadOptions; }))
   {
       // Further code will be placed here
   }
   ```

4. **Retrieve and Fix Invalid Form Fields**

   Obtain the `FormFieldManager` instance from the `Editor` and read the `FormFieldCollection`. Attempt to auto-fix invalid form fields, then check if any invalid form fields remain. If so, generate unique names for the invalid fields and fix them.

   ```csharp
   FormFieldManager fieldManager = editor.FormFieldManager;
   FormFieldCollection collection = fieldManager.FormFieldCollection;

   fieldManager.FixInvalidFormFieldNames(new InvalidFormField[0]);
   collection = fieldManager.FormFieldCollection;

   bool hasInvalidFormFields = fieldManager.HasInvalidFormFields();
   System.Console.WriteLine("FormFieldCollection contains invalid items: {0}", hasInvalidFormFields);

   var invalidFormFields = fieldManager.GetInvalidFormFieldNames();
   foreach (var invalidItem in invalidFormFields)
   {
       invalidItem.FixedName = string.Format("{0}_{1}", invalidItem.Name, Guid.NewGuid());
   }
   fieldManager.FixInvalidFormFieldNames(invalidFormFields);
   collection = fieldManager.FormFieldCollection;
   ```

5. **Create and Set Save Options**

   Initialize the `WordProcessingSaveOptions` with the desired format. Optimize memory usage if necessary, and protect the document from writing with a password.

   ```csharp
   WordProcessingFormats docFormat = WordProcessingFormats.Docx;
   WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(docFormat);
   saveOptions.OptimizeMemoryUsage = true;
   saveOptions.Protection = new WordProcessingProtection(WordProcessingProtectionType.AllowOnlyFormFields, "write_password");
   ```

6. **Save the Document**

   Prepare a stream for saving and use the `Save` method of the `Editor` to save the updated document.

   ```csharp
   using (MemoryStream outputStream = new MemoryStream())
   {
       editor.Save(outputStream, saveOptions);
   }
   ```

7. **Completion Message**

   Print a confirmation message to indicate the successful completion of the operation.

   ```csharp
   System.Console.WriteLine("FixInvalidFormFieldCollectionAndSave routine has successfully finished");
   ```

### Complete Example Code

Below is the complete example code demonstrating the entire process:

```csharp
/// <summary>
/// This example demonstrates how to fix invalid form fields in a collection and save the document using GroupDocs.Editor for .NET.
/// </summary>
internal static class FixInvalidFormFieldCollectionAndSave
{
    /// <summary>
    /// Runs the example to demonstrate loading, fixing invalid form fields, and saving a document.
    /// </summary>
    internal static void Run()
    {
        // 1. Get a path to the input file (or stream with file content).
        // In this case, it is a sample DOCX with form fields.
        string inputFilePath = Constants.SampleLegacyFormFields_docx;

        // 2. Create a stream from this path
        using (FileStream fs = File.OpenRead(inputFilePath))
        {
            // 3. Create load options for this document
            WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
            // 3.1. If the input document is password-protected, specify the password for its opening...
            loadOptions.Password = "some_password_to_open_a_document";
            // 3.2. ...but, because the document is unprotected, this password will be ignored

            // 4. Load the document with options into the Editor instance
            using (Editor editor = new Editor(delegate { return fs; }, delegate { return loadOptions; }))
            {
                // 4.1. Read the FormFieldManager instance
                FormFieldManager fieldManager = editor.FormFieldManager;
                // 4.2. Read the FormFieldCollection in the document
                FormFieldCollection collection = fieldManager.FormFieldCollection;

                // 4.3. Try to auto-fix invalid form fields
                fieldManager.FixInvalidFormFieldNames(new InvalidFormField[0]);
                collection = fieldManager.FormFieldCollection;

                bool hasInvalidFormFields = fieldManager.HasInvalidFormFields();
                System.Console.WriteLine("FormFieldCollection contains invalid items: {0}", hasInvalidFormFields);

                // 4.4. Try to fix invalid form fields
                // 4.4.1. Get all invalid form field names


                var invalidFormFields = fieldManager.GetInvalidFormFieldNames();

                // 4.4.2. Create unique names for invalid fields
                foreach (var invalidItem in invalidFormFields)
                {
                    invalidItem.FixedName = string.Format("{0}_{1}", invalidItem.Name, Guid.NewGuid());
                }

                // 4.4.3. Fix invalid fields
                fieldManager.FixInvalidFormFieldNames(invalidFormFields);
                collection = fieldManager.FormFieldCollection;

                // 5. Create document save options
                WordProcessingFormats docFormat = WordProcessingFormats.Docx;
                WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(docFormat);

                // 5.1. If the document is large and causes OutOfMemoryException, set the memory optimization option
                saveOptions.OptimizeMemoryUsage = true;

                // 5.2. Protect the document from writing (allow only form fields) with a password
                saveOptions.Protection = new WordProcessingProtection(WordProcessingProtectionType.AllowOnlyFormFields, "write_password");

                // 6. Save the document
                // 6.1. Prepare a stream for saving
                using (MemoryStream outputStream = new MemoryStream())
                {
                    // 6.2. Save the document
                    editor.Save(outputStream, saveOptions);
                }
            }
        }
        System.Console.WriteLine("FixInvalidFormFieldCollectionAndSave routine has successfully finished");
    }
}
```

