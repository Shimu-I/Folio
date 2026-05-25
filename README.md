![Visitor Count](https://visitor-badge.laobi.icu/badge?page_id=shimu-i/Folio)


 📗 Folio

> A lightweight, fully offline research notebook for CSE researchers.  
> Built for deep work — not the cloud.

---

## What is Folio?

**Folio** is a local-first research notebook designed specifically for Computer Science & Engineering researchers. It replaces cloud-dependent tools like Notion and Zotero with a single, privacy-first, offline-capable desktop app that stores everything on your own machine. No subscriptions. No limits. No internet required.

Named after the large-format pages scholars once used to record knowledge — Folio gives you that same sense of ownership over your research.

---

## ✨ Core Philosophy

- **Local first** — all data lives on your machine (SQLite + local file system)
- **Researcher focused** — built around CSE workflows, not generic note-taking
- **Lightweight** — Tauri-based, ~5–8 MB install size
- **Beautiful** — glassmorphism UI, handwriting-style font, forest green + dark themes
- **Yours** — full export, backup, and restore at any time
- **Zotero-class** — PDF annotation, web bookmarking, collections, and citation management built in

---

## 🖥️ Tech Stack

| Layer | Technology |
|---|---|
| Desktop shell | Tauri (Rust) |
| Frontend | React + Vite |
| Styling | Tailwind CSS |
| Local database | SQLite (via Tauri SQLite plugin) |
| File storage | Local filesystem (user-defined vault folder) |
| Font | Caveat / Kalam (Google Fonts, bundled locally) |
| PDF rendering + annotation | PDF.js + custom annotation layer |
| Drawing canvas | Excalidraw (embedded, offline) |
| Math / LaTeX | KaTeX (bundled) |
| Code highlighting | Shiki (bundled) |
| Graph visualization | D3.js (bundled) |
| Metadata fetch | CrossRef API / arXiv API (one-time, then offline) |

---

## 🎨 Design System

### Themes
- **Forest Green** — light mode with `#2d6a35` accent, soft white glassmorphism surfaces
- **Dark Forest** — deep `#1a1f1a` background, green-tinted glass panels, reduced eye strain

### Glassmorphism
All panels use frosted-glass effect:
```css
/* Light / Forest Green theme */
background: rgba(255, 255, 255, 0.12);
backdrop-filter: blur(14px);
-webkit-backdrop-filter: blur(14px);
border: 1px solid rgba(255, 255, 255, 0.22);
border-radius: 16px;
box-shadow: 0 4px 24px rgba(45, 106, 53, 0.08);

/* Dark Forest theme */
background: rgba(30, 45, 32, 0.45);
backdrop-filter: blur(16px);
-webkit-backdrop-filter: blur(16px);
border: 1px solid rgba(80, 140, 90, 0.18);
border-radius: 16px;
box-shadow: 0 4px 32px rgba(0, 0, 0, 0.35);
```

### Typography
- **UI font**: `Caveat` or `Kalam` — handwriting-style, warm and personal, similar in feel to Ubuntu
- **Code font**: `JetBrains Mono` (bundled locally)
- **Body size**: 15px, line-height 1.75
- All fonts are bundled locally — zero external requests at runtime

---

## 📐 App Layout

```
┌──────────────────────────────────────────────────────────────────┐
│  📗 Folio                                          🌙  ⚙️  👤   │
├───────────────┬──────────────────────────────────────────────────┤
│               │                                                  │
│  LEFT PANEL   │             MAIN CONTENT AREA                   │
│               │                                                  │
│  Dashboard    │   [ Active note / PDF viewer / tracker /        │
│  Papers       │     template / graph / timeline ]               │
│  Collections  │                                                  │
│  Bookmarks    │                                                  │
│  Lit Review   │                                                  │
│  My Notes     │                                                  │
│  Snippets     │                                                  │
│  Graph        │                                                  │
│  Timeline     │                                                  │
│  ───────────  │                                                  │
│  Tags         │                                                  │
│  Settings     │                                                  │
│  Backup       │                                                  │
│               │                                                  │
└───────────────┴──────────────────────────────────────────────────┘
```

---

## 🗂️ Feature Overview

---

### 1. Research Dashboard
The home screen. At a glance:
- Total papers read, in-progress, annotated
- Reading time this week (tracked automatically per paper)
- Recent activity feed (last opened, last annotated)
- Tag cloud showing most active research areas
- Annotation summary (how many highlights, notes made this week)
- Quick-add: paper, note, bookmark, or snippet

---

### 2. PDF Annotation (Zotero-class)

Full annotation support for any PDF imported into Folio — all annotations stored in Folio's database, never modifying the original PDF file.

#### Annotation Tools
| Tool | Description |
|---|---|
| 🟡 Highlight | Select text and highlight in yellow, green, pink, blue, or orange |
| 📝 Sticky note | Add a floating comment anchored to a position on the page |
| ✏️ Underline | Underline selected text |
| ~~S~~ Strikethrough | Strike through selected text |
| 🖊️ Freehand draw | Draw directly on the page (pen tool) |
| 🔲 Area select | Draw a rectangle to capture a figure or diagram as an image crop |
| 🔗 Link to note | Link a highlighted passage to any note in Folio |

#### Annotation Panel
A collapsible side panel shows all annotations for the open PDF:
- Sorted by page number or by date added
- Click any annotation to jump to that page
- Filter by annotation type or color
- Export all annotations as a structured markdown summary

#### Annotation Export
```
Paper: Attention Is All You Need
Exported: 2025-08-12

── Page 3 ──
[Highlight - Yellow]
"The dominant sequence transduction models are based on complex recurrent
or convolutional neural networks"
Note: "This is the gap Folio's graph approach addresses"

── Page 5 ──
[Sticky Note]
"Why softmax here and not sigmoid? Check section 3.2"

── Page 8 ──
[Area Crop] → saved as figure_p8.png
```

---

### 3. Web Bookmarking (Zotero-class)

Capture research from the web without leaving Folio.

#### Bookmark Methods
- **Paste URL** — paste any URL into the Bookmarks section; Folio fetches title, authors, and description (requires a one-time internet connection)
- **Local HTML snapshot** — saves a full offline copy of the page so you can read it without internet
- **Browser extension** (optional, built separately) — one-click save from Chrome/Firefox; sends page data to Folio via localhost
- **Manual entry** — add a bookmark with custom title, URL, notes, and tags

#### Bookmark Fields
| Field | Description |
|---|---|
| Title | Page or article title |
| URL | Original URL |
| Snapshot | Offline HTML copy (optional) |
| Authors | Auto-detected or manually entered |
| Date saved | Timestamp |
| Tags | Research topic tags |
| Notes | Your comments on this source |
| Collection | Which collection this belongs to |
| Status | `to-read` / `reading` / `done` |

---

### 4. Collections (Zotero-class)

Collections are named groups for organizing papers, PDFs, notes, and bookmarks — separate from tags.

- A paper or bookmark can belong to **multiple collections**
- Collections can be **nested** (e.g. `PhD Research > Chapter 2 > Related Works`)
- Create a collection per project, chapter, or research theme
- Filter any view to show only items in a specific collection
- Export an entire collection as a literature review draft or BibTeX file

**Difference from tags**: Tags describe *what something is about*. Collections describe *where something belongs* in your workflow.

---

### 5. Paper Tracker

Track every research paper you encounter:

| Field | Description |
|---|---|
| Title | Paper title |
| Authors | Author list |
| Year | Publication year |
| Venue | Conference / journal (e.g. NeurIPS, CVPR, IEEE) |
| DOI / arXiv ID | Paste to auto-fetch metadata (one-time fetch, then offline) |
| Status | `unread` / `reading` / `done` / `revisit` |
| Tags | Topic tags |
| Collections | Which collections this paper belongs to |
| Notes | Quick notes |
| Rating | 1–5 star personal rating |
| Reading time | Auto-logged when PDF is open |
| Annotation count | How many highlights/notes you've made |

#### Metadata Auto-Fetch
Paste a DOI (e.g. `10.1145/3292500.3330919`) or arXiv ID (e.g. `1706.03762`) and Folio queries the CrossRef or arXiv API once to auto-fill title, authors, abstract, venue, and year. After that initial fetch, Folio works fully offline.

#### BibTeX Support
- **Import** `.bib` files to bulk-add papers
- **Export** any paper or collection as a `.bib` file
- **Citation generator** — output IEEE, ACM, APA, or MLA format with one click
- **Auto-insert** citations in notes using `@cite[bibtex-key]` syntax

---

### 6. Literature Review Template

Structured templates for writing lit reviews:

- **Problem statement** section
- **Research gap** analysis block
- **Related works table** — auto-pulls from Paper Tracker by tag or collection
- **Methodology comparison** table
- **Contribution summary** block
- **References section** — auto-generated from tagged/collected papers

Custom templates can be created, saved, and reused. Export to **PDF** or **DOCX**.

---

### 7. Markdown Editor (Writing Section)

Full-featured markdown editor with live preview.

#### Supported Syntax
```
# Heading 1          →  large heading
## Heading 2
### Heading 3
---                  →  horizontal rule
**bold**             →  bold text
*italic*             →  italic
`code`               →  inline code
```python             →  code block with syntax highlighting
> quote              →  blockquote
- item               →  bullet list
1. item              →  numbered list
[[note name]]        →  internal link to another Folio note
@cite[key]           →  citation from your library
$x = \frac{a}{b}$   →  inline LaTeX (KaTeX)
$$\sum_{i=1}^n$$    →  block LaTeX
```

#### PDF Import in the Writing Section

Import a PDF directly into any note via three modes:

1. **Embed as viewer** — a scrollable, annotatable PDF viewer is inserted inline (PDF.js, fully offline). Annotations sync back to the paper record.
2. **Extract text** — PDF text content is extracted and pasted into the editor as formatted markdown, ready to edit and quote from.
3. **Insert as reference block** — creates a linked reference card showing title, authors, page count, and a local file path link.

**How to import:**
- Click the **📎 Import PDF** button in the editor toolbar, or
- Drag and drop a `.pdf` file directly onto the editor area

All PDF processing is local. No file is uploaded anywhere.

---

### 8. Media & Canvas

- **Drag & drop images** — `.png`, `.jpg`, `.webp`, `.svg` supported; stored in vault folder
- **Clipboard paste** — paste screenshots directly into any note
- **Inline drawing canvas** — Excalidraw embedded per note; sketches saved as `.excalidraw` JSON alongside the note
- **Area crop from PDF** — capture a figure from a PDF page as an image, embed it in a note
- **Image annotation** — draw arrows and labels on any image in a note

---

### 9. Code Snippets Library

- Language tagging (Python, C++, Java, CUDA, Bash, LaTeX, etc.)
- Syntax highlighting via Shiki (offline)
- One-click copy
- Link snippet to a specific paper, note, or collection
- Full-text search across all snippets

---

### 10. Knowledge Graph

Visual map of your research:

- **Nodes** = notes, papers, bookmarks, tags
- **Edges** = manual links (`[[note name]]`) or shared tags/collections
- Click any node to open that item
- Cluster view groups related topics automatically
- Filter by tag, collection, date, or connection depth
- Zoom, pan, and drag nodes to arrange your knowledge map

---

### 11. Research Timeline

Gantt-style personal project tracker:

- Create milestones (Literature Review, Experiments, Writing, Submission, Defense)
- Assign date ranges and completion status
- Link milestones to specific notes, papers, or collections
- Visual progress bar per milestone
- Export as image or PDF for supervisor meetings

---

### 12. Tag System & Search

- Hierarchical tags (e.g. `ml/nlp/transformers`)
- Filter any view by one or multiple tags
- Saved search presets
- Full-text search across notes, papers, snippets, bookmarks, and PDF annotation text
- Search by annotation color, collection, or status

---

## 💾 Backup & Restore

### Manual Backup
Export your entire vault at any time:
- Go to **Settings → Backup → Export Vault**
- Saves as a `.folio` file (structured zip: JSON + media + annotations + settings)
- Store anywhere: USB, external drive, or cloud storage of your choice

### Account Deletion Flow
```
1. Folio prompts: "Would you like a backup before deleting?"
2. User confirms email address and gives explicit consent
3. Folio generates a .folio export file locally
4. File is sent via the system's default mail client (mailto: / MAPI)
   — no external mail server, no data leaves your machine via Folio itself
5. Account and local data are securely deleted
```

### Restore / New Install
```
1. Install Folio on new machine
2. Create a local profile (name + optional PIN)
3. Go to Settings → Restore from Backup
4. Select your .folio file
5. All notes, papers, tags, collections, bookmarks, annotations,
   templates, settings, and media are restored exactly as left
```

### Backup File Format
```
vault_backup_2025-08-12.folio   (renamed .zip internally)
├── db.json                      ← notes, papers, tags, citations, bookmarks
├── annotations.json             ← all PDF annotations with page positions
├── settings.json                ← theme, preferences, templates
├── media/                       ← images, imported PDFs
│   ├── img_001.png
│   └── attention_paper.pdf
├── snapshots/                   ← offline HTML snapshots of bookmarked pages
│   └── page_001.html
└── drawings/                    ← excalidraw canvas files
    └── sketch_001.excalidraw
```

---

## 🔒 Privacy

- **Zero telemetry** — Folio never phones home
- **No accounts required** — the "account" is a local profile with an optional PIN lock
- **No cloud sync** — everything stays on your machine
- **One-time API calls only** — metadata fetch for DOI/arXiv happens once per paper, then cached locally forever
- **Open format** — `.folio` files are standard zips; readable with any zip tool

---

## 🗃️ Project Structure

```
folio/
├── src-tauri/                    ← Tauri (Rust) backend
│   ├── src/
│   │   ├── main.rs
│   │   ├── db.rs                 ← SQLite operations
│   │   ├── backup.rs             ← export / import logic
│   │   ├── pdf.rs                ← PDF text extraction
│   │   ├── annotations.rs        ← annotation storage
│   │   ├── metadata.rs           ← DOI / arXiv fetch + cache
│   │   └── snapshot.rs           ← web page snapshot
│   └── tauri.conf.json
│
├── src/                          ← React frontend
│   ├── components/
│   │   ├── LeftPanel/
│   │   ├── Dashboard/
│   │   ├── Editor/               ← markdown editor (CodeMirror)
│   │   ├── PDFViewer/            ← PDF.js + annotation layer
│   │   ├── AnnotationPanel/      ← annotation sidebar
│   │   ├── PaperTracker/
│   │   ├── Bookmarks/
│   │   ├── Collections/
│   │   ├── LitReview/
│   │   ├── KnowledgeGraph/       ← D3.js graph
│   │   ├── Timeline/
│   │   └── Snippets/
│   ├── styles/
│   │   ├── themes/
│   │   │   ├── forest.css
│   │   │   └── dark-forest.css
│   │   └── glass.css             ← glassmorphism utilities
│   ├── store/                    ← Zustand state management
│   └── main.tsx
│
├── public/
│   └── fonts/                    ← bundled Caveat + JetBrains Mono
│
├── README.md
└── package.json
```

---

## 🚀 Getting Started (Development)

### Prerequisites
- Node.js 18+
- Rust (stable, via rustup)
- Tauri CLI: `cargo install tauri-cli`

### Install & Run
```bash
git clone https://github.com/yourname/folio
cd folio
npm install
npm run tauri dev
```

### Build for Production
```bash
npm run tauri build
# Output: src-tauri/target/release/bundle/
```
Produces a self-contained installer for Windows (.msi), macOS (.dmg), or Linux (.AppImage / .deb).

---

## 📦 Key Dependencies (all bundled, no CDN at runtime)

| Package | Purpose |
|---|---|
| `@tauri-apps/api` | Tauri bridge (Rust ↔ JS) |
| `react` + `vite` | UI framework |
| `tailwindcss` | Utility CSS |
| `zustand` | State management |
| `codemirror` | Markdown editor |
| `pdfjs-dist` | PDF rendering and annotation layer |
| `@excalidraw/excalidraw` | Inline drawing canvas |
| `katex` | LaTeX math rendering |
| `shiki` | Code syntax highlighting |
| `better-sqlite3` | Local SQLite database |
| `d3` | Knowledge graph visualization |
| `bibtex-parse` | BibTeX import / export |

---

## 🗺️ Roadmap

### v1.0 — Core
- [x] Glassmorphism UI — forest green + dark forest themes
- [x] Left panel navigation (Notion-style)
- [x] Markdown editor with live preview + LaTeX + code blocks
- [x] PDF import — embed, extract text, reference block
- [x] Paper tracker with DOI/arXiv metadata fetch
- [x] BibTeX import / export + citation generator
- [x] Image drag & drop + inline drawing canvas (Excalidraw)
- [x] Code snippet library
- [x] Tag system + full-text search
- [x] Backup / restore (.folio format)
- [x] Account deletion with email backup consent flow

### v1.1 — Zotero-class
- [x] PDF annotation — highlight, sticky note, underline, freehand, area crop
- [x] Annotation export as markdown summary
- [x] Web bookmarking with offline HTML snapshots
- [x] Collections (nested, multi-assign)
- [x] Annotation panel sidebar (jump to any annotation)

### v1.2 — Power Features
- [ ] Knowledge graph view (D3.js, click-to-open)
- [ ] Research timeline / Gantt view
- [ ] @cite[ ] auto-insert in editor
- [ ] Literature review auto-build from collection
- [ ] PDF annotation ↔ note linking

### v2.0 — Extended
- [ ] Browser extension (Chrome/Firefox → Folio bookmarking)
- [ ] Mobile companion app (read-only, local Wi-Fi sync)
- [ ] Multi-vault support (separate vaults per project)
- [ ] Collaborative export for supervisor sharing

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

*Folio — your research, your machine, your rules.*
