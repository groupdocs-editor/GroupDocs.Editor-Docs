---
id: inserting-edited-slide-into-existing-presentation
url: editor/python-net/inserting-edited-slide-into-existing-presentation
title: Inserting edited slide into existing presentation
linkTitle: Inserting edited slide into existing presentation
weight: 2
description: "This article describes how to insert edited presentation slide into existing PowerPoint presentation using GroupDocs.Editor for Python via .NET."
keywords: edit powerpoint, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
By default the full presentation editing pipeline (cycle) is the next:

1. Load presentation in a form of a file or stream into constructor of the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class.
2. Select a specific slide to edit and specify its index in the [slide_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationeditoptions/slidenumber) property of [PresentationEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationeditoptions) class.
3. Open presentation for editing by calling [editor.edit()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/edit) method, passing [PresentationEditOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationeditoptions) instance with selected slide into it, and obtaining [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance from this method.
4. Emitting HTML and CSS markup, which represents a content of an edited document, from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, using different methods like [get_content()](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/getcontent).
5. Passing this HTML and CSS markup into the WYSIWYG editor, which is located on client-side and is running in the browser.
6. End-user edits the document content in the WYSIWYG editor.
7. Edited document content, in form of HTML and CSS markup, is passed back to the server-side.
8. Then this content is passed into the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class, by using the [from_file](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/fromfile), [from_markup](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkup), or [from_markup_and_resource_folder](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument/frommarkupandresourcefolder/) static methods.
9. Created [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, which holds an edited document content, is passed into the [editor.save](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method.
10. [editor.save](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor/save) method generates a new presentation, which contains only one single slide — those slide, that was edited on the client-side and which content was passed via [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) class into this method.

The 10th step of described pipeline can be altered — edited slide can be inserted into original presentation, which was loaded on the 1st step.

[PresentationSaveOptions](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions) class contains two properties: integer [slide_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) and boolean flag [insert_as_new_slide](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/insertasnewslide). Both of them have "usual" default values: [slide_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) has `0` and [insert_as_new_slide](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/insertasnewslide) is set to `False`.

```python
save_options.slide_number = 0
save_options.insert_as_new_slide = False
```

By default, when these properties are not touched or at least [slide_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) has a default value `0`, GroupDocs.Editor will generate new single-slide presentation, as before. However, if [slide_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property contains number, distinct from `0`, and valid presentation is loaded into [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class (it is expected to be the original presentation, which was edited, but it actually can be any presentation, even those, which has no relation to the original), then edited slide will be **inserted** into given presentation.

## slide_number property

[slide_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property, if it is not zero, defines, *where*, at *which exact position* in the given presentation the new edited slide should be inserted. `insert_as_new_slide` parameter, which is a boolean flag, determines, *how* this slide should be inserted: should it *replace* the existing slide, that is located on specified position (`False`, default value), or it should be *injected between* existing slides, without rewriting them, and thus increasing the total amount of slides in the given presentation by one.

Because default `0` value of `slide_number` parameter is reserved, actual slide numbering starts from `1`. This is different from widely used 0-based indexing, however, makes sense, thus it is not an *index* of a slide, but rather *number* of a slide, the same as MS PowerPoint uses, for instance. This means that, for example, for given presentation, that consists of 5 slides, 1st one has a `1` slide number, and 5th — `5`. If user has specified a slide number, which exceeds the total amount of slides in presentation, this number will be automatically adjusted to the latest. This means that if, for example, for the same 5-slide presentation user will specify a `6`, `7` or even a very big value, it will be internally set a `5` — number of the latest slide.

Along with positive slide numbers, the `slide_number` property also supports negative numbering, which implies count from the end of presentation. In this case, `-1` is treated as the last slide, `-2` — the last but one, and so on. Again, like with positive numbering, in case when number exceeds the amount of slides, it will be adjusted to the closest — first slide for negative numbers.

Part of source code below explains this numbering system:

```python
save_options = PresentationSaveOptions(PresentationFormats.PPTX)

# let's say we have a presentation with 5 slides

save_options.slide_number = 0   # default value, given presentation will be ignored

# positive numbering
save_options.slide_number = 1   # first slide
save_options.slide_number = 2   # second slide
save_options.slide_number = 3   # third slide
save_options.slide_number = 4   # fourth slide
save_options.slide_number = 5   # fifth slide
save_options.slide_number = 6   # fifth slide, because value '6' exceeds the slides amount '5' and thus is adjusted to the closest

# negative numbering
save_options.slide_number = -1  # fifth slide, which is first from end (last)
save_options.slide_number = -2  # fourth slide, which is second from end (last but one)
save_options.slide_number = -3  # third slide, which is third from end
save_options.slide_number = -4  # second slide, which is fourth from end
save_options.slide_number = -5  # first slide, which is fifth from end
save_options.slide_number = -6  # first slide, because value '-6' exceeds the slides amount '5' and thus is adjusted to the closest
```

## insert_as_new_slide property

[insert_as_new_slide](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/insertasnewslide) property complements the previously described [slide_number](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property and is ignored, when `slide_number` is set to `0`. If `slide_number` points on the slide of specific number, [insert_as_new_slide](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/insertasnewslide) determines how to treat this slide number and how to insert the slide:

* If [insert_as_new_slide](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/insertasnewslide) property has default `False` value, the existing slide in given presentation will be completely erased, and the content of the new slide (which is located in the [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance) will be putted on this place. As a result, presentation will preserve the same untouched amount of slides, but one of its slides (specified by the `slide_number`) will be replaced onto new one.
* If [insert_as_new_slide](https://reference.groupdocs.com/editor/python-net/groupdocs.editor.options/presentationsaveoptions/insertasnewslide) property has `True` value, the edited slide, obtained from [EditableDocument](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editabledocument) instance, will be injected among existing slides in given presentation, so its amount of slides will be incremented by one. New slide is inserted at position, specified by `slide_number`, and all subsequent slides (following or preceding) will be shifted to the end or to the beginning accordingly, depending on positive or negative numbering.

Source code below shows, how slide number is treated when `insert_as_new_slide` property is enabled:

```python
save_options = PresentationSaveOptions(PresentationFormats.PPTX)

# let's say we have a presentation with 5 slides
save_options.slide_number = 0   # default value, given presentation will be ignored, as well as insert_as_new_slide
save_options.insert_as_new_slide = True  # enabling the adding of slide instead of replacing

# positive numbering
save_options.slide_number = 1   # new slide is injected as first, while all following (including 'old' 1st) are shifting to the end
save_options.slide_number = 2   # new slide is injected as second, while 2nd, 3rd, 4th and 5th are shifting to the end
save_options.slide_number = 3   # new slide is injected as third, while 3rd, 4th and 5th are shifting to the end
save_options.slide_number = 4   # new slide is injected as fourth, while 4th and 5th are shifting to the end
save_options.slide_number = 5   # new slide is injected as fifth, while 5th is shifting to the end and becomes 6th
save_options.slide_number = 6   # new slide is injected as sixth, it already becomes the latest, none of existing slides are shifting to the end
save_options.slide_number = 7   # same as previous

# negative numbering
save_options.slide_number = -1  # new slide is injected as first from end (it becomes sixth if starting from beginning), none of existing slides are shifting to the end
save_options.slide_number = -2  # new slide is injected as second from end (it becomes fifth if starting from beginning), following single slide is shifting to the end
save_options.slide_number = -3  # new slide is injected as third from end (it becomes fourth if starting from beginning), two following slides are shifting to the end
save_options.slide_number = -4  # new slide is injected as fourth from end (it becomes third if starting from beginning), three following slides are shifting to the end
save_options.slide_number = -5  # new slide is injected as fifth from end (it becomes second if starting from beginning), four following slides are shifting to the end
save_options.slide_number = -6  # new slide is injected as sixth from end (it becomes first if starting from beginning), five following slides are shifting to the end
save_options.slide_number = -7  # same as previous
```

The complete example below loads a presentation, edits its first slide, and saves the result by inserting the edited slide into the original presentation as a new slide, keeping the original slides intact.

{{< tabs "code-example-inserting-edited-slide-into-existing-presentation">}}
{{< tab "inserting_edited_slide_into_existing_presentation.py" >}}  
```python
import os
from groupdocs.editor import Editor, EditableDocument, License
from groupdocs.editor.options import PresentationLoadOptions, PresentationEditOptions, PresentationSaveOptions
from groupdocs.editor.formats import PresentationFormats

def inserting_edited_slide_into_existing_presentation():
    # Optionally set a license
    license_path = os.path.abspath("./GroupDocs.Editor.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    # Load input presentation to the Editor and specify loading options
    with Editor("./sample-presentation.pptx", PresentationLoadOptions()) as editor:
        # Prepare edit options and set the 1st slide to edit
        edit_options = PresentationEditOptions()
        edit_options.slide_number = 0  # index is 0-based, so this is the 1st slide

        # Generate EditableDocument with the original content of the slide
        slide_opened_for_edit = editor.edit(edit_options)

        # Get the HTML-markup from the EditableDocument with the original content
        original_html = slide_opened_for_edit.get_embedded_html()

        # Emulate HTML content editing in a WYSIWYG-editor in the browser or somewhere else
        edited_html = original_html.replace("</body>", "<p>Edited content</p></body>")

        # Generate EditableDocument with the edited content
        slide_after_edit = EditableDocument.from_markup(edited_html)

        # Prepare save options that insert the edited slide into the original presentation
        save_options = PresentationSaveOptions(PresentationFormats.PPTX)
        save_options.slide_number = 1  # 1-based; insert at the 1st position
        save_options.insert_as_new_slide = True  # keep the edited slide alongside the original ones

        # Save the presentation with the inserted edited slide
        editor.save(slide_after_edit, "./edited-presentation.pptx", save_options)

        slide_opened_for_edit.dispose()
        slide_after_edit.dispose()

    print("Saved presentation with the inserted edited slide to edited-presentation.pptx")

if __name__ == "__main__":
    inserting_edited_slide_into_existing_presentation()
```
{{< /tab >}}
{{< tab "sample-presentation.pptx" >}}  
{{< tab-text >}}
`sample-presentation.pptx` is the sample file used in this example. Click [here](/editor/python-net/_sample_files/developer-guide/edit-document/edit-powerpoint/inserting-edited-slide-into-existing-presentation/sample-presentation.pptx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "edited-presentation.pptx" >}}  
```text
Binary file (PPTX, 232 KB)
```
[Download full output](/editor/python-net/_output_files/developer-guide/edit-document/edit-powerpoint/inserting-edited-slide-into-existing-presentation/inserting_edited_slide_into_existing_presentation/edited-presentation.pptx)
{{< /tab >}}
{{< /tabs >}}

### Additional notes

It is worth mentioning that the described feature doesn't modify the original presentation document, which was originally loaded into the [Editor](https://reference.groupdocs.com/editor/python-net/groupdocs.editor/editor) class through its constructor. When saving a presentation and inserting the edited slide into existing presentation, the GroupDocs.Editor creates a full and exact copy of the original document, and only then adds or replaces the slide onto the edited. So the original document is not touched in any case.
