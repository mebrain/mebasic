---
title: mebasic
summary: An AI-native, plain-text interpretation of source code that helps users understand their project.
description: >
  mebasic explains a project and its source code in plain words, independent
  of the implementation language. It helps users understand the program;
  it does not define how the program runs. This file
  defines the language. It is edited only by the architect, or with the
  architect's approval; an LLM never changes it on its own.
---
# mebasic

## Purpose

MeBasic is a plain-text language for interpreting a project and its source code
so people can understand them without knowing the underlying implementation
language. It is independent of any editor, IDE, model provider or source layout.
Descriptions use flexible prose rather than a strict grammar. The top-level folder
structure below is mandatory; flexibility applies to the explanations inside it,
not to folder names or placement. Omit unnecessary detail within descriptions.

Start with what the project does, who uses it, and its main concepts. Explain
purpose and useful behaviour first, then let links lead to deeper detail.
MeBasic operates above the function level: describe concepts, responsibilities,
relationships and journeys, without a function-by-function inventory or call
signatures. Source details remain available as supporting evidence.

## Project identity and containment

A MeBasic project has one JSON `mebasic-src/<name>.mebasic` configuration with
`format: "mebasic"`, `version: 1`, and a nonempty `name`. It identifies the project
and may hold generation status, maintenance preferences and tool-specific settings.
Preserve unknown fields. A directory alone does not imply consent to generate or
maintain descriptions; initial generation requires explicit user approval.

All files created or changed by MeBasic generation and maintenance stay inside
`mebasic-src/`. Descriptions interpret implementation files in their existing
locations; generating an interpretation never authorises changing, moving or
deleting those implementation files. Private authoring records belong under
`mebasic-src/.mebasic/`; portable understanding belongs under `mebasic-src/context/`.

## Folder Structure

- MeBasic Markdown descriptions live in `<project>/mebasic-src/`. The implementation
  may use any language or directory layout. Supporting code paths refer to the
  actual implementation relative to the project root.
- Keep captured UI images in `<project>/mebasic-src/screenshots/` (PNG or JPEG).
  These are real screenshots of the project UI, not invented mockups. Keep their
  provenance and capture date in the related description when useful. Omit a
  screenshot reference when no verified image is available.
- Every generated MeBasic project must create these exact folders directly under
  `mebasic-src/`, with these exact lowercase names:

  ```text
  mebasic-src/
    modules/
    objects/
    flow/
    algorithms/
    constraints/
    ui/
    data/
    widgets/
    events/
    screenshots/
    context/
  ```

  These names and their placement directly under `mebasic-src/` are mandatory.
  Each category is a sibling of the others, with its own content.
  Datastore descriptions belong in `data/` and use the `_store.md` suffix.
  Populate each description category with its own source-grounded content as
  defined below: modules contain modules, objects contain objects, and so on.
  If the project has no applicable concept for a category, keep that folder and
  include a short Markdown explanation of why it does not apply; never invent
  program features to fill it. `screenshots/` contains only real available UI
  captures and may remain empty; `context/` contains the portable project context.
  Context is authoring memory rather than a description category.
  Subfolders may group descriptions within their correct category but must not
  relocate another category beneath it.
- One file per thing described, named in lowercase letters and digits, then
  its primitive's suffix: `_mod.md`, `_obj.md`, `_flow.md`, `_alg.md`,
  `_con.md`, `_ui.md`, `_store.md`, `_widget.md` or `_event.md`, respectively.
- Statements are bullets inside modules, not separate files or folders.
- No README or index files: each file is its own read-me.
- `modules/project_mod.md` is the starting point: purpose, users, main concepts
  and major parts, with links into the project.

## File Format

Description files normally follow this order:

1. **Front matter**, between `---` lines:
   - `title`: the name of what it describes.
   - `summary`: one sentence, at most 160 characters.
   - `description: >` then indented lines: a few sentences in plain words.
   - For a UI page, `screenshot: screenshots/page.png` points to the relevant UI
     image, relative to `mebasic-src/`. It must exist inside `screenshots/`.
     The image illustrates the described UI page. For example:
     `screenshot: screenshots/settings.png`.
2. **`# Title`**: the one heading of that level, the same as `title`.
3. **The body**, as its primitive gives it below.
4. **Bottom matter**, between `---` lines at the very end: `code:`, then
   an indented YAML list of paths naming supporting code from the repository
   root. Use `code: []` when there is no implementation evidence yet.

The front-matter `title` is the human-readable name of a description; the filename
and primitive suffix identify its file and kind.

