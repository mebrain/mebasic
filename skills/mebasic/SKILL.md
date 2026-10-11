---
name: mebasic
description: After coding, decide whether the project's MeBasic interpretation needs a scoped update and maintain it directly with the user's current agent.
---
# MeBasic

MeBasic explains source code through concepts, responsibilities and relationships.
Follow [mebasic.md](../../mebasic.md). Use the current agent, conversation context,
and the user's selected model and reasoning settings, or the agent's configured
defaults. Do not delegate this work, create a worker handoff or switch models.

Check for a valid `mebasic-src/<name>.mebasic` configuration. Without it, do nothing:
opening a folder or coding is not consent. Initial generation and retries require
explicit approval; only ready projects receive automatic maintenance.

On every project chat, also apply [mebasic-constraints](../mebasic-constraints/SKILL.md)
for significant architect statements, derived prohibitions and contradictions, even
when the description update below is skipped.

After coding, decide whether the interpretation changed meaningfully:
- Skip when descriptions remain accurate, including trivial edits and internal refactors.
- Otherwise update affected descriptions and necessary links before your final reply.
- Explicitly approved generation or regeneration may cover the whole interpretation,
  continuing from existing files and unfinished work; adapt and edit in place.
  Re-check prior LLM text against current source; correct it freely without
  assuming hidden knowledge. Protect explicitly identified human-authored or
  human-approved content with recorded provenance. Only explicit full refresh
  discards the interpretation. Follow the specification's collection hierarchy.

Before writing, read project `AGENTS.md`/`agents.md`, applicable nested instructions,
referenced guidance, actual source, relevant tests and constraints. Understand the
project deeply. Read and maintain `mebasic-src/context/project-context.md` with
concise understanding, accepted decisions, relative evidence and remaining work.
Verify remembered facts against current files; exclude secrets and transcripts.

Every MeBasic write stays inside `mebasic-src/`; generation does not authorise
implementation changes. Preserve explicitly identified human wording and accepted terminology, repair
links after moves, and maintain private history and baselines under `.mebasic/`.
Record proposed restructuring in `.mebrain/restructuring.md` within `mebasic-src/`.
Use only real screenshots. The IDE owns the project configuration; do not remove
it or create competing markers. Report what changed, checks and unresolved work.
