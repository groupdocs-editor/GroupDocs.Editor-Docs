---
id: mcp-tool-get-form-fields
url: editor/mcp/tools-reference/get-form-fields
title: get_form_fields
weight: 5
description: "The get_form_fields MCP tool lists the fillable fields of a Word form — text, check box, drop-down, number and date — with each field's name, type and current value."
keywords: get_form_fields MCP tool, list Word form fields AI, docx form field names, read form values agent
productName: GroupDocs.Editor MCP Server
generated: true
serverVersion: 26.10.0
toc: True
---

`get_form_fields` lists the **fillable fields** of a Word form — text boxes, check boxes, drop-downs, number and date fields — with each field's **exact name**, type and current value. It is the call to make before [`fill_form_fields`]({{< ref "editor/mcp/tools-reference/fill-form-fields.md" >}}): field names are rarely what the label on the page says. Example prompt: *"What fields does this form have?"*

**Tool description (as the AI agent sees it):**

> Lists the fillable form fields in a Word document — text boxes, check boxes, drop-downs, number and date fields — with each field's name, type and current value. Call it before fill_form_fields to learn the exact field names. Supports DOCX, DOC, DOCM, DOT, DOTX, RTF and ODT. Do NOT pre-check whether files exist — just pass the filename the user provided; a missing file returns the list of available files. Returns JSON: { fileName, count, hasInvalidFormFields, fields: [ { name, type, value } ] }. type is Text, CheckBox, DropDown, Number, Date, CurrentDate or CurrentTime; a check box value is true/false; a drop-down value is { options[], selected }. hasInvalidFormFields is true when names are duplicated or malformed. On failure isError is set and the text starts with 'Form-field lookup failed for'; a file that cannot be found returns the list of available files instead.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "editor/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password, for password-protected documents |

## Example call

```json
{
  "name": "get_form_fields",
  "arguments": {
    "file": {
      "filePath": "new-employee-form.docx"
    }
  }
}
```

## Result

JSON — captured from server 26.10.0 (three text fields trimmed to one):

```json
{
  "fileName": "new-employee-form.docx",
  "count": 6,
  "hasInvalidFormFields": false,
  "fields": [
    { "name": "FullName", "type": "Text", "value": "" },
    { "name": "Department", "type": "DropDown",
      "value": {
        "options": [ "Engineering", "Finance", "Sales", "Operations" ],
        "selected": "Engineering"
      } },
    { "name": "Agree", "type": "CheckBox", "value": false },
    { "name": "Newsletter", "type": "CheckBox", "value": false }
  ]
}
```

A drop-down reports its `options` — the only values it accepts — and the `selected` one; a check box reports `true` or `false`. `hasInvalidFormFields` flags fields Word itself would not accept (duplicate or empty names). Word formats only: DOCX, DOC, DOCM, DOT, DOTX, RTF, ODT. A document without fields returns `"count": 0`. On failure `isError` is set.

## Example prompts

* *"What fields does new-employee-form.docx have?"*
* *"Which values does the Department drop-down accept?"*
* *"Show me what is already filled in on this form."*

See it used end-to-end: [Fill Word forms from data]({{< ref "editor/mcp/use-cases/fill-word-forms-from-data.md" >}}).