Links are relative paths to `.md` files and must lead to a file that exists.
Link related descriptions where they help readers follow ownership, use,
dependencies or data movement. The folders group descriptions by kind.

### Module

1. **`## Inputs`**: the meaningful things it receives. Each bullet starts
   with the input in bold, then what it is, where it comes from, and what
   responsibility handles it.
2. **`## Processing`**: first the rules it keeps, in a short paragraph; then
   one or more processing groups, each a `### Group` followed by its
   statements.
3. **`## Output`**: its meaningful results, each bullet starting
   with the output in bold. Notes, such as known breaches, follow as
   `###` headings.

### Object

- **`## Attributes`**: a list of what it holds. Each bullet starts with the
  attribute in bold, then what it is.
- Explain what the object represents and why it exists in its description.
  **`## Behaviour`** explains its useful operations and the rules it preserves;
  link the modules that use it and related objects where helpful.

### Other descriptions

Choose short headings that answer these questions:

- **Flow**: what starts it, what information travels through which modules,
  how it changes, and what useful result or failure ends it?
- **Algorithm**: what problem does it solve, with what inputs, main steps,
  result and limitations? Link the descriptions that use it.
- **Constraint**: what rule applies, why, where does it come from, and what
  happens when it cannot be met? Link the affected things.
- **UI page**: what can the user understand or do here, what actions are
  available, and what feedback or errors appear? Link the underlying work.
- **Datastore**: what is kept, why, where, who reads or changes it, and how
  long does it last? Explain relevant loss or recovery behaviour.
- **Widget**: what meaningful part of a UI page is this, what does it show
  or let the user do, and which events connect it to the program? Link its
  containing page and the objects or modules involved.
- **Event**: what occurrence triggers a response, where does it originate,
  what information accompanies it, and which object or module responds?
  Explain the visible result and link the relevant flow, without enumerating
  individual handler functions. Events can come from users or the system.

## Understand the project before generating

Before generating or regenerating MeBasic files, read the project's `AGENTS.md`
(or `agents.md`), applicable nested instructions, and the guidance they reference.
If no such file exists, study the project's own documentation and source directly.
Develop a deep understanding of the project's purpose, architecture, major
responsibilities, relationships, user journeys and actual behaviour. Read relevant
implementation and tests; names and folder listings alone are not enough.
Distinguish intended behaviour from implemented behaviour and unresolved questions.

### Portable project context

Keep reusable understanding in `mebasic-src/context/project-context.md`. Before
initial generation, regeneration or maintenance, read this file if it exists.
If it is absent, build the understanding from project guidance and actual source,
then create it. Update it before finishing each generation or meaningful maintenance
run so a fresh worker can continue across authoring sessions and project moves.

Use concise prose to capture project purpose, architecture, important concepts and
relationships, accepted decisions and terminology, relevant source locations,
existing interpretation, unresolved questions and remaining work. Include evidence
and distinguish observed behaviour from intention. Use project-relative paths so
it remains portable. This is a working summary, not a transcript or a rigid schema;
do not copy credentials or unrelated private content into it.

The context supplements `AGENTS.md`, the current specification and actual source.
Read the instructions first, then check remembered understanding against current
files and corrections; update stale facts rather than carrying them forward.
Preserve user edits and record disagreements instead of overwriting them silently.
All context writes stay inside `mebasic-src/context/`. Continuity lives with the
project and does not depend on a particular authoring session or model provider.

## Generating an interpretation

After explicit user approval, understand the project as described above, create the
required folder structure, and populate each category with its own source-grounded
explanations. Begin with `modules/project_mod.md` and link the project's purpose,
users, concepts and responsibilities to their detailed descriptions.

Preserve existing user content and accepted terminology. Review the interpretation
for useful coverage, correct category placement, valid links and supporting source
references before declaring completion. Use judgment for explanatory content within
the required structure. Report unfinished work, uncertainty and unavailable real
screenshots clearly rather than inventing content or claiming completion.

## Accuracy

Ground explanations in actual source and keep supporting code paths current.
After coding, evaluate whether the interpretation has meaningfully changed. Skip
formatting, internal refactors and other trivial changes when existing descriptions
remain accurate. Changes to behaviour, concepts, responsibilities, relationships,
user terminology or constraints require updates to the affected descriptions and
links only. Judge meaning, not diff size; a one-line behaviour change can matter.
Generate an initial interpretation only after explicit user approval. Automatic
maintenance requires prior project authorisation. Never regenerate an
existing tree wholesale automatically. A user may explicitly confirm regeneration
to refresh the whole interpretation from current source while preserving user edits.
Distinguish observed behaviour from inference, intended behaviour and missing
implementation. Update affected descriptions when source changes; flag stale
or uncertain explanations instead of presenting guesses as facts.

