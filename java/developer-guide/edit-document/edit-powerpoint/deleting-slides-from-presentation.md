---
id: deleting-slides-from-presentation
url: editor/java/deleting-slides-from-presentation
title: Deleting slides from presentation
weight: 3
description: "This article describes the new feature of the GroupDocs.Editor for Java version 26.1 - deleting (removing) one or many slides from the loaded and edited presentation during its saving to the output format"
keywords: Edit PowerPoint, delete slide from PowerPoint presentation, delete slide from PPT PPTX presentation, delete slide from presentation, remove slide from presentation
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---
There are many [presentation](https://docs.fileformat.com/presentation/) document formats — [PPT](https://docs.fileformat.com/presentation/ppt/), [PPTX](https://docs.fileformat.com/presentation/pptx/), [ODP](https://docs.fileformat.com/presentation/odp/) and many more. They all have one common thing — the *slides*. Each presentation document has one or more slides, and these slides may be treated as containers of different content: text, geometric shapes, raster and vector images, animations, video, audio, comments, embedded objects and many more. The GroupDocs.Editor from its beginning had an ability to edit presentations, but only on a per-slide basis: a user can edit only the content of one slide at a time.

Initially, when the content of the slide was edited, it was only possible to save it as a new one-slide presentation. In particular, the pipeline was the next: user loads a presentation to the [`Editor`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/) class, selects the slide to edit with help of [`PresentationEditOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationeditoptions/), obtains [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/), edits the HTML-content of the slide, saves this content back to the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/), and finally generates a new presentation file with one slide using the [`Editor.Save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#save-java.io.OutputStream-) method.

After release of the [version 20.4](https://releases.groupdocs.com/editor/net/release-notes/2020/groupdocs-editor-for-net-20-4-release-notes/), when the new SlideNumber
property was introduced, this pipeline was expanded — it became possible to insert an edited slide into the original presentation using two ways:

1. by replacing the original slide, that was initially set for editing, onto its edited version,
2. and by putting the edited slide to “co-exist” together with its original version before edit.

This mechanism is explained in detail in a [separate article](https://docs.groupdocs.com/editor/java/inserting-edited-slide-into-existing-presentation/).

Starting from the [version 26.1](https://releases.groupdocs.com/editor/java/release-notes/2026/groupdocs-editor-for-java-26-1-release-notes/), it is possible not only to edit and replace slides in the resultant presentation, but also delete them from it. The [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/) class now contains one new property — a `int[]` [`SlideNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/#setSlideNumbersToDelete-int---), which basically is an array of integers, where each integer represents a number of one slide that should be removed. Slide numbers here are 1-based, so the 1st slide has number `1`, not `0`.

With the new slides removal feature the GroupDocs.Editor performs the next algorithm during saving the presentation:

1. User edits a content of the slide in the WYSIWYG-editor, passes the edited content to the instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) class [using one of its static methods](https://docs.groupdocs.com/editor/java/create-editabledocument-from-file-or-markup/), creates and adjusts the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/), and passes the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instance, [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/) instance, and output stream or file path for writing to the [`Editor.save()`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editor/#save-java.io.OutputStream-) method.
2. If the value of the [`SlideNumber`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/#setSlideNumber-int-) property in the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/) instance is set to the default value `0`, the GroupDocs.Editor generates a new presentation with one slide inside it. The [`SlideNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/#setSlideNumbersToDelete-int---) property is ignored regardless of its value.
3. If the value of the [`SlideNumber`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/#setSlideNumber-int-) property in the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/) instance has some non-zero value, the GroupDocs.Editor treats it as a command to insert the edited slide into the original presentation.
4. Depending on the [`InsertAsNewSlide`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/#setInsertAsNewSlide-boolean-) property, the GroupDocs.Editor replaces old slide, specified by the value of the [`SlideNumber`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/#setSlideNumber-int-) property, on the new one, taken from the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instance, or puts the new slide to be placed among existing slides without replacing any of them.
5. Finally, when all slide insertions and rearrangements are finished, the GroupDocs.Editor reads the value of the [`SlideNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/#setSlideNumbersToDelete-int---) property, iterates over all numbers and removes specified slides from the presentation.
6. The final presentation, with updated and deleted slides, is written to the output stream or file.

Example below shows full roundtrip of input PPT file: presentation with 21 slides inside is loaded, its second slide is converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/), then HTML-markup is emitted from the [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument/) instance, modified, and then edited HTML-markup is converted back to the another [`EditableDocument`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor/editabledocument) instance. From this point two instances of the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/) class are created — one is intended for generating a first version of an output presentation with a new slide and without deletion and finally it has 22 slides in total (21 original + 1 edited). The second [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/) instance has the [`SlideNumbersToDelete`](https://reference.groupdocs.com/editor/java/com.groupdocs.editor.options/presentationsaveoptions/#setSlideNumbersToDelete-int---) property set to remove the first and last slides, so the 2nd version of an output presentation will have 20 slides in total (19 original + 1 edited).

{{< tabs "code-example">}}
{{< tab "DeleteSlidesFromLoadedAndEditedPresentations.java" >}}  
```java
import com.groupdocs.editor.EditableDocument;
import com.groupdocs.editor.Editor;
import com.groupdocs.editor.examples.Constants;
import com.groupdocs.editor.formats.PresentationFormats;
import com.groupdocs.editor.options.*;

public class DeleteSlidesFromLoadedAndEditedPresentations {
    public static void edit() {
		// Load input presentation to the Editor and specify loading options
        Editor editor = new Editor("Presentations-Tips.ppt", new PresentationLoadOptions());
        try {
            // Prepare edit options and set 2nd slide to edit
            PresentationEditOptions editOptions = new PresentationEditOptions();
            editOptions.setShowHiddenSlides(true);
            editOptions.setSlideNumber(1); // 2nd slide to edit, here SlideNumber is 0-based (historical reasons)

            // generate EditableDocument with original content of 2nd slide
            EditableDocument slide2OpenedForEdit = editor.edit(editOptions);
            try {
                // Get the HTML-markup from the EditableDocument with original content
                String originalHtmlContentOf2ndSlide = slide2OpenedForEdit.getEmbeddedHtml();

                // emulate HTML content editing in WYSIWYG-editor in browser or somewhere else
                String editedHtmlContentOf2ndSlide =
                        originalHtmlContentOf2ndSlide.replace("Tips to be Covered", "Edited tips on 2nd slide");

                // generate EditableDocument with edited content of 2nd slide
                EditableDocument slide2AfterEdit = EditableDocument.fromMarkup(editedHtmlContentOf2ndSlide, null);
                try {
                    // prepare save options without deletions
                    PresentationSaveOptions saveOptionsWithoutDelete = new PresentationSaveOptions(PresentationFormats.Pptx);
                    // let it be the 2nd slide...
                    saveOptionsWithoutDelete.setSlideNumber(2); // here SlideNumber is 1-based
                    // ... and we also save the original 2nd slide, which is pushed to the 3rd position
                    saveOptionsWithoutDelete.setInsertAsNewSlide(true);

                    // So now the presentation must have 22 slides, not 21. Save it to file
                    String outputSlides_without_delete = Constants.getOutputDirectoryPath("Output22Slides-without-delete.pptx");
                    editor.save(slide2AfterEdit, outputSlides_without_delete, saveOptionsWithoutDelete);

                    // Create another save options, with deletions at this time
                    PresentationSaveOptions saveOptionsWithDelete = new PresentationSaveOptions(PresentationFormats.Pptx);
                    // let the new slide be 2nd for now, but after deleting it will become the 1st later
                    saveOptionsWithDelete.setSlideNumber(2);

                    // ... and we also save the original 2nd slide, which will be next sibling to the newly inserted
                    saveOptionsWithDelete.setInsertAsNewSlide(true);

                    // delete the 1st and last slide
                    saveOptionsWithDelete.setSlideNumbersToDelete(new int[]{
                            1,  // 1st slide
                            22  // last slide is 22 (because we inserted a new slide without rewriting the original 2nd slide)
                    });

                    // Save it again to distinct file. Output PPTX should have 20 slides now.
                    String outputSlides_with_delete = Constants.getOutputDirectoryPath("Output20Slides-with-delete.pptx");
                    editor.save(slide2AfterEdit, outputSlides_with_delete, saveOptionsWithDelete);
                } finally {
                    slide2AfterEdit.dispose();
                }
            } finally {
                slide2OpenedForEdit.dispose();
            }
        } finally {
            editor.dispose();
        }
    }

    public static void main(String[] args){
        edit();
    }
}
```
{{< /tab >}}
{{< tab "Presentations-Tips.ppt" >}}  
{{< tab-text >}}
`Presentations-Tips.ppt` is sample file used in this example. Click [here](/editor/java/sample-files/Presentations-Tips.ppt) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Output22Slides-without-delete.pptx" >}}  
{{< tab-text >}}
`Output20Slides-with-delete.pptx` is edited document with deleted slides. Click [here](/editor/java/sample-files/Output20Slides-with-delete.pptx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "Output22Slides-without-delete.pptx" >}}  
{{< tab-text >}}
`Output22Slides-without-delete.pptx` is edited document without deleted slides. Click [here](/editor/java/sample-files/Output22Slides-without-delete.pptx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< /tabs >}}
