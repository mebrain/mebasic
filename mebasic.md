---
title: mebasic
version:
  revision: 11
  updated: "2026-10-11"
  summary: "Keep module discovery project-independent and grounded in the target source."
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

Every object, module, flow, UI page, widget, data description, event and algorithm
has its own file. Constraints instead use the two numbered lists defined below. Do not combine unrelated items into a catalogue
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

Start by identifying the core application's entry point: the boundary where an
outside caller engages its central capability and gets a result or effect back.
Describe that entry point as the first substantive module, using its actual name
in the code. Trace past process startup, GUI windows and UI event handlers to the
core heart of the application; a UI class or `main` function is not automatically
the module that owns the capability.

If the core has multiple independently callable entry points, keep them as separate
modules. Retain recognisable code names rather than replacing them with broad,
invented summaries. For each entry point, explain what it accepts, does and
returns. Discover names and boundaries from the project being described; do not
carry over module names, categories of responsibility or architectural assumptions
from another project.

For each entry point, identify and link the submodules that carry out its work.
Give each encapsulated submodule with a distinct responsibility its own module
file, preserving its code name and linking it back to the entry points that use
it. Follow meaningful boundaries, not every helper function or trivial structure.
Ground both the entry points and this decomposition in actual source and callers.
The project overview is an orientation page; it does not replace the first core
entry-point module or merge independent entry points into a generic controller.

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
  just its type. For example, "Health â€” how much damage the player can still
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

Constraints preserve what the architect has explicitly said and the limits that
follow from it. They guide both the project and the AI building it. They are not
an AI-generated catalogue of best practices or rules guessed from existing code.
Record only decisions about the project being described: its purpose, behaviour,
architecture, or architect-defined limits on implementing and maintaining it.
Identify that project before collecting statements. A message supplied by an IDE
is not automatically an architect statement: distinguish the architect's decisions
from instructions that merely operate the authoring tools. Generation requests,
MeBasic language/format rules, skills, model selection, agent permissions and
one-turn workflow instructions are not project constraints. Follow them while
working, but do not copy them into either list or derive prohibitions from them.
Rules from another project apply only when the architect explicitly adopts them
for this project. Project-specific decisions in its own instructions and decision
records may qualify; their provenance and relevance must both be clear. If either
is unclear, ask instead of creating a binding rule. For example, a decision that
the application must store data locally belongs here; an instruction to generate
its MeBasic descriptions with the selected model does not.

Use exactly two files in `mebasic-src/constraints/`:

- **`architect_said.md`:** significant things the architect has told us about this
  project: decisions, instructions and corrections. Record their meaning faithfully
  in simple English. Do not include assistant opinions, plans or results, passing
  remarks, or questions that decide nothing. Treat a relayed review as a relayed
  review, not an accepted instruction unless the architect accepts it.
- **`disallowed.md`:** things the project or its implementing AI must **not** do.
  Derive these limits from the statements and cite the supporting statement
  numbers. Mark inferred limits as inferred; never present them as the architect's
  own words. Do not invent a prohibition that the statements do not support.

Before initial generation and subsequent maintenance, look for prior architect
decisions throughout this project: architecture and design documents, `AGENTS.md`
or `agents.md`, their referenced guidance, decision/chat records, and relevant
source code and comments. Follow references to recover context and superseding
decisions. Do not rely only on the current conversation or the existing lists.
A comment or implemented behaviour alone does not prove the architect said it:
record an explicit decision only with attributable evidence, and label supported
inferred prohibitions as inferred. Cite the source location. Merge duplicates,
reconcile superseded decisions and surface unresolved contradictions. These
sources must pass the same project-relevance and provenance checks above.

**Architect Said** means significant things the architect said about this project.
**Disallowed** means "Do not do this": succinct prohibitions on building or
maintaining it, grounded in Architect Said entries.

Each file has front matter with `title` (Architect Said or Disallowed) and a one-line
`summary`, then a heading and a continuously numbered Markdown list, starting at
`1.`. No introductory paragraph or per-rule sections are needed. Write one short point per entry, usually one
sentence, with a compact date/source reference. Use actual increasing numbers,
not repeated `1.` markers. Do not create a separate file or long explanation for
each rule. If no entries are supported yet, leave the list empty and say so.

For example, Architect Said might contain:

1. Keep project data on the user's device. (Architect, 2026-10-11.)

Disallowed might contain:

1. Do not upload project data to a remote service. (Inferred from statement 1.)