## Guidance for the implementing LLM

- Read the project-local specification, existing `mebasic-src/` descriptions and
  `mebasic-src/constraints/` before working. Maintain the MeBasic interpretation of the
  current project.
  Work at the level of concepts and responsibilities, not individual functions.
- Read the relevant implementation in its actual locations before explaining it.
  After an implementation change, evaluate whether descriptions remain accurate;
  when needed, update only affected descriptions, relationships and supporting paths.
  Keep user-facing names consistent across `mebasic-src/`.
- Never overwrite user changes in `mebasic-src/`. Re-read each file immediately before
  editing and compare it with the last recorded baseline. Preserve user wording,
  names, additions and deletions; make small edits around them. If a change
  conflicts with the user's text, leave that text intact, record the conflict
  and ask for a resolution. Missing history means unknown authorship, not
  permission to regenerate a file. Never replace the whole tree from a template.
- Carry user renames, terminology changes and changed concepts into all affected
  `mebasic-src/` descriptions and links, whether the change came from an edited file,
  implementation change or conversation. Do not silently restore an old name.
  Preserve a requested behaviour as intent if the implementation does not yet
  match it, and explain the discrepancy. A description edit alone is not proof
  that the implementation changed or permission for unrelated code changes.
- Record the user's project instructions and corrections in relevant files under
  `mebasic-src/constraints/`, including their origin, scope and affected descriptions.
  Merge related rules without losing earlier context; explicitly mark superseded
  instructions. Keep requested rules distinguishable from verified enforcement.
- Keep an internal change record in `mebasic-src/.mebasic/change-history.jsonl` and a latest
  baseline index in `mebasic-src/.mebasic/change-state.json`. Retain exact baseline content
  under `mebasic-src/.mebasic/baselines/`. Record UTC time, actor (`user`, `llm` or `unknown`),
  evidence of authorship, paths and renames, before/after hashes, what changed,
  why, propagated descriptions, constraint updates and unresolved conflicts.
  Observe external edits before adding your own entry; do not attribute all
  uncommitted edits to yourself or assume every external edit was the user.
  Append history, preserve existing records, and advance baselines only after
  re-reading the saved result. If files change during reconciliation, stop that
  write and preserve both versions for review. This record supports careful LLM
  maintenance; recorded authorship must be supported by evidence.

## Primitives

- **Module**: a group of related work, like an endpoint group such as auth:
  its inputs, its processing as groups of statements, and its outputs.
- **Statement**: a single explanation of what happens inside a module, as
  one bullet: what comes in, what is done, what goes out, and a link to the
  algorithm it uses, when it uses one.
- **Object**: a concrete, encapsulated thing, like a class; its file
  describes its meaning, attributes and behaviour.
- **Flow**: a path data takes across modules, from what starts it to what it
  ends in.
- **Algorithm**: a reusable procedure that modules and flows use.
- **Constraint**: a group of rules: what must never happen, what happens
  instead, and where each rule comes from.
- **UI page**: a surface a person uses: a window, a panel, a command line,
  an API.
- **Datastore**: where data is kept: what is stored, how it is written and
  read, and how long it lasts.
- **Widget**: a meaningful UI element, such as a button, field or list,
  described through its purpose and interaction with the program.
- **Event**: an occurrence, such as a click, file change or completed task,
  that causes an object or module to respond.

## Organisation

Explain concepts as Objects and related work as Modules with clear
responsibilities. Describe encapsulation and relationships as they exist in the
source; this interpretation does not require rewriting the implementation.

## Example

In a text editor, `modules/project_mod.md` explains that users open, edit and
save documents, and links to `objects/document_obj.md` and `flow/save_flow.md`.
The Document description explains its text, unsaved changes and editing
behaviour. The Save flow follows the user's Save action through the file module
to disk, linking `constraints/externalchange_con.md` to explain why a conflicting
disk edit prevents saving. A Save Widget links its UI page to a Save-requested
Event, which leads to that flow. These example files are illustrative, not required
project concepts; create them before using actual Markdown links.

## IDE settings

The following optional presentation metadata is for editors and IDEs that display
MeBasic projects.
`hidden_folders` names folders to exclude from the MeBasic navigation view while
retaining them on disk for authoring. Implementations choose their own UI controls
and layout.

```yaml
hidden_folders:
  - context
  - .mebasic
```

---
code: []
---
