---
id: installation
url: editor/nodejs-java/installation
title: Installation
weight: 4
description: "GroupDocs.Editor for Node.js installation"
keywords: "groupdocs editor Node.js, installation, maven"
productName: GroupDocs.Editor for Node.js
hideChildren: False
toc: True
---


## Install using npm

All Node.js packages are hosted at [GroupDocs Artifact Repository](https://repository.groupdocs.com/). You can easily reference GroupDocs.Editor for Node.js API directly in your npm project using the following steps.

1. **Install GroupDocs.Editor for Node.js:**

   After configuring the repository, you can install the GroupDocs.Editor package using npm:
   ```sh
   npm install @groupdocs/editor
   ```

2. **Example usage:**

   Here is a basic example of how to use GroupDocs.Editor in your Node.js application:
   ```javascript
   const groupdocsEditor = require('@groupdocs/editor');

   // Create an instance of the Editor class
   const editor = new groupdocsEditor.Editor("path/to/document");

   // Get document information
   const documentInfo = editor.getDocumentInfo();
   console.log(documentInfo);

   // Convert document to HTML
   const htmlContent = editor.edit();
   console.log(htmlContent);

   // Save the edited HTML back to the original document format
   editor.save(htmlContent, "path/to/save/document");
   ```

By following these steps, you can integrate and use GroupDocs.Editor for Node.js in your projects. For more details, refer to the official [GroupDocs documentation](https://docs.groupdocs.com/editor/nodejs-java/).