---
id: mcp-tool-fill-form-fields
url: editor/mcp/tools-reference/fill-form-fields
title: fill_form_fields
weight: 6
description: "The fill_form_fields MCP tool fills the fields of a Word form by name — text, check boxes, drop-downs — saves a filled copy, and reports names that do not exist."
keywords: fill_form_fields MCP tool, fill Word form with AI agent, complete docx form automatically, tick check box Word AI
productName: GroupDocs.Editor MCP Server
generated: true
serverVersion: 26.10.0
toc: True
---

`fill_form_fields` fills a Word form **by field name** and saves a filled copy, leaving the rest of the document untouched. `values` maps names to values — text, `"true"`/`"false"` for a check box, one of the options for a drop-down. Names it does not know are **reported, not guessed**. Example prompt: *"Fill the form for Amara Okafor, Finance, starting 1 December."*

**Tool description (as the AI agent sees it):**

> Fills in the form fields of a Word document and saves a filled copy, leaving the rest of the document untouched. Use it when the user asks to complete, fill out or tick boxes in a Word form. Call get_form_fields first for the exact field names. Supports DOCX, DOC, DOCM, DOT, DOTX, RTF and ODT. Do NOT pre-check whether files exist — just pass the filename the user provided; a missing file returns the list of available files. values maps field name to text: any text for text fields, true/false for check boxes, an option (or its 0-based index) for drop-downs, a number for number fields, yyyy-MM-dd for date fields. Every field with that name is set. Returns 'Updated N field(s)', any names that were not found or values that did not fit, and the saved path ('Saved to: …') of '<name>_edited.<ext>'. When no field could be updated no file is written. On failure isError is set and the text starts with 'Form-field update failed for'; a file that cannot be found returns the list of available files instead.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `values` | object | yes | Field name -> value, e.g. { "Text1": "Jane Doe", "Check1": "true" } — [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `outputName` | string | no | Output file name (default '<name>_edited.<ext>') |
| `format` | string | no | Output format: docx (default: same as input), doc, docm, rtf, odt, … |
| `password` | string | no | Password, for password-protected documents. The filled copy keeps the same password |

## Example call

```json
{
  "name": "fill_form_fields",
  "arguments": {
    "file": {
      "filePath": "new-employee-form.docx"
    },
    "values": {
      "FullName": "Amara Okafor",
      "Department": "Finance",
      "Agree": "true",
      "Phone": "+31 10 555 0100"
    }
  }
}
```

## Result

Plain text — captured from server 26.10.0 (evaluation line trimmed; the call also set `Email` and `StartDate`):

```text
Updated 5 field(s) in 'new-employee-form.docx': FullName, Email, StartDate,
Department, Agree.
Not found: Phone. Call get_form_fields for the exact names.
Saved to: …/new-employee-form_edited.docx
```

(Wrapped for width.) The form has no `Phone` field, so the server filled the five it knows and named the one it did not — the agent can tell you, rather than claim the form is complete. Reading the copy back with [`get_form_fields`]({{< ref "editor/mcp/tools-reference/get-form-fields.md" >}}) shows `"selected": "Finance"` and `"Agree": true`.

When no name matches, nothing is written:

```text
Updated 0 field(s) in 'new-employee-form.docx'.
Not found: FullName, Email, StartDate, Department, Agree, Phone. Call
get_form_fields for the exact names.
No file was written.
```

`outputName` names the copy — one per applicant, for example; `format` converts it (`pdf` for a copy to send). A protected form keeps its password. On failure `isError` is set.

## Example prompts

* *"Fill the new-employee form for Amara Okafor, Finance, starting 1 December, and tick the code of conduct."*
* *"Fill one copy of this form per row of applicants.csv, named after each applicant."*
* *"Tick the newsletter box and save it as a PDF."*

See it used end-to-end: [Fill Word forms from data]({{< ref "editor/mcp/use-cases/fill-word-forms-from-data.md" >}}).
