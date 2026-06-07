## Verification & Honesty

- NEVER fabricate figures, data, references, or any source-specific values. If a value isn't in the source, explicitly say 'not found in source' and ask.

## Scoping & Approach

- For document/spreadsheet revisions, ASK before adding charts, new tabs, or 'enhancements' that weren't requested. Prefer minimal edits over rebuilds.
- Start summaries at the high-level module/workflow level, not script-by-script, unless detail is requested.

## Document Handling

- When the user references a `.pdf`, `.docx`, `.xlsx`, `.pptx`, `.doc`, `.xls`, or `.ppt` file, automatically use the `/documents` skill to read, summarise, or extract data from it.
- When the user asks to convert any document, webpage, image, or file to Markdown, automatically use the `/markitdown` skill.
- Do not attempt to read Office or PDF files with the Read tool, always route through `/documents` or `/markitdown` instead.

## Excel/Office File Handling

- ALWAYS check if Excel/Word files are open before attempting writes; if locked, save with a versioned filename (e.g., `_v02.xlsx`) and inform the user.
- NEVER use Excel COM automation for refresh/recalc, it causes hangs. Use openpyxl directly or write formulas that recalc on open.

## Charts — always native, never PNG

- ALL charts in deliverables should be native chart objects, not PNG/image inserts. This applies to `.xlsx`, `.docx`, and `.pptx` outputs.
- In `.xlsx`: use openpyxl's chart classes (BarChart, LineChart, AreaChart, PieChart, etc.) — never `add_image` with a matplotlib PNG.
- In `.pptx`: use python-pptx's native chart API (`shapes.add_chart`), not images.
- Rationale: native charts are editable, recolourable, theme-aware, and update with the data. PNGs become stale and read as AI-generated.
- Hard exception: only when the user explicitly asks for an image/PNG output.