On every project chat, apply the **mebasic-constraints** skill, especially when the
architect corrects the AI. Check for significant new statements and their implied
limits; append them in that same turn. The lists grow as decisions accumulate,
not just when code changes. Do not duplicate an existing point or manufacture a
new entry for every message. Read the current prohibitions before implementation.

Keep earlier attributable architect decisions and stable numbers. Re-check existing
LLM-authored entries against their sources; correct unsupported attribution rather
than treating an old entry as human approval. A later explicit change gets a new entry
identifying the statements or prohibitions it supersedes. Check each addition against
both lists. If the architect's instructions appear contradictory and the intended
replacement is unclear, show the relevant numbered points and ask one concise
question. Do not silently choose, weaken or invent a compromise. Mark the issue
unresolved and pause only work that depends on that decision. If the architect
has already explicitly resolved it, record that resolution without asking again.

Follow project opt-in and write permissions. Never create these files in an
unregistered project merely because chat occurred. In a read-only conversation,
raise contradictions and suggest entries without writing. Keep both lists concise;
they record architectural intent, not proof of implementation or enforcement.

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
run so the user's agent can continue across authoring sessions and project moves.

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

## Agent and model choice

The user's current agent generates and maintains MeBasic directly, using the
model and reasoning settings the user selected. When no model is selected,
use that agent's normal configured default. Keep the available conversation
context and the portable project context when continuing the work.

Do not delegate MeBasic generation, regeneration or maintenance to a separate
agent, external worker or cheaper model. MeBasic does not prescribe a model or
change the user's selection. Complete any needed description updates as part of
the same task, before the agent's final response. If the agent cannot complete
that work, explain what remains; do not silently substitute another agent.

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
Alternatively, the user may explicitly choose a fresh regeneration that discards
existing descriptions and edits, screenshots, portable context, restructuring notes
and generation history. Explain that deletion before asking the user to choose.
Human authorship travels with the Markdown file. Record only the actual text
inserted or replaced by the human; changing one character does not attribute a
paragraph, list, heading or whole document to them. Loading a file or an AI update
never makes its text human-authored. Editors highlight the precise human ranges
and hide provenance metadata from the editable text, selections and clipboard.

Store precise attribution in a terminal HTML comment on its own line:
`<!-- mebasic:human-ranges {"version":1,"ranges":[{"from":0,"to":4,"text":"Mine"}]} -->`.
Offsets are zero-based UTF-8 byte offsets, end-exclusive, into the Markdown before
the newline introducing this comment. Each sorted, non-overlapping range repeats
its exact text for integrity checking. Escape `<` as `\u003c` in JSON strings.
This metadata works for front matter and fenced code without changing their syntax.
When editing surrounding generated content, adjust the offsets and verify every
range against its text; never transfer attribution to unrelated text. Preserve
human text unless its deletion was explicitly approved. Metadata mismatches require
reconciliation against evidence, not guessed authorship.

Existing paired `<!-- mebasic:author=human -->` / `<!-- /mebasic:author -->` block
markers (or YAML `#` equivalents) remain readable. Hide these markers in editors.
Do not widen their attribution on further edits. Their original fine-grained human
edits cannot be recovered from the block alone; do not guess which words were human.
Never treat marker examples inside fenced code as attribution.

Regeneration has three explicit choices: continue in place; rebuild generated
content while retaining attributed human text at its original file paths; or
rebuild including deletion of human blocks. The last choice requires explicit
human selection. Rebuild with preservation leaves fragments for the agent to
complete into valid descriptions; preserve their exact wording and attribution.
Screenshots and generated history/context may be regenerated. Ask about conflicts
with retained human instructions rather than silently rewriting them.

Ordinary regeneration continues from the existing files and unfinished work.
Adapt and edit the interpretation in place, adding missing descriptions and
repairing links as understanding improves. This continuity does not certify the
old content: re-check every affected claim against the current project. Previous
LLM text and portable context are revisable drafts, not authority; never assume a
previous LLM knew something unavailable to the current one. Correct or replace
unsupported, stale or inaccurate generated explanations freely. Protect content
explicitly identified as human-authored or human-approved, with recorded provenance;
file existence, age or missing history is not a human-authorship marker. If protected
content conflicts with evidence, preserve it as intent and raise the discrepancy.
Only an explicitly chosen full refresh discards the interpretation and starts over.
Fresh regeneration rebuilds the interpretation from
current source; it does not restore deliberately discarded material from an earlier
conversation. It never authorises deleting implementation files, project identity,
IDE preferences, conversations, tasks or unrelated files.
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
- Re-read each file immediately before editing and compare with the last baseline.
  Preserve explicitly identified human wording, names, additions and deletions;
  record their provenance. Ask about conflicts with protected human content.
  Other existing text is a revisable interpretation: update it to the latest
  evidence without assuming prior LLM authority. Unknown authorship is not proof
  of human authorship. Preserve concurrent edits for review and never replace the
  whole tree from a template during ordinary regeneration. Explicitly approved
  full refresh may discard the interpretation as described above.
