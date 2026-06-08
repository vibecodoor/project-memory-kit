---
name: project-memory-kit
description: "Analyze an existing repo and install or upgrade a dev-time memory system (the Project Memory Kit shape: a thin always-loaded PROJECT.md control plane plus lazy STATE / DECISIONS / JOURNAL / specs surfaces). Use whenever the user wants to add project memory, make a repo resumable across sessions, stop their AI agent from forgetting context between sessions, or fix a memory setup that has gone wrong — e.g. 'set up project memory here', 'add a memory system to this repo', 'my PROJECT.md / CLAUDE.md is bloated', 'the agent re-reads everything every session', 'migrate my Cline Memory Bank / Agent OS / scattered docs into something leaner'. Trigger this even when the user describes the symptom (context loss, bloat, rot, re-explaining the project each session) without naming a 'memory system'. Do NOT use for runtime/product memory, for the assistant's own ~/.claude MEMORY.md, or for a brand-new empty project that has no code or docs to analyze yet."
---

# Dev Memory — install or upgrade a project's memory system

Point this at an existing repo. It does one of two things:

- **No dev-memory present** → analyze the project and lay down the kit, pre-filled from what the repo already tells you.
- **Some memory already exists** (a bloated `PROJECT.md`, an overloaded `CLAUDE.md`, a Cline Memory Bank, an Agent OS install, scattered `docs/` / `NOTES.md` / ADRs) → diagnose it against the kit's principles and **migrate it to the stronger shape without losing content**.

The output is the **Project Memory Kit**: a thin, always-loaded `PROJECT.md` spine plus surfaces that are *born on triggers*, not pre-created. The full doctrine ships *inside* the generated `PROJECT.md` (its `## Dev Memory Protocol` block) — so once installed, the repo is self-governing and needs no further reference to this skill.

## Core principles (these are the whole point — internalize them)

