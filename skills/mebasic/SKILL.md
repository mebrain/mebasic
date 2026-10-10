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

After coding, compare this task's changes with the existing interpretation:
- Skip when descriptions remain accurate, including trivial edits and internal
  refactors. Judge meaning, not line count.
- Update when behaviour, concepts, relationships, terminology or constraints
  change. Identify the affected implementation and descriptions, including links.
- If `mebasic-src/` is absent, generate an initial interpretation only for
  substantive project work or an explicit generation request.

Delegate needed work to a separate agent after coding finishes; pass the reason,
affected paths and accepted user instructions. Use the IDE's supplied handoff
when available, otherwise a native subagent call. Use this skill's nonempty
provider model override above, or choose the smallest inexpensive capable model
from the current provider catalog. Do not infer price from a default selection;
if no suitable model is identifiable or available, report the pending update.

The worker reads the specification, actual source and relevant existing
constraints; edits only affected `mebasic-src/` descriptions and links; preserves
user edits and implementation locations; and maintains private `.mebasic/` history
and baselines. Initial generation may create the tree and screenshots folder.
Use only real screenshots. Report changes, checks and unresolved conflicts.
Do not delegate again from the worker or regenerate unrelated descriptions.
