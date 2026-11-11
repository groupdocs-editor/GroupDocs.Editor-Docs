---
id: mcp-net
url: editor/net/mcp
title: MCP server for .NET
linkTitle: MCP Server
weight: 7
description: "Install and configure the GroupDocs.Editor MCP server for .NET — one-click install links for VS Code and Cursor, per-OS setup for Windows, Linux, and macOS, and the full environment-variable reference."
keywords: GroupDocs.Editor MCP .NET, install MCP server dnx, MCP server Docker image, MCP server configuration, Model Context Protocol .NET
productName: GroupDocs.Editor MCP Server for .NET
hideChildren: True
toc: True
---

Everything needed to **install and run** the GroupDocs.Editor MCP server on the .NET platform. What the server *does* — its tools, use cases, and licensing model — is platform-independent and lives in the [MCP server section]({{< ref "editor/mcp/_index.md" >}}).

| The .NET build at a glance | |
|---|---|
| Package | [`GroupDocs.Editor.Mcp`](https://www.nuget.org/packages/GroupDocs.Editor.Mcp) (current **26.10.0**) |
| One-command run | `dnx GroupDocs.Editor.Mcp --yes` |
| Container images | `ghcr.io/groupdocs-editor/editor-net-mcp` · `groupdocs/editor-net-mcp` |
| Prerequisites | [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0) for the NuGet channel, or Docker |
| Source | [GroupDocs.Editor.Mcp on GitHub](https://github.com/groupdocs-editor/GroupDocs.Editor.Mcp) |
| Release notes | [changelog](https://github.com/groupdocs-editor/GroupDocs.Editor.Mcp/tree/master/changelog) · [GitHub releases](https://github.com/groupdocs-editor/GroupDocs.Editor.Mcp/releases) |

## Start here

1. **Install** for your operating system — [Windows]({{< ref "editor/net/mcp/windows-installation.md" >}}) · [Linux]({{< ref "editor/net/mcp/linux-installation.md" >}}) · [macOS]({{< ref "editor/net/mcp/macos-installation.md" >}})
2. **Register it in your AI client** — [one-click links and per-client configs]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}})
3. **Point it at your documents** — [configuration]({{< ref "editor/net/mcp/configuration.md" >}})
4. **License it** — evaluation, a license file, or metered keys: [Licensing]({{< ref "editor/mcp/getting-started/licensing.md" >}})

## In this section

* [Install on Windows]({{< ref "editor/net/mcp/windows-installation.md" >}})
* [Install on Linux]({{< ref "editor/net/mcp/linux-installation.md" >}})
* [Install on macOS]({{< ref "editor/net/mcp/macos-installation.md" >}})
* [Register in AI clients]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}}) — VS Code, Cursor, Claude, Visual Studio, Windsurf, Cline, Codex, Rider
* [Configuration]({{< ref "editor/net/mcp/configuration.md" >}}) — storage, output, license, metered keys
* [System requirements]({{< ref "editor/net/mcp/system-requirements.md" >}})
* [Troubleshooting (.NET)]({{< ref "editor/net/mcp/troubleshooting.md" >}}) — `dnx`, native libraries, Docker daemon

## Platform-independent reference

* [Tools reference]({{< ref "editor/mcp/tools-reference/_index.md" >}}) — `get_document_info`, `replace_text`, `extract_html`, `save_from_html`, `get_form_fields`, `fill_form_fields`, `get_supported_formats`, `get_license_status`
* [Use cases]({{< ref "editor/mcp/use-cases/_index.md" >}}) · [Supported formats]({{< ref "editor/mcp/supported-formats.md" >}}) · [Troubleshooting & FAQ]({{< ref "editor/mcp/troubleshooting-faq.md" >}})
