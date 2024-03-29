---
id: load-document
url: editor/java/load-document
title: Load document
weight: 2
description: "Following this guide you will learn how to load document from local disk or file stream for editing with GroupDocs.Editor for Java API."
keywords: Load document with GroupDocs.Editor, Load and edit document, edit document, edit spreadsheet, edit presentation
productName: GroupDocs.Editor for Java
hideChildren: False
structuredData:
    showOrganization: True
    application:    
        name: Load document into GroupDocs.Editor
        description: Load document into GroupDocs.Editor for subsequent usage on Java language
        productCode: editor
        productPlatform: java 
    showVideo: True
    howTo:
        name: How to load document to the GroupDocs.Editor in Java
        description: Learn how to load document to the GroupDocs.Editor in Java step by step
        steps:
        - name: Prepare a LoadOptions instance if needed 
          text: If input document is password-protected or requires some adjusting using load, or it is required to specify its format family explicitly, create an appropriate inheritor of the ILoadOptions interface
        - name: Invoke the appropriate Editor class constructor
          text: Pass a filepath as a string or a byte stream through delegate into the most appropriate overload of the constructor of GroupDocs.Editor.Editor class
        - name: Make all necessary edits and savings
          text: After document is successfully loaded, you can get its metainfo, generate its editable version, and finally save it to the resultant file
---
> This article describes how to load input document to the [**GroupDocs.Editor**](https://products.groupdocs.com/editor/java) and how to apply load options.

First of all the input document, which should be accessible as a byte stream or through valid file path, should be loaded into the GroupDocs.Editor by creating an instance of the [Editor](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor) class through one of the constructor overloads.  
Source code below shows two ways of loading documents: from path and from stream.

```java
//through path
String inputFilePath = "C:\\input_path\\document.docx"; //path to some document
Editor editor = new Editor(inputFilePath);

//through stream
InputStream inputStream = new FileInputStream(inputFilePath);
Editor editor = new Editor(inputStream);
```

When two overloads from example above are used, GroupDocs.Editor automatically detects the format of input document and applies the most appropriate default loading options for the input document.  
However, it is possible and even recommended to specify correct loading options explicitly using constructor overloads, which accept two parameters. Source code below shows using such options.

```java
//through path
String inputFilePath = "C:\\input_path\\document.docx"; //path to some document
WordProcessingLoadOptions wordLoadOptions = new WordProcessingLoadOptions();
Editor editor1 = new Editor(inputFilePath, wordLoadOptions); //passing path and load options (via delegate) to the constructor

//through stream
InputStream inputStream = new ByteArrayInputStream(new byte[0]); //obtained from somewhere
SpreadsheetLoadOptions spreadsheetLoadOptions = new SpreadsheetLoadOptions();
Editor editor = new Editor(inputStream, spreadsheetLoadOptions);
```

Please note that not all document formats have appropriate classes, that represent load options. As for version 19.10, only WordProcessing, Spreadsheet and Presentation family formats have load options. For other document types, such as DSV, TXT or XML, there are no load options.

| Format family | Example formats | Load options class |
| --- | --- | --- |
| WordProcessing | DOC, DOCX, DOCM, DOT, ODT | `WordProcessingLoadOptions` |
| Spreadsheet | XLS, XLSX, XLSM, XLSB | `SpreadsheetLoadOptions` |
| Presentation | PPT, PPTX, PPS, POT | `PresentationLoadOptions` |

Using load options is the only way for working with password-protected input documents. Any document can be loaded into the `Editor` instance, even encoded document without the password. However, on the next step — opening for editing, — the exception will be thrown. GroupDocs.Editor handles passwords and encoded documents in the next way:

1. If document is not encoded, password is ignored anyway, whether or not it was specified.
2. If document is password-protected, but password is not specified, the [PasswordRequiredException](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/passwordrequiredexception) will be thrown later during editing.
3. If document is password-protected, and password is specified, but is incorrect, the [IncorrectPasswordException](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/incorrectpasswordexception) will be thrown later during editing.

Example below shows specifying password for opening some password-protected WordProcessing document.

```java
Stream inputStream = getDocumentStreamFromSomewhere();
WordProcessingLoadOptions wordLoadOptions = new WordProcessingLoadOptions();
wordLoadOptions.setPassword("correct_password");
Editor editor = new Editor(inputFilePath, wordLoadOptions);
```

{{< alert style="info" >}}Same approach is applicable for Spreadsheet and Presentation documents too.{{< /alert >}}
