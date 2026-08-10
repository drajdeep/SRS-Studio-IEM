# SRS Studio 📘

**A free, browser-based Software Requirements Specification (SRS) generator built on the ISO/IEC/IEEE 29148:2018 standard.**

> Made with ❤️ by **Rajdeep Das** (Dept. of CSE-AIML, IEM-UEM Kolkata) for the students of the **Institute of Engineering and Management, Kolkata** — because every existing requirements tool worth using (ReqView, Jama, DOORS, Helix RM…) is paid, and students shouldn't have to pay to learn how to write proper requirements.

**Live demo:** `https://drajdeep.github.io/SRS-Studio-IEM/` 

---

## ✨ Why this exists

Writing an SRS is a core part of every Software Engineering curriculum, and ISO/IEC/IEEE 29148 is *the* international standard for requirements engineering. But:

- Professional tools like **ReqView** cost hundreds of dollars per seat.
- Word templates give no structure, no requirement attributes, no discipline.
- Students end up writing essays instead of verifiable requirements.

SRS Studio gives students a **ReqView-style three-pane editor**, the **full 29148 SRS outline pre-loaded**, and **one-click export** to professional documents — 100% free, no login, no server, no installation.

---

## 🧭 Features in detail

### 1. Standard-compliant document structure
- The complete SRS outline from **ISO/IEC/IEEE 29148:2018, clause 9.6.4** is pre-loaded:
  - **1. Introduction** — Purpose · Scope · Product overview (Perspective, Functions, User characteristics, Limitations) · Definitions
  - **2. References**
  - **3. Specific requirements** — Functions · Performance · Usability · Interfaces · Logical database · Design constraints · Software system attributes · Supporting information
  - **4. Verification**
  - **Appendices** — Assumptions & dependencies · Acronyms
- Every standard section carries an **inline guidance hint** explaining what the standard expects there — the tool teaches while you write.

### 2. Fully editable outline
- ➕ Add new sections and subsections anywhere
- ✏️ Rename any heading inline
- ↑ ↓ Reorder sections
- ⇤ ⇥ Promote / demote heading levels (up to 3 levels deep)
- 🅰️ Toggle **Appendix mode** (switches numbering to A, B, C…)
- ✕ Remove sections you don't need
- **Automatic clause renumbering** — restructure freely, numbers stay correct

### 3. ReqView-style three-pane workspace
| Pane | What it does |
|---|---|
| **Left — Outline tree** | Clause-numbered navigation with live requirement counts per section |
| **Centre — Document** | Section prose, figures, and the requirements table |
| **Right — Attributes** | Full attribute editor for the selected requirement |

