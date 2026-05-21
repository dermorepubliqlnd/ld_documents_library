# Dermorepubliq Document Library — Cowork Project Reference
**Prepared for:** Sandy | L&D Director, Dermorepubliq Corporation  
**Purpose:** Context file for Cowork project — document-to-HTML batch conversion  
**Last updated:** May 2026

---

## 1. Project Overview

This project converts Dermorepubliq's official corporate documents (Policies, SOPs, Work Instructions, Guidelines, Test Methods) into branded HTML files and organizes them in a Document Library portal.

**Deliverables:**
- Individual branded HTML files per document
- A Document Library portal (`dermorepubliq-document-library.html`) that houses all converted documents with navigation, search, and PDF links

---

## 2. Brand Specifications

### Typography
- **Font:** Poppins (Google Fonts)
- **Import line:** `<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet"/>`
- **Fallback:** sans-serif

### Color Palette

#### Identity Colors (Base)
| Name | Hex | Usage |
|---|---|---|
| Clinical White | `#F7F6F3` | Page background |
| Soft Linen | `#DBD3C9` | Borders, dividers |
| Stone Taupe | `#989187` | Muted text, labels |
| Warm Mahogany | `#5f5144` | Secondary text |
| Dermo Onyx | `#121212` | Primary text, headings |

#### Document Type Accent Colors
| Document Type | Accent Color | Light Tint | Button Text |
|---|---|---|---|
| Policy | `#121212` | `#EBEBEB` | `#ffffff` |
| SOP | `#83AA6E` | `#EAF2E5` | `#ffffff` |
| Work Instructions | `#E18B64` | `#FBF0E9` | `#ffffff` |
| Guidelines | `#7843A6` | `#F0E8F8` | `#ffffff` |
| Test Method | `#F1D53D` | `#FDFBE8` | `#121212` |

> **Note:** Test Method uses dark text (`#121212`) on the yellow button because the yellow background has low contrast with white text.

---

## 3. Document Type Identification Rules

When converting documents, identify the document type from:
1. The **filename** (e.g., `DR-SOP-`, `DR-POL-`, `DR-WI-`, `DR-GDL-`, `DR-TM-`)
2. The **document header** or title block inside the document
3. The **content structure** if neither above is available

### Document ID Naming Convention
| Type | ID Prefix | Example |
|---|---|---|
| Policy | `DR-POL-[DEPT]-[###]` | DR-POL-L&D-001 |
| SOP | `DR-SOP-[DEPT]-[###]` | DR-SOP-L&D-001 |
| Work Instructions | `DR-WI-[DEPT]-[###]` | DR-WI-L&D-001 |
| Guidelines | `DR-GDL-[DEPT]-[###]` | DR-GDL-L&D-001 |
| Test Method | `DR-TM-[DEPT]-[###]` | DR-TM-QA-001 |

---

## 4. HTML Document Template Structure

Each converted document must follow this structure:

```
[Left accent bar — 6px, document type color]
[Header: Logo + doc type badge | Document ID, version, date]
[Title block: Doc type label + Document title — light tint background]
[Info grid: Department | Document Owner | Responsible Dept | Classification]
[Body sections: numbered, each with colored section header bar]
  - Section 1: Purpose
  - Section 2: Scope
  - Section 3: Procedure / Content (with numbered steps)
  - Section 4: Definitions (if present)
  - Section 5: References (if present)
  - Section 6: Confirmation and Signatures
[Footer: Doc ID + Version + Date | Page number + Uncontrolled when printed]
```

### Key HTML/CSS Rules
- Use CSS variables for all brand colors (see template file)
- `--doc-accent` controls all color elements for the active document type
- `--doc-accent-light` is used for section title backgrounds and callout boxes
- Signature table header row uses `--doc-accent` as background
- Section number squares use `--doc-accent` as background
- Test Method: all colored elements use dark text (`#121212`) not white

### Callout Box (for notes/warnings)
```html
<div class="callout">
  <div class="callout-icon">📌</div>
  <div class="callout-text">
    <strong>Note Title</strong>
    Note content here.
  </div>
</div>
```

