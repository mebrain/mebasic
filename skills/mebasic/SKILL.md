---
name: mebasic
description: After coding, decide whether the project's MeBasic interpretation needs a scoped update and delegate it to a capable agent.
metadata:
  codex-model: "gpt-6.1-sol"
  codex-effort: "medium"
  claude-model: ""
---
# MeBasic

MeBasic explains actual source code to users through concepts, responsibilities
and relationships above individual functions. Follow [mebasic.md](../../mebasic.md).

First check for a valid `mebasic-src/<name>.mebasic` project configuration.
Without it, do nothing: opening a folder or coding is not consent. Initial
generation requires explicit user approval. Pending generation
requires an explicit retry; only ready projects receive automatic maintenance.

After coding in an opted-in project, compare this task's changes with the existing interpretation:
- Skip when descriptions remain accurate, including trivial edits and internal
  refactors. Judge meaning, not line count.
- Update when behaviour, concepts, relationships, terminology or constraints
  change. Identify the affected implementation and descriptions, including links.
- Generate an initial interpretation only for an explicitly approved request.
  Preserve existing user descriptions. For approved generation, listed paths are
  starting points, not limits: choose useful descriptions throughout `mebasic-src/`.

Delegate needed work to a separate agent after coding finishes; pass the reason,
affected paths and accepted user instructions. Use the IDE's supplied handoff
when available, otherwise a native subagent call. Use the
project configuration's maintenance model override, then this skill's nonempty
provider model override above. Use the configured reasoning effort (medium by
default for Codex). If no model override is set, choose a model with capability
comparable to GPT-6.1 Sol from the current catalog, not the cheapest small model;
if no suitable model is identifiable or available, report the pending update.

Before writing, the worker reads project-root `AGENTS.md` (or `agents.md`),
applicable nested instructions and referenced guidance, then the current specification,
actual source, relevant tests and constraints. Understand purpose, architecture,
relationships and user journeys deeply; filenames alone are not evidence.
Read `context/project-context.md` inside `mebasic-src/` if present. Before finishing,
create or update this concise, source-grounded understanding with decisions,
project-relative evidence paths, unresolved questions and remaining work. Refresh
stale facts against current files and corrections; preserve user edits. This file
carries context across fresh workers, IDE restarts and project moves. Keep secrets
and conversation transcripts out of it. The worker edits only affected `mebasic-src/` descriptions and links; preserves
user edits and implementation locations; and maintains private history and
baselines under `mebasic-src/.mebasic/`. Every MeBasic write stays inside
`mebasic-src/`; source elsewhere is read-only evidence. Initial generation must inspect actual source,
follow the specification's collection definitions and exact folder hierarchy,
and fill each collection with its own content. Record proposed restructuring
under `mebasic-src/.mebrain/restructuring.md`; generation does not authorise
implementation changes. Preserve content and user edits when changing organisation,
repair links and context references, and record moves. Review coverage and placement
before completion, using only real screenshots. The IDE owns the project
configuration; do not remove it or create competing marker files.
Report changes, checks and unresolved conflicts.
Do not delegate again from the worker. Automatic maintenance must not regenerate
unrelated descriptions; explicitly confirmed regeneration may refresh the whole
interpretation while preserving user edits.
