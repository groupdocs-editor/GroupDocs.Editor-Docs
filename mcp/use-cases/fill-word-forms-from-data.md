---
id: mcp-uc-fill-word-forms-from-data
url: editor/mcp/use-cases/fill-word-forms-from-data
title: How to fill Word form fields from data with an AI agent
linkTitle: Fill Word forms
weight: 3
description: "Fill the text boxes, check boxes and drop-downs of a Word form with an AI agent over MCP, locally: read the exact field names, fill them by name, and get the names that do not exist reported."
keywords: fill Word form with AI, complete docx form automatically, Word form fields AI agent, fill form from spreadsheet, MCP form filling
productName: GroupDocs.Editor MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to fill Word form fields from data with an AI agent"
        description: "Fill the text boxes, check boxes and drop-downs of a Word form with an AI agent over MCP, locally: read the exact field names, fill them by name, and get the names that do not exist reported."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Editor MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Fill new-employee-form.docx for Amara Okafor, amara.okafor@northwind.example, Finance, starting 1 December 2026; she agrees to the code of conduct. Her phone is +31 10 555 0100."
---

A Word form has named fields — text boxes, check boxes, drop-downs — and an agent that fills it by name fills it correctly. Run the GroupDocs.Editor MCP server locally: the agent reads the field names with `get_form_fields`, fills them with `fill_form_fields`, and the server reports any value it had no field for:

```bash
dnx GroupDocs.Editor.Mcp --yes
```

with `GROUPDOCS_MCP_STORAGE_PATH` pointing at your documents folder — or the Docker image, `ghcr.io/groupdocs-editor/editor-net-mcp`. Per-client config: [Register in AI clients]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}}).

> Fill new-employee-form.docx for Amara Okafor, amara.okafor@northwind.example, Finance, starting 1 December 2026; she agrees to the code of conduct. Her phone is +31 10 555 0100.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "editor/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## Prerequisites

* A Word form with legacy or content form fields (DOCX, DOC, DOCM, DOT, DOTX, RTF, ODT) in the storage folder.
* The server registered in your AI client — [quick start]({{< ref "editor/mcp/getting-started/_index.md" >}}).

## The calls

```text
get_form_fields {"file":{"filePath":"new-employee-form.docx"}}
  -> {"count":6,"fields":[{"name":"FullName","type":"Text"},
       {"name":"Department","type":"DropDown","value":{"options":[...]}},
       {"name":"Agree","type":"CheckBox","value":false}, ...]}
fill_form_fields {"values":{"FullName":"Amara Okafor",
                            "Department":"Finance","Agree":"true",
                            "Phone":"+31 10 555 0100"}}
  -> "Updated 5 field(s) ... Not found: Phone.
      Saved to: .../new-employee-form_edited.docx"
```

## What the agent sees

Captured from server 26.10.0 against a synthetic six-field form. The server prints the full path inside your storage folder; it is shortened to `…/` here, and the evaluation line every response starts with is trimmed unless shown.

```text
-> get_form_fields {"file":{"filePath":"new-employee-form.docx"}}
<- { "count": 6, "fields": [
     { "name": "FullName", "type": "Text", "value": "" },
     { "name": "Email", "type": "Text", "value": "" },
     { "name": "StartDate", "type": "Text", "value": "" },
     { "name": "Department", "type": "DropDown", "value": {
         "options": [ "Engineering", "Finance", "Sales", "Operations" ],
         "selected": "Engineering" } },
     { "name": "Agree", "type": "CheckBox", "value": false },
     { "name": "Newsletter", "type": "CheckBox", "value": false } ] }

-> fill_form_fields {"file":{"filePath":"new-employee-form.docx"},
     "values":{"FullName":"Amara Okafor",
               "Email":"amara.okafor@northwind.example",
               "StartDate":"2026-12-01","Department":"Finance",
               "Agree":"true","Phone":"+31 10 555 0100"}}
<- Updated 5 field(s) in 'new-employee-form.docx': FullName, Email,
   StartDate, Department, Agree.
   Not found: Phone. Call get_form_fields for the exact names.
   Saved to: …/new-employee-form_edited.docx
```

