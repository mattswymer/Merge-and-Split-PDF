# Merge-and-Split-PDF
### Overview

The provided `index.html` is a client-side PDF manipulation tool that allows users to merge, reorder, and split PDF documents within a web browser. It leverages three primary JavaScript libraries: `pdf.js` for rendering, `pdf-lib` for document modification, and `Sortable.js` for UI interactions. The tool operates entirely in the client's memory, ensuring that no document data is transmitted to a server.

---

### Technical Architecture

#### 1. PDF Rendering and Processing

* **Library:** `pdf.js`
* **Mechanism:** The application uses a worker-based rendering pipeline. When a file is uploaded, it is converted to a `Uint8Array` and loaded via `pdfjsLib.getDocument`.
* **Concurrency:** A `RENDER_CONCURRENCY` constant (set to 4) manages simultaneous page rendering to maintain UI responsiveness while processing large documents.
* **Thumbnail Generation:** Each page is rendered onto a `<canvas>` element at a scale of 0.5 for the grid view. A higher resolution scale (2.0) is used for the modal zoom feature.

#### 2. Document Manipulation

* **Library:** `pdf-lib`
* **State Management:** The application maintains a `sourcePdfs` object, keyed by a unique `fileId` (UUID), which stores the original `Uint8Array` data.
* **Operations:**
* **Reordering:** Managed via `Sortable.js` which manipulates the DOM order of page cards. The export function reads this DOM order to determine the sequence of the output PDF.
* **Splitting:** A "split-zone" toggle on each page card adds an `is-split` class. During export, the logic detects this flag and triggers the creation of a new `PDFDocument` instance, resulting in multiple downloaded files.
* **Merging:** Pages are copied from source documents into a target document using `copyPages`.



#### 3. Performance and Memory Management

* **Memory Pruning:** The `refreshUI` function contains logic to delete `sourcePdfs` entries and destroy `pdfProxies` when their associated page cards are removed from the DOM.
* **Non-blocking Execution:** The application utilizes `scheduler.yield()` or `setTimeout` (via `yieldToMain`) during the export process to prevent the main thread from freezing during heavy cryptographic or serialization tasks.
* **Constraints:**
* Max size: 50 MB.
* Max files: 50.



---

### UI Components

| Component | Description |
| --- | --- |
| **Dropzone** | Handles `dragover` and `drop` events for file ingestion. |
| **Grid System** | A CSS Grid layout (`auto-fill`) displaying individual page cards. |
| **Page Card** | An interactive element containing a delete button, a split trigger, and a thumbnail canvas. |
| **Zoom Modal** | A fixed-position overlay for full-page inspection. |
| **Progress Bar** | Visual feedback for the export serialization process. |

---

### Workflow Specifications

1. **Ingestion:** Files are validated by MIME type. Each file is assigned a UUID.
2. **Mapping:** Each page in the grid retains metadata (`data-file-id` and `data-page-index`) linking it back to the original source buffer and its specific page number.
3. **Export:**
* Iterate through DOM elements in current order.
* Load the source `pdf-lib` document if not already in the `loadedDocs` cache.
* Copy the specific page to the active output document.
* If a split point is reached, save the current document and initialize a new one.
* Generate Blobs and trigger downloads via transient `<a>` elements.



---

### Dependencies

* **pdf.js**: `v3.11.174` (Mozilla)
* **pdf-lib**: `v1.17.1` (Hopding)
* **Sortable.js**: `v1.15.0` (RubaXa)

Would you like me to implement any additional features, such as page rotation or password-protected PDF support?
