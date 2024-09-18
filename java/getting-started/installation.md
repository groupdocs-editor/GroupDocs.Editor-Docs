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

First, you need to specify repository configuration/location in your project as follows:

{{< tabs "example1">}}
{{< tab "Maven" >}}
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
{{< tab "Gradle" >}}
```xml
repositories {
    maven {
        url "https://repository.groupdocs.com/repo/"
    }
}
```
{{< /tab >}}
{{< tab "Kotlin" >}}
```xml
repositories {
    maven(url = "https://repository.groupdocs.com/repo/")
}
```
{{< /tab >}}
{{< tab "Ivy" >}}
```xml
<ivysettings>
    <settings defaultResolver="chain"/>
    <resolvers>
        <chain name="chain">
            <ibiblio name="GroupDocs Repository" m2compatible="true" root="https://releases.groupdocs.com/java/repo/"/>
        </chain>
    </resolvers>
</ivysettings>
```
{{< /tab >}}
{{< tab "Sbt" >}}
```xml
resolvers += Resolver.url("GroupDocs Repository", url("https://releases.groupdocs.com/java/repo/"))
```
{{< /tab >}}
{{< /tabs >}}

### Add GroupDocs.Editor as a dependency

Then define GroupDocs.Editor for Java API dependency in your `pom.xml` as follows:

{{< tabs "example2">}}
{{< tab "Maven" >}}
```xml
<dependencies>
    <dependency>
        <groupId>com.groupdocs</groupId>
        <artifactId>groupdocs-editor</artifactId>
        <version>24.9</version>
    </dependency>
</dependencies>
```
{{< /tab >}}
{{< tab "Gradle" >}}
```xml
dependencies {
    implementation 'com.groupdocs:groupdocs-editor:24.9'
}
```
{{< /tab >}}
{{< tab "Kotlin" >}}
```xml
dependencies {
    implementation("com.groupdocs:groupdocs-editor:24.9")
}
```
{{< /tab >}}
{{< tab "Ivy" >}}
```xml
<dependency org="com.groupdocs" name="groupdocs-editor" rev="24.9">
   <artifact name="groupdocs-editor" ext="jar"/>
</dependency>
```
{{< /tab >}}
{{< tab "Sbt" >}}
```xml
libraryDependencies += "com.groupdocs" % "groupdocs-editor" % "24.9"
```
{{< /tab >}}
{{< /tabs >}}

_The latest version of Editor API can be [found here](https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-editor/)_
