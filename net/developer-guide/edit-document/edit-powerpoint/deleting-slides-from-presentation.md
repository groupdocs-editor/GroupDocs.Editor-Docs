---
id: deleting-slides-from-presentation
url: editor/net/deleting-slides-from-presentation
title: Deleting slides from presentation
weight: 3
description: "This article describes the new feature of the GroupDocs.Editor for .NET version 25.11 - deleting (removing) one or many slides from the loaded and edited presentation during its saving to the output format"
keywords: Edit PowerPoint, delete slide from PowerPoint presentation, delete slide from PPT PPTX presentation, delete slide from presentation, remove slide from presentation
productName: GroupDocs.Editor for .NET
hideChildren: False
toc: True
---
There are many [resentation](https://docs.fileformat.com/presentation/) document formats — PPT, PPTX, ODP and many more. They all have one common thing — the *slides*. Each presentation document has one or more slides, and these slides may be treated as containers of different content: text, geometric shapes, raster and vector images, animations, video, audio, comments, embedded objects and many more. The GroupDocs.Editor from its beginning had an ability to edit presentations, but only on a per-slide basis: a user can edit only the content of one slide at a time.

Initially, when the content of the slide was edited, it was only possible to save it as a new one-slide presentation. In particular, the pipeline was the next: user loads a presentation to the [`Editor`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor) class, selects the slide to edit with help of [`PresentationEditOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationeditoptions), obtains [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument), edits the HTML-content of the slide, saves this content back to the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument), and finally generates a new presentation file with one slide using the [`Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method.

After release of the [version 20.4](https://releases.groupdocs.com/editor/net/release-notes/2020/groupdocs-editor-for-net-20-4-release-notes/), when the new SlideNumber
property was introduced, this pipeline was expanded — it became possible to insert an edited slide into the original presentation using two ways:

1. by replacing the original slide, that was initially set for editing, onto its edited version,
2. and by putting the edited slide to “co-exist” together with its original version before edit.

This mechanism is explained in detail in a [separate article](https://docs.groupdocs.com/editor/net/inserting-edited-slide-into-existing-presentation/).

Starting from the [version 25.11](https://releases.groupdocs.com/editor/net/release-notes/2025/groupdocs-editor-for-net-25-11-release-notes/), it is possible not only to edit and replace slides in the resultant presentation, but also delete them from it. The [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions) class now contains one new property — a `int[] SlideNumbersToDelete`, which basically is an array of integers, where each integer represents a number of one slide that should be removed. Slide numbers here are 1-based, so the 1st slide has number `1`, not `0`.

With the new slides removal feature the GroupDocs.Editor performs the next algorithm during saving the presentation:

1. User edits a content of the slide in the WYSIWYG-editor, passes the edited content to the instance of the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) class [using one of its static methods](https://docs.groupdocs.com/editor/net/create-editabledocument-from-file-or-markup/), creates and adjusts the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions), and passes the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance, [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions) instance, and output stream or file path for writing to the [`Editor.Save()`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editor/save) method.
2. If the value of the [`SlideNumber`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property in the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions) instance is set to the default value `0`, the GroupDocs.Editor generates a new presentation with one slide inside it. The `SlideNumbersToDelete` property is ignored regardless of its value.
3. If the value of the [`SlideNumber`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property in the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions) instance has some non-zero value, the GroupDocs.Editor treats it as a command to insert the edited slide into the original presentation.
4. Depending on the [`InsertAsNewSlide`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions/insertasnewslide) property, the GroupDocs.Editor replaces old slide, specified by the value of the [`SlideNumber`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions/slidenumber) property, on the new one, taken from the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance, or puts the new slide to be placed among existing slides without replacing any of them.
5. Finally, when all slide insertions and rearrangements are finished, the GroupDocs.Editor reads the value of the `SlideNumbersToDelete` property, iterates over all numbers and removes specified slides from the presentation.
6. The final presentation, with updated and deleted slides, is written to the output stream or file.

Example below shows full roundtrip of input PPT file: presentation with 21 slides inside is loaded, its second slide is converted to the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument), then HTML-markup is emitted from the [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance, modified, and then edited HTML-markup is converted back to the another [`EditableDocument`](https://reference.groupdocs.com/editor/net/groupdocs.editor/editabledocument) instance. From this point two instances of the [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions) class are created — one is intended for generating a first version of an output presentation with a new slide and without deletion and finally it has 22 slides in total (21 original + 1 edited). The second [`PresentationSaveOptions`](https://reference.groupdocs.com/editor/net/groupdocs.editor.options/presentationsaveoptions) instance has the `SlideNumbersToDelete` property set to remove the first and last slides, so the 2nd version of an output presentation will have 20 slides in total (19 original + 1 edited).

Input PPT as well as two versions of the output PPTX files may be downloaded here:
- [Presentations-Tips.ppt](/editor/net/sample-files/Presentations-Tips.ppt)
- [Output22Slides-without-delete.pptx](/editor/net/sample-files/Output22Slides-without-delete.pptx)
- [Output20Slides-with-delete.pptx](/editor/net/sample-files/Output22Slides-with-delete.pptx)

{{< tabs "DeleteSlidesFromLoadedAndEditedPresentations">}}
{{< tab "C#" >}}
```csharp
using GroupDocs.Editor;
using GroupDocs.Editor.Formats;
using GroupDocs.Editor.Options;
// ...

// Load input spreadsheet to the Editor and specify loading options
using (Editor editor = new Editor("Input3Worksheets.xlsx", new SpreadsheetLoadOptions()))
{
    // Prepare edit options and set 2nd worksheet to edit
    SpreadsheetEditOptions editOptions = new SpreadsheetEditOptions();
    editOptions.WorksheetIndex = 1;//2nd worksheet to edit, here WorksheetIndex is 0-based due to historical reasons

    //generate EditableDocument with original content of 2nd worksheet
    using (EditableDocument worksheet2OpenedForEdit = editor.Edit(editOptions))
    {
        // Get the HTML-markup from the EditableDocument with original content
        string originalHtmlContentOf2ndWorksheet = worksheet2OpenedForEdit.GetEmbeddedHtml();
        
        //emulate HTML content editing in WYSIWYG-editor in browser or somewhere else
        string editedHtmlContentOf2ndWorksheet = originalHtmlContentOf2ndWorksheet.Replace("2nd row", "Edited 2nd row at 1st column");

        //generate EditableDocument with edited content of 2nd worksheet
        using (EditableDocument worksheet2AfterEdit = EditableDocument.FromMarkup(editedHtmlContentOf2ndWorksheet))
        {
            //prepare save options without deletions
            SpreadsheetSaveOptions saveOptionsWithoutDelete = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx);
            // let it be the 2nd worksheet...
            saveOptionsWithoutDelete.WorksheetNumber = 2;//here WorksheetNumber is 1-based
            //... and we also save the original 2nd worksheet, which is pushed to the 3rd position
            saveOptionsWithoutDelete.InsertAsNewWorksheet = true;

            // So now the spreadsheet must have 4 worksheets, not 3. Save it to file
            editor.Save(worksheet2AfterEdit, "Output4Worksheets-without-delete.xlsx", saveOptionsWithoutDelete);

            // Create another save options, with deletions at this time
            SpreadsheetSaveOptions saveOptionsWithDelete = new SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx);
            // let the new worksheet will be 2nd for now, but after deleting it will became the 1st later
            saveOptionsWithDelete.WorksheetNumber = 2;

            //... and we also save the original 2nd worksheet, which will be next sibling to the newly inserted
            saveOptionsWithDelete.InsertAsNewWorksheet = true;

            // delete the 1st and last worksheet
            saveOptionsWithDelete.WorksheetNumbersToDelete = new int[2]
            {
                1,//1st worksheet
                4//last worksheet is 4, not 3, because we inserted the edited 2nd worksheet as the new worksheet instance, without rewriting the original 2nd worksheet
            };
            // Save it again to distinct file. Output XLSX should have 2 worksheets now.
            editor.Save(worksheet2AfterEdit, "Output2Worksheets-with-delete.xlsx", saveOptionsWithDelete);
        }
    }
}
```
{{< /tab >}}
{{< tab "VB.NET">}}
```vb
Imports GroupDocs.Editor
Imports GroupDocs.Editor.Formats
Imports GroupDocs.Editor.Options
' ...

' Load input spreadsheet to the Editor and specify loading options
Using editor As New Editor("Input3Worksheets.xlsx", New SpreadsheetLoadOptions())
    ' Prepare edit options and set 2nd worksheet to edit
    Dim editOptions As New SpreadsheetEditOptions()
    editOptions.WorksheetIndex = 1 '2nd worksheet to edit, here WorksheetIndex is 0-based due to historical reasons

    ' generate EditableDocument with original content of 2nd worksheet
    Using worksheet2OpenedForEdit As EditableDocument = editor.Edit(editOptions)
        ' Get the HTML-markup from the EditableDocument with original content
        Dim originalHtmlContentOf2ndWorksheet As String = worksheet2OpenedForEdit.GetEmbeddedHtml()

        ' emulate HTML content editing in WYSIWYG-editor in browser or somewhere else
        Dim editedHtmlContentOf2ndWorksheet As String = originalHtmlContentOf2ndWorksheet.Replace("2nd row", "Edited 2nd row at 1st column")

        ' generate EditableDocument with edited content of 2nd worksheet
        Using worksheet2AfterEdit As EditableDocument = EditableDocument.FromMarkup(editedHtmlContentOf2ndWorksheet)
            ' prepare save options without deletions
            Dim saveOptionsWithoutDelete As New SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx)

            ' let it be the 2nd worksheet...
            saveOptionsWithoutDelete.WorksheetNumber = 2 'here WorksheetNumber is 1-based
            '... and we also save the original 2nd worksheet, which is pushed to the 3rd position
            saveOptionsWithoutDelete.InsertAsNewWorksheet = True

            ' So now the spreadsheet must have 4 worksheets, not 3. Save it to file
            editor.Save(worksheet2AfterEdit, "Output4Worksheets-without-delete.xlsx", saveOptionsWithoutDelete)

            ' Create another save options, with deletions at this time
            Dim saveOptionsWithDelete As New SpreadsheetSaveOptions(SpreadsheetFormats.Xlsx)
            ' let the new worksheet will be 2nd for now, but after deleting it will became the 1st later
            saveOptionsWithDelete.WorksheetNumber = 2
            '... and we also save the original 2nd worksheet, which will be next sibling to the newly inserted
            saveOptionsWithDelete.InsertAsNewWorksheet = True

            ' delete the 1st and last worksheet
            saveOptionsWithDelete.WorksheetNumbersToDelete = New Int32(1) {
                1, '1st worksheet
                4 'last worksheet is 4, not 3, because we inserted the edited 2nd worksheet as the new worksheet instance, without rewriting the original 2nd worksheet
            }
            ' Save it again to distinct file. Output XLSX should have 2 worksheets now.
            editor.Save(worksheet2AfterEdit, "Output2Worksheets-with-delete.xlsx", saveOptionsWithDelete)
        End Using
    End Using
End Using
```
{{< /tab >}}
{{< /tabs >}}
