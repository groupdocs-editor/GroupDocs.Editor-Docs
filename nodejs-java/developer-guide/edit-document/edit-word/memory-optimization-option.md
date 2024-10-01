---

id: memory-optimization-option
url: editor/nodejs-java/memory-optimization-option
title: Memory Optimization Option
weight: 9
description: "This article explains how to optimize memory utilization when editing large Word documents using GroupDocs.Editor for Node.js via Java API."
keywords: large document edit performance, memory optimization when edit document
productName: GroupDocs.Editor for Node.js via Java
hideChildren: False
toc: True
---

By default, [**GroupDocs.Editor**](https://products.groupdocs.com/editor/nodejs-java) aims to perform computations and complete tasks as quickly as possible. If a task requires a lot of memory, GroupDocs.Editor utilizes it to optimize performance. However, in certain scenarios—such as processing very large documents, running in a 32-bit application limited to 2GiB per process, or operating on a machine with limited free memory—a `SystemOutOfMemoryException` may occur. To address this issue, the [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.WordProcessingSaveOptions) class provides the `optimizeMemoryUsage` property:

```javascript
// Getter and Setter for optimizeMemoryUsage
public boolean getOptimizeMemoryUsage()
public void setOptimizeMemoryUsage(boolean value)
```

By default, this property is set to `false`, meaning memory optimization is disabled in favor of achieving the best possible performance. Setting it to `true` enables an alternative document generation mechanism that significantly reduces memory consumption when generating large documents. However, this comes at the cost of slower generation time when performing the `editor.save()` method.

## Example Usage

The following example demonstrates how to enable memory optimization when saving a large WordProcessing document:

```javascript
// Import necessary modules
const groupdocs_editor = require('groupdocs-editor');
const fs = require('fs');

// Load the source WordProcessing document
const editor = new groupdocs_editor.Editor('LargeDocument.docx');

// Create WordProcessingEditOptions if needed
const editOptions = new groupdocs_editor.WordProcessingEditOptions();

// Edit the document to get an EditableDocument instance
const editableDocument = editor.edit(editOptions);

// Create WordProcessingSaveOptions
const saveOptions = new groupdocs_editor.WordProcessingSaveOptions();

// Enable memory optimization
saveOptions.optimizeMemoryUsage = true;

// Save the edited document with memory optimization enabled
editor.save(editableDocument, 'OutputDocument.docx', saveOptions);

console.log('Document saved successfully with memory optimization enabled.');
```

**Note:**

- Replace `'LargeDocument.docx'` with the path to your large WordProcessing document.
- Enabling `optimizeMemoryUsage` reduces memory consumption but may increase the time it takes to save the document.

## Important Considerations

- **Performance Trade-off:** Enabling memory optimization reduces memory usage but may result in slower document generation. Use this option when dealing with very large documents or limited memory environments.
- **Default Behavior:** By default, `optimizeMemoryUsage` is set to `false` to prioritize performance.
- **Exception Handling:** If you encounter a `SystemOutOfMemoryException` or similar memory-related issues, consider enabling this option.

## Conclusion

The `optimizeMemoryUsage` property in `WordProcessingSaveOptions` offers a way to handle large documents efficiently when using GroupDocs.Editor for Node.js via Java. By adjusting this setting, you can balance memory consumption and performance according to your application's requirements.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.