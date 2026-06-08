---
status: planned   # planned | in-progress | done
created: {{YYYY-MM-DD}}
---
# {{Feature title}}

> Load when: {{one-line trigger — e.g. "working on auth / sessions"}}
<!-- applies-to: {{glob, e.g. src/auth/** — optional; lets the agent auto-pick this spec by the files in play}} -->

## Description

{{What this feature is, and the one user-visible outcome that proves it works.}}
Core requirement: "The system MUST {{the one normative obligation}}."

## Acceptance Criteria

<!-- AC:BEGIN — flip [ ]→[x] as each is verified; #N = stable id, reference it from STATE/DECISIONS/JOURNAL without quoting the text -->
- [ ] #1 **{{Scenario name}}:** Given … When … Then … (falsifiable)
- [ ] #2 **{{…}}:** …
<!-- AC:END -->

## Implementation Plan

1. {{step}}

## Notes

{{gotchas local to this feature; promote durable decisions to ../DECISIONS.md}}
{{on completion, one line: what shipped + how it was verified — the durable outcome, not the play-by-play}}
