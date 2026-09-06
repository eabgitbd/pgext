# 📄 PDF Range Extractor

A lightweight, single-file browser tool for extracting multiple page ranges from a large PDF and downloading them as individual PDFs or as a single ZIP — with **zero quality loss**, no server, and no file uploads.

---

## ✨ Features

- **Multi-range extraction** — define as many named page ranges as you need (e.g. "Chapter 1", "Appendix", "Invoice Pages")
- **Lossless output** — uses `pdf-lib`'s `copyPages()` to transfer raw PDF page objects without re-rendering or re-encoding
- **Two download modes** — download each range individually, or grab all ranges at once as a ZIP
- **Drag & drop or browse** — load any PDF from your local machine
- **Live page count badge** — see how many pages each range covers as you type
- **Input validation** — red highlighting and clear error messages for out-of-range or incomplete inputs
- **Auto-deduplication** — duplicate range labels are automatically suffixed to avoid filename conflicts
- **Memory safe** — the source PDF is parsed once and reused; `yieldToUI()` between ranges keeps the browser responsive on large files
- **100% client-side** — nothing leaves your device; no server, no analytics, no dependencies beyond two CDN scripts
- **Single HTML file** — zero build step, zero configuration, just open and use

---

## 🚀 Live Demo

> Deploy with **GitHub Pages** in one step:
> 1. Push this repo
> 2. Go to **Settings → Pages → Source: main branch / root**
> 3. Your tool is live at `https://<your-username>.github.io/<repo-name>/`

Or just download `pdf-range-extractor.html` and open it in any modern browser.

---

## 🖥️ How to Use

| Step | Action |
|------|--------|
| **1** | Drag & drop a PDF onto the drop zone, or click **Browse File** |
| **2** | Define one or more page ranges — enter start page, end page, and an optional label |
| **3** | Click **⚡ Extract All Ranges** |
| **4** | Download each range individually, or use **⬇ Download All as ZIP** |

---

## 📸 Screenshot

> *(Add a screenshot here after deploying — `![Screenshot](screenshot.png)`)*

---

## 🛠️ Tech Stack

| Library | Version | Purpose |
|---------|---------|---------|
| [pdf-lib](https://pdf-lib.js.org/) | 1.17.1 | Lossless PDF page extraction and document creation |
| [JSZip](https://stuk.github.io/jszip/) | 3.10.1 | Bundling multiple PDFs into a single ZIP download |

Both are loaded from [cdnjs.cloudflare.com](https://cdnjs.cloudflare.com) — no npm, no bundler.

---

## 📁 Repository Structure

```
/
├── pdf-range-extractor.html   # The entire application (HTML + CSS + JS)
└── README.md
```

---

## ⚙️ Technical Notes

**Why lossless?**
`pdf-lib`'s `copyPages()` copies the raw PDF object graph from the source document into the new document. No pixels are decoded, re-rendered, or re-compressed — fonts, images, vectors, and annotations are preserved exactly.

**Why no memory issues on large PDFs?**
The source `PDFDocument` is loaded once and shared across all range extractions. Each range creates a lightweight new document referencing copied page objects. A `yieldToUI()` (`setTimeout(0)`) call between ranges gives the browser event loop time to breathe, preventing "page unresponsive" freezes.

**Why does the download use `createObjectURL`?**
Blob URLs are revoked after 10 seconds — enough for the browser to start the download — avoiding memory leaks from long-lived object URLs. For the ZIP, JSZip assembles all blobs in memory using `arrayBuffer()` and generates the archive client-side.

**File input reliability**
The `<input type="file">` is a hidden element outside the drop zone, triggered programmatically by `.click()`. This avoids cross-browser quirks with opacity-0 overlays and ensures `change` fires reliably even when the same file is re-selected (the input is reset to `''` before each trigger).

---

## 🌐 Browser Compatibility

| Browser | Support |
|---------|---------|
| Chrome / Edge 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 15+ | ✅ Full |
| Mobile Chrome / Safari | ✅ Full |

Requires: `File API`, `Blob`, `URL.createObjectURL`, `async/await` — all standard in any browser from 2021 onward.

---

## 🔒 Privacy

All processing happens **in your browser**. The PDF never leaves your device. No data is sent to any server. No cookies, no tracking, no analytics.

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

## 🤝 Contributing

Pull requests are welcome. To suggest a feature or report a bug, open an [Issue](../../issues).

Some ideas for future improvements:
- [ ] PDF thumbnail preview per range
- [ ] Page range input via text notation (e.g. `1-5, 8, 12-15`)
- [ ] Reorder ranges before extraction via drag-and-drop
- [ ] Dark/light theme toggle
- [ ] Offline support via Service Worker / PWA
