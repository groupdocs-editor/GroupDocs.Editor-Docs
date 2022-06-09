---
id: product-overview
url: editor/net/product-overview
title: Product Overview
weight: 1
description: "GroupDocs.Editor for .NET is a C# api allows to edit Microsoft Office documents like Word, Excel or PowerPoint or PDF without third party software installed."
keywords: edit Microsoft Office documents, C#
productName: GroupDocs.Editor for .NET
toc: True
---

<img src="editor/net/images/home.png" width="110" height="110" alt="groupdocs-editor-net-home" align="left" style="margin: 0 30px 30px 0"/>

GroupDocs.Editor for .NET is a powerful and lightweight library which allows you to edit most popular document formats using 3rd party front-end WYSIWYG HTML-editors without any additional software.

GroupDocs.Editor supports most of the popular document formats such as PDF, DOCX, XLSX, PPTX, XPS and others.

By using GroupDocs.Editor for .NET you can edit files of various formats and no third-party applications required!

## How GroupDocs.Editor Works?

GroupDocs.Editor is a .NET class library (DLL), that is designed to allow users to load their documents of different format, open them for editing, edit, and save the edited version to some format, which may be the exactly same as input, or may differ. The most basic thing is that GroupDocs.Editor is GUI-less, it has no GUI, but only a public API, so it should be used within end-user environment, but not as the standalone application. Despite GroupDocs.Editor can be used anywhere, where .NET Framework is installed, it is intended to be used in web applications. Main and most common GroupDocs.Editor usage scenario implies that GroupDocs.Editor is used in a web-application and is located on the server-side. Server-based code invokes GroupDocs.Editor methods, passes input documents and different parameters, and obtains results, that are transmitted to the client-side into the WYSIWYG HTML-editor, like CKEditor or TinyMCE. When user has edited the document in the browser and is sending back the edited content to the server, server-based code again invokes GroupDocs.Editor, passes edited content and obtains edited document in desired format.
  
## Benefits of using GroupDocs.Editor

Using GroupDocs.Editor for .NET in your project gives you the following benefits:

- Rich set of file editing features;
- Platform independence;
- Independence from third-party applications;
- Performance and scalability;
- Simple public API.

### Rich set of file editing features

GroupDocs.Editor for .NET main features are the following:

- Translate document to HTML/CSS markup with resources, that is compatible with HTML WYSIWYG editors;
- Save edited HTML/CSS to source document format;
- Export edited document to PDF format;
- Plenty of additional options to customize editing process - edit password protected documents, extract document fonts, export document language information (useful for spell-checkers), specify document encoding or write-protection during saving etc.
- Document information extraction - page count, size, encrypted flag etc.

### Platform Independence

GroupDocs.Editor for .NET covers most of the popular development environments and deployment platforms. Its API can be used to develop applications for a wide range of operating systems, such as Windows, Linux, and Mac OS, and various platforms. Read ["System Requirements"]({{< ref "editor/net/getting-started/system-requirements" >}}) for more details.

You can use GroupDocs.Editor for .NET to build any type of 32-bit or 64-bit .NET application, including ASP.NET, ASP.NET Core, WCF, WinForms, etc.

### Independence from Other Applications

GroupDocs.Editor does not require third-party applications, for example, Microsoft Office, to be installed on the machine in order to work. All GroupDocs components are completely independent. This makes GroupDocs.Editor a great alternative to automation in terms of security, stability, scalability/speed, price and features for working with documents and related tasks.

### Performance and Scalability

We do care about performance. GroupDocs.Editor is designed to be used to process thousands of files and utilize as minimum resources as possible. We do performance testing to make sure we do not have performance degradations from version to version.

GroupDocs.Editor is a single .NET assembly that can be deployed with any .NET application by simply copying it or installing via NuGet. You do not need to worry about any other services or modules.

### Simple Public API

GroupDocs.Editor for .NET public API was designed to be simple and intuitive. The methods are doing what you wold expect from them and nothing more.

## Pricing and Policies

Please visit the ["Licensing and Subscription"]({{< ref "editor/net/getting-started/licensing-and-subscription.md" >}}) page for information on licenses and review the ["Pricing Information"](https://purchase.groupdocs.com/pricing/editor/net) page for details on pricing.

## Technical Support

We do provide free and paid support for all of our users, including evaluation. For more information on GroupDocs.Editor technical support please check ["Technical Support"]({{< ref "technical-support" >}}) page.