- Carry user renames, terminology changes and changed concepts into all affected
  `mebasic-src/` descriptions and links, whether the change came from an edited file,
  implementation change or conversation. Do not silently restore an old name.
  Preserve a requested behaviour as intent if the implementation does not yet
  match it, and explain the discrepancy. A description edit alone is not proof
  that the implementation changed or permission for unrelated code changes.
- Apply the constraints skill on every project chat, even when no coding or
  description update is needed. Record significant architect instructions in
  `constraints/architect_said.md` and supported prohibitions in `constraints/disallowed.md`,
  within `mebasic-src/`. Keep stable numbering, provenance and explicit supersession;
  surface unresolved contradictions to the architect.
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
    architect_said.md
    disallowed.md
  context/
  .mebrain/                 # restructuring notes when needed
  .mebasic/                 # private authoring records
```

Each description collection contains its own source-grounded content. Widgets
and screenshots are inside `ui/`; the other collections are top-level siblings.
Subfolders may organise a collection's own descriptions without relocating another
collection. These names and placements are required, not discretionary.

If a description collection genuinely does not apply, retain its folder with a
short Markdown explanation of why; never invent features to fill it. For constraints,
keep the two named lists and state when there are no supported entries. Screenshots
are real PNG/JPEG captures in `ui/screenshots/` and may be absent. `context/`
contains portable authoring memory, not user-facing descriptions. `.mebrain/`
contains restructuring notes, and `.mebasic/` contains private history and baselines.

Use one file per thing described, with a lowercase name and the primitive suffix:
`_ui.md`, `_widget.md`, `_mod.md`, `_obj.md`, `_flow.md`, `_store.md`, `_event.md`,
`_alg.md`. Constraints are the exception: use `architect_said.md` and `disallowed.md`.
Processing statements inside module descriptions are separate from the architect's
Architect Said list.
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

## Specification version

This specification's front matter contains a structured `version` record:
`revision` is a positive, monotonically increasing integer; `updated` is the
revision date in YYYY-MM-DD form; `summary` briefly describes the change.
When an author or AI makes an approved change to the specification, increment
`revision`, set the date and update the summary in the same edit. Compare revisions
numerically to identify newer specifications; do not use timestamps or file size
as version precedence. Keep the revision unchanged when copying the specification.
This versions the language document, independently of a project's configuration
format. Version maintenance does not authorise an AI to revise the language
without the architect's approval.

## IDE settings

The terminal `ide_settings` mapping provides presentation and generation settings for
consuming editors. `hidden_folders` lists folder names to omit from MeBasic navigation while
retaining their files for authoring. `folder_order` orders top-level collection
folders; other folders follow in alphabetical order. Widgets and screenshots are
shown under UI according to their on-disk placement. Editors choose their own
controls and layout.
`counted_lists` names description paths relative to `mebasic-src/` whose sidebar
labels include their number of top-level numbered entries, such as Architect Said (12)
and Disallowed (8) when Constraints is expanded. Count entries, including superseded
ones retained for history, not the highest number, examples or nested lists.

`generation_prompt` is the actual kickoff message submitted to the user's current
agent after the user approves generation or regeneration. Editors read this value
from the current specification on each request; they must not hardcode, infer or
silently substitute another kickoff prompt. If it is missing or unreadable, report
the problem before starting generation. The prompt assumes the agent's working
directory is `mebasic-src/`, with read access to the parent project and write access
limited to `mebasic-src/`. Editors establish that context before submitting it.
`human_generation_prompt` starts a rebuild around retained human blocks.
`fresh_generation_prompt` is the corresponding kickoff message when the user
explicitly chooses to discard the existing interpretation and start fresh. Read
and check the selected prompt before clearing any files. Both choices use the
user's current agent and model. Never substitute the preservation prompt when the
fresh prompt is missing. Editors clear only interpretation content and authoring
memory; they retain identity and unrelated runtime data. If an interpretation
location contains unexpected files or redirects elsewhere, stop before deleting.

---
ide_settings:
  counted_lists:
    - constraints/architect_said.md
    - constraints/disallowed.md
  generation_prompt: |-
    The user has explicitly approved MeBasic generation or regeneration. Generate
    the interpretation yourself in this turn, using the user-selected agent, model
    and reasoning settings. Do not delegate or create a worker handoff.
    Your working directory is mebasic-src; the project root is one directory above it.
    Read the project-root AGENTS.md (or agents.md), applicable nested instructions
    and their referenced guidance. Read .mebasic/mebasic.md and deeply understand
    the project purpose, architecture and actual source before generating descriptions.
    Read context/project-context.md if present and maintain it for future runs.
    Continue from existing files and unfinished work; adapt and edit in place.
    Re-verify prior LLM text and context against current evidence; correct them freely.
    Protect explicitly identified human-authored or human-approved content, not
    assumptions about what a previous LLM knew. Follow the current specification's
    required collection hierarchy and use actual source to decide useful descriptions
    within each category. Fill each collection with its own content as specified.
    Recover prior architect decisions from project architecture/docs, AGENTS.md,
    referenced records, source and comments; check attribution and supersession.
    Populate constraints only from architect decisions about this project. Do not
    record this generation prompt, MeBasic rules, skills or tool/agent instructions
    as project statements or prohibitions, or import decisions from another project.
    Review the interpretation for omissions and report unfinished work.
    Use only real screenshots. All MeBasic writes, including private history and
    baselines, must stay in this directory. Do not change implementation files.
    The editor owns the project configuration file with the .mebasic extension;
    never remove it or create another one.
  fresh_generation_prompt: |-
    The user explicitly chose Start fresh and approved discarding the existing
    interpretation and user edits. Generate
    the interpretation yourself in this turn, using the user-selected agent, model
    and reasoning settings. Do not delegate or create a worker handoff.
    Your working directory is mebasic-src; the project root is one directory above it.
    Read the project-root AGENTS.md (or agents.md), applicable nested instructions
    and their referenced guidance. Read .mebasic/mebasic.md and deeply understand
    the project purpose, architecture and actual source before generating descriptions.
    Create new context/project-context.md and new generation history for future runs.
    Rebuild from current source. Do not restore deliberately discarded descriptions,
    edits or context from earlier conversation. Leave unrelated retained files intact.
    Follow the current specification's
    required collection hierarchy and use actual source to decide useful descriptions
    within each category. Fill each collection with its own content as specified.
    Recover prior architect decisions from project architecture/docs, AGENTS.md,
    referenced records, source and comments; check attribution and supersession.
    Populate constraints only from architect decisions about this project. Do not
    record this generation prompt, MeBasic rules, skills or tool/agent instructions
    as project statements or prohibitions, or import decisions from another project.
    Review the interpretation for omissions and report unfinished work.
    Use only real screenshots. All MeBasic writes, including private history and
    baselines, must stay in this directory. Do not change implementation files.
    The editor owns the project configuration file with the .mebasic extension;
    never remove it or create another one.
  human_generation_prompt: |-
    The user approved rebuilding generated MeBasic content while preserving human
    text. Generate the interpretation yourself with the current user-selected agent,
    model and reasoning settings; do not delegate or create a handoff.
    Your working directory is mebasic-src; the project root is one directory above.
    Read project-root AGENTS.md (or agents.md), applicable nested guidance and
    referenced documents. Read .mebasic/mebasic.md and deeply understand the actual
    project architecture and source. Read retained context if any; re-verify it.
    Files now contain retained human fragments. Keep all marked human blocks and
    their exact wording at their original paths; fill in the generated content around
    them. Never infer human authorship from old LLM text or mark your output human.
    Follow the specification's collection hierarchy and explain every applicable
    collection from current source. Rebuild context/project-context.md and history.
    Recover architect decisions from this project's docs, architecture, AGENTS.md,
    source/comments and referenced records, checking attribution and supersession.
    Constraints describe only this project, never generation instructions, MeBasic
    rules, skills or another project's decisions. Ask about conflicting human text.
    Do not restore deliberately discarded AI text from earlier conversation.
    Review omissions and report unfinished work. Use only real screenshots.
    All writes stay in mebasic-src; implementation files remain untouched. The
    editor owns the root .mebasic configuration: never remove or duplicate it.
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
