# Claude Code shared config

A general-purpose Claude Code config: 9 skills, a starter `CLAUDE.md`, sensible `settings.json`, and a multi-line statusline. No personal or work-specific content.

## What's included

**Skills** (`skills/`)
- `brainstorming` — structured creative ideation
- `documents` — read/summarise/extract from `.docx`, `.pdf`, `.pptx`, `.xlsx`
- `markitdown` — convert files / webpages / images / audio to Markdown
- `planning-with-files` — create and work from file-based plans
- `skill-creator` — create a new skill
- `skill-vetter` — review/improve an existing skill
- `stop-slop` — strip AI writing tells from prose
- `theme-factory` — style artifacts with preset/custom themes
- `find-skills` — list/browse installed skills

**Other**
- `CLAUDE.md` — general working preferences (honesty, scoping, document/chart handling)
- `settings.json` — high effort level, `auto` permission mode, Playwright + Chrome MCP servers, the karpathy-guidelines plugin, and a statusline
- `statusline.sh` — multi-line statusline (dir, model, skill/MCP count, context %, rate limits)

## Install

1. Back up any existing config: rename `~/.claude` to `~/.claude.bak` (or copy individual files you want to keep).
2. Clone into place:
   ```bash
   git clone https://github.com/<your-username>/claude-config-shared.git ~/.claude
   ```
   If `~/.claude` already exists, clone elsewhere and copy `skills/`, `CLAUDE.md`, `settings.json`, and `statusline.sh` into it.
3. Restart Claude Code.

## Notes

- The statusline tries an Anaconda Python path first, then falls back to `python3` / `python`. It needs `bash` available (Git Bash on Windows).
- MCP servers (Playwright, Chrome) install on first use via `npx`; remove them from `settings.json` if you don't want them.
- `effortLevel`, the theme, and the model are personal preferences — adjust to taste.
