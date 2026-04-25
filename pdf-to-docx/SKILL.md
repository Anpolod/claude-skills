---
name: pdf-to-docx
description: >
  Converts a PDF file into a fully formatted, professional Word document (.docx).
  Use this skill ANY TIME the user uploads or references a PDF and asks to:
  - "convert to Word", "convert to doc", "make a docx", "переделай в документ"
  - "reformat as a document", "turn into a Word file"
  - edit/fill/update content that's currently locked in a PDF
  Handles all PDF types: CVs/resumes, contracts, reports, forms, tables, invoices, certificates.
  Always triggers when the input is a .pdf and the output should be .docx.
---

# PDF → DOCX Conversion Skill

## Overview

Convert any PDF into a clean, editable Word document while preserving structure, tables, and formatting. The output should be a professional .docx that faithfully reproduces the PDF's content and visual organization.

## Algorithm (follow in this exact order)

### Step 1 — Extract text from the PDF

Use `pdfplumber` (Python). It preserves table structure and row alignment better than `pypdf`.

```bash
pip install pdfplumber --break-system-packages -q
```

```python
import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    for i, page in enumerate(pdf.pages):
        print(f"=== PAGE {i+1} ===")
        print(page.extract_text())
        # extract tables separately if present:
        for table in page.extract_tables():
            print(table)
```

**Edge cases:**
- If `extract_text()` returns empty or garbled text → the PDF is likely scanned (image-based). Use OCR: `pytesseract` + `pdf2image`. Install: `pip install pytesseract pdf2image --break-system-packages`
- If columns are merging into one line → try `page.extract_text(layout=True)` for layout-aware extraction

### Step 2 — Analyze the document structure

Before writing any code, mentally map the document into sections. Ask yourself:
- What are the top-level sections? (e.g., "Personal Info", "Experience", "Certificates")
- Which sections contain tables vs. key-value pairs vs. plain prose?
- What is the visual hierarchy? (title → section headers → body)

This analysis determines your DOCX structure. Don't skip it — a clear mental model of the document means the output will feel native, not like a raw text dump.

### Step 3 — Set up the Node.js docx environment

Create a local project folder and install the `docx` library:

```bash
mkdir -p /tmp/docx_work && cd /tmp/docx_work && npm install docx
```

Then write your generation script at `/tmp/docx_work/create_doc.js`.

**Always start your script with this import block:**

```javascript
const {
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
  AlignmentType, HeadingLevel, BorderStyle, WidthType, ShadingType,
  VerticalAlign, LevelFormat, PageBreak, ExternalHyperlink
} = require('docx');
const fs = require('fs');
```

### Step 4 — Build the document

#### Page setup (always explicit — never rely on defaults)

```javascript
sections: [{
  properties: {
    page: {
      size: { width: 12240, height: 15840 },    // US Letter (1440 DXA = 1 inch)
      margin: { top: 1000, right: 1000, bottom: 1000, left: 1000 }
    }
  },
  children: [ /* all content */ ]
}]
```

Use A4 (width: 11906, height: 16838) if the PDF is clearly A4 (European documents, metric measurements).

#### Typography baseline

```javascript
styles: {
  default: {
    document: { run: { font: "Arial", size: 20 } }  // 10pt = 20 half-points
  }
}
```

#### Section headers — use this pattern for visual separation

```javascript
new Paragraph({
  spacing: { before: 240, after: 80 },
  border: { bottom: { style: BorderStyle.SINGLE, size: 6, color: "1F5C8B", space: 4 } },
  children: [new TextRun({ text: "SECTION NAME", bold: true, size: 24, color: "1F5C8B", font: "Arial" })]
})
```

#### Key-value pairs (personal info, metadata)

```javascript
new Paragraph({
  spacing: { before: 40, after: 40 },
  children: [
    new TextRun({ text: "Label: ", bold: true, size: 20, font: "Arial" }),
    new TextRun({ text: "value here", size: 20, font: "Arial" })
  ]
})
```

#### Tables — the most critical section, read carefully

Tables require **dual width specification** — on the table AND on every cell. Without both, rendering breaks in Word and Google Docs.

```javascript
// Content width for US Letter with 1000 DXA margins on each side:
// 12240 - 1000 - 1000 = 10240 DXA

const border = { style: BorderStyle.SINGLE, size: 1, color: "CCCCCC" };
const borders = { top: border, bottom: border, left: border, right: border };

new Table({
  width: { size: 10240, type: WidthType.DXA },   // total table width
  columnWidths: [3000, 3500, 3740],               // MUST sum to 10240
  rows: [
    new TableRow({
      tableHeader: true,
      children: cols.map((col, i) =>
        new TableCell({
          width: { size: columnWidths[i], type: WidthType.DXA },  // repeat per cell
          margins: { top: 60, bottom: 60, left: 100, right: 100 },
          shading: { fill: "1F5C8B", type: ShadingType.CLEAR },   // CLEAR not SOLID!
          borders,
          children: [new Paragraph({ children: [new TextRun({ text: col, bold: true, color: "FFFFFF", font: "Arial", size: 18 })] })]
        })
      )
    }),
    // data rows...
  ]
})
```

