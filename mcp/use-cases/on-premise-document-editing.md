---
id: mcp-uc-on-premise-document-editing
url: editor/mcp/use-cases/on-premise-document-editing
title: "Running GroupDocs MCP servers on-premise: architecture and security model"
linkTitle: On-premise deployment
weight: 6
description: "Run document editing for AI agents fully on-premise: the GroupDocs.Editor MCP server uses local stdio transport, no external endpoints, no inbound ports and no telemetry."
keywords: on-premise MCP server, air-gapped document editing, MCP security model, local document editing AI, self-hosted document editor for agents
productName: GroupDocs.Editor MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "Running GroupDocs MCP servers on-premise: architecture and security model"
        description: "Run document editing for AI agents fully on-premise: the GroupDocs.Editor MCP server uses local stdio transport, no external endpoints, no inbound ports and no telemetry."
        steps:
        - name: "Run the pinned image inside the perimeter"
          text: "Start the GroupDocs.Editor MCP server from its versioned Docker image as a child process of the AI client."
        - name: "Mount only the folders the agent may reach"
          text: "Map the document folder read-write and the license folder read-only."
        - name: "Choose the license mode"
          text: "Use a license file for fully offline operation; metered licensing needs outbound egress for usage reports."
---

Run document editing for AI agents **fully on-premise**, locally on machines you control: the GroupDocs.Editor MCP server uses local stdio transport with **no external endpoints, no inbound ports, and no telemetry**. This page is the one to send your security reviewer.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "editor/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The architecture in one picture

```text
+--------------+          +--------------------+         +------------------+
|  AI client   |  stdio   | MCP server process | reads / | local filesystem |
| (Claude, VS  | <----->  | (GroupDocs engine) | <-----> | storage / output |
| Code, agent) | JSON-RPC |   child process    |  writes |     folders      |
+--------------+          +--------------------+         +------------------+
```

* **Transport:** the AI client *starts the server as a child process* (`dnx` or `docker run -i`) and communicates over standard input/output. The server never listens on a network socket.
* **Data path:** agent → local server → local filesystem. Documents are read from, and edited copies written to, the folders you configure; originals are never overwritten.
* **Network use:** only at install time (nuget.org, or ghcr.io / docker.io). At runtime the server makes no outbound calls of its own — see *remote resources* below.
* **Telemetry:** none. The engine edits documents in-process.

## Docker deployment inside the perimeter

```bash
docker run --rm -i \
  -v /srv/documents:/data \
  -v /srv/licenses:/license:ro \
  -e GROUPDOCS_MCP_STORAGE_PATH=/data \
  -e GROUPDOCS_LICENSE_PATH=/license/GroupDocs.Editor.lic \
  ghcr.io/groupdocs-editor/editor-net-mcp:26.10.0
```

* Pin the tag (`:26.10.0`, not `:latest`).
* Mount only the folders the agent should reach; the licence read-only. Set `GROUPDOCS_MCP_OUTPUT_PATH` to keep edited copies apart from the originals.
* On Linux, add `--user "$(id -u):$(id -g)"` so the container can write into a folder you own.
* Multi-arch images (linux/amd64 + linux/arm64) with the fonts PDF output needs.

## Remote resources in documents

HTML or Office input that references **remote images or stylesheets** may cause the engine to fetch them while loading. For documents you did not write, run the container without network egress (`--network none`): the edit still works, the remote reference just does not resolve.

## The part that still travels

The server does not send documents anywhere. But the agent works with **text from the documents**: the HTML it edits, the form values it fills, the preview `extract_html` returns, the wording it is asked to replace. With a cloud-hosted model, that text reaches your model provider as part of the conversation. For sensitive documents, prefer `replace_text` and `fill_form_fields`, which change the file without the agent reading it, over the HTML round-trip — or use a locally hosted model.

## License management

* **License file** — read from local disk by the local process. Fully offline; the right answer for air-gapped deployments.
* **Metered (pay-per-use)** — reports *usage* to GroupDocs servers, so it needs outbound egress. Document content is never part of that report.

Both: [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}}).

## What this fits — honestly

**A good fit:** contract and policy maintenance, form filling, spreadsheet and deck updates, inside a network that does not allow document uploads.

**Not what this is:** a collaborative editor with tracked changes, or a shared service — one stdio server, one client, one machine.

## FAQ

**Does any document content leave the machine?** Not from the server. Text the agent reads or writes travels in the conversation to your model provider.

**Does it need internet at runtime?** No — only at install, for metered licensing, and if a document references remote resources (block that with `--network none`).

**Can I run it air-gapped?** Yes: pre-pull the image or pre-cache the NuGet package, use a license file, pin the version.

**What ports does it open?** None. stdio only.

**How do I prove that?** The [verification script]({{< ref "editor/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end) performs a real handshake and a real engine call so you can watch exactly what happens.

## Read more

* [3 architectures for AI document processing, and the one that keeps files inside your network](https://blog.groupdocs.com/total/on-premise-ai-document-processing-mcp/) — the same scenario on the GroupDocs blog, with worked examples and the trade-offs.

