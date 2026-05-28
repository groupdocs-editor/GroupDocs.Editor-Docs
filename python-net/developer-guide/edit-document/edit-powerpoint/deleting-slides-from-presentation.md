---
id: deleting-slides-from-presentation
url: editor/python-net/deleting-slides-from-presentation
title: Deleting slides from presentation
linkTitle: Deleting slides from presentation
weight: 3
description: "This article describes the feature of the GroupDocs.Editor for Python via .NET - deleting (removing) one or many slides from the loaded and edited presentation during its saving to the output format"
keywords: Edit PowerPoint, delete slide from PowerPoint presentation, delete slide from PPT PPTX presentation, delete slide from presentation, remove slide from presentation, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
There are many [presentation](https://docs.fileformat.com/presentation/) document formats — [PPT](https://docs.fileformat.com/presentation/ppt/), [PPTX](https://docs.fileformat.com/presentation/pptx/), [ODP](https://docs.fileformat.com/presentation/odp/) and many more. They all have one common thing — the *slides*. Each presentation document has one or more slides, and these slides may be treated as containers of different content: text, geometric shapes, raster and vector images, animations, video, audio, comments, embedded objects and many more. The GroupDocs.Editor from its beginning had an ability to edit presentations, but only on a per-slide basis: a user can edit only the content of one slide at a time.

Initially, when the content of the slide was edited, it was only possible to save it as a new one-slide presentation. In particular, the pipeline was the next: user loads a presentation to the [`Editor`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class, selects the slide to edit with help of [`PresentationEditOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationeditoptions), obtains [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), edits the HTML-content of the slide, saves this content back to the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), and finally generates a new presentation file with one slide using the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.

Later this pipeline was expanded — it became possible to insert an edited slide into the original presentation using two ways:

1. by replacing the original slide, that was initially set for editing, onto its edited version,
2. and by putting the edited slide to "co-exist" together with its original version before edit.

This mechanism is explained in detail in a [separate article](https://docs.groupdocs.com/editor/python-net/inserting-edited-slide-into-existing-presentation/).

It is possible not only to edit and replace slides in the resultant presentation, but also delete them from it. The [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions) class contains a [`slide_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumberstodelete/) property, which basically is a list of integers, where each integer represents a number of one slide that should be removed. Slide numbers here are 1-based, so the 1st slide has number `1`, not `0`.

With the slides removal feature the GroupDocs.Editor performs the next algorithm during saving the presentation:

1. User edits a content of the slide in the WYSIWYG-editor, passes the edited content to the instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class [using one of its static methods](https://docs.groupdocs.com/editor/python-net/create-editabledocument-from-file-or-markup/), creates and adjusts the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions), and passes the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions) instance, and output stream or file path for writing to the [`editor.save()`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.
2. If the value of the [`slide_number`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property in the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions) instance is set to the default value `0`, the GroupDocs.Editor generates a new presentation with one slide inside it. The [`slide_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumberstodelete/) property is ignored regardless of its value.
3. If the value of the [`slide_number`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property in the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions) instance has some non-zero value, the GroupDocs.Editor treats it as a command to insert the edited slide into the original presentation.
4. Depending on the [`insert_as_new_slide`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/insertasnewslide) property, the GroupDocs.Editor replaces old slide, specified by the value of the [`slide_number`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property, on the new one, taken from the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, or puts the new slide to be placed among existing slides without replacing any of them.
5. Finally, when all slide insertions and rearrangements are finished, the GroupDocs.Editor reads the value of the [`slide_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumberstodelete/) property, iterates over all numbers and removes specified slides from the presentation.
6. The final presentation, with updated and deleted slides, is written to the output stream or file.

The example below shows a full roundtrip of an input PPTX file: a presentation is loaded, its first slide is converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument), then HTML-markup is emitted from the [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, modified, and the edited HTML-markup is converted back to another [`EditableDocument`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance. This edited slide is then inserted into the original presentation, while the [`slide_numbers_to_delete`](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumberstodelete/) property is set to remove the original second slide during saving.

{{< tabs "code-example-deleting-slides-from-presentation">}}
{{< tab "deleting_slides_from_presentation.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.options import PresentationLoadOptions, PresentationEditOptions, PresentationSaveOptions
from groupdocs.editor.formats import PresentationFormats

def deleting_slides_from_presentation():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load input presentation to the Editor and specify loading options
    with Editor("./sample-presentation.pptx", PresentationLoadOptions()) as editor:
        # Prepare edit options and set the 1st slide to edit
        edit_options = PresentationEditOptions()
        edit_options.show_hidden_slides = True
        edit_options.slide_number = 0  # index is 0-based, so this is the 1st slide

        # Generate EditableDocument with the original content of the slide
        slide_opened_for_edit = editor.edit(edit_options)

        # Get the HTML-markup from the EditableDocument with the original content
        original_html = slide_opened_for_edit.get_embedded_html()

        # Emulate HTML content editing in a WYSIWYG-editor in the browser or somewhere else
        edited_html = original_html.replace("</body>", "<p>Edited content</p></body>")

        # Generate EditableDocument with the edited content
        slide_after_edit = EditableDocument.from_markup(edited_html)

        # Prepare save options that delete a slide during saving
        save_options = PresentationSaveOptions(PresentationFormats.PPTX)
        # A non-zero slide_number is required so the presentation is rebuilt and deletions are applied
        save_options.slide_number = 1  # 1-based; insert the edited slide at the 1st position
        save_options.insert_as_new_slide = True  # keep the edited slide alongside the original ones
        save_options.slide_numbers_to_delete = [1]  # delete the 1st original slide (1-based)

        # Save the presentation with the deleted slide
        editor.save(slide_after_edit, "./edited-presentation.pptx", save_options)

        slide_opened_for_edit.dispose()
        slide_after_edit.dispose()

    print("Saved presentation with a deleted slide to edited-presentation.pptx")

if __name__ == "__main__":
    deleting_slides_from_presentation()
```
{{< /tab >}}
{{< tab "sample-presentation.pptx" >}}  
{{< tab-text >}}
`sample-presentation.pptx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-powerpoint/deleting-slides-from-presentation/sample-presentation.pptx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-presentation.pptx" >}}  
```text
Binary file (PPTX, 144 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-powerpoint/deleting-slides-from-presentation/deleting_slides_from_presentation/edited-presentation.pptx)
{{< /tab >}}
{{< /tabs >}}
