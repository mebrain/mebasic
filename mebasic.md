---
title: mebasic
summary: An AI-native language for understanding a project and shaping its structure around meaningful concepts.
description: >
  mebasic explains source code in plain words and guides its organisation around
  meaningful controllers, objects and flows, independent of implementation language
  or IDE. This file defines the language. It is edited only by the architect,
  or with the architect's approval; an LLM never changes it on its own.
---
# mebasic

## Purpose

MeBasic is a plain-text language layered over any project's source code. It has
two purposes: explain the project in its own terms, and guide restructuring of
the project into the MeBasic way. It is independent of any editor, IDE, model
provider or implementation layout. It does not define how the program runs.

Start with what the project does, who uses it, and its main concepts. Explain
purpose and useful behaviour first, then let links lead to deeper detail.
Descriptions operate above individual functions: explain responsibilities,
relationships and journeys, without inventories of functions or call signatures.
The collection structure is mandatory; prose within it is flexible.

Before generating or regenerating descriptions, read the project's `AGENTS.md`
(or `agents.md`), applicable nested instructions and referenced guidance. Read
existing portable context, documentation, relevant implementation and tests.
Develop a deep understanding of purpose, architecture, responsibilities and actual
behaviour. Names and folder listings alone are not enough. If there are no agent
instructions, study project documentation and source directly.

## Collections: meaning and generation

Each collection is a distinct view of the same project. Choose meaningful concepts
from a deep reading of the source, and link the views rather than repeating them.
Explain implemented behaviour faithfully; distinguish proposed structure and
unresolved questions from what exists today.

### UI and widgets

The UI collection describes the pages of the application: what each page is for,
what users can understand or do there, and how actions, feedback and errors appear.
Discover pages from actual user journeys and interfaces. Link their events,
modules and objects so a reader can follow an action into the application.

Widgets are the smaller, ideally reusable parts of these pages. Describe their
purpose, displayed information, inputs and interactions, and link their containing
pages and relevant events. Group by meaningful UI responsibility, not every visual
component or implementation class. Widget descriptions belong inside UI.

Real screenshots also belong inside UI. A page's front matter can point to its
relevant capture. Use verified project screenshots, with provenance and capture
date when useful; omit the reference when no verified image is available.

### Modules

Modules are the controllers of the application. Think of a module as an API with
meaningful input paths: each path receives some input, performs a responsibility
and returns a result. These paths can originate in UI actions, events, other
modules or external systems; they need not be network endpoints.

Determine controller boundaries by deeply understanding how the project works.
One module may correspond to one source file or a small collection of files.
Group related operations by coherent responsibility, ownership and rules, rather
than mechanically mapping directories or creating a module for every file.

For each module, describe:

- **Inputs:** its input paths, what each accepts, where the input comes from,
  and the result that path returns, including meaningful failure outcomes.
- **Processing:** the rules it preserves and how it coordinates objects, data,
  algorithms and other modules. Group related work into readable statements.
- **Output:** the useful results and effects, linked back to the paths that
  produce them and the flows that use them.

A statement is a bullet explaining a meaningful step: what comes in, what is
done and what goes out. Link an algorithm when it explains the step. Statements
stay within modules; they are not individual functions or separate files.

`modules/project_mod.md` introduces the whole project's purpose, users, main
concepts and controller groups, linking to their descriptions.

### Objects

Objects are the major encapsulated classes or models of the project: meaningful
concepts that can have instances, attributes and behaviour. In an object-oriented
project, find the large logical groupings rather than listing every class. The
same concepts can be represented across files in a non-object-oriented project.

A Player is a meaningful object; a trivial Rect or a structure used only to bundle
parameters is not. Identify the concept's identity, lifecycle, ownership, useful
behaviour and rules. Describe its **Attributes** and **Behaviour**, and link the
modules that operate on it and the other objects it relates to.

Aim for object-classes that express the project's major instantiable concepts
clearly. Map each concept to the actual implementation, even when that
implementation is scattered or poorly encapsulated. If a better grouping would
serve the MeBasic structure, record the proposal in restructuring notes and
identify it as a proposal, rather than claiming the source already has that design.

### Flow

Flow views the project as a factory: what comes in, how it is changed, and what
comes out. It sits above code structure and follows a meaningful journey through
the application, rather than reproducing its call graph.

Identify the starting input or trigger, the transformations and decisions, and
the useful output or failure outcome. For each stage, link the MeBasic modules
and objects involved. Explain how information and responsibility pass between
them so the reader can understand the whole process independently of file layout.

### Data

Data describes the project's information structures and where they are stored:
databases, schemas, persisted models, records, files, caches and relevant in-memory
representations. Discover what information exists, how it is shaped, who reads or
changes it, and where it lives. Explain lifetime, persistence, loss and recovery
where relevant.

