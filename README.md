# Project Memory Kit

A lightweight, drop-in **dev-time memory system** for building projects with an AI coding agent (Claude Code, Cursor, Codex, …). One thin, always-loaded `PROJECT.md` control plane plus on-demand `STATE` / `DECISIONS` / `JOURNAL` / `specs` surfaces — so your agent resumes cold without re-reading everything, and your project notes don't bloat or rot. Works for anything with a repo, not just software: an app, a markdown knowledge base, a research pipeline.

This repo *is* an agent skill: install it once, then in any repo it either **installs** the kit (pre-filled from what your repo already tells it) or **upgrades** an existing memory setup (a bloated `PROJECT.md`, a Cline Memory Bank, scattered docs…) into the leaner shape — without losing content. Don't use an agent? The templates live in `assets/templates/` and work by hand too.

## Why

AI-assisted projects tend to fail their own memory in one of two ways:

- **Bloat** — a giant always-loaded `PROJECT.md`/`CLAUDE.md` where 80% is an append-only decisions log. Every session pays for context it doesn't need.
- **Rot** — stale architecture maps, checked-in file trees, abandoned status notes. The memory drifts from reality until nobody trusts it.

The kit cures both with three rules: **just-in-time loading** (read only the surface a task needs), **volatility separation** (the churning cursor lives apart from the stable spine), and **supersede-don't-append** (decisions get replaced, not piled up). The whole governing protocol ships *inside* the generated `PROJECT.md`, so a repo using the kit is self-explaining — no dependency on this repo or any tool.

## What's in the box

```
project-memory-kit/            the skill — drop this whole folder into your skills directory
├── SKILL.md                   the workflow: scan a repo → install or upgrade its memory
├── references/
│   ├── diagnosis.md           detect & score an existing memory setup
│   └── migration.md           recipes for migrating known systems into the kit
└── assets/templates/          the memory templates (used by the skill, or by hand)
    ├── PROJECT.md             the always-loaded spine (embeds the Dev Memory Protocol)
    ├── STATE.md               the live cursor — current task / next / blockers
    ├── DECISIONS.md           supersede-don't-rewrite decision log
    ├── JOURNAL.md             problem → dead-end → fix → lesson
    └── specs/_template.md     per-feature spec + acceptance criteria
```

## Quick start

The skill follows the open `SKILL.md` standard, so it runs in any skill-aware agent.

**Install — pick one:**

- **`npx skills`** (recommended) — the [cross-agent installer](https://github.com/vercel-labs/skills); it drops the skill into the right place for whichever agent you use (Claude Code, Cursor, Codex, Copilot, Gemini CLI, …):
  ```bash
  npx skills add vibecodoor/project-memory-kit -g
  ```
  `-g` installs it globally for your user (all projects); drop it to scope to the current project.

- **Ask your agent** (no terminal needed) — paste this to Claude Code or Codex:
  > Install the project-memory-kit skill from `https://github.com/vibecodoor/project-memory-kit` into my skills directory, then reload.

- **Manual** (no Node needed) — clone into your agent's skills directory (`~/.claude/skills/` for Claude Code, `~/.agents/skills/` for Codex; use the project-local `.claude/skills/` etc. to scope to one repo):
  ```bash
  git clone https://github.com/vibecodoor/project-memory-kit ~/.claude/skills/project-memory-kit
  ```

> Start a new session to pick up the skill (in Claude Code you can also run `/reload-plugins`).

**Use it** — in any repo, just ask your agent:

- *"set up project memory for this repo"* — or — *"my PROJECT.md is bloated, clean it up"*

The skill scans the repo, then either installs the kit (pre-filled) or proposes a migration plan from your existing setup. It always shows the plan before writing, and archives originals rather than deleting them.

### Prefer to do it by hand?

You don't need the skill at all. Create a `devkit/` folder at your repo root (so the memory files don't mix with your code/docs), copy `assets/templates/PROJECT.md` into it, fill in the `{{placeholders}}`, and leave the `## Dev Memory Protocol` block at the bottom **as-is** — that block is the manual. Then add a one-line pointer to your root `CLAUDE.md` / `AGENTS.md` (`Dev memory lives in devkit/ — read devkit/PROJECT.md first`) so a fresh agent session finds the kit, since it auto-loads those root files, not `devkit/PROJECT.md` by name. Keep `## State` inline at first; you're running on a single file. Let the other surfaces be *born on triggers*, never pre-created:

- `STATE.md` → the first time you ask "where was I?" (work spans sessions)
- `DECISIONS.md` → on your 3rd recorded decision
- `JOURNAL.md` → on the first non-trivial problem worth not repeating
- `specs/<slug>.md` → when a feature outlives a single session (copy `specs/_template.md`)

An empty surface is rot on day one — don't create one until it has something to hold.

## The core idea: earned growth

Default end-state for a small project is **one file** (`PROJECT.md` with an inline State section and a short Decisions digest). A surface splits out only under real pressure:

- **Unbounded growth** — the full decisions log / journal would bloat the spine → extract it, keep a digest inline.
- **Volatility** — a section that rewrites every session would dirty the cache-stable spine → extract the cursor to `STATE.md`.

A surface with neither pressure stays merged. Structure is *permission to grow, not an obligation*.

## Adapting it to your project

- The templates are deliberately generic. Fill `Project Card` / `Stack` / `Invariants` with your real constraints.
- The `Memory Map` table is boilerplate — add a row or two only for orientation that `ls` can't give you ("`src/core/` — the engine, start here"). Never check in a literal file tree; it's the #1 thing that rots.
- Everything here is pure markdown and tool-agnostic — the templates work with any agent or none, and the skill follows the open `SKILL.md` standard, so it runs in Claude Code, Codex, and other skill-aware agents alike.

## Contributing

Issues and PRs welcome — especially new migration recipes in `references/migration.md` for memory systems not yet covered.

## License

MIT — see [LICENSE](LICENSE).
