---
id: form-field-management
url: editor/java/form-field-management
title: Field Management in GroupDocs.Editor for Java
weight: 100
description: "This documentation section explains how to the management and manipulation of form fields within documents."
keywords: fields, form fields
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
structuredData:
    showOrganization: True
---

The [`com.groupdocs.editor.words.fieldmanagement`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/) namespace provides classes and interfaces for the management and manipulation of form fields within Word documents. This feature allows users to interact with various types of form fields programmatically, including text fields, checkboxes, dropdowns, and more. This article covers the main features and usage of field management in GroupDocs.Editor for Java.

## Key Classes and Interfaces

### IFormField

The [`IFormField`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/iformfield/) interface represents a general form field and defines common properties shared among all form fields.

#### Properties

- **getStylesheet()**: Gets the stylesheet associated with the form field.
- **getReadonly()**: Gets or sets a value indicating whether the form field is read-only.
- **getName()**: Gets the name of the form field.
- **getType()**: Gets the type of the form field.
- **getLocaleId()**: Gets or sets the locale identifier for the form field.
- **getStatusText()**: Gets or sets the status text associated with the form field.
- **getHelpText()**: Gets or sets the help text associated with the form field.

### Specific Form Field Types

The namespace includes several classes that implement `IFormField` for specific types of form fields:

- **[`CheckBoxForm`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/checkboxform/)**: Represents a checkbox form field.
- **[`DropDownFormField`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/dropdownformfield/)**: Represents a dropdown form field with a list of string values.
- **[`NumberFormField`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/numberformfield/)**: Represents a number input form field.
- **[`TextFormField`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/textformfield/)**: Represents a text input form field.
- **[`DateFormField`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/dateformfield/)** Represents a date input form field.

### FormFieldCollection

The [`FormFieldCollection`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/formfieldcollection/) class manages a collection of form fields within a document. It allows you to enumerate, insert, and retrieve form fields by their name.

#### Methods

- **insert(IFormField field)**: Adds a new form field to the collection.
- **iterator()**: Returns an enumerator that iterates through the collection.
- **get(String name)**: Gets the form field with the specified name.
- **getFormField(String name, Class type)**: Retrieves a form field of type `T` by its name.

### InvalidFormField

The [`InvalidFormField`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.words.fieldmanagement/invalidformfield/) class represents an invalid form field that needs renaming. It is used in the context of fixing invalid form field names.

#### Properties

- **getName()**: Gets the original name of the form field.
- **getFixedName()**: Gets or sets the new name for the form field after repair. The default value is `String.format("%s_fixed", name);`.


### FieldManager

The [`FormFieldManager`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/formfieldmanager/) class provides a high-level interface for managing form fields in a document.

#### Properties

- **getFormFieldCollection()**: Gets the collection of form fields in the document.

#### Methods

- **updateFormFiled(FormFieldCollection formFieldCollection)**: Updates form fields in the document.
- **fixInvalidFormFieldNames(List<UpdateInvalidFormFieldNames> updateInvalidFormFieldNames)**: Fixes invalid form field names in the document.
- **hasInvalidFormFields()**: Checks if the document has invalid form fields.
- **removeFormFiled(IFormField formField)**: Removes a specific form field.
- **removeFormFileds(List<IFormField> formFields)**: Removes multiple form fields.

## Conclusion

The `com.groupdocs.editor.words.fieldmanagement` namespace provides a powerful set of tools for managing form fields in Word documents. With these tools, developers can programmatically interact with, modify, and validate form fields, ensuring that documents are correctly formatted and free of duplicate or invalid form fields. For more details, refer to the official [GroupDocs.Editor for Java documentation](https://docs.groupdocs.com/editor/java/).