An object describes an encapsulated domain concept; a data description explains
its representation and storage. A model can participate in both views. Link them
and explain their different responsibilities. Data descriptions use the Datastore
primitive and belong in `data/`.

### Events

Events are occurrences that trigger the application and the reactions they cause.
They can originate with users, external systems or internal work completing.
Identify the trigger, accompanying information, responding modules or objects,
and observable result. Link the flow it starts or continues. Explain meaningful
reactions rather than enumerating individual handler functions.

### Algorithms

Algorithms explain significant procedures used by modules and flows. Identify the
problem, inputs, important steps, result and limitations. Describe enough reasoning
to understand the procedure, and link the concepts that use it; avoid translating
implementation code line by line.

### Constraints

Constraints describe rules the project must respect. Explain the rule, its origin
and reason, its scope, and what happens when it cannot be met. Link affected
concepts. Include accepted user instructions and corrections, and distinguish a
requested rule from verified enforcement in the source.

## Restructuring the project

MeBasic also aims to bring the implementation into a clearer organisation around
its controllers, major objects and flows. While understanding the project, look
for better responsibility boundaries, encapsulation and relationships. Record
these in `mebasic-src/.mebrain/restructuring.md` when improvements are identified.

Explain the current structure, proposed structure, reason for the improvement,
affected source locations and concepts, and relevant tradeoffs or open decisions.
Keep accepted, proposed and completed changes distinguishable. Preserve earlier
notes and user decisions. Descriptions can explain the ideal concept while making
its current implementation and any gap explicit.

Generating descriptions and restructuring notes does not itself authorise changing
implementation code. Carry out implementation restructuring when the user requests
or approves that work, then update the affected descriptions to reflect the result.

## Portable project context

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

## Folder structure

MeBasic files live in `<project>/mebasic-src/`. The implementation can use any
language or layout; supporting code paths name its actual files relative to the
project root. Create the following exact lowercase folders and hierarchy:

```text
mebasic-src/
  <name>.mebasic
  ui/
    widgets/
    screenshots/
  modules/
  objects/
  flow/
  data/
  events/
  algorithms/
  constraints/
  context/
  .mebrain/                 # restructuring notes when needed
  .mebasic/                 # private authoring records
```

Each description collection contains its own source-grounded content. Widgets
and screenshots are inside `ui/`; the other collections are top-level siblings.
Subfolders may organise a collection's own descriptions without relocating another
collection. These names and placements are required, not discretionary.

If a description collection genuinely does not apply, retain its folder with a
short Markdown explanation of why; never invent features to fill it. Screenshots
are real PNG/JPEG captures in `ui/screenshots/` and may be absent. `context/`
contains portable authoring memory, not user-facing descriptions. `.mebrain/`
contains restructuring notes, and `.mebasic/` contains private history and baselines.

Use one file per thing described, with a lowercase name and the primitive suffix:
`_ui.md`, `_widget.md`, `_mod.md`, `_obj.md`, `_flow.md`, `_store.md`, `_event.md`,
`_alg.md` or `_con.md`. Statements are bullets within modules. Do not add README
or index descriptions: each description is its own read-me. The project entry point
is `modules/project_mod.md`.

## File format

Description files normally follow this order:

1. **Front matter**, between `---` lines:
   - `title`: the human-readable name of what is described.
   - `summary`: one sentence, at most 160 characters.
   - `description: >` followed by indented plain-word sentences.
   - For a UI page, optional `screenshot: ui/screenshots/settings.png`, relative
     to `mebasic-src/`, pointing to an existing PNG/JPEG inside `ui/screenshots/`.
2. **`# Title`**, matching the front-matter title.
3. **The body**, explaining the concept using its collection guidance above.
4. **Bottom matter**, between `---` lines at the very end: `code:` followed by
   an indented YAML list of supporting implementation paths relative to the project
   root. Use `code: []` when implementation evidence is absent.

The title names the concept for readers; the filename and suffix identify the
file and its primitive. Links between descriptions use relative paths to existing
Markdown files. Link where it helps explain ownership, use, dependencies or data
movement. Keep supporting evidence distinct from proposals.

## IDE settings

The terminal `ide_settings` mapping provides presentation metadata for consuming
editors. `hidden_folders` lists folder names to omit from MeBasic navigation while
retaining their files for authoring. `folder_order` orders top-level collection
folders; other folders follow in alphabetical order. Widgets and screenshots are
shown under UI according to their on-disk placement. Editors choose their own
controls and layout.

---
ide_settings:
  hidden_folders:
    - context
    - .mebasic
    - .mebrain
  folder_order:
    - ui
    - modules
    - objects
    - flow
    - data
    - events
    - algorithms
    - constraints
code: []
---