Reading the filled copy back returns `"value": "Amara Okafor"`, `"selected": "Finance"`, `"Agree": true` — and `Newsletter` still `false`, because nobody asked to tick it.

The form has no phone field. The server filled the five it knows and **named the one it did not**, so the agent can tell you the phone number is not on the form, rather than report a complete form.

## One form per row

With a list of people — a CSV, a spreadsheet the agent reads with its own tools, a table pasted into the chat — the agent repeats `fill_form_fields` per row and names each copy:

> Fill one copy of new-employee-form.docx for each row of new-hires.csv, named form-<last name>.docx, and list any column that has no field on the form.

`outputName` keeps the copies apart; without it each call would write `new-employee-form_edited.docx` again.

## Expected result

A filled copy per person, the original form untouched, and a list of values the form had no place for.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| *"Updated 0 field(s) … Not found: … No file was written."* | the names are the labels on the page, not the field names | call `get_form_fields` first and use its `name` values |
| `"count": 0` | the document has no form fields — just text and underscores | fill it with [`replace_text`]({{< ref "editor/mcp/tools-reference/replace-text.md" >}}) on a placeholder, or add fields in Word |
| A drop-down keeps its old value | the value is not one of its `options` | pass one of the listed options exactly |

## FAQ

**Which values does a check box take?** `"true"` or `"false"`.

**Can it fill PDF forms?** No — Word formats only. Convert the PDF first ([turn a PDF into Word]({{< ref "editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document.md" >}})) or use a PDF form tool.

**Can the filled copy be a PDF?** Yes — `"format": "pdf"` saves the filled form as a PDF to send.

## Add to your CLAUDE.md or AGENTS.md

Agents follow project instructions about *when* to use a tool. Paste this into the instruction file your agent reads (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `.cursorrules`):

```text
To fill Word forms in ./forms, call groupdocs-editor get_form_fields first and
fill by the exact names it returns; one outputName per person. Report every value
the server lists under 'Not found' instead of claiming the form is complete.
```

## Honest limits

* **Evaluation mode:** every edited document starts with an evaluation banner, a saved workbook gains an `Evaluation Warning` worksheet, and only worksheets and slides 1–2 can be edited (a PDF extraction holds two pages). [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}).
* **One formatting run:** `replace_text` does not see a phrase whose words are formatted differently; use the HTML round-trip for those.
* **No tracked changes:** the edited copy carries the new text, not a redline.
* **PDF editing is a conversion:** text and structure come across; exact page layout does not.

## Related

* [Edit documents with AI agents]({{< ref "editor/mcp/use-cases/edit-documents-with-ai-agents.md" >}})
* [Find and replace across a folder]({{< ref "editor/mcp/use-cases/find-and-replace-across-a-folder.md" >}})
* [Edit one worksheet or slide]({{< ref "editor/mcp/use-cases/edit-one-worksheet-or-slide-and-put-it-back.md" >}})
* [Turn a PDF into editable Word]({{< ref "editor/mcp/use-cases/turn-a-pdf-into-an-editable-word-document.md" >}})
* [On-premise deployment]({{< ref "editor/mcp/use-cases/on-premise-document-editing.md" >}})
* [Tools reference]({{< ref "editor/mcp/tools-reference/_index.md" >}}) · [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}) · [Troubleshooting & FAQ]({{< ref "editor/mcp/troubleshooting-faq.md" >}})
* Questions: [Editor forum](https://forum.groupdocs.com/c/editor/20)

## Read more

* [Fill Word forms from a spreadsheet with an AI agent, locally](https://blog.groupdocs.com/editor/fill-word-forms-from-spreadsheet-ai-agent-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

