---
id: how-to-run-examples
url: editor/python-net/how-to-run-examples
title: How to Run Examples
linkTitle: How to Run Examples
weight: 6
description: "We offer multiple solutions on how you can run GroupDocs.Editor examples, by building your own or cloning our ready-to-run Python examples repository."
keywords: GroupDocs.Editor examples, python examples, run examples, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
{{< alert style="warning" >}}Before running an example make sure that GroupDocs.Editor for Python via .NET has been installed successfully.{{< /alert >}}

We offer multiple solutions for how you can run GroupDocs.Editor examples, by building your own or by cloning our ready-to-run Python examples repository.

Please choose one from the following list:

## Build a project from scratch

* Create and activate a virtual environment, then install **GroupDocs.Editor for Python via .NET** following this [guide]({{< ref "editor/python-net/getting-started/installation.md" >}}).

{{< tabs "create_venv">}}
{{< tab "Windows" >}}
```ps
py -m venv .venv
.venv\Scripts\activate
py -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< /tabs >}}

* Code your first application with **GroupDocs.Editor for Python via .NET** like this:

{{< tabs "code-example-run_first_example">}}
{{< tab "run_first_example.py" >}}  
```python
import os
from groupdocs.editor import Editor, License
from groupdocs.editor.formats import WordProcessingFormats
from groupdocs.editor.options import WordProcessingEditOptions, WordProcessingSaveOptions

def run_first_example():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load the document
    with Editor("./sample-document.docx") as editor:
        # Obtain an editable document from the original DOCX document
        edit_options = WordProcessingEditOptions()
        editable_document = editor.edit(edit_options)

        # Pass the editable document to a WYSIWYG editor and edit there...
        # ...

        # Save the edited document back to a WordProcessing format - DOCX, for example
        save_options = WordProcessingSaveOptions(WordProcessingFormats.DOCX)
        editor.save(editable_document, "./edited-document.docx", save_options)

if __name__ == "__main__":
    run_first_example()
```
{{< /tab >}}
{{< tab "sample-document.docx" >}}  
{{< tab-text >}}
`sample-document.docx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/getting-started/how-to-run-examples/sample-document.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-document.docx" >}}  
```text
Binary file (DOCX, 49 KB)
```
[Download full output](/editor/python-net/_output_files/getting-started/how-to-run-examples/run_first_example/edited-document.docx)
{{< /tab >}}
{{< /tabs >}}

* Run your script. The edited document will appear in your working directory.

{{< tabs "run_first_example_run">}}
{{< tab "Windows" >}}
```ps
py run_first_example.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 run_first_example.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 run_first_example.py
```
{{< /tab >}}
{{< /tabs >}}

## Run the examples from the repository

The complete examples package of **GroupDocs.Editor for Python via .NET** is hosted on [GitHub](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Python-via-.NET). You can either download the ZIP file or clone the repository using your favourite git client.

Clone the repository:

```bash
git clone https://github.com/groupdocs-editor/GroupDocs.Editor-for-Python-via-.NET
```

Create and activate a virtual environment, then install the dependencies listed in `requirements.txt`:

{{< tabs "install_requirements">}}
{{< tab "Windows" >}}
```ps
cd GroupDocs.Editor-for-Python-via-.NET
py -m venv .venv
.venv\Scripts\activate
py -m pip install -r requirements.txt
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
cd GroupDocs.Editor-for-Python-via-.NET
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
cd GroupDocs.Editor-for-Python-via-.NET
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
```
{{< /tab >}}
{{< /tabs >}}

Run all the examples at once:

```bash
python run_all_examples.py
```

Or run an individual example by passing its file name:

```bash
python <example>.py
```

The repository contains all the sample documents and resources used in the examples, so the scripts run out of the box.

## Contribute

If you would like to add or improve an example, we encourage you to contribute to the project. All examples in this repository are open source and can be freely used in your own applications.
To contribute, you can fork the repository, edit the code, and create a pull request. We will review the changes and include them in the repository if found helpful.
