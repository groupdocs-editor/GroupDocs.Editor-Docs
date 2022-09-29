---
id: groupdocs-editor-for-java-22-4-release-notes
url: editor/java/groupdocs-editor-for-java-22-4-release-notes
title: GroupDocs.Editor for Java 22.4 Release Notes
weight: 70
description: ""
keywords: 
productName: GroupDocs.Editor for Java
hideChildren: False
---

{{< alert style="info" >}}## NOTE: Java 7 will not be supported since the next release of GroupDocs.Editor for Java. Supported versions are Java 8 and above.{{< /alert >}}

{{< alert style="info" >}}This page contains release notes for GroupDocs.Editor for Java 22.4{{< /alert >}}

GroupDocs.Editor for Java version 22.4 presents several small but useful new features, several improvements and a lot of different bugfixes; all of these are described below. As in most of previous releases, these new features, which touch a public API, do not break a backward compatibility.

## New features

Version 22.4 of GroupDocs.Editor contains three new features, which are described below

#### New Mime property in IDocumentFormat inheritors

Now a public interface [Formats.IDocumentFormat](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/IDocumentFormat) contains a new public property: `String getMime()`.

All implementing types — [WordProcessingFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/WordProcessingFormats), [SpreadsheetFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/SpreadsheetFormats), [PresentationFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/PresentationFormats), [TextualFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/TextualFormats), and [EBookFormats](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.formats/EBookFormats), — also contain this property. With this property users can easily obtain a MIME-code for a particular format. This may be very useful in web-applications, where it is required to write a MIME-code to the response along with document content.

#### New XmlContent property in SvgImage class

SVG image is in fact a text document in XML format. Before this moment it was not possible to obtain this XML content, because a [getTextContent()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/SvgImage#getTextContent()) property of [com.groupdocs.editor.htmlcss.resources.images.vector.SvgImage](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/SvgImage) class returns a base64-encoded content. Now we introduced a new property [getXmlContent()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/SvgImage#getXmlContent()), which returns exactly a described XML content of a particular SVG image.

#### Extended conversion methods for all vector images

Starting from version 22.4 the [VectorImageResourceBase](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/VectorImageResourceBase) class has a new abstract method `void saveToPng(InputStream outputPngContent)`, so now all implementing types ([SvgImage](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/SvgImage), [WmfImage](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/WmfImage) and [EmfImage](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/EmfImage)) support conversion and saving to the PNG raster format.

Also [MetaImageBase](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.htmlcss.resources.images.vector/MetaImageBase) abstract class has a new abstract method `void saveToSvg(InputStream outputSvgContent`), which allows to save WMF or EMF to SVG. This is very useful, because WMF and EMF formats are commonly used in WordProcessing documents, but are not supported by the browsers; on the other hand, only SVG is supported in the Web.

## Improvements

GroupDocs.Editor for Java version 22.4 contains several important improvements, which may be divided onto two parts: improvements related to SVG and to the Presentation module.

#### Presentation module

Now Presentation module preserves and supports some basic text formatting features when converrting document to and from HTML. This includes:
* Bold text
* Italic text
* Underline text
* Subscript
* Superscript
* Strikethrough text

Now, if some slide contains such text formatting, this formatting will be present in HTML and then in the resultant Presentation document.

#### Better SVG support

First of all, starting from version 22.4, HTML/CSS module supports an SVG content, inlined inside HTML markup. This means that if given HTML document contains SVG images, which are inlined directly inside HTML using SVG elements, GroupDocs.Editor will correctly process such markup with SVG images without throwing an exception.

Another improvement is related to the serialization: now, when [EditableDocument.getEmbeddedHtml()](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument#getEmbeddedHtml()) method is invoked, all SVG files will not be encoded to the base64 and placed inside the values of "src" attributes of corresponding IMG elements, but rather will be stored in their native XML-compliant form, with SVG elements and so on.

## Bugs

GroupDocs.Editor version 22.4 contains big amount of bugfixes and performance improvements, which address different issues in different modules of GroupDocs.Editor, including WordProcessing, Spreadsheet, Presentation, and HTML processing.

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| EDITORNET-2092 | Add new property with MIME code for all supportable document formats | New feature |
| EDITORNET-2106 | Add ability to save SVG as PNG and improve public API in vector images | New feature |
| EDITORNET-2129 | Add new public property to obtain XML-content of SVG image | New feature |
| EDITORNET-2093 | Preserve most common text formatting features during roundtrip in Presentation module | Improvement |
| EDITORNET-2103 | SVG is not supported if is inlined into HTML markup as HTML element | Improvement |
| EDITORNET-2128 | Implement SVG inlining instead of base64 embedding | Improvement |
| EDITORNET-1989 | Images are not correctly transfered during conversion Markdown to Docx | Bug |
| EDITORNET-2066 | HTML-markup loose styles after file open and saving without changes | Bug |
| EDITORNET-2086 | OutOfMemoryException while converting specific Cells file to HTML | Bug |
| EDITORNET-2089 | Presentation: convert html to presentation without stylesheet | Bug |
| EDITORNET-2090 | Exception while import Presentation with missing load options | Bug |
| EDITORNET-2094 | Exception with message Font name cannot be NULL, empty or whitespaces\r\nParameter name: fontName during call editor.Edit() for RTF-file	 | Bug |
| EDITORNET-2096 | Unknown field is improperly processed | Bug |
| EDITORNET-2100 | Editor hang on PowerPoint files | Bug |
| EDITORNET-2101 | Image is not JPEG exception in Presentation | Bug |
| EDITORNET-2127 | Fix bug in markup parser | Bug |

## Public API and Backward Incompatible Changes

New public methods:

1. void VectorImageResourceBase.saveToPng(InputStream outputPngContent)
2. void SvgImage.saveToPng(InputStream outputPngContent)
3. void WmfImage.saveToPng(InputStream outputPngContent)
4. void EmfImage.saveToPng(InputStream outputPngContent)
5. void MetaImageBase.saveToSvg(InputStream outputSvgContent)
6. void WmfImage.saveToSvg(InputStream outputSvgContent)
7. void EmfImage.saveToSvg(InputStream outputSvgContent)

New public properties:

1. String com.groupdocs.editor.formats.IDocumentFormat.getMime()
2. String com.groupdocs.editor.formats.WordProcessingFormats.getMime()
3. String com.groupdocs.editor.formats.SpreadsheetFormats.getMime()
4. String com.groupdocs.editor.formats.PresentationFormats.getMime()
5. String com.groupdocs.editor.formats.TextualFormats.getMime()
6. String com.groupdocs.editor.formats.EBookFormats.getMime()
7. String com.groupdocs.editor.htmlcss.resources.images.vector.SvgImage.getXmlContent()
