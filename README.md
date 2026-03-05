# Merge-and-Split-PDF

A browser-based utility for reordering, merging, and splitting PDF documents. This tool processes all files locally in the browser, ensuring that document data is never uploaded to a server.

---

## Features

* **File Consolidation**: Combine multiple PDF files into a single document.
* **Visual Reordering**: Drag and drop individual pages to change their sequence.
* **Document Splitting**: Define custom split points to export a single source into multiple PDF files.
* **Page Management**: Remove unnecessary pages from a document before exporting.
* **Preview Mode**: Double-click any page thumbnail to view a high-resolution zoom of the content.

---

## Instructions for Use

1. **Upload**: Click the upload area or drag PDF files directly into the browser window.
2. **Organize**:
* Click and hold a page thumbnail to drag it to a new position.
* Click the **X** icon on any page to remove it from the project.


3. **Split**: Click the vertical gap between pages to toggle a split point. A highlighted divider indicates where the document will be broken into a separate file.
4. **Export**: Click the **Export PDF(s)** button to process the changes and download the resulting files to your device.
5. **Reset**: Use the **New** button to clear the current workspace and start a new project.

---

## Technical Specifications

* **Privacy**: All processing is performed client-side. No data is transmitted to external servers.
* **File Limits**: The application supports a cumulative total of 50 files or 50 MB of data.
* **Compatibility**: Optimized for modern web browsers with JavaScript enabled.
* **Dependencies**: Utilizes PDF.js for rendering, PDF-Lib for document assembly, and Sortable.js for the drag-and-drop interface.

---

## Limitations

* **Encrypted Files**: Password-protected or highly restricted PDFs may fail to load.
* **Memory Usage**: Large documents may impact browser performance as all pages are rendered as image thumbnails in the workspace.