---

## 5. Cowork Batch Conversion Prompt

Use this prompt when running the batch conversion task in Cowork:

---

> You are converting Dermorepubliq corporate documents into branded HTML files.
>
> **Reference files in this project folder:**
> - `dermorepubliq-sop-template.html` — the master branded HTML template
> - `dermorepubliq-doclibrary-cowork-reference.md` — this reference document with all brand specs
>
> **For each Word (.docx) or PDF document in the Source Documents folder:**
>
> 1. Read the full document content
> 2. Identify the document type from the filename or document header:
>    - Prefix `DR-POL-` or content says "Policy" → **Policy** (accent: #121212)
>    - Prefix `DR-SOP-` or content says "Standard Operating Procedure" → **SOP** (accent: #83AA6E)
>    - Prefix `DR-WI-` or content says "Work Instructions" → **Work Instructions** (accent: #E18B64)
>    - Prefix `DR-GDL-` or content says "Guidelines" → **Guidelines** (accent: #7843A6)
>    - Prefix `DR-TM-` or content says "Test Method" → **Test Method** (accent: #F1D53D, dark button text)
> 3. Convert the document to a branded HTML file using `dermorepubliq-sop-template.html` as the base
> 4. Apply the correct accent color for the document type throughout (accent bar, badges, section numbers, table headers, callouts)
> 5. Preserve all original content: section headings, numbered steps, tables, definitions, references, and signature blocks
> 6. Save the output HTML file using the same base filename with `.html` extension into the **HTML Output** folder
> 7. After completing all files, provide a summary table:
>    - File name
>    - Document type detected
>    - Status (converted / skipped / needs review)
>    - Any issues flagged (missing sections, unclear doc type, formatting problems)
>
> **Do a test run on 2–3 documents first and show me the output before proceeding with the full batch.**

---

## 6. Document Library Portal

The file `dermorepubliq-document-library.html` is the main library portal.

### How to Add a New Document

Open the file and find the `documents` array in the `<script>` section. Add a new entry:

```javascript
{
  id: 'DR-SOP-L&D-004',            // Document ID
  title: 'Your Document Title',     // Full document title
  type: 'SOP',                      // Policy | SOP | WI | Guidelines | Test Method
  version: '1.0',                   // Version number
  date: 'May 2026',                 // Effective or published date
  department: 'L&D',               // Owning department
  htmlUrl: 'filename.html',         // Path to the HTML version
  pdfUrl: 'filename.pdf',           // Path to the source PDF
},
```

### Document Types Reference (for the `type` field)
Use exactly these values: `Policy` | `SOP` | `WI` | `Guidelines` | `Test Method`

---

## 7. Folder Structure (Recommended)

```
/Dermorepubliq Document Library/
│
├── dermorepubliq-document-library.html   ← Main portal (open this in browser)
│
├── /documents/                            ← All converted HTML files go here
│   ├── DR-POL-LD-001.html
│   ├── DR-SOP-LD-001.html
│   └── ...
│
├── /source/                               ← Original Word/PDF source files
│   ├── DR-POL-LD-001.docx
│   ├── DR-SOP-LD-001.pdf
│   └── ...
│
└── /assets/                               ← Reference files for Cowork
    ├── dermorepubliq-sop-template.html
    └── dermorepubliq-doclibrary-cowork-reference.md
```

---

## 8. Files in This Project

| File | Purpose |
|---|---|
| `dermorepubliq-sop-template.html` | Master branded HTML template with doc type switcher |
| `dermorepubliq-document-library.html` | Document Library portal with nav, search, and cards |
| `dermorepubliq-doclibrary-cowork-reference.md` | This reference document (upload to Cowork as context) |

---

## 9. Contact & Ownership

| Role | Name |
|---|---|
| Document Library Owner | Sandy — L&D Director |
| L&D Supervisor | Joseph |
| Approving Officer | Brad (COO) |

---

*This reference document was prepared to support the Cowork batch conversion project for the Dermorepubliq Document Library. Upload this file as a context document in your Cowork Project so Claude has the full brief without re-explanation.*
