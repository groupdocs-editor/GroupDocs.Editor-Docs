---
id: edit-document
url: editor/net/edit-document
title: Edit document
weight: 6
description: "Follow this guide and learn how to edit text documents, spreadsheets and presentations using GroupDocs.Editor for .NET API features."
keywords: Edit document, edit presentation, edit spreadsheet, GroupDocs.Editor
productName: GroupDocs.Editor for .NET
hideChildren: False
structuredData:
    showOrganization: True
    application:    
        name: Edit document in the GroupDocs.Editor
        description: Edit document using the GroupDocs.Editor C# language
        productCode: editor
        productPlatform: net 
    showVideo: True
    howTo:
        name: How to edit document using the GroupDocs.Editor in C#
        description: Learn how to edit document using the GroupDocs.Editor in C# step by step
        steps:
        - name: Create appropriate EditOptions instance
          text: Each format family has its own implementation of IEditOptions interface. You need to create an inheritor of the IEditOptions interface, that is corresponding to the format family of the input document
        - name: Adjust EditOptions
          text: When IEditOptions is created, you need to adjust it to meet your needs — select pagination mode (for WordProcessing documents), desired tab (Spreadsheet), slide (Presentation), or separator (Delimiter-separated values) etc.        
        - name: Invoke Editor.Edit method
          text: After document is successfully loaded to the Editor class instance and appropriate EditOptions are ready, call Editor.Edit method with specified options and obtain an instance of generated EditableDocument
        - name: Obtain HTML markup and resources from EditableDocument
          text: When EditableDocument is generated, use its methods and properties to obtain HTML-markup and all related HTML resuources (stylesheets, fonts, images, audio) in order to send and use them in the WYSIWYG HTML-editor.
---
> This article describes how to open for editing a previously loaded document, which options should be applied, and how to send document content to the WYSIWYG HTML-editor or any other editing application.

When document is loaded into the instance of the [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class, it is possible to open it for editing. In terms of [**GroupDocs.Editor**](https://products.groupdocs.com/editor/net), open a document for edit implies creating an instance of [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class by calling an [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor).[Edit()](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) instance method. There are two overloads of the [Edit](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) method. First one obtains a single parameter — inheritor of [IEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ieditoptions) interface.  
Each format family has its own implementation of [IEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ieditoptions) interface. They are listed in the table below.

| Format family | Example formats | Edit options class |
| --- | --- | --- |
| WordProcessing | DOC, DOCX, DOCM, DOT, ODT | [WordProcessingEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/wordprocessingeditoptions) |
| Spreadsheet | XLS, XLSX, XLSM, XLSB | [SpreadsheetEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/spreadsheeteditoptions) |
| Delimiter-Separated Values (DSV) | CSV, TSV | [DelimitedTextEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/delimitedtexteditoptions) |
| Presentation | PPT, PPTX, PPS, POT | [PresentationEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationeditoptions) |
| Plain Text documents | TXT | [TextEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/texteditoptions) |
| Fixed-layout format | PDF | [PdfEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/pdfeditoptions) |
| Fixed-layout format | XPS (including OpenXPS) | [XpsEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xpseditoptions) |
| XML | Any XML-based format like CSPROJ, SVG, and so on | [XmlEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/xmleditoptions) |
| eBook | MOBI, AZW3, ePub | [EbookEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/ebookeditoptions) |
| Email | EML, EMLX, TNEF, MSG, HTML, MHTML, ICS, VCF, PST, MBOX, OFT | [EmailEditOptions](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/emaileditoptions) |

Second overload is parameterless — it chooses the most appropriate default edit options based on input document format.

[EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance holds a version of input document, converted to internal intermediate format according to edit options. When it is ready, it can emit HTML, CSS and other appropriate content, that can be passed directly to the WYSIWYG-editor. This is demonstrated below.

```csharp
string inputFilePath = "C:\\input_path\\document.docx"; //path to some document
Editor editor = new Editor(inputFilePath, new WordProcessingLoadOptions());
EditableDocument openedDocument = editor.Edit();//with default edit options

WordProcessingEditOptions editOptions = new WordProcessingEditOptions();
editOptions.EnableLanguageInformation = true;
editOptions.EnablePagination = true;

EditableDocument anotherOpenedDocument = editor.Edit(editOptions);
```

There can be generated several [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instances from a single [Editor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) instance with different edit options. For example, for WordProcessing document, first time user can call [Edit()](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit) method with disabled paged mode, and for the second time — with enabled. In other words, there can be generated several different [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) representations of the single original document. Other example — there can be multiple [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instances for a single input Spreadsheet document, where each instance represents different tab of the Spreadsheet document. Such example is shown below.

```csharp
string inputXlsxPath = "C://input/spreadsheet.xlsx";
Editor editor = new Editor(inputXlsxPath, new SpreadsheetLoadOptions());

SpreadsheetEditOptions editOptions1 = new SpreadsheetEditOptions();
editOptions1.WorksheetIndex = 0;//index is 0-based, so this is 1st tab
editOptions1.ExcludeHiddenWorksheets = true;
  
SpreadsheetEditOptions editOptions2 = new SpreadsheetEditOptions();
editOptions2.WorksheetIndex = 1;//index is 0-based, so this is 2nd tab
editOptions2.ExcludeHiddenWorksheets = false;
  
EditableDocument firstTab = editor.Edit(editOptions1);
EditableDocument secondTab = editor.Edit(editOptions2);
```

When [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance is ready, it can emit HTML-markup, CSS-markup and other resources in different forms for passing them to the client-side WYSIWYG HTML-editor or any other application, that can edit HTML documents. It is briefly shown in the example below.

```csharp
EditableDocument document = editor.Edit();
string bodyContent = document.GetBodyContent();
List<IImageResource> onlyImages = document.Images;
List<IHtmlResource> allResourcesTogether = document.AllResources;
```

For more information about obtaining HTML markup and resources from [EditableDocument](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) please visit [Get HTML markup in different forms]({{< ref "editor/net/developer-guide/editabledocument/get-html-markup-in-different-forms.md" >}}), [Working with resources]({{< ref "editor/net/developer-guide/editabledocument/working-with-resources.md" >}}), and [Save HTML to folder]({{< ref "editor/net/developer-guide/editabledocument/save-html-to-folder.md" >}}) articles.
