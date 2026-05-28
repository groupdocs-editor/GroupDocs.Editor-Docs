---
id: system-requirements
url: editor/python-net/system-requirements
title: System Requirements
linkTitle: System Requirements
weight: 3
description: GroupDocs.Editor for Python via .NET ships as a self-contained wheel and runs on Windows, Linux, and macOS without MS Office, Open Office, or any other third-party software installed
keywords: GroupDocs.Editor for Python via .NET, Editor, python, system requirements
productName: GroupDocs.Editor for Python via .NET
hideChildren: False
toc: True
---
## Overview

GroupDocs.Editor for Python via .NET does not require MS Office, Open Office, or any other external software or third-party tool to be installed. The package is a self-contained wheel that bundles everything it needs, so the only prerequisites are a supported version of Python and the operating-system packages listed below. Just follow one of the ways described in [Development Environment, Installation and Configuration]({{< ref "editor/python-net/getting-started/installation.md" >}}).

## Supported Python Versions

GroupDocs.Editor for Python via .NET supports the following Python versions:

* Python 3.5
* Python 3.6
* Python 3.7
* Python 3.8
* Python 3.9
* Python 3.10
* Python 3.11
* Python 3.12
* Python 3.13
* Python 3.14

## Supported Operating Systems

The package is distributed as a self-contained wheel that runs on the following platforms:

### Windows

* Windows x64
* Windows x86

No additional dependencies are required on Windows.

### Linux

* Linux x64

On Linux you need to install a few system packages for graphics and font rendering:

```bash
apt install libgdiplus libfontconfig1 ttf-mscorefonts-installer
```

### macOS

* macOS x64 (Intel)
* macOS ARM64 (Apple Silicon)

On macOS install the graphics library via Homebrew:

```bash
brew install mono-libgdiplus
```

## No MS Office Required

Unlike many document-processing tools, GroupDocs.Editor for Python via .NET does not depend on Microsoft Office, Open Office, or any other application being installed on the machine. All loading, editing, and saving of Word Processing documents, Spreadsheets, Presentations, PDFs, emails, eBooks, and text/markup formats is performed entirely by the bundled engine.
