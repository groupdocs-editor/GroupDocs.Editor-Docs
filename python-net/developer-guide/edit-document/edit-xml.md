---
id: edit-xml
url: editor/python-net/edit-xml
title: How to edit XML file
linkTitle: How to edit XML file
weight: 54
description: "This article demonstrates how to edit XML files and XML documents using the Python programming language."
keywords: GroupDocs.Editor XML support, XML format, editing XML files, edit XML documents, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
structuredData:
    showOrganization: True
    application:
        name: How to edit XML document in the GroupDocs.Editor
        description: How to edit XML document using the GroupDocs.Editor in Python language
        productCode: editor
        productPlatform: python-net
    showVideo: True
    howTo:
        name: How to edit content of the XML document in the GroupDocs.Editor in Python
        description: Learn how to edit the content of an XML document with different options and adjustments using the GroupDocs.Editor in Python step by step
        steps:
        - name: Load the desired XML document into the Editor class
          text: Create an instance of the Editor class using the most suitable constructor overload, by passing the desired XML document into it. Load options are not needed.
        - name: Prepare an XmlEditOptions class
          text: Create an instance of the XmlEditOptions class and adjust its properties to meet your needs if necessary, for example fixing incorrect structure or recognizing URIs and emails.
        - name: Call editor.edit and send the obtained EditableDocument to the WYSIWYG-editor
          text: Invoke the editor.edit method with a previously prepared XmlEditOptions and obtain an instance of the EditableDocument class, which is ready for editing.
        - name: Obtain HTML markup and resources from EditableDocument
          text: When the EditableDocument is generated, use its methods and properties to obtain the HTML markup and resources, or simply save it to the disk.
---
> This example demonstrates opening, editing, and saving XML documents, using different options and adjustments.

## Introduction

