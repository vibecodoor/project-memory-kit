# Diagnosis — detect the existing system, then score it

Two jobs: (1) recognize what memory system (if any) the repo already uses, (2) score how badly it suffers from the failure modes the kit exists to cure. Used in Phase 1 and Phase 3b.

## Part 1 — Detection signatures

Match by files/dirs first, then confirm by content. A repo can have more than one (e.g. an ADR dir *and* a bloated CLAUDE.md) — note all of them.

| System | Signature (files / dirs) | Confirm by |
|---|---|---|
| **Project Memory Kit** (this) | `PROJECT.md` containing a `## Dev Memory Protocol` heading | the protocol block + Memory Map table → go to Health check (3c) |
| **Cline Memory Bank** | `memory-bank/` dir; `.clinerules` | the canonical 6: `projectbrief.md`, `productContext.md`, `activeContext.md`, `systemPatterns.md`, `techContext.md`, `progress.md` |
| **Agent OS** (buildermethods) | `.agent-os/` dir | `product/{mission,roadmap,tech-stack,decisions}.md`, `standards/`, `specs/`, `instructions/` |
| **GitHub spec-kit** | `.specify/` dir; `memory/constitution.md` | per-feature `specs/NNN-*/{spec,plan,tasks}.md` |
| **Backlog.md** | `backlog/` dir | task files with frontmatter `id:`/`status:`/`assignee:`; a task-graph, not memory |
| **ADR / MADR** | `docs/adr/`, `doc/adr/`, `decisions/` | numbered `NNNN-title.md`, MADR `## Status` / `## Context` / `## Decision` |
| **BMAD-Method** | `.bmad-core/`, `.bmad/` | agent persona files, story/epic dirs |
| **Overloaded CLAUDE.md / AGENTS.md** | a `CLAUDE.md`/`AGENTS.md` > ~150 lines | mixes *project facts* (what/scope/architecture) with *agent behavior* (coding rules) and/or a decisions log — three concerns in one always-loaded file |
| **Scattered / ad-hoc** | some of `NOTES.md`, `TODO.md`, `CONTEXT.md`, `ARCHITECTURE.md`, `DESIGN.md`, a loose `docs/` | human-written, no routing, no load-when discipline, often stale |

If none match and there's no `PROJECT.md`-like control file → **Install** (Phase 3a).

### Boundary calls

- **CLAUDE.md / AGENTS.md is not automatically a memory system.** If it's a lean set of *agent behavior rules* (how to code in this repo), leave it alone — that's its job, and it's a different concern from project memory. Only treat it as a migration source when it has swelled to also carry project facts, scope, architecture, or a decisions log. In that case the migration *splits* it: project facts → `PROJECT.md`; pure behavior rules stay in `CLAUDE.md`.
- **An ADR dir is healthy on its own.** One rich file per decision that's never bulk-loaded is fine (it's already lazy). Don't migrate it just to migrate it — only fold it in if the user wants a single decisions surface, and even then a digest in `PROJECT.md` + pointer to the ADR dir may beat moving everything.
- **A task tracker (Backlog.md, issues) is not dev-memory** — it's a work queue. Different species. Don't absorb it; at most reference it.

## Part 2 — The rubric (score what you found)

The kit exists to cure two diseases — **bloat** (too much always-loaded) and **rot** (stale, derivable, or contradictory content) — by enforcing **just-in-time loading**, **volatility separation**, and **supersede-don't-append**. Score the existing system on each axis. You don't need numbers; name the concrete failures.

| Axis | Healthy | Failing — what to look for |
|---|---|---|
| **Always-loaded weight** | spine + cursor + decisions digest ≈ 1k tokens | a multi-thousand-token file (or a stack of files) loaded *every* session regardless of task; a decisions log that's 80% of the file |
| **Just-in-time loading** | reference/history/specs fetched on a trigger | everything in one always-read blob; no "load when X" routing; agent re-reads the whole corpus each session |
| **Volatility separation** | the churning cursor is isolated from the stable spine | "current status / today I…" edited into the same file as the stable architecture → every status edit dirties the cache-stable prefix |
| **Decision discipline** | supersede, newest-first, old marked superseded | append-only sprawl with no supersession; contradictory live decisions; can't tell what's current |
| **Derivable content** | none stored | a checked-in file tree / dir listing (the #1 rot surface — regenerate on demand); duplicated READMEs |
| **Earned structure** | surfaces exist only with real content | empty stubs, ceremony folders, phase scaffolding nobody fills |
| **Cold-resume cost** | next action obvious from the cursor | you must re-read everything to know "where was I?"; the user re-explains the project each session |
| **Self-containment** | works dropped into the repo, no external deps | relies on a global config / external tool / sync validator to make sense |

### Output of the diagnosis

A short block, concrete and specific — the 3-5 real weaknesses, each tied to a fix the migration will make. Example:

```
Diagnosis — Cline Memory Bank (6 files, all always-loaded)
- Bloat: all 6 files read every session (~6k tok) regardless of task → just-in-time gap.
- Volatility: activeContext.md (a cursor) sits in the always-load set, same tier as the stable brief.
- No supersession: progress.md is append-only, oldest-first; current state is buried.
- Healthy: clear separation of concerns across the 6 files — maps cleanly onto kit surfaces.
→ Migrate to: PROJECT.md (stable spine, always) + STATE.md (cursor, lazy) + DECISIONS.md (born when ≥3 decisions). Net always-loaded weight ~1k.
```

Keep it honest — if the existing system does something *well*, say so (it tells the user what's preserved, and which mappings are clean).
