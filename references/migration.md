# Migration — map an existing system onto kit surfaces

Used in Phase 3b. The job is to **move and condense** existing knowledge into the kit shape, losing nothing real. Read the recipe for the system you detected; the general rules apply to all.

## General rules (all migrations)

- **Archive, never delete.** Move every original into `devkit/.memory-archive/<YYYY-MM-DD>/` (preserving relative paths) *before* rewriting. The user can diff and recover. Say this in the plan.
- **Destinations live in `devkit/`.** Every kit surface (`PROJECT.md`, `STATE.md`, `DECISIONS.md`, `JOURNAL.md`, `specs/`) is written under the repo's `devkit/` folder — create it if absent. The table below names surfaces by their short name; the actual path is `devkit/<that name>`.
- **Condense, don't transcribe.** Migrating is a chance to cut bloat: drop derivable content (file trees, dir listings), fold duplicates, and collapse append-only sprawl into a superseded, newest-first form.
- **Preserve the kit's two-tier shape.** Always-loaded = `PROJECT.md` (spine + inline State + Decisions digest). Everything else lazy. Don't recreate the old "load everything" pattern with new filenames.
- **Born-on-trigger still applies.** Only create `STATE.md` / `DECISIONS.md` / `JOURNAL.md` / `specs/` if there's real content to put in them now. If the old system had no decision history, don't create `DECISIONS.md` — let it be born later.
- **Keep the protocol verbatim.** The `## Dev Memory Protocol` block from the template is non-negotiable — it's what makes the result self-governing.
- **Present a source→destination table** before applying. Every row is either a destination surface or `dropped (reason)`.

## Cline Memory Bank → kit

The cleanest migration — the 6 files map almost 1:1 (Cline already separates concerns; it just loads them all every session).

| Cline file | → kit destination |
|---|---|
| `projectbrief.md` | `PROJECT.md` → What This Is, Core Value |
| `productContext.md` | `PROJECT.md` → User & Problem, Core Loop |
| `techContext.md` | `PROJECT.md` → Project Card (Stack), Invariants (constraints) |
| `systemPatterns.md` | `PROJECT.md` → Memory Map (orientation rows) + Architecture/Invariants; **drop any literal file tree** (derivable) |
| `activeContext.md` | `STATE.md` (the cursor — this is *the* reason to create STATE) |
| `progress.md` | `STATE.md` → Now/Next; promote any durable "decided to…" lines into the `PROJECT.md` Decisions digest (or `DECISIONS.md` if ≥3) |

Net: 6 always-loaded files (~6k tok) → `PROJECT.md` always (~1k) + `STATE.md` lazy.

## Agent OS → kit

| Agent OS | → kit destination |
|---|---|
| `product/mission.md` | `PROJECT.md` → What This Is, Core Value, User & Problem |
| `product/roadmap.md` | `PROJECT.md` → Scope (Active / Out of Scope); near-term items → `STATE.md` Next |
| `product/tech-stack.md` | `PROJECT.md` → Project Card (Stack), Invariants |
| `product/decisions.md` | `DECISIONS.md` (reformat to supersede/newest-first) + a ≤7-line digest in `PROJECT.md` |
| `specs/<feature>/` | `specs/<slug>.md` each, with a `> Load when:` trigger added; one spec per feature, lazy |
| `standards/`, `instructions/` | these are *agent behavior*, not project memory → leave in place or route to `CLAUDE.md`/`AGENTS.md`; do **not** absorb into `PROJECT.md` (Boundary) |

## Bloated PROJECT.md / monolithic doc → kit

The file is right, the shape is wrong. Split by pressure:
1. **Extract the cursor** — any "current status / now / today / in progress" prose → `STATE.md`. (Volatility split — stops status edits from dirtying the stable spine.)
2. **Extract the decisions log** — if it's a long append-only history → `DECISIONS.md` (newest-first, mark superseded), leave a ≤7-line digest inline. (Unbounded-growth split.)
3. **Drop derivable content** — checked-in file trees, dir listings, anything `ls` regenerates → delete (archived in the backup). Replace a file tree with 1-3 semantic Memory Map rows ("what this dir owns / start here").
4. **Reorder** — stable spine first, volatile State + Decisions digest last (cache-stable prefix).
5. **Add the protocol** — append the `## Dev Memory Protocol` block so the result is self-governing.

## Overloaded CLAUDE.md / AGENTS.md → split, don't absorb

Separate the two concerns it has fused:
- **Project facts** (what it is, scope, architecture, decisions) → migrate into `PROJECT.md` as above.
- **Agent behavior** (coding conventions, "always run tests", style rules) → **stays** in `CLAUDE.md`/`AGENTS.md`. That's its proper job and it's a different species from project memory (the kit's Boundary invariant). The result: a lean behavior file + a proper `PROJECT.md`, instead of one swollen file doing both.

## ADR / MADR dir → usually leave, optionally digest

An ADR dir is already lazy and healthy. Default: **leave it**, and add a Decisions digest in `PROJECT.md` (the load-bearing few) plus a Canonical-References row pointing at the ADR dir. Only consolidate into `DECISIONS.md` if the user explicitly wants a single decisions surface — and warn that one-file-per-decision (MADR) has richer per-decision structure you'd be flattening.

## Scattered / ad-hoc (NOTES.md, TODO.md, ARCHITECTURE.md, loose docs) → consolidate

No structure to preserve, just signal to harvest:
- Stable facts (what/why/architecture) → `PROJECT.md`.
- Current work / next steps (`TODO.md`, "now" notes) → inline `## State` (or `STATE.md` if multi-session).
- Real decisions buried in notes → Decisions digest (or `DECISIONS.md` if ≥3).
- `ARCHITECTURE.md` → keep as its own file *only if* the semantic map outgrows a few Memory-Map rows (the unbounded-growth split); otherwise fold a few rows into the Memory Map and archive the rest.
- Stale/contradictory notes → flag for the user per-file; don't silently carry rot into the new system.
