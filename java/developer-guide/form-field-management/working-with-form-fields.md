---
id: working-with-form-fields
url: editor/java/working-with-form-fields
title: Working with Form Fields
weight: 4
description: "This article demonstrates how to load, edit, and read form fields in a Word document using GroupDocs.Editor for Java"
keywords: Edit HTML document, GroupDocs.Editor
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---


This article demonstrates how to load, edit, and read form fields in a Word document using GroupDocs.Editor for Java. We will go through the process of opening a document, retrieving form fields, and processing them based on their types.


#### Step-by-Step Guide

1. **Get the Path to the Input File**

2. **Create a InputStream from the Path**

   Create a `InputStream` from the input file path.

   ```java
   InputStream fs = new FileInputStream(inputFilePath);
   {
       // Further code will be placed here
   }
   ```

3. **Create Load Options**

   Initialize the `WordProcessingLoadOptions` for loading the document. If your document is password-protected, specify the password. In this example, the document is unprotected, so the password will be ignored.

   ```java
   WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
   loadOptions.setPassword("some_password_to_open_a_document)";
   ```

4. **Load the Document into the Editor Instance**

   Use the `Editor` class to load the document with the specified load options.

   ```java
   Editor editor = new Editor(fs, loadOptions);
   {
       // Further code will be placed here
   }
   ```

5. **Retrieve and Process the Form Fields**

   Obtain the `FormFieldManager` instance from the `Editor`. Use it to read the `FormFieldCollection` and process each form field based on its type.

   ```java
        FormFieldManager fieldManager = editor.getFormFieldManager();
        FormFieldCollection collection = fieldManager.getFormFieldCollection();
        for (IFormField formField : collection)
        {
            switch (formField.getType())
            {
                case FormFieldType.Text:
                    TextFormField textFormField = collection.getFormField(formField.getName(),TextFormField.class);
                    System.out.println("TextFormField detected, name: "+formField.getName()+", value: "+ textFormField.getValue());
                    break;
                case FormFieldType.CheckBox:
                    CheckBoxForm checkBoxFormField = collection.getFormField(formField.getName(),CheckBoxForm.class);
                    System.out.println("CheckBoxForm detected, name: "+formField.getName()+", value: "+ checkBoxFormField.getValue());
                    break;
                case FormFieldType.Date:
                    DateFormField dateFormField = collection.getFormField(formField.getName(),DateFormField.class);
                    System.out.println("DateFormField detected, name: "+formField.getName()+", value: "+ dateFormField.getValue());
                    break;
                case FormFieldType.Number:
                    NumberFormField numberFormField = collection.getFormField(formField.getName(),NumberFormField.class);
                    System.out.println("NumberFormField detected, name: "+formField.getName()+", value: "+ numberFormField.getValue());
                    break;
                case FormFieldType.DropDown:
                    DropDownFormField dropDownFormField = collection.getFormField(formField.getName(),DropDownFormField.class);
                    System.out.println("DropDownFormField detected, name: "+formField.getName()+", value selected: "+ dropDownFormField.getValue().get(dropDownFormField.getSelectedIndex()));
                    break;
            }
        }
   ```

### Complete Example Code

Below is the complete example code demonstrating the entire process:

```java
/**
  * This example demonstrates loading a FormFieldCollection and reading form fields.
  */
public class LegacyFormFieldCollection {
   /**
    * Runs the example to demonstrate loading, editing, and reading form fields from a document.
    */
    public static void run() throws Exception
    {
        // 1. Get a path to the input file (or stream with file content).
        // In this case, it is a sample Docx with form fields.
        String inputFilePath = Constants.SampleLegacyFormFields_docx;

        // 2. Create a stream from this path
        InputStream fs = new FileInputStream(inputFilePath);

        // 3. Create load options for this document
        WordProcessingLoadOptions loadOptions = new WordProcessingLoadOptions();
        // 3.1. In case the input document is password-protected, we can specify a password for its opening...
        loadOptions.setPassword("some_password_to_open_a_document");
        // 3.2. ...but, because the document is unprotected, this password will be ignored

        // 4. Load the document with options to the Editor instance
        Editor editor = new Editor(fs, loadOptions);

        // 4.1. Read the FormFieldManager instance
        FormFieldManager fieldManager = editor.getFormFieldManager();
        // 4.2. Read the FormFieldCollection in the document
        FormFieldCollection collection = fieldManager.getFormFieldCollection();
        for (IFormField formField : collection)
        {
            switch (formField.getType())
            {
                case FormFieldType.Text:
                    TextFormField textFormField = collection.getFormField(formField.getName(),TextFormField.class);
                    System.out.println("TextFormField detected, name: "+formField.getName()+", value: "+ textFormField.getValue());
                    break;
                case FormFieldType.CheckBox:
                    CheckBoxForm checkBoxFormField = collection.getFormField(formField.getName(),CheckBoxForm.class);
                    System.out.println("CheckBoxForm detected, name: "+formField.getName()+", value: "+ checkBoxFormField.getValue());
                    break;
                case FormFieldType.Date:
                    DateFormField dateFormField = collection.getFormField(formField.getName(),DateFormField.class);
                    System.out.println("DateFormField detected, name: "+formField.getName()+", value: "+ dateFormField.getValue());
                    break;
                case FormFieldType.Number:
                    NumberFormField numberFormField = collection.getFormField(formField.getName(),NumberFormField.class);
                    System.out.println("NumberFormField detected, name: "+formField.getName()+", value: "+ numberFormField.getValue());
                    break;
                case FormFieldType.DropDown:
                    DropDownFormField dropDownFormField = collection.getFormField(formField.getName(),DropDownFormField.class);
                    System.out.println("DropDownFormField detected, name: "+formField.getName()+", value selected: "+ dropDownFormField.getValue().get(dropDownFormField.getSelectedIndex()));
                    break;
            }
        }

        System.out.println("ReadFormFieldCollection routine has successfully finished");
    }
}
```

### Conclusion

This guide demonstrates how to work with form fields in Word documents using GroupDocs.Editor for Java. By following these steps, you can load a document, retrieve form fields, and process them based on their types. This functionality is essential for applications that require manipulation and extraction of form field data from documents.