GroupDocs.Editor supports importing documents in the [XML (eXtensible Markup Language)](https://docs.fileformat.com/web/xml/) format. This article describes the XML processing mechanism and the available editing options.

## Loading XML documents

Loading XML documents into the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class is usual and the same as for other formats. There are no dedicated load options for the XML format; it is enough to specify the file itself through a file path or a byte stream.

If loading through a file path, the file extension does not matter, so you may freely load an XML file not only with the `*.xml` extension, but with any other extension like `*.csproj`, `*.svg`, or any other — only the valid internal structure matters.

Also, please note that you cannot treat HTML files like XML — only [XHTML](https://en.wikipedia.org/wiki/XHTML) can be treated like valid XML.

```python
from groupdocs.editor import Editor

# Load from a file path
editor_from_path = Editor("sample.xml")

# Load from a binary stream
with open("sample.xml", "rb") as stream:
    editor_from_stream = Editor(stream)
```

## Editing XML documents

Like for other format families in GroupDocs.Editor, there is a special [`XmlEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xmleditoptions) class for editing XML documents. As always, it is not mandatory when editing a document, so the parameterless [`editor.edit()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) overload may be used — GroupDocs.Editor will automatically detect the format and apply the default options.

The [`XmlEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xmleditoptions) class has different properties. The most useful and important ones are described below:

- `encoding` — allows setting the encoding that is applied while opening the input XML file (any XML is first of all a text file). By default all XML files are UTF-8, so the default value of this option is also UTF-8.
- `fix_incorrect_structure` — a boolean flag. GroupDocs.Editor can handle without error any XML document: corrupted, truncated, or with an invalid structure. If `fix_incorrect_structure` is enabled (`True`), GroupDocs.Editor scans the XML document and tries to fix its structure — it escapes prohibited characters, properly closes unclosed tags, opens unopened tags, fixes overlapping tags, and so on. By default it is disabled (`False`).
- `recognize_uris` — a boolean flag that enables the mechanism of recognizing and preparing URIs (web addresses). By default it is disabled (`False`). When enabled (`True`), GroupDocs.Editor scans the XML document for any valid URIs and represents them as external links in the resultant HTML using the A element.
- `recognize_emails` — a boolean flag, very similar to `recognize_uris`, but for email addresses. By default it is disabled. When enabled (`True`), all valid email addresses are represented with the [mailto](https://en.wikipedia.org/wiki/Mailto) scheme and the A element.
- `trim_trailing_whitespaces` — a boolean flag that enables truncation of trailing whitespaces in text nodes. By default it is disabled (`False`) — trailing whitespaces are preserved.
- `attribute_values_quote_type` — allows redefining the quote type used in attribute values in the resultant HTML (single quote or double quote). By default double quotes are used.

The runnable example below loads an XML file, edits it with adjusted boolean options, and obtains the resulting HTML content.

{{< tabs "code-example-edit-xml">}}
{{< tab "edit_xml.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.options import XmlEditOptions

def edit_xml():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load an input XML file into the Editor
    with Editor("./sample.xml") as editor:
        # Create and adjust the XML edit options
        edit_options = XmlEditOptions()
        edit_options.fix_incorrect_structure = True
        edit_options.recognize_uris = True
        edit_options.recognize_emails = True
        edit_options.trim_trailing_whitespaces = True

        # Edit the XML document and obtain an EditableDocument
        editable = editor.edit(edit_options)

        # Obtain the HTML content (in practice it is sent to the WYSIWYG-editor)
        content = editable.get_content()
        print("Generated HTML content length:", len(content))

        editable.dispose()

if __name__ == "__main__":
    edit_xml()
```
{{< /tab >}}
{{< tab "sample.xml" >}}  
{{< tab-text >}}
`sample.xml` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-xml/sample.xml) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edit-xml.txt" >}}  
```text
Generated HTML content length: 9345
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-xml/edit_xml/edit-xml.txt)
{{< /tab >}}
{{< /tabs >}}

The resulting [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) may be passed to the WYSIWYG-editor or any other HTML editing software, or simply saved to disk with the [`save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/save) method:

```python
with Editor("./sample.xml") as editor:
    edited = editor.edit()
    edited.save("./edited.html")
    edited.dispose()
```

## Advanced highlight and format options

The [`XmlEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/xmleditoptions) class also has two compound properties, `highlight_options` and `format_options`, which are wrappers around the `XmlHighlightOptions` and `XmlFormatOptions` types respectively. An already created instance is set in each of these properties, and only the members of those instances are meant to be changed — a new instance cannot be assigned.

`highlight_options` controls the fonts (name, size, color, weight, style, and decoration) used to represent XML tags, attribute names, attribute values, inner text, HTML comments, and CDATA sections in the resultant HTML. `format_options` controls how the XML hierarchy is laid out — whether each attribute goes on a new line, whether leaf text nodes go on a new line, and the size of the left indent per nesting level. These properties operate on complex CSS-related value types, so adjust their members carefully. The snippet below is a schematic, non-runnable illustration of accessing these compound properties:

```python
from groupdocs.editor.options import XmlEditOptions

edit_options = XmlEditOptions()

# Access the already-created compound sub-options (do not assign a new instance)
highlight_options = edit_options.highlight_options
format_options = edit_options.format_options

# Adjust members of the compound options using CSS-related value types
# (see the API reference for the exact value types and their constructors)
```

## Getting document metainfo

The article [Extracting document metainfo]({{< ref "editor/python-net/developer-guide/extracting-document-metainfo.md" >}}) describes the [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) method, which allows you to detect the document format and extract its metadata without editing it. The XML format is supported as well.

When [`get_document_info()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/getdocumentinfo) is called for an [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) instance loaded with an XML document, the method returns a metadata view corresponding to the `TextualDocumentInfo` type — a common type for all document formats of a textual nature, like HTML, XML, and TXT. It exposes the `format`, `page_count`, `size`, `is_encrypted`, and `encoding` properties. For XML documents `page_count` always returns 1 and `is_encrypted` always returns `False`.

```python
from groupdocs.editor import Editor

with Editor("./sample.xml") as editor:
    info = editor.get_document_info()
    print("Format:", info.format.name)
    print("Page count:", info.page_count)
    print("Size:", info.size)
    print("Is encrypted:", info.is_encrypted)
```
