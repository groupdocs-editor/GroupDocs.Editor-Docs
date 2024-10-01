---

id: locales-for-output-document
url: editor/nodejs-java/locales-for-output-document
title: Locales for Output Document
weight: 8
description: "This guide demonstrates how to edit RTL documents and specify locales for Word documents when using GroupDocs.Editor for Node.js via Java API."
keywords: Edit document with locale, GroupDocs.Editor, Edit RTL documents
productName: GroupDocs.Editor for Node.js via Java
hideChildren: False
---

The [`WordProcessingSaveOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.WordProcessingSaveOptions) class contains three similar properties of the type `Locale`:

```javascript
// Getter and Setter for locale
public Locale getLocale()
public void setLocale(Locale value)

// Getter and Setter for localeBi
public Locale getLocaleBi()
public void setLocaleBi(Locale value)

// Getter and Setter for localeFarEast
public Locale getLocaleFarEast()
public void setLocaleFarEast(Locale value)
```

In most cases, the output WordProcessing document generated from the [`EditableDocument`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.EditableDocument) instance by the [`Editor.save()`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor/Editor#save(com.groupdocs.editor.EditableDocument,%20java.io.OutputStream,%20com.groupdocs.editor.options.ISaveOptions)) method contains valid locales for the textual content. However, in some scenarios, it is necessary to explicitly set a specific locale for the output document. These three options provide that capability. Keep in mind that they are suitable only when the document should have a single locale. If the document is multilingual and contains, for example, English and Spanish text, setting the locale to English ("en-GB") will mark the Spanish text as English too. As a result, spell checking in MS Word may not work properly for the Spanish text.

By default, all these three locale-related properties are not specified; their values are `null`. In such cases, MS Word (or other programs) will detect (or choose) the document locale according to their own settings or other factors. Additionally, if the document is multilingual, it is strongly recommended to enable the [`enableLanguageInformation`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.WordProcessingEditOptions#isEnableLanguageInformation()) property in the [`WordProcessingEditOptions`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.WordProcessingEditOptions) class while editing the original document, and not to use these three properties.

- The [`locale`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.WordProcessingSaveOptions#getLocale()) property is intended to set the locale for standard left-to-right text, which consists of letters.
- The [`localeBi`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.WordProcessingSaveOptions#getLocaleBi()) property should be used when the text is right-to-left (RTL), for example, [Arabic script](https://en.wikipedia.org/wiki/Arabic_script) or [Hebrew alphabet](https://en.wikipedia.org/wiki/Hebrew_alphabet).
- The [`localeFarEast`](https://reference.groupdocs.com/editor/nodejs-java/com.groupdocs.editor.options.WordProcessingSaveOptions#getLocaleFarEast()) property should be used for East Asian (Far East) text, including [CJK characters](https://en.wikipedia.org/wiki/CJK_characters).

## Example Usage

The following example demonstrates how to set the locale properties when saving a WordProcessing document:

```javascript
// Import necessary modules
const groupdocs_editor = require('groupdocs-editor');
const fs = require('fs');

// Load the source WordProcessing document
const editor = new groupdocs_editor.Editor('SampleDoc.docx');

// Create WordProcessingEditOptions and enable language information if needed
const editOptions = new groupdocs_editor.WordProcessingEditOptions();
editOptions.enableLanguageInformation = true;

// Edit the document to get an EditableDocument instance
const editableDocument = editor.edit(editOptions);

// Create WordProcessingSaveOptions
const saveOptions = new groupdocs_editor.WordProcessingSaveOptions();

// Set the desired locale (e.g., English - United Kingdom)
saveOptions.locale = java.util.Locale.UK;

// For right-to-left languages (e.g., Arabic - Saudi Arabia)
// saveOptions.localeBi = new java.util.Locale("ar", "SA");

// For Far East languages (e.g., Japanese)
// saveOptions.localeFarEast = java.util.Locale.JAPANESE;

// Save the edited document with the specified locale settings
editor.save(editableDocument, 'OutputDoc.docx', saveOptions);

console.log('Document saved successfully with specified locale.');
```

**Note:**

- Replace `'SampleDoc.docx'` with the path to your source document.
- The `java.util.Locale` class is used to specify the locale. You can create a new locale by specifying the language and country codes.

## Important Considerations

- **Single Locale Documents:** These properties are suitable only when the document contains text in a single language. If your document is multilingual, using these properties might not yield the desired results.
- **Multilingual Documents:** For documents containing multiple languages, it's recommended to enable the `enableLanguageInformation` property in the `WordProcessingEditOptions` class and avoid setting the locale properties in `WordProcessingSaveOptions`.
  
  ```javascript
  const editOptions = new groupdocs_editor.WordProcessingEditOptions();
  editOptions.enableLanguageInformation = true;
  ```

- **Default Behavior:** If the locale properties are not set, their values are `null`, and the locale will be determined by MS Word or other programs based on their settings.

## Understanding the Locale Properties

- **`locale`:** Sets the locale for standard left-to-right text, such as English, French, or German.
- **`localeBi`:** Sets the locale for right-to-left (RTL) text, such as Arabic or Hebrew.
- **`localeFarEast`:** Sets the locale for East Asian languages, including Chinese, Japanese, and Korean.

## Specifying Locales

You can specify a locale using the `java.util.Locale` class. Here are some examples:

```javascript
// English (United Kingdom)
saveOptions.locale = java.util.Locale.UK;

// English (United States)
saveOptions.locale = java.util.Locale.US;

// Spanish (Spain)
saveOptions.locale = new java.util.Locale("es", "ES");

// Arabic (Saudi Arabia)
saveOptions.localeBi = new java.util.Locale("ar", "SA");

// Chinese (Simplified)
saveOptions.localeFarEast = java.util.Locale.SIMPLIFIED_CHINESE;

// Japanese
saveOptions.localeFarEast = java.util.Locale.JAPANESE;
```

## Conclusion

By setting the locale properties in `WordProcessingSaveOptions`, you can control the language settings of your output WordProcessing documents when using GroupDocs.Editor for Node.js via Java. This is particularly useful when dealing with documents in specific languages or scripts, such as RTL or East Asian languages.

For more examples and detailed information, you can refer to the [GroupDocs.Editor for Node.js via Java Examples](https://github.com/groupdocs-editor/GroupDocs.Editor-for-Node.js-via-Java/tree/master/Examples) repository on GitHub.