---
id: save-document
url: editor/python-net/save-document
title: Save document
linkTitle: Save document
weight: 8
description: "This article demonstrates how to save edited text documents, spreadsheets and presentations with GroupDocs.Editor for Python via .NET API."
keywords: Save edited document, edit document, GroupDocs.Editor, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: Save document in the GroupDocs.Editor
        description: Save document using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to save document using the GroupDocs.Editor in Python
        description: Learn how to save document using the GroupDocs.Editor in Python step by step
        steps:
        - name: Submit edited HTML-document to the server-side
          text: After document was edited on the client-side with WYSIWYG HTML-editor, its edited HTML-markup and resources must be submitted to the server-side, because the GroupDocs.Editor is a server-based middleware.
        - name: Create EditableDocument instance from edited content
          text: Once the content of edited document is transferred to the server-side, you should create an instance of EditableDocument class from this content by using the most appropriate class method from this type.
        - name: Create and adjust the appropriate save options
          text: Depending on what format the input document had and which output format you want to obtain, you need to create an appropriate save options class and adjust it by selecting the desired format, protection, and other options.
        - name: Invoke the editor.save method
          text: Finally, when the edited document is stored within the EditableDocument instance and the save options are prepared, you should invoke the desired overload of the editor.save method (one saves the document to a file, while another to a byte stream).
---
> This article describes how to obtain edited document content from the client, process it and save it to the resultant document of some specified format.

In a nutshell, the core process of document editing, where a user types some text across the pages of the document, inserts images, makes some edits, removes words or paragraphs, or moves some document parts from one location to another, is performed in some 3rd party software with GUI outside of GroupDocs.Editor. This software may be, for example, but not limited to:

- a web-based WYSIWYG HTML-editor, that is usually a pure client-side application, written in JavaScript and running in the browser. This may be, for example, a TinyMCE or CKEditor;
- a desktop application;
- a mobile application, running on Android or iOS.

