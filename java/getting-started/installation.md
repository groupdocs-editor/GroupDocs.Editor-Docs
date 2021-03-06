---
id: installation
url: editor/java/installation
title: Installation
weight: 4
description: "This guide explains how to install GroupDocs.Editor for Java to your environment"
keywords: 
productName: GroupDocs.Editor for Java
hideChildren: False
toc: true
---
## Operating Systems

**GroupDocs.**Editor** for Java** can be executed on any Operating System with Java JDK installed.

* Windows Desktops and Servers
* Linux
* Mac OS

## Supported Runtime

**GroupDocs.Editor for Java** supports Java run-time version 6 (1.6) and above.

## Development Environments

* NetBeans
* IntelliJ IDEA
* Eclipse

## Installation from GroupDocs Repository using Maven

GroupDocs hosts all Java APIs on [GroupDocs Repository](https://repository.groupdocs.com/webapp/#/artifacts/browse/tree/General/repo). You can easily use [GroupDocs.Editor for Java](https://repository.groupdocs.com/webapp/#/artifacts/browse/tree/General/repo/com/groupdocs/groupdocs-editor) API directly in your Maven projects with simple configurations.

### Specify GroupDocs Repository Configuration

First, you need to specify GroupDocs repository configuration/location in your Maven `pom.xml` as follows:

```java
<repositories>
	<repository>
		<id>GroupDocs Artifact Repository</id>
        	<name>GroupDocs Artifact Repository</name>
        	<url>https://repository.groupdocs.com/repo/</url>
	</repository>
</repositories>
```

### Define GroupDocs.Editor for Java API Dependency

Then define GroupDocs.Editor for Java API dependency in your `pom.xml` as follows:

```java
<dependencies>
    <dependency>
        <groupId>com.groupdocs</groupId>
        <artifactId>groupdocs-editor</artifactId>
        <version>22.6</version>
    </dependency>
</dependencies>
```

After performing above-mentioned steps, GroupDocs.Editor for Java dependency will finally be added to your Maven project.