**Table rules (all critical):**
- `WidthType.DXA` always — never `WidthType.PERCENTAGE` (breaks Google Docs)
- `columnWidths` array must sum exactly to the table width
- Each cell's `width.size` must match its corresponding `columnWidths` entry
- `ShadingType.CLEAR` for colored backgrounds — `ShadingType.SOLID` produces black
- Always add `margins` to cells for readable padding
- Never use a Table as a horizontal rule or divider — use a Paragraph border instead

#### Lists — never use unicode bullet characters manually

```javascript
// In the Document root:
numbering: {
  config: [
    { reference: "bullets",
      levels: [{ level: 0, format: LevelFormat.BULLET, text: "•", alignment: AlignmentType.LEFT,
        style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] }
  ]
}

// In content:
new Paragraph({
  numbering: { reference: "bullets", level: 0 },
  children: [new TextRun({ text: "item text", font: "Arial", size: 20 })]
})
```

#### Line breaks — never use `\n` inside TextRun

Wrong: `new TextRun({ text: "line1\nline2" })`  
Right: two separate `Paragraph` elements.

### Step 5 — Run the script and validate

```bash
cd /tmp/docx_work && node create_doc.js
```

Then validate the output using the bundled validator:

```bash
# The validator is bundled inside this skill's scripts/ folder.
# SKILL_DIR is wherever this skill is installed:
#   globally:  ~/.claude/skills/pdf-to-docx
#   in project: .claude/skills/pdf-to-docx
python ~/.claude/skills/pdf-to-docx/scripts/validate.py output.docx
# or, if installed locally in the project:
python .claude/skills/pdf-to-docx/scripts/validate.py output.docx
```

If validation fails, the error message will point to the specific XML issue. Common fixes:
- Mismatched column widths → recount and ensure they sum correctly
- Missing `xml:space` → validator usually auto-repairs this
- Invalid shading → switch `ShadingType.SOLID` to `ShadingType.CLEAR`

### Step 6 — Save to outputs

```bash
cp /tmp/docx_work/output.docx /path/to/outputs/filename.docx
```

Use a descriptive filename matching the original PDF name (e.g., `aleksa_cv.pdf` → `aleksa_cv.docx`).

---

## Design Guidelines

**Reproduce the PDF's intent, not just its text.** A CV should look like a CV. A contract should look like a contract. Use visual hierarchy (title → section headers → body) to match what the PDF communicates.

**Color scheme:** Pick one accent color from the PDF if it has one, otherwise default to `"1F5C8B"` (professional dark blue) for headers and table headers. Keep body text `"000000"` or `"333333"`.

**Font:** Arial is universally supported across Word, Google Docs, LibreOffice. Use it as the default. Only deviate if the PDF clearly uses a different font and it's important to preserve.

**Tables in PDFs often have misaligned columns** due to pdfplumber's text extraction. When you see columns merging or splitting unexpectedly in the raw text, cross-reference with the PDF visually and hardcode the correct column structure.

**Wide tables** (6+ columns) should use smaller font sizes (16 half-points = 8pt) and tighter cell margins to fit on a page. Calculate: if 10240 DXA ÷ number of columns < 1000 DXA per column, reduce font size.

---

## Quick Reference — Units

| Unit | Value |
|------|-------|
| 1 inch | 1440 DXA |
| 1 cm | ~567 DXA |
| US Letter width | 12240 DXA |
| US Letter height | 15840 DXA |
| A4 width | 11906 DXA |
| A4 height | 16838 DXA |
| 10pt font | 20 half-points (size: 20) |
| 12pt font | 24 half-points (size: 24) |
| 1 inch margin | 1440 DXA |

---

## Common Document Types and Their Patterns

**CV / Resume**
- Title block: Name (large, colored) + Position (smaller, muted)
- Sections: Personal Info (key-value), Certificates (table), Sea/Work Service (table), Education
- Service tables often have 8-9 columns → use small font (size: 16-18), tight margins

**Contract / Agreement**
- Preserve all clause numbering exactly
- Use `Paragraph` with `numbering` config for numbered clauses
- Bold defined terms on first use

**Invoice / Financial document**
- Line items as table with right-aligned numbers
- Total row with bold, possibly shaded background (`ShadingType.CLEAR` + fill color)

**Report / Technical document**
- Use `HeadingLevel.HEADING_1` / `HEADING_2` for navigation
- Preserve all figure captions and footnotes

---

## Dependencies

- Python: `pdfplumber` (text/table extraction), `pytesseract` + `pdf2image` (OCR fallback)
- Node.js: `docx` package (`npm install docx` locally)
- Validator: `scripts/office/validate.py` from the `docx` skill
