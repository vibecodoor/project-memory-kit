# {{PROJECT_NAME}} — Project Control Plane

<!-- DEV MEMORY KIT · self-contained, no external deps · drop into any repo.
     This file is the spine AND the kit's own manual (see ## Dev Memory Protocol, bottom).
     Stable content only. Volatile state → STATE.md. History → DECISIONS.md.
     Smallest complete map to understand, continue, and verify this project. ~5-min read.
     Soft budget ≤150 lines. Order: stable → volatile. Supersede, don't append. -->

## Project Card

| Field | Value |
|---|---|
| Status | {{idea / planning / active / paused / archived}} |
| Started | {{YYYY-MM-DD}} |
| Owner | {{you}} |
| Type | {{code / KB / mixed / library / service}} |
| Stack | {{languages, frameworks, key deps}} |
| Source of truth | {{where canonical state lives: git / DB / external}} |

## What This Is

<!-- 2-3 sentences: what it does, for whom, what makes it different. -->

## Core Value

<!-- One sentence. If everything else breaks, this must still work. -->

## User & Problem

| Field | Answer |
|---|---|
| Primary user | <!-- specific, not "everyone" --> |
| Pain | <!-- the real problem + current workaround --> |
| Trigger | <!-- why this is worth building now --> |

## Core Loop

```text
{{input}} -> {{processing}} -> {{human review / action}} -> {{saved state}} -> {{next}}
```

## Scope

### Validated

<!-- Shipped / tested / proven. -->

- [ ] {{requirement}} — {{evidence, date}}

### Active

<!-- Current bets, not yet shipped. -->

- [ ] {{active 1}}

### Out of Scope

<!-- Explicit exclusions + why — cheapest guard against scope creep. -->

- {{excluded}} — {{why}}

## Memory Map & Routing

> The dev-memory surfaces and when to read each. Precedence for "what do I do now": **STATE > DECISIONS > PROJECT**.
> Read only the surface whose trigger matches the task — not all of them. If one is already in context, reuse it; don't re-read.