1. **REMOVE, not ADD.** The disease this kit cures is bloat and rot. Every surface you create must earn its place. The default end-state for a small repo is **one file** (`PROJECT.md` with inline State + a short Decisions digest), not five.
2. **Never pre-create empty stubs.** `STATE.md` / `DECISIONS.md` / `JOURNAL.md` / `specs/*.md` are *born on their triggers*, not laid down hopefully. An empty `JOURNAL.md` is rot on day one.
3. **Earned growth.** A surface splits out only under real pressure — unbounded growth (the full decisions log would bloat the spine) or volatility (a cursor that rewrites every session dirties the cache-stable spine). Never for tidiness.
4. **Content-preserving migration.** When upgrading an existing system, you are *moving and condensing* knowledge, never discarding it. Archive originals (don't delete) so nothing is lost — the user can diff.
5. **Human-in-the-loop between diagnosis and apply.** Always show the plan/diff *before* writing files. The classification of "what's bloat vs. signal" will sometimes be wrong; let the user correct it.
6. **Self-contained result.** The installed `PROJECT.md` carries its own protocol. No dependency on this skill, on global config, or on the user's other projects.

## Workflow

Follow the phases in order. If a phase surfaces something unexpected, stop and ask.

### Phase 1 — Scan the repo

Learn two things in parallel. Read, don't guess.

**A. Project profile** (raw material to fill the spine):
- **What it is / who for** — `README*`, the repo description, top-level docs.
- **Kind & stack** — manifest files (`package.json`, `pyproject.toml`/`requirements.txt`, `go.mod`, `Cargo.toml`, `Gemfile`, `composer.json`…), file composition (code-heavy / md-heavy / mixed / library / service).
- **Orientation** — entry points, the "start here" module, what the main directories own. (For the Memory Map — only what `ls`/`glob` *can't* tell you.)
- **Activity & horizon** — `git log` (age, cadence, contributors), open `TODO`/`FIXME`, whether work clearly spans many sessions. This decides whether `STATE.md` is justified from day one.

**B. Existing memory artifacts** (decides install vs upgrade). Look for:
- **The kit itself** — `PROJECT.md` with a `## Dev Memory Protocol` block, `STATE.md`, `DECISIONS.md`, `JOURNAL.md`, `specs/`.
- **Rival / ad-hoc systems** — see `references/diagnosis.md` for detection signatures (Cline Memory Bank, Agent OS, spec-kit, Backlog.md, ADR dirs, an overloaded `CLAUDE.md`/`AGENTS.md`, scattered `NOTES.md`/`docs/`).

Read `references/diagnosis.md` now if any memory-like artifact is present — it has the signatures and the bloat/rot rubric.

### Phase 2 — Branch

| What you found | Path |
|---|---|
| Nothing memory-like | **Phase 3a — Install** |
| The kit is already here | **Phase 3c — Health check** (it's already the target shape; tune, don't migrate) |
| Another / ad-hoc system | **Phase 3b — Upgrade** |

State which branch you're taking and why, in one line.

### Phase 3a — Install (no memory present)

1. **Choose the starting shape by earned growth — start minimal.**
   - Default: a single **`PROJECT.md`** (spine + inline `## State` + a ≤7-line `## Decisions` digest). Nothing else.
   - Add **`STATE.md`** at init *only if* the repo clearly has multi-session work in flight (active git history, a half-done feature) — then a cold resume needs a real cursor on day one. Otherwise leave State inline; it graduates on the first "where was I?".
   - Do **not** create `DECISIONS.md` / `JOURNAL.md` / `specs/` — they are born on their triggers (3rd decision · first non-trivial problem · a feature outliving one session). The embedded protocol tells the agent this.
2. **Fill `PROJECT.md` from Phase 1.** Copy `assets/templates/PROJECT.md` and replace every `{{placeholder}}` with real inferred content: Project Card, What This Is, Core Value, User & Problem (best-effort — mark guesses), Core Loop, Scope (Active from current work/issues · Out of Scope if stated), Invariants (only real always/never rules — security, architecture boundaries), Memory Map (keep the table as boilerplate; add a code-orientation row or two only if a path is genuinely non-obvious). Keep the `## Dev Memory Protocol` block **verbatim** — it is the self-bootstrapping manual. Leave no placeholder unfilled; if you truly can't infer a field, write a one-line `<!-- TODO: confirm -->` rather than fake content.
3. **Show the draft, get approval, then write.** Present the filled `PROJECT.md` (and `STATE.md` if used) for review before writing to disk. This is a multi-file change — plan first.

### Phase 3b — Upgrade (another system present)

This is the high-value path. Four steps, approval-gated.

1. **Diagnose.** Score the existing system against the rubric in `references/diagnosis.md`: always-loaded token weight (bloat), rot (stale maps, derivable file trees, dead links), no just-in-time loading (everything always in context), no volatility separation (cursor mixed into the stable spine), append-not-supersede decision logs, empty ceremony/stubs. Write a short diagnostic — name the 3-5 concrete weaknesses, not a generic lecture.
2. **Map content → kit surfaces.** Read `references/migration.md` for per-system recipes (Cline Memory Bank, Agent OS, bloated PROJECT.md, scattered docs…). Decide where each piece of existing content lands, what gets condensed, and what gets dropped — and *why* (only ever drop derivable or stale content, e.g. a checked-in file tree; never drop genuine decisions or rationale).
3. **Present the migration plan.** A table: `source → destination (or dropped, with reason)`. State explicitly that originals will be **archived, not deleted** (move to `.memory-archive/<date>/` or `docs/legacy/`), so nothing is lost and the user can diff. Wait for approval. One round of adjustments.
4. **Apply, then verify (Phase 4).** Write the new surfaces, archive the originals, fix internal links.

### Phase 3c — Health check (kit already present)

The shape is right; look for drift, not migration:
- **Bloat** — `PROJECT.md` over ~150 lines, or a Decisions digest / inline State that outgrew its cap → extract per the split rules.
- **Empty stubs** — a `JOURNAL.md`/`DECISIONS.md`/`specs/*` that was pre-created and is still hollow → it violates earned-growth; recommend deletion.
- **Rot** — a checked-in file tree in the Memory Map, stale `Last touched`, dead links, a STATE cursor that contradicts PROJECT.
- **Missing triggers** — specs without a `> Load when:` line; volatile sections not at the bottom.

Report findings and offer fixes. Don't rewrite a healthy file just to touch it.

### Phase 4 — Verify

Run a final pass and report:
- Spine `PROJECT.md` ≤ ~150 lines; volatile sections (State, Decisions digest) **last** (cache-stable prefix intact).
- **No empty stubs** — every surface that exists has real content.
- Lazy surfaces carry their triggers (`specs/*` open with `> Load when:`; the protocol block is intact).
- **Cold-resume test** — from `STATE` (or inline State) alone, is the next action obvious? If not, the cursor is too thin.
- Internal markdown links resolve.
- (Upgrade path) originals are archived, not lost.

Final report, ≤5 lines:

```
✓ Installed/Upgraded: PROJECT.md (+ STATE.md)
✓ Migrated: 4 source files → 2 surfaces; 1 file-tree dropped (derivable)
✓ Archived originals → .memory-archive/2026-06-08/
⚠ Deferred (born on trigger): DECISIONS.md, JOURNAL.md, specs/
→ Next: <the concrete first action, matching STATE's Now>
```

## Templates

Bundled in `assets/templates/` (the canonical templates). Copy, then fill — never invent a different structure:

| File | Used when |
|---|---|
| `PROJECT.md` | always — the spine (embeds the full Dev Memory Protocol) |
| `STATE.md` | the cursor graduates to its own file (multi-session work) |
| `DECISIONS.md` | on the 3rd recorded decision |
| `JOURNAL.md` | on the first non-trivial problem worth not repeating |
| `specs/_template.md` | copy-source for a per-feature spec |

## References

- `references/diagnosis.md` — detection signatures for known memory systems + the bloat / rot / lazy-loading scoring rubric. Read in Phase 1 when any memory-like artifact is present.
- `references/migration.md` — per-system content→surface mapping recipes for the upgrade path. Read in Phase 3b.
