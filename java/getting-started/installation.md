---
id: installation
url: editor/java/installation
title: Installation
weight: 4
description: "GroupDocs.Editor for Java installation"
keywords: "groupdocs editor java, installation, maven"
productName: GroupDocs.Editor for Java
hideChildren: False
toc: True
---

## Install using Maven

All Java packages are hosted at [GroupDocs Artifact Repository](https://repository.groupdocs.com/). You can easily reference GroupDocs.Editor for Java API directly in your Maven project using following steps.

### Add GroupDocs Artifact Repository

First, you need to specify repository configuration/location in your Maven `pom.xml` as follows:

{{< tabs "example1">}}
{{< tab "pom.xml" >}}
```xml
<repositories>
	<repository>
		<id>GroupDocs Artifact Repository</id>
        	<name>GroupDocs Artifact Repository</name>
        	<url>https://releases.groupdocs.com/java/repo/</url>
	</repository>
</repositories>
```
{{< /tab >}}
{{< /tabs >}}

### Add GroupDocs.Editor as a dependency

Then define GroupDocs.Editor for Java API dependency in your `pom.xml` as follows:

{{< tabs "example2">}}
{{< tab "pom.xml" >}}
```xml
<dependencies>
    <dependency>
        <groupId>com.groupdocs</groupId>
        <artifactId>groupdocs-editor</artifactId>
        <version>22.11</version>
    </dependency>
</dependencies>
```
{{< /tab >}}
{{< /tabs >}}