### 4. Requirement management
- **Auto-generated unique IDs** (`SRS-001`, `SRS-002`, …) — never reused, even after deletion
- Per-requirement attributes:
  - Short name & requirement statement
  - **Type**: Functional / Performance / Usability / Interface / Database / Design constraint / Quality attribute / Verification (auto-guessed from the section)
  - **Priority**: MoSCoW (Must / Should / Could / Won't)
  - **Verification method**: Test / Inspection / Analysis / Demonstration (the four methods defined by the standard)
  - **Status** workflow: Draft → Reviewed → Approved
  - Rationale and Source/stakeholder traceability fields
- **EARS pattern buttons** — one click inserts the *Easy Approach to Requirements Syntax* templates (Ubiquitous, Event-driven, State-driven, Unwanted-event) so students learn to write well-formed "shall" statements
- Colour-coded chips for verification method and status at a glance

### 5. Images & figures
- Insert any number of images per section (diagrams, screenshots, ER models, use-case diagrams…)
- Automatic **figure numbering** across the whole document
- Editable captions
- Large images are automatically downscaled (max 1400 px wide) to stay within browser storage limits
- Figures are embedded in **every export format**

### 6. Export formats
| Format | How | Notes |
|---|---|---|
| **.docx** | `docx.js` generated client-side | Real Word file: styled headings, cover page with institute logo, requirement tables, embedded figures |
| **.pdf** | Print-optimised view → browser "Save as PDF" | Zero dependencies, pixel-perfect |
| **.html** | Standalone single file | All images embedded as base64 — share or print anywhere |
| **.md** | Markdown | For GitHub repos and wikis |
| **.json** | Native project file | Full-fidelity save/restore, ideal for submissions |

Every export opens with a **title page**: institute banner, project name, document ID, version, date, author(s), organization — per the standard's information items.

### 7. Persistence & data safety
- **Autosave**: every keystroke is saved to the browser's `localStorage` within 0.4 s (the "saved locally · HH:MM:SS" indicator in the toolbar)
- **Save JSON / Open JSON**: portable project files that work across devices and browsers — recommended for backups and submissions
- Automatic **migration** of projects created in older versions of the app
- Storage-full warning if a project with many images exceeds the ~5 MB browser quota

### 8. Zero-infrastructure by design
- **One single `index.html` file** — no build step, no npm install, no backend, no database, no accounts
- All user data stays **on the student's own device**; nothing is ever uploaded anywhere
- Works offline after first load (the only network call is fetching the Word-export library from a CDN, with a graceful `.doc` fallback if offline)

---

## 🛠️ Tech stack

| Layer | Technology | Why |
|---|---|---|
| Markup / styling | **HTML5 + CSS3** (custom properties, flexbox, grid-free layout) | No framework overhead; instant load |
| Logic | **Vanilla JavaScript (ES2020)** | ~1,000 lines, zero dependencies at runtime |
| Typography | **IBM Plex Sans / IBM Plex Mono** (Google Fonts) | Engineering-document character |
| Word export | **[docx](https://github.com/dolanmiu/docx) v8.5** (UMD, loaded on-demand from CDN with 3-level fallback: cdnjs → jsDelivr → unpkg) | True OOXML `.docx` generation fully client-side |
| PDF export | Native **`window.print()`** with a print-optimised stylesheet | No heavy PDF library; best output quality |
| Image handling | **FileReader + Canvas API** | Client-side resize/compress to JPEG/PNG data-URLs |
| Persistence | **localStorage** (autosave) + **Blob/File APIs** (JSON project files) | Fully serverless |
| Hosting | **GitHub Pages** (static) | Free, global CDN, HTTPS |

**Architecture in one sentence:** a single-page application where the entire document is one JSON object (`meta` + ordered `secs[]`, each holding `body`, `reqs[]`, `imgs[]`), rendered reactively into the three panes, and serialised on demand into DOCX / HTML / PDF / Markdown.

---

### Run locally
Download repo and Double-click `index.html`. That's it.

---

## 🎓 Suggested classroom workflow

1. Student opens the shared link, enters the project name and fills **Document info**.
2. Works through the outline top-to-bottom, guided by the built-in hints.
3. Uses **EARS buttons** to phrase requirements; sets priority, verification method, status.
4. Inserts diagrams (use-case, ER, architecture) as figures.
5. **Save JSON** → submits the `.srs.json` file (perfect for versioned feedback) **plus** the exported **.docx/.pdf**.
6. Instructor opens the JSON in the same app to review, or reads the exported document.

---

## ⚠️ Known limitations

- Autosave is **per-browser, per-device** — students must use *Save JSON* to move between machines.
- Browser storage caps a project at roughly **5 MB** (mainly a concern with many large images); the JSON export has no such limit.
- The `.docx` export needs an internet connection the first time (CDN library); offline it falls back to a Word-compatible `.doc`.
- Rich-text formatting (bold/italics inside prose) is not supported — the standard favours plain, unambiguous prose anyway.

---

## 🗺️ Roadmap ideas

- Requirement quality checker (flag missing "shall", vague words like *fast*, *user-friendly*, *etc.*)
- Traceability matrix view & export
- Requirement-to-requirement links (derives / satisfies / conflicts)
- Dark mode

Pull requests and issues are welcome!

---

## 👨‍💻 Developer

**Rajdeep Das**
Department of CSE-AIML
Institute of Engineering and Management (IEM), Salt Lake, Kolkata
*A constituent institute of the University of Engineering and Management (UEM), Kolkata*

Built free and open-source so that no IEM student ever needs a paid licence just to write a proper SRS.

---

## 📄 License

Released under the **MIT License** — see [LICENSE](LICENSE). Free to use, modify, and redistribute, including for teaching at other institutions.

## 🙏 Acknowledgements

- **ISO/IEC/IEEE 29148:2018** — *Systems and software engineering — Life cycle processes — Requirements engineering* (document structure and guidance)
- **EARS** — Easy Approach to Requirements Syntax (Alistair Mavin et al.)
- **ReqView** — UI inspiration for the three-pane requirements workspace
- **[docx](https://github.com/dolanmiu/docx)** by Dolan Miu — client-side Word generation
