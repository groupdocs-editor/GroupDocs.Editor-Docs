---
id: mcp-system-requirements
url: editor/net/mcp/system-requirements
title: System requirements
weight: 8
description: "The GroupDocs.Editor MCP server runs anywhere Docker runs, or on any OS with the .NET 10 SDK — this page lists exact requirements per channel and operating system."
keywords: MCP server requirements, does MCP server need Docker, .NET 10 MCP server
productName: GroupDocs.Editor MCP Server for .NET
toc: True
---

The GroupDocs.Editor MCP server runs **anywhere Docker runs**, or on any OS with the **.NET 10 SDK** — pick one channel; you do not need both.

| | Docker channel (recommended) | NuGet channel (`dnx`) |
|---|---|---|
| Runtime | Docker Desktop / docker engine | .NET 10 SDK (includes `dnx`) |
| Native deps | bundled in the image | Windows: nothing · Linux: `libfontconfig1`, MS core fonts (only for PDF and XPS output) · macOS: nothing |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) | any .NET 10 platform |
| Installer scripts | PowerShell 5.1 (Windows built-in) or PowerShell 7 (`pwsh`, macOS/Linux) | same |
| Disk | image ~download once per version | package cached by `dnx`/NuGet |
| Network | ghcr.io / docker.io when the image is pulled; a pulled, pinned image runs offline | nuget.org each time `dnx` starts the server (it resolves the latest version); pin the version and keep the package cached to run offline |
| Linux file ownership | run as your user: `--user "$(id -u):$(id -g)"` | runs as you already |

Per-OS setup, including the one-shot prerequisite bootstrappers (`setup/windows.ps1`, `setup/linux.sh`, `setup/macos.sh`):

* [Install on Windows]({{< ref "editor/net/mcp/windows-installation.md" >}})
* [Install on Linux]({{< ref "editor/net/mcp/linux-installation.md" >}})
* [Install on macOS]({{< ref "editor/net/mcp/macos-installation.md" >}})

AI client requirements: any MCP-compatible client (Claude Desktop, Claude Code, VS Code / GitHub Copilot, Visual Studio 2022 17.14+, Cursor, Windsurf, Cline, Codex CLI) — see [Register in AI clients]({{< ref "editor/net/mcp/install-in-ai-clients.md" >}}).

The engine's own platform requirements (frameworks, OS versions) are those of the library: [GroupDocs.Editor system requirements]({{< ref "editor/net/getting-started/system-requirements.md" >}}).
