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

### Shared opening and reading pattern

Every object, module, flow, UI page, widget, data description, event, algorithm
and constraint has its own file. Do not combine unrelated items into a catalogue
file. Begin every description with the same readable opening:

1. **Name:** a clear, human-readable title identifying the item.
2. **One-line summary:** one succinct sentence saying what it does or represents.
3. **One paragraph:** explain why it exists, its role in the project and how it
   relates to the larger picture. Assume the reader has not read the source code.

Follow this opening with the category sections below. Use the given section names
and order as reading templates; modules use the flexible service-contract guidance
below. Omit sections that add no useful information rather than padding them with
empty headings. Keep explanations short and concrete.
Use bullets for parallel facts, numbered steps for sequences, and tables when they
make inputs, attributes or relationships easier to compare. Do not list functions,
method signatures or incidental implementation details.

Link related descriptions where they are discussed so readers can move between
views naturally. Keep supporting source references at the bottom. Clearly label
proposed improvements and link relevant restructuring notes; do not present them
as implemented behaviour. The file-format section below defines the metadata.

### UI pages

The UI collection describes the pages of the application: what each page is for,
what users can understand or do there, and how actions, feedback and errors appear.
Discover pages from actual user journeys and interfaces. Link their events,
modules and objects so a reader can follow an action into the application.

Real screenshots also belong inside UI. A page's front matter can point to its
relevant capture. Use verified project screenshots, with provenance and capture
date when useful; omit the reference when no verified image is available.

After the shared opening, present:

- **Screenshot:** the real page capture when available, with a useful caption.
- **Purpose:** the task the user comes here to accomplish.
- **Actions:** what the user can do and the result of each action; link events
  or flows where useful.
- **Feedback and states:** meaningful loading, empty, success and error states
  that actually exist, and how the user can respond.
- **Widgets:** the meaningful components of the page, linked to their own files.

### Widgets

Widgets are the smaller, ideally reusable parts of UI pages. Group by meaningful
UI responsibility, not every visual component or implementation class. Each widget
has its own description inside UI and links back to its containing pages.

After the shared opening, present:

- **Displays:** the information or state the widget shows.
- **Interactions:** what users can do, what input is accepted and what changes.
- **Used by:** the pages that contain it and meaningful variations in use.
- **Events:** what it produces or responds to, linking relevant events, modules
  and objects.

### Modules

Think of a module description as a microservice or API specification: it explains
what a coherent part of the application offers to its callers and what they can
rely on. Modules organise controller responsibilities around meaningful capabilities,
with a clear boundary, purpose and public contract.

Find these boundaries by deeply understanding the project. Group capabilities
that belong together through responsibility, ownership and rules. A module may
be implemented by one file or a small collection of files. Its boundary is logical:
it does not require a separate process, deployment or network API. Describe actual
interfaces and interactions; do not invent HTTP endpoints or a microservice
architecture for a project that does not have them.

Keep the shared name, one-line summary and introductory paragraph. Then organise
the description around the capabilities the module exposes, in whatever layout
makes its contract clearest. Short operation sections, examples, prose or a table
can all work. There are no mandatory module headings, column names or section
order after the opening.

Help the reader understand:

- What responsibility the module owns, the capabilities it offers, and who uses
  them. Explain boundaries with neighbouring modules where that clarifies ownership.
- For each meaningful operation or input path, what the caller asks for or sends,
  what result or effect to expect, and the conditions and failures that matter.
  A response can be immediate, an event or later work; describe the actual behaviour.
- Relevant promises and rules: for example, whether an operation changes stored
  state, requires permission or can be repeated safely. Include these only when
  they help explain this module's real contract.
- The objects, data and other modules it owns or depends on, linked where they
  explain how to use the capability or understand its effects.

These are questions to answer, not fields to fill. Describe meaningful public
capabilities rather than listing every function or method signature. Explain
internal processing only when needed to understand an observable result or rule;
link algorithms and flows for deeper detail. Brief statements can explain useful
steps inside the description without becoming a required processing inventory.

For example, a document service might offer Open, Save and Watch for changes.
Its Save description would explain the document and destination it accepts,
what successful saving means, how conflicts are reported, and any resulting
notification. This illustrates a contract; use the capabilities and behaviour
actually supported by the project.

`modules/project_mod.md` introduces the whole project's purpose, users, main
concepts and service responsibilities, linking to their descriptions.

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

After the shared opening, present:

- **Example:** one concrete instance or situation that makes the object tangible.
- **Attributes:** what it holds, explaining each attribute's meaning rather than
  just its type. For example, "Health — how much damage the player can still
  withstand."