The core requirement for this application is to be able to open, view and edit HTML content. GroupDocs.Editor on its side accepts various document formats (WordProcessing, Spreadsheet, Presentation, and many more) and prepares them for editing in external applications by converting them to the HTML markup and placing it in the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) container. So when calling the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor).[`edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method, the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) with the **original content** inside it is generated.

Then the user obtains this **original content** from the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), pushes it to the external HTML-editor, makes edits, and when these edits are done, the user has the **modified content**. In the GroupDocs.Editor terminology, saving a document means obtaining the **modified content** and generating the document in the output format (WordProcessing, Spreadsheet, Presentation, and many more) from it. And this article explains how to do this.

In short, saving a document implies the next three steps:

1. Obtain the **modified content** from somewhere (HTML-editor or any other storage or method) and create an instance of [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) from it. [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) actually serves as an object-oriented wrapper of the document content.
2. Select a desired output format, into which the **modified content** should be saved, and optional parameters.
3. Save the **modified content** to the document of the previously chosen format by specified file path or into a specified stream using the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor).[`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.

These three steps are explained in detail and with code samples below.

## Obtaining the modified content

Different content editors provide the content in different forms. Some may return HTML markup as a string, while the external resources like stylesheet(s) and/or image(s) are located in some specific folder as files. Others may return both main content and resources as a collection of byte streams. Some HTML-editors may generate a single string, which already contains all HTML markup with resources baked into it using base64 encoding. There may be unlimited possibilities to do that, and thus the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class has different class methods (factories), which obtain the **modified content** of the HTML document in different forms on input and according to this generate the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance:

1. [`from_file`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/fromfile) is designed for opening HTML-documents from disk — it obtains a path to a `.html` file (that contains HTML-markup) and a path to the corresponding resource folder that contains different resources, like stylesheets, images, font files, audio files and so on.
2. [`from_markup`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup) is designed for opening HTML-documents from memory — it obtains the HTML-markup as a string with resources baked into it.
3. [`from_markup_and_resource_folder`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkupandresourcefolder) is designed for opening HTML-documents from mixed storages — it obtains the HTML-markup as a string, but resources are obtained from a path to the corresponding resource folder, which, unlike the previous method, is mandatory (such a directory should exist).

## Create and adjust saving options

When the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance with the **modified content** is created, it's time to save it to the resultant document of some defined format, and GroupDocs.Editor needs to know this format. Like with load and edit options, every family format has its own save options class. These classes are listed below.

| Format family | Example formats | Save options class | Format class |
| --- | --- | --- | --- |
| WordProcessing | DOC, DOCX, DOCM, DOT, ODT | [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) | [WordProcessingFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/wordprocessingformats) |
| Spreadsheet | XLS, XLSX, XLSM, XLSB | [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions) | [SpreadsheetFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/spreadsheetformats) |
| Delimiter-Separated Values (DSV) | CSV, TSV | [DelimitedTextSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/delimitedtextsaveoptions) | [SpreadsheetFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/spreadsheetformats/) |
| Presentation | PPT, PPTX, PPS, POT | [PresentationSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions) | [PresentationFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/presentationformats) |
| Plain Text documents | TXT | [TextSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/textsaveoptions) | [TextualFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/textualformats) |
| Fixed-layout format | PDF | [PdfSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/pdfsaveoptions) | [FixedLayoutFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/fixedlayoutformats) |
| Fixed-layout format | XPS | [XpsSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xpssaveoptions) | [FixedLayoutFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/fixedlayoutformats) |
| Email | EML, EMLX, TNEF, MSG, HTML, MHTML, ICS, VCF, PST, MBOX, OFT | [EmailSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emailsaveoptions) | [EmailFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/emailformats) |
| e-Books | ePub, Mobi, AZW3 | [EbookSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebooksaveoptions/) | [EBookFormats](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/ebookformats) |

So, let's say it is necessary to save the **modified content** to a document of DOCX format. DOCX is part of the WordProcessing family. So an instance of the [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions) class should be created, and the [`WordProcessingFormats.DOCX`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.formats/wordprocessingformats/) value should be specified in its constructor. Like this:

```python
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions

save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
```

If it is also necessary to encode the resultant document with a password, it can be done using one line of code:

```python
save_options.password = "some-password"
```

Of course, different formats have different options. For example, there is a `password` property in [WordProcessingSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/wordprocessingsaveoptions), [SpreadsheetSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/spreadsheetsaveoptions), [PresentationSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions), but there is nothing similar in [XpsSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xpssaveoptions), [EmailSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/emailsaveoptions), [TextSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/textsaveoptions), and [EbookSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/ebooksaveoptions/), because these formats do not support password protection.

Also need to mention that the format of the input document with the **original content** and the format of the resultant document with the **modified content** may be different. For example, the original document can be in some WordProcessing format, while the output document can be TXT or PDF. Or the original document can be a PDF, while the output is DOCX. The same transitions are allowed between Spreadsheets and DSV (two-ways). But, of course, they are not allowed where formats are theoretically incompatible in their essence, like WordProcessing and Spreadsheet.

## Saving modified content to the document

Finally, when the instance of the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class with modified content inside is created, and the format of the resultant document is defined, it is possible to generate this resultant document using the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor).[`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method. This method has two overloads. These overloads differ only in the way the output document is specified: as a path, where the file should be created, or as a byte stream, into which the document content should be written. All other parameters are the same.

```python
editor.save(input_document, file_path, save_options)
editor.save(input_document, output_stream, save_options)
```

Here:
- The 1st parameter — `input_document` — is an [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance with the **modified content** inside, created on the 1st step.
- The 2nd parameter is a stream, into which the resultant document of the defined format should be written, or a string, that represents a file path, where the resultant document of the defined format should be stored.
- The 3rd parameter is an instance of some save options, which defines the format of the resultant document and additional adjustments, created on the 2nd step.

There is also a simplified overload that analyzes the file extension of the `file_path` argument and infers the default saving options automatically:

```python
editor.save(input_document, file_path)
```

## Complete code example

Because the WYSIWYG HTML-editor is not a part of GroupDocs.Editor, it is hard to provide a lightweight code example with fully functional editing. So in this sample the content will be edited programmatically, using the string `replace` method. The example loads a DOCX document, edits its content, and saves the **modified content** to several output formats — RTF, DOCM, and TXT.

{{< tabs "code-example-save-document">}}
{{< tab "save_document.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingSaveOptions, TextSaveOptions

def save_document():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Create the Editor by loading an input document in DOCX format
    with Editor("./sample-document.docx") as editor:
        # Open the document for editing and obtain an EditableDocument
        original = editor.edit()

        # Get the original content as a string
        original_content = original.get_embedded_html()

        # Get the modified content by editing the original content
        modified_content = original_content.replace("Title of the document", "Title of the edited document")

        # Create an EditableDocument from the modified content
        modified = EditableDocument.from_markup(modified_content)

        # Save the modified content to several output formats
        editor.save(modified, "./edited-document.rtf", WordProcessingSaveOptions(WordProcessingFormats.RTF))
        editor.save(modified, "./edited-document.docm", WordProcessingSaveOptions(WordProcessingFormats.DOCM))
        editor.save(modified, "./edited-document.txt", TextSaveOptions())

        # Release the EditableDocument instances
        original.dispose()
        modified.dispose()

if __name__ == "__main__":
    save_document()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/save-document/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "save-document-outputs.zip" >}}  
```text
edited-document.docm (49 KB)
edited-document.rtf (1648 KB)
edited-document.txt (5 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/save-document/save_document/save-document-outputs.zip)
{{< /tab >}}
{{< /tabs >}}

In this example, three different save options are created and three different [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor).[`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) calls are made for saving the modified content to the RTF, DOCM, and TXT formats.
