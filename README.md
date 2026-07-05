# Claude Code shared config

A general-purpose Claude Code config to onboard from: 9 skills, a starter `CLAUDE.md`, sensible `settings.json`, a multi-line statusline, and an optional multi-model orchestration workflow (Claude orchestrates; subagents and OpenAI Codex do the work). No personal or work-specific content.

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

**Agents** (`agents/`)
- `deep-reasoner` (Opus) — reasoning-intensive phases: analysis, architecture, planning, difficult problems; returns concise conclusions
- `fast-worker` (Sonnet) — mechanical implementation: code to spec, boilerplate, refactoring, testing; verifies its own work

**Other**
- `CLAUDE.md` — general working preferences (honesty, scoping, document/chart handling) plus the optional orchestration workflow contract
- `settings.json` — high effort level, `auto` permission mode, Playwright + Chrome MCP servers, the karpathy-guidelines and codex plugins, and a statusline
- `statusline.sh` — multi-line statusline (dir, model, skill/MCP count, context %, rate limits)

## Install

1. Back up any existing config: rename `~/.claude` to `~/.claude.bak` (or copy individual files you want to keep).
2. Clone into place:
   ```bash
   git clone https://github.com/martinliworking-debug/claude-config-shared.git ~/.claude
   ```
   If `~/.claude` already exists, clone elsewhere and copy `skills/`, `agents/`, `CLAUDE.md`, `settings.json`, and `statusline.sh` into it.
3. (Only if you want the orchestration workflow) install and authenticate the OpenAI Codex CLI:
   ```bash
   npm install -g @openai/codex
   codex login          # ChatGPT login in the browser
   codex login status   # confirm
   ```
4. Restart Claude Code. The codex and karpathy plugins auto-install on first launch from `settings.json`. Verify with `/agents` (should list `deep-reasoner` and `fast-worker`).

## The orchestration workflow (optional)

Defined in `CLAUDE.md` under "Orchestration Workflow". In plain terms:

- The main session only plans, decomposes, and synthesises. It delegates analysis to `deep-reasoner` and implementation to `fast-worker`, then verifies the result by running it.
- OpenAI Codex acts as an independent second opinion from a different model lineage: `/codex:rescue --background <problem>`, collect later with `/codex:status` and `/codex:result`.
- For big decisions, both `deep-reasoner` and Codex get the identical question in parallel without seeing each other's answers; the orchestrator synthesises the best of both and investigates any disagreement.

You just type tasks normally — the CLAUDE.md rules make the session orchestrate instead of doing everything itself.

**Requirements:** works best when your Claude plan/account has access to a top-tier model for the main session and Opus/Sonnet for the agents; the Codex side needs a ChatGPT account. Without Codex, everything except the `/codex:*` commands still works.

**Opting out:** delete the "Orchestration Workflow" section from `CLAUDE.md`, delete `agents/`, and remove the `openai-codex` marketplace and `codex@openai-codex` plugin entries from `settings.json`.

## Notes

- The statusline tries an Anaconda Python path first, then falls back to `python3` / `python`. It needs `bash` available (Git Bash on Windows).
- MCP servers (Playwright, Chrome) install on first use via `npx`; remove them from `settings.json` if you don't want them.
- `effortLevel`, the theme, and the model are personal preferences — adjust to taste. Pin a model with `"model"` in `settings.json` if you want the same one every session.
- The codex plugin registers an optional turn-end review gate (a Stop hook). It is off by default (`stopReviewGate: false` in the plugin's state) — leave it off unless you want every turn-end blocked on a Codex review.
