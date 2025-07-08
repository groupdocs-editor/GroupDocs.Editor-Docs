---
id: create-document
url: editor/net/create-document
title: Create and edit new WordProcessing document
weight: 8
description: "This article demonstrates how to create and edit WordProcessing documents using GroupDocs.Editor for .NET. It also covers supported formats like spreadsheets and presentations."
keywords: Create new document, edit document, WordProcessing, GroupDocs.Editor, DOCX
productName: GroupDocs.Editor for .NET
hideChildren: False
structuredData:
    showOrganization: True
    application:    
        name: Create and edit new WordProcessing document
        description: Create and edit new documents using GroupDocs.Editor for .NET in C#
        productCode: editor
        productPlatform: net 
    showVideo: True
    howTo:
        name: How to create and edit a new WordProcessing document using GroupDocs.Editor in C#
        description: Step-by-step guide for creating, editing, and saving DOCX documents using GroupDocs.Editor for .NET
        steps:
        - name: Invoke the Editor class constructor with a document format
          text: Use the appropriate constructor overload of the GroupDocs.Editor.Editor class and pass the desired format (e.g., DOCX)
        - name: Edit and save the document
          text: Generate an editable document, modify the HTML content if needed, and save the document to a stream or file

---

This guide shows how to create a new blank document in a specific format using the `Editor` class from GroupDocs.Editor for .NET.

It is possible to create a new blank document in all major document formats, including text (DOCX), workbooks (XLSX), presentations (PPTX), e-books (EPUB) and emails (EML). See the full list of [supported document formats](https://docs.groupdocs.com/editor/net/supported-document-formats/), take a note on a "Create" column, which indicates whether it is possible to create a new document of a particular format.


---

## Prerequisites

* **.NET Framework 4.6.2**
* **.NET 6.0**

> ✅ Officially supported target frameworks based on the latest package version (25.7.0).
> 🔗 See more here: [NuGet – GroupDocs.Editor](https://www.nuget.org/packages/GroupDocs.Editor/)

### Install GroupDocs.Editor for .NET

```bash
dotnet add package GroupDocs.Editor
```

---

## Steps to Create and Edit a Document

### 1. Create a new document and apply edit options

```csharp
using System;
using System.IO;
using GroupDocs.Editor;
using GroupDocs.Editor.Formats;
using GroupDocs.Editor.Options;


    using (Editor editor = new Editor(WordProcessingFormats.Docx))
    WordProcessingEditOptions editOptions = new WordProcessingEditOptions
    {
        EnablePagination = false,
        EnableLanguageInformation = true
    };

    EditableDocument editable = editor.Edit(editOptions);
```

### 2. Modify the HTML content

```csharp
    string html = editable.GetEmbeddedHtml();

    string updatedHtml = html.Replace(
        "<span class = \"text001\">&#xa0;</span>",
        "<span class = \"text001\">New text</span>");

    EditableDocument updatedDoc = EditableDocument.FromMarkup(updatedHtml);
```

### 3. Save the final document

```csharp
    WordProcessingSaveOptions saveOptions = new WordProcessingSaveOptions(WordProcessingFormats.Docx);
    saveOptions.EnablePagination = true;
    using Stream memoryStream = new MemoryStream();
    editor.Save(updatedDoc, memoryStream, saveOptions);

```

---

## Result

The code creates a new DOCX document, modifies its HTML body, and saves the final version to memory.
You can optionally save it to a physical file or return it via API.

---

## See also

* [`Editor` class constructor](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/editor/#constructor)
* [`Editor.Edit(WordProcessingEditOptions)`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/edit/)
* [`Editor.Save(EditableDocument, Stream, WordProcessingSaveOptions)`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save/)
* ▶ [GitHub Sample: EditNewDocument.cs](https://github.com/groupdocs-editor/GroupDocs.Editor-for-.NET/blob/master/Examples/GroupDocs.Editor.Examples.CSharp/AdvancedUsage/EditNewDocument.cs)

