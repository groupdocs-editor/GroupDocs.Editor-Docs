---
id: working-with-form-fields
url: editor/net/working-with-form-fields
title: Working with Form Fields
weight: 4
description: "This article demonstrates how to load, edit, and read form fields in a Word document using GroupDocs.Editor for .NET"
keywords: Edit HTML document, GroupDocs.Editor
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---


This article demonstrates how to load, edit, and read form fields in a Word document using GroupDocs.Editor for .NET. We will go through the process of opening a document, retrieving form fields, and processing them based on their types.


#### Step-by-Step Guide

1. **Get the Path to the Input File**

2. **Create a Stream from the Path**

   Create a `FileStream` from the input file path.

   ```csharp
   using (FileStream fs = File.OpenRead(inputFilePath))
   {
       // Further code will be placed here
   }
   ```

3. **Create Load Options**

   Initialize the `WordProcessingLoadOptions` for loading the document. If your document is password-protected, specify the password. In this example, the document is unprotected, so the password will be ignored.

   ```csharp
   WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
   loadOptions.Password = "some_password_to_open_a_document";
   ```

4. **Load the Document into the Editor Instance**

   Use the `Editor` class to load the document with the specified load options.

   ```csharp
   using (Editor editor = new Editor(delegate { return fs; }, delegate { return loadOptions; }))
   {
       // Further code will be placed here
   }
   ```

5. **Retrieve and Process the Form Fields**

   Obtain the `FormFieldManager` instance from the `Editor`. Use it to read the `FormFieldCollection` and process each form field based on its type.

   ```csharp
   FormFieldManager fieldManager = editor.FormFieldManager;
   FormFieldCollection collection = fieldManager.FormFieldCollection;

   foreach (var formField in collection)
   {
       switch (formField.Type)
       {
           case FormFieldType.Text:
               TextFormField textFormField = collection.GetFormField<TextFormField>(formField.Name);
               System.Console.WriteLine("TextFormField detected, name: {0}, value: {1}", formField.Name, textFormField.Value);
               break;

           case FormFieldType.CheckBox:
               CheckBoxForm checkBoxFormField = collection.GetFormField<CheckBoxForm>(formField.Name);
               System.Console.WriteLine("CheckBoxForm detected, name: {0}, value: {1}", formField.Name, checkBoxFormField.Value);
               break;

           case FormFieldType.Date:
               DateFormField dateFormField = collection.GetFormField<DateFormField>(formField.Name);
               System.Console.WriteLine("DateFormField detected, name: {0}, value: {1}", formField.Name, dateFormField.Value);
               break;

           case FormFieldType.Number:
               NumberFormField numberFormField = collection.GetFormField<NumberFormField>(formField.Name);
               System.Console.WriteLine("NumberFormField detected, name: {0}, value: {1}", formField.Name, numberFormField.Value);
               break;

           case FormFieldType.DropDown:
               DropDownFormField dropDownFormField = collection.GetFormField<DropDownFormField>(formField.Name);
               System.Console.WriteLine("DropDownFormField detected, name: {0}, value selected: {1}", formField.Name, dropDownFormField.Value[dropDownFormField.SelectedIndex]);
               break;
       }
   }
   ```

### Complete Example Code

Below is the complete example code demonstrating the entire process:

```csharp
/// <summary>
/// This example demonstrates loading a FormFieldCollection and reading form fields.
/// </summary>
internal static class LegacyFormFieldCollection
{
    /// <summary>
    /// Runs the example to demonstrate loading, editing, and reading form fields from a document.
    /// </summary>
    internal static void Run()
    {
        // 1. Get a path to the input file (or stream with file content).
        // In this case, it is a sample Docx with form fields.
        string inputFilePath = Constants.SampleLegacyFormFields_docx;

        // 2. Create a stream from this path
        using (FileStream fs = File.OpenRead(inputFilePath))
        {
            // 3. Create load options for this document
            WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
            // 3.1. In case the input document is password-protected, we can specify a password for its opening...
            loadOptions.Password = "some_password_to_open_a_document";
            // 3.2. ...but, because the document is unprotected, this password will be ignored

            // 4. Load the document with options to the Editor instance
            using (Editor editor = new Editor(delegate { return fs; }, delegate { return loadOptions; }))
            {
                // 4.1. Read the FormFieldManager instance
                FormFieldManager fieldManager = editor.FormFieldManager;
                // 4.2. Read the FormFieldCollection in the document
                FormFieldCollection collection = fieldManager.FormFieldCollection;

                foreach (var formField in collection)
                {
                    switch (formField.Type)
                    {
                        case FormFieldType.Text:
                            TextFormField textFormField = collection.GetFormField<TextFormField>(formField.Name);
                            System.Console.WriteLine("TextFormField detected, name: {0}, value: {1}", formField.Name, textFormField.Value);
                            break;

                        case FormFieldType.CheckBox:
                            CheckBoxForm checkBoxFormField = collection.GetFormField<CheckBoxForm>(formField.Name);
                            System.Console.WriteLine("CheckBoxForm detected, name: {0}, value: {1}", formField.Name, checkBoxFormField.Value);
                            break;

                        case FormFieldType.Date:
                            DateFormField dateFormField = collection.GetFormField<DateFormField>(formField.Name);
                            System.Console.WriteLine("DateFormField detected, name: {0}, value: {1}", formField.Name, dateFormField.Value);
                            break;

                        case FormFieldType.Number:
                            NumberFormField numberFormField = collection.GetFormField<NumberFormField>(formField.Name);
                            System.Console.WriteLine("NumberFormField detected, name: {0}, value: {1}", formField.Name, numberFormField.Value);
                            break;

                        case FormFieldType.DropDown:
                            DropDownFormField dropDownFormField = collection.GetFormField<DropDownFormField>(formField.Name);
                            System.Console.WriteLine("DropDownFormField detected, name: {0}, value selected: {1}", formField.Name, dropDownFormField.Value[dropDownFormField.SelectedIndex]);
                            break;
                    }
                }
            }
        }
        System.Console.WriteLine("ReadFormFieldCollection routine has successfully finished");
    }
}
```

### Conclusion

This guide demonstrates how to work with form fields in Word documents using GroupDocs.Editor for .NET. By following these steps, you can load a document, retrieve form fields, and process them based on their types. This functionality is essential for applications that require manipulation and extraction of form field data from documents.