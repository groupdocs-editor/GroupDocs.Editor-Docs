---
id: memory-optimization-option
url: editor/java/memory-optimization-option
title: Memory optimization option
weight: 9
description: "This article explains how to optimize memory utilization when editing large Word documents using GroupDocs.Editor for Java API."
keywords: large document edit performance, memory optimization when edit document
productName: GroupDocs.Editor for Java
hideChildren: False
---
By default [**GroupDocs.Editor**](https://products.groupdocs.com/editor/java) tries to perform computations and complete the task as fast as possible, and if this challenge requires a lot of memory to be used, GroupDocs.Editor does it. However, in some very specific cases, when processing document is very huge, and GroupDocs.Editor works in 32-bit application, which is limited to 2GiB per process, or user machine has very limited amount of free memory, the SystemOutOfMemoryException may occur. In order to solve such problem the [WordProcessingSaveOptions](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions) class contains the [`OptimizeMemoryUsage`](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor.options/wordprocessingsaveoptions/properties/optimizememoryusage) property:

```java
public boolean getOptimizeMemoryUsage()
public void getOptimizeMemoryUsage(boolean)
```

By default it has a "*false*" value, which means that the memory optimization is disabled for the sake of the best possible performance. By setting a "*true*" user can enable another document generating mechanism, which can significantly decrease memory consumption while generating large documents at the cost of slower generation time while performing the [`editor.Save()](https://apireference.groupdocs.com/editor/java/com.groupdocs.editor/editor#save()) method.
