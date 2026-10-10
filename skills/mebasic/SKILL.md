---
name: mebasic
description: After coding, decide whether the project's MeBasic interpretation needs a scoped update and delegate it to a small, inexpensive agent.
metadata:
  codex-model: ""
  claude-model: ""
---
# MeBasic

MeBasic explains actual source code to users through concepts, responsibilities
and relationships above individual functions. Follow [mebasic.md](../../mebasic.md).

First check for a valid `mebasic-src/<name>.mebasic` project configuration.
Without it, do nothing: opening a folder or coding is not consent. Initial
generation requires explicit user approval through the IDE. Pending generation
requires an explicit retry; only ready projects receive automatic maintenance.

After coding in an opted-in project, compare this task's changes with the existing interpretation:
- Skip when descriptions remain accurate, including trivial edits and internal
  refactors. Judge meaning, not line count.
- Update when behaviour, concepts, relationships, terminology or constraints
  change. Identify the affected implementation and descriptions, including links.
- Generate an initial interpretation only for an explicitly approved request.
  Preserve existing descriptions even when adopting a legacy folder.

Delegate needed work to a separate agent after coding finishes; pass the reason,
affected paths and accepted user instructions. Use the IDE's supplied handoff
when available, otherwise a native subagent call. Use the
project configuration's maintenance model override, then this skill's nonempty
provider model override above, or choose the smallest inexpensive capable model
from the current provider catalog. Do not infer price from a default selection;
if no suitable model is identifiable or available, report the pending update.

The worker reads the specification, actual source and relevant existing
constraints; edits only affected `mebasic-src/` descriptions and links; preserves
user edits and implementation locations; and maintains private history and
baselines under `mebasic-src/.mebasic/`. Every MeBasic write stays inside
`mebasic-src/`; source elsewhere is read-only evidence. Initial generation may
create descriptions and screenshots inside that directory. The IDE owns the
project configuration; do not remove it or create competing marker files.
Use only real screenshots. Report changes, checks and unresolved conflicts.
Do not delegate again from the worker or regenerate unrelated descriptions.
