---
id: styles-export
url: editor/python-net/styles-export
title: Export styles during document editing
linkTitle: Export styles during document editing
weight: 10
description: "This article describes the procedure of preserving and exporting all built-in and custom styles in the source WordProcessing document during its editing."
keywords: Styles export, export styles, save document styles, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
The [family of WordProcessing formats](https://docs.fileformat.com/word-processing/), commonly represented by the [Office Open XML](https://docs.fileformat.com/word-processing/docx/) formats and [Open Document Format (ODT)](https://docs.fileformat.com/word-processing/odt/) formats, have a concept of _styles_. Here a _style_ is a defined set of formatting options, which can be applied to some span of text, paragraphs, lists, or tables. Styles can be built-in (defined by the word processor program, MS Word, for example), or user-defined. Every style has a unique name and can be derived from another style. In MS Word styles are usually located in the "Styles" area in the "Main" tab; users can select a desired text or paragraph of the document and then click on the preferred style in order to apply the style formatting.

Every style has the following characteristics and parameters:

1. It has a unique name.
2. It can be a character style (applied for textual content within a paragraph), a paragraph style (applied to paragraph(s)), a list style (applied for list items), or a table style (applied to the whole table).
3. It can be either built-in in MS Word or user-defined.
   * For built-in styles only: it can be a heading style or not.
4. It can be a Quick Style or not; if yes, this style is shown in the Quick Style gallery inside the MS Word UI.
5. It may be based on some other style or not.

GroupDocs.Editor supports styles on export. This feature is not an option, it is always turned on. GroupDocs.Editor exports the WordProcessing styles by transforming them into CSS rulesets and referencing these rulesets from the HTML markup.

Here is an example of CSS markup, which contains an MS Word built-in style named "Heading 1":

```css
.Heading_1, .Quick-style, .BuiltIn-style, .Heading-style, .Paragraph-style { 
	-aw-style-name: heading1; 
	-gd-style-name: 'Heading 1'; 
	font-family: Cambria; 
	font-size: 14pt; 
	font-weight: bold; 
	font-style: normal; 
	color: rgb(54, 95, 145); 
	text-align: left; 
	margin-top: 24pt; 
	margin-bottom: 0pt; 
	line-height: 134.16667%; 
}
```

The original style name is preserved in the custom property "-gd-style-name". Because it is a built-in style, it also has a style identifier in the custom property "-aw-style-name". The class selectors inside the grouped selector of this ruleset also store the style's parameters:

- The first class selector is a unique style name, adjusted to meet the "CSS identifier" requirements.
- The second class selector shows whether it is a quick style.
- The third class selector shows whether it is a built-in style or a user style.
- The fourth class selector shows whether it is a built-in heading style.
- The fifth class selector defines the style type: Character, Paragraph, List, or Table.

Here is an example of CSS markup, which contains a custom user-defined character style named "Подзаголовок Знак":

```css
.Подзаголовок_Знак, .User-style, .Character-style { 
	-gd-style-name: 'Подзаголовок Знак'; 
	font-family: Cambria; 
	font-size: 12pt; 
	font-weight: normal; 
	font-style: italic; 
	color: rgb(79, 129, 189); 
	letter-spacing: 0.75pt; 
}
```

Despite the fact that the HTML markup has been modified slightly, and the CSS markup has been modified drastically, the feature was developed in such a way to not disturb or distort the representation of the document in the browser. So when opening and comparing in the web-browser two edited documents side-by-side, there will be no visual difference.

## Complete example

The CSS markup, which contains all the exported WordProcessing styles, can be obtained from the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance using the [`get_css_content()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/) method. It returns a list of stylesheet strings. The example below loads the sample document, edits it, and prints the number of exported stylesheets and their total size.

{{< tabs "code-example-styles-export">}}
{{< tab "export_styles.py" >}}  
```python
import os
from groupdocs.editor import Editor, License

def export_styles():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Editor("./sample-document.docx") as editor:
        # Open the document for editing
        editable = editor.edit()

        # Obtain the CSS content with all exported WordProcessing styles
        css = editable.get_css_content()  # list of stylesheet strings
        print("Number of exported stylesheets:", len(css))
        print("Total CSS length, characters:", sum(len(sheet) for sheet in css))

        editable.dispose()

if __name__ == "__main__":
    export_styles()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-word/styles-export/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "export-styles.txt" >}}  
```text
Number of exported stylesheets: 1
Total CSS length, characters: 34134
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-word/styles-export/export_styles/export-styles.txt)
{{< /tab >}}
{{< /tabs >}}
