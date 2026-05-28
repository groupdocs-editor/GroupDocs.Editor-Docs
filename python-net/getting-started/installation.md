---
id: installation
url: editor/python-net/installation
title: Installation
linkTitle: Installation
weight: 4
description: "Install GroupDocs.Editor for Python via .NET on Windows, Linux, or macOS — from PyPI or from a pre-downloaded wheel, including Intel and Apple Silicon builds."
keywords: install, installation, pip, pypi, wheel, whl, Windows, Linux, macOS, Apple Silicon, manylinux, requirements.txt, GroupDocs.Editor, python
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---

GroupDocs.Editor for Python via .NET is distributed as a pre-built wheel on [PyPI](https://pypi.org/project/groupdocs-editor-net/). The PyPI index hosts a separate wheel for each supported platform, and `pip` picks the correct one automatically. Each wheel is self-contained (~120 MB): it bundles the embedded .NET runtime and every native dependency, so no MS Office, OpenOffice, or separate .NET install is required.

Before installing, confirm your environment matches the supported platforms and Python versions listed in the [System Requirements]({{< ref "editor/python-net/getting-started/system-requirements.md" >}}) topic.

## Install Package from PyPI

Open a terminal and run the install command for your platform:

{{< tabs "install-pypi">}}
{{< tab "Windows" >}}
```ps
py -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m pip install groupdocs-editor-net
```
{{< /tab >}}
{{< /tabs >}}

After running the command you should see output similar to:

```bash
Collecting groupdocs-editor-net
  Downloading groupdocs_editor_net-26.5-py3-none-win_amd64.whl.metadata (6.0 kB)
  Downloading groupdocs_editor_net-26.5-py3-none-win_amd64.whl (104.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 104.0/104.0 MB 2.8 MB/s eta 0:00:00
Installing collected packages: groupdocs-editor-net
Successfully installed groupdocs-editor-net-26.5
```

The wheel file name will include a platform suffix that matches your operating system — for example `manylinux1_x86_64` on Ubuntu/Debian, `macosx_11_0_arm64` on Apple Silicon, or `win_amd64` on 64-bit Windows.

## Add the Package to `requirements.txt`

For reproducible environments, pin the package version in your `requirements.txt`:

```txt
groupdocs-editor-net==26.5
```

Then install all dependencies in one step:

```bash
pip install -r requirements.txt
```

## Install from a Pre-Downloaded Wheel

If your build environment cannot reach PyPI, download the appropriate wheel from the [GroupDocs Releases website](https://releases.groupdocs.com/editor/python-net/) and install it locally. The following wheels are published for each release:

- **Windows 64-bit**: file name ends with `win_amd64.whl`
- **Linux x64 (glibc)**: file name ends with `manylinux1_x86_64.whl`
- **macOS Apple Silicon**: file name ends with `macosx_11_0_arm64.whl`
- **macOS Intel**: file name ends with `macosx_10_14_x86_64.whl`

Place the downloaded wheel into your project folder, then install it:

{{< tabs "install-wheel">}}
{{< tab "Windows (64-bit)" >}}
```ps
py -m pip install groupdocs_editor_net-26.5-py3-none-win_amd64.whl
```
{{< /tab >}}
{{< tab "Linux (glibc)" >}}
```bash
python3 -m pip install groupdocs_editor_net-26.5-py3-none-manylinux1_x86_64.whl
```
{{< /tab >}}
{{< tab "macOS (Apple Silicon)" >}}
```bash
python3 -m pip install groupdocs_editor_net-26.5-py3-none-macosx_11_0_arm64.whl
```
{{< /tab >}}
{{< tab "macOS (Intel)" >}}
```bash
python3 -m pip install groupdocs_editor_net-26.5-py3-none-macosx_10_14_x86_64.whl
```
{{< /tab >}}
{{< /tabs >}}

Expected output:

```bash
Processing groupdocs_editor_net-26.5-py3-none-*.whl
Installing collected packages: groupdocs-editor-net
Successfully installed groupdocs-editor-net-26.5
```

## Platform Prerequisites

On Windows no extra steps are required. On Linux and macOS, install the native libraries the rendering engine depends on:

{{< tabs "platform-prereqs">}}
{{< tab "Linux" >}}
```bash
apt install libgdiplus libfontconfig1 ttf-mscorefonts-installer
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
brew install mono-libgdiplus
```
{{< /tab >}}
{{< /tabs >}}

## Next Steps

- Follow the [Quick Start Guide]({{< ref "editor/python-net/getting-started/quick-start-guide" >}}) to run your first edit.
- Clone the [examples repository](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Python-via-.NET) and read [Running Examples]({{< ref "editor/python-net/getting-started/how-to-run-examples.md" >}}) to try every documented scenario locally.
- If you work with AI agents or LLMs, see [Agents and LLMs]({{< ref "editor/python-net/agents-and-llm-integration" >}}) for MCP and `AGENTS.md` integration details.