| Surface | Holds | Load when |
|---|---|---|
| `PROJECT.md` (this) | why / scope / architecture / invariants — the stable spine | always (you're in it) |
| `STATE.md` | current task · next · blockers — the live cursor | resuming work / "where was I?" |
| `DECISIONS.md` | append-only decisions + rationale | "why did we…?" / revisiting a choice |
| `JOURNAL.md` | problem → dead-end → fix → lesson | "have we hit this before?" / a recurring bug |
| `specs/<slug>.md` | per-feature spec + acceptance criteria | working that feature (match its `> Load when:` / `applies-to`) |
| `RESEARCH.md` *(if present)* | stack / competitor / architecture evidence | planning / challenging a decision |

<!-- Lazy surfaces (DECISIONS / JOURNAL / specs / RESEARCH) are never auto-loaded — born/loaded on the triggers above.
     Code map: store only what `glob`/`ls` CAN'T tell you — entry points, "start here", what a module owns. Never
     mirror the raw file tree (derivable + the #1 drift surface; regenerate it on demand). Small project → a few rows
     inline here, e.g. `| src/core/ | the engine — start here | editing core logic |`. Grows past a few rows / bloats
     the spine → extract to ARCHITECTURE.md, keep one row pointing to it (the unbounded-growth split rule). -->

## Invariants

<!-- Hard always/never rules NOT enforced by tooling/CI/linter. Terse. Delete if none. -->

- Never: {{prohibition + scope + via-what — e.g. "direct DB calls from client; all queries via /api/"}}
- Required: {{rule that must hold — e.g. "all DB access through backend; secrets in env only"}}
- Security: {{none / secrets in env only / + threat-model when money|auth|PII handled}}
- Quality gates (optional): {{ship only when — e.g. TS errors:0 / coverage:≥N% / p95:<Xms}}

## Canonical References

> **External / other** canonical files only — the dev-memory surfaces live in the Memory Map above.
> Stable load-map (keep above the volatile sections — it is part of the cache-stable prefix). Delete if none.

| File / Link | What it decides | Load when |
|---|---|---|
| {{path or URL}} | {{}} | {{always / planning / debugging / on handoff}} |

<!-- Volatile sections last: State + Decisions digest are the only parts that churn — keeping them
     at the bottom means their changes don't bust the cache of the stable spine above. -->

## State

> Current task, next action, blockers → **[STATE.md](STATE.md)**. Inline cap ≤7 lines.
> Not created yet? Born on the first "where was I?" — until then this is single-session work.
> When this exceeds ~7 lines or churns every session → extract to STATE.md, keep a 3-line headline here.

## Decisions

> Full history → **[DECISIONS.md](DECISIONS.md)** (append-only; supersede, don't rewrite).
> Active digest worth always seeing (≤7 lines; when it outgrows that, it lives only in DECISIONS.md):
> - {{1-line decision}}

---

## Dev Memory Protocol

<!-- Self-contained. This kit = the root surfaces below, no external deps. Read once; it governs the rest. -->

**Surfaces**

- `PROJECT.md` — stable spine (this file): why / scope / architecture / invariants. Edit on structural change.
- `STATE.md` — cursor: current task / next / blockers. Overwrite freely. ≤40 lines.
- `DECISIONS.md` — append-only history (forward commitments). Supersede, never rewrite.
- `JOURNAL.md` — append-only problem/solution log: what broke, dead ends, fix, lesson. **Lazy — never always-loaded** (it grows unbounded + is only sometimes relevant). Want an always-visible nudge? Keep a 2-line "Lessons" note in the spine, not the whole journal. (DECISIONS = what we'll do; JOURNAL = what we hit & learned.)
- `specs/<slug>.md` — per-feature spec + acceptance criteria (copy `specs/_template.md`). **Lazy** — each starts with a `> Load when:` trigger; load only the one whose trigger/`applies-to` matches the task, not all specs. If a lazy file is already in this context, reuse it — don't re-read it (re-reading after `/compact` is fine; thrashing it every turn is not).

**Create on demand — never pre-create empty stubs**

- `STATE.md` → on the first "where was I?" (work spans more than one session)
- `DECISIONS.md` → on the 3rd recorded decision
- `JOURNAL.md` → on the first non-trivial problem worth not repeating (cost real time, had dead ends, or non-obvious fix)
- `specs/<slug>.md` → when a feature will outlive a single session

**Always-loaded budget** — what rides in *every* session = **spine + cursor + decisions digest** (~1k tokens, <1% of the context window). Everything else (full DECISIONS log, JOURNAL, other specs) is fetched on a trigger. This is the design for **every tier** — a $20 sub and a Max sub run the same shape; lean memory isn't a cheap mode, it's what leaves the window free for the actual code.

**Merge vs Split** — default is **one file** (`PROJECT.md` with inline State + a ≤7-line Decisions digest). Split a surface out **only** when it hits one of two physical pressures — never for tidiness:
1. **Unbounded growth** — the full decisions log / journal / a feature spec would bloat the spine → extract it, keep a small digest/pointer inline.
2. **Volatility (cache)** — a section that rewrites every session dirties the stable spine → extract the cursor to `STATE.md`.
A surface with neither pressure stays merged. Triggers are *permissions to grow, not obligations*; solo/small work may sit at ~one file indefinitely. Never pre-create empty stubs.

**Precedence** for "what do I do now": `STATE > DECISIONS > PROJECT`.
STATE is a cursor, not truth. A *standing* contradiction is a bug → fix by editing PROJECT or appending to DECISIONS.

**Boundary** — this kit holds only build-time facts (how the project is made). Anything the running product needs at runtime belongs in the product's own storage, not here.

**Hygiene** — supersede, don't append. If `PROJECT.md` outgrows ~150 lines, move content out (→ STATE / DECISIONS / specs / delete). Re-check staleness whenever you resume cold or onboard someone.

**Autonomous upkeep** — the agent keeps these surfaces current as a normal part of doing the work, without waiting for a user command. Record a decision when it's made, journal a non-trivial problem when it's solved, advance STATE as the cursor moves, flip a spec AC `[ ]→[x]` when verified. Write on the event, not every turn (thrashing the files each turn is noise). Update PROJECT.md on: purpose/user change · requirement validated/invalidated/cut · major architecture decision · source-of-truth change · milestone done — a *structural* PROJECT.md rewrite is fine to make, but surface it in your reply so the owner sees it.

**`end session` checkpoint** — when the user signals end of work ("end session", "wrapping up", or equivalent), run a memory sweep before stopping:
1. STATE — refresh `Now` / `Recent:` / `Next` / `Blockers` / `In flight` so a cold resume is cheap. (Born here if it doesn't exist yet and work spans sessions.)
2. DECISIONS — append any decision made this session (file born on the 3rd decision).
3. JOURNAL — append any non-trivial problem→fix→lesson (file born on the first one).
4. specs — flip any verified AC `[ ]→[x]`.
5. PROJECT.md — check staleness; update if a milestone/scope change landed.
6. Report a ≤3-line summary of what was written where. Write nothing if nothing changed — an empty sweep is a valid outcome, not a reason to invent entries.

**Working when** — sessions resume without re-explaining, decisions don't get re-litigated, and the context window stays mostly code, not memory overhead. If you're re-explaining the project each session, the kit is stale, not done.