- **Behaviour:** the useful things it can do and how they affect its state.
- **Ownership and lifecycle:** who creates or owns it, what it owns, and when it
  is changed, completed or destroyed.
- **Rules:** important conditions it preserves, such as health never falling
  below zero, where supported by the project.
- **Relationships:** linked objects and modules, explaining how they participate.

For example, an illustrative object description begins like this; adapt the facts
to the actual project rather than copying an invented game model:

```markdown
# Player

Represents one participant in a match and tracks their progress.

A Player holds the state that belongs to one participant for the duration of a
match. It connects that participant's identity, current health and score so the
match controller can apply actions and determine the outcome. Its state lasts
until the match ends, when the results are recorded.

## Example

The participant you control during a match is one Player instance.

## Attributes

- **Health:** how much damage the player can still withstand.
- **Score:** the points the player has earned in this match.
```

Continue with the applicable sections above and supporting source references.

### Flow

Flow views the project as a factory: what comes in, how it is changed, and what
comes out. It sits above code structure and follows a meaningful journey through
the application, rather than reproducing its call graph.

Identify the starting input or trigger, the transformations and decisions, and
the useful output or failure outcome. For each stage, link the MeBasic modules
and objects involved. Explain how information and responsibility pass between
them so the reader can understand the whole process independently of file layout.

After the shared opening, present:

- **What comes in:** the starting input or trigger and any necessary conditions.
- **Transformation steps:** a numbered sequence showing how input becomes output.
  Each step states what changes and links the modules and objects involved.
  Include meaningful branches or failure paths where they occur.
- **What comes out:** the useful result, destination and observable outcome,
  including how the flow ends when it cannot complete.

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

After the shared opening, present:

- **Structure:** the meaningful records, fields or relationships and what they
  represent; group detail around the information the reader needs to understand.
- **Storage:** where the information lives and in what form.
- **Readers and writers:** linked modules or objects that read, create or change it.
- **Lifetime:** when it is created, how long it survives, and how it is removed.
- **Loss and recovery:** relevant behaviour when information is missing, invalid
  or unavailable, where supported by the project.

### Events

Events are occurrences that trigger the application and the reactions they cause.
They can originate with users, external systems or internal work completing.
Identify the trigger, accompanying information, responding modules or objects,
and observable result. Link the flow it starts or continues. Explain meaningful
reactions rather than enumerating individual handler functions.

After the shared opening, present:

- **Trigger:** the occurrence that produces the event and where it originates.
- **Information carried:** the meaningful information available to responders.
- **Response:** the modules or objects that react and what they do.
- **Outcome:** the observable result and links to flows started or continued.

### Algorithms

Algorithms explain significant procedures used by modules and flows. Identify the
problem, inputs, important steps, result and limitations. Describe enough reasoning
to understand the procedure, and link the concepts that use it; avoid translating
implementation code line by line.

After the shared opening, present:

- **Problem:** what the procedure solves and why it is needed.
- **Inputs:** the information and conditions it needs.
- **Steps:** a numbered explanation of the significant decisions and transformations.
- **Result:** what it produces, linking the modules or flows that use it.
- **Limitations:** assumptions, tradeoffs and known cases it does not handle.

### Constraints

Constraints describe rules the project must respect. Explain the rule, its origin
and reason, its scope, and what happens when it cannot be met. Link affected
concepts. Include accepted user instructions and corrections, and distinguish a
requested rule from verified enforcement in the source.

After the shared opening, present:

- **Rule:** a direct statement of what must or must not happen.
- **Reason and origin:** why it matters and the instruction, decision or external
  requirement it comes from.
- **Applies to:** linked concepts and the conditions under which the rule matters.
- **Enforcement:** how the implementation currently upholds it, or an explicit
  statement that enforcement is incomplete or unverified.
- **When unmet:** the actual or required response when the rule cannot be met,
  distinguishing observed behaviour from intent.

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
`_alg.md` or `_con.md`. Statements, when useful, stay within module descriptions.
Do not add README or index descriptions: each description is its own read-me. The project entry point
is `modules/project_mod.md`.

## File format

Description files normally follow this order:

1. **Front matter**, between `---` lines:
   - `title`: the human-readable name of what is described.
   - `summary`: one sentence, at most 160 characters.
   - `description: >` followed by the one-paragraph introduction as indented text.
   - For a UI page, optional `screenshot: ui/screenshots/settings.png`, relative
     to `mebasic-src/`, pointing to an existing PNG/JPEG inside `ui/screenshots/`.
2. **`# Title`**, matching the front-matter title.
3. **The body**, beginning with the one-line summary and one-paragraph introduction,
   matching the front matter, followed by the applicable category sections above.
   This keeps the opening readable when metadata is hidden or displayed separately.
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
