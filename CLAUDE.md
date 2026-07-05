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

## Orchestration Workflow (optional, machine-wide)

- These rules bind the top-level session only. If you are a subagent executing a delegated spec, ignore this section and execute your spec directly.
- The top-level session is the orchestrator: planning, task decomposition, and synthesising solutions. The orchestrator does not implement or write code — it delegates.
- Delegation map: reasoning-intensive analysis/architecture → `deep-reasoner` agent (Opus); mechanical implementation, boilerplate, refactoring, testing → `fast-worker` agent (Sonnet); read-only search/recon → Explore agent. For the hardest problems, call deep-reasoner with a per-call model override set to your strongest available model (the per-call model parameter takes precedence over the agent file's model; use sparingly — it is the most expensive tier).
- Delegation specs are self-contained (subagents do not see the conversation) and state: task and context, acceptance criteria ("run X, expect Y"), any skills or scripts the agent must apply by name, and the required return format. Batch small edits into one fast-worker call rather than one spin-up per edit.
- Verify fast-worker output by executing it, not by reading the diff. Treat deep-reasoner conclusions as claims, not facts; spot-check the load-bearing ones.
- Codex (`codex@openai-codex` plugin) is an independent peer from a different training lineage, not a reviewer of Claude output: `/codex:rescue --background <task>`, then collect with `/codex:status` and `/codex:result` (background jobs do not announce completion). Keep Codex read-only unless it is the sole writer on an isolated copy.
- High-stakes decisions (architecture choices with long-lived consequences, irreversible or client-facing changes, a bug that survived two failed fixes): blind parallel opinions. Write one neutral spec, note your own position first, send it identically to deep-reasoner (strongest-model override) and `/codex:rescue --background` in the same turn — neither sees the other's answer, or yours. Synthesise by adopting the stronger answer as the base and grafting improvements from the other; investigate disagreements before choosing. Not for routine work: it doubles cost by design.
- Keep the orchestrator's context lean: reads beyond a few files go through subagents; require structured returns (findings, file:line references, a recommendation); long-running work goes to the background. Orchestrator turns are for planning, decomposition, verification, synthesis.
- To opt out: delete this section, delete the `agents/` folder, and remove the `openai-codex` marketplace and `codex@openai-codex` plugin entries from `settings.json`.
