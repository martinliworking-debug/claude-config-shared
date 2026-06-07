---
name: documents
description: Use when working with complex file formats including Word documents (.docx), PDFs (.pdf), PowerPoint presentations (.pptx), and Excel spreadsheets (.xlsx). Handles reading, summarising, extracting data, converting, and analysing these file types.
---

You are helping the user work with document files. Follow these guidelines based on file type:

## PDF (.pdf)
- Use the Read tool directly — Claude Code supports PDF reading natively
- For large PDFs (more than 10 pages), use the `pages` parameter to read specific page ranges (e.g., pages: "1-5")
- Maximum 20 pages per Read call — chunk large documents accordingly
- Extract text, tables, and structure as needed

## Word (.docx)
- Use the Bash tool with Python to extract content:
  ```bash
  python -c "import docx; doc=docx.Document('file.docx'); [print(p.text) for p in doc.paragraphs]"
  ```
- If python-docx is not installed: `pip install python-docx`
- For tables: iterate `doc.tables` and access `table.rows[i].cells[j].text`

## Excel (.xlsx)
- Use the Bash tool with Python and pandas or openpyxl:
  ```bash
  python -c "import pandas as pd; df=pd.read_excel('file.xlsx', sheet_name=None); print(df)"
  ```
- If not installed: `pip install pandas openpyxl`
- For multiple sheets use `sheet_name=None` to load all sheets as a dict
- For large files, read specific columns or rows to avoid memory issues

## PowerPoint (.pptx)
- Use the Bash tool with Python and python-pptx:
  ```bash
  python -c "from pptx import Presentation; prs=Presentation('file.pptx'); [print(shape.text) for slide in prs.slides for shape in slide.shapes if hasattr(shape, 'text')]"
  ```
- If not installed: `pip install python-pptx`
- Access slide notes via `slide.notes_slide.notes_text_frame.text`
- Access tables via `shape.table` when `shape.has_table` is True

## General approach
1. Identify the file type from the extension
2. Check if required Python libraries are installed before using them
3. For analysis tasks: extract content first, then analyse
4. For large files: summarise section by section
5. Always show the user extracted content before drawing conclusions
6. If the user wants to convert formats, use the appropriate library to write output

## Common tasks
- **Summarise**: Extract all text, then provide a structured summary
- **Extract tables**: Use pandas for Excel, iterate `.tables` for Word/PowerPoint
- **Compare documents**: Read both files, then diff content
- **Find specific info**: Extract full text first, then search/filter

$ARGUMENTS
