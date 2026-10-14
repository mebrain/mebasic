---
title: mebasic
version:
  revision: 29
  updated: "2026-10-14"
  summary: "Adds a Writing style section, the module sections What it does, Vocabulary and Where it happens, and lets corrected or unpinned text lose human attribution."
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
Write everything the way the Writing style section below describes.

Descriptions operate above individual functions: explain responsibilities,
relationships and journeys, without inventories of functions or call signatures.
The collection structure is mandatory; prose within it is flexible.

Before generating or regenerating descriptions, read the project's `AGENTS.md`
(or `agents.md`), applicable nested instructions and referenced guidance. Read
existing portable context, documentation, relevant implementation and tests.
Develop a deep understanding of purpose, architecture, responsibilities and actual
behaviour. Names and folder listings alone are not enough. If there are no agent
instructions, study project documentation and source directly.

Also read the project's existing folder structure as evidence of how its authors
organised the code. Folders often group related concepts on purpose, for example
`shapes/` holding several kinds of shape, or `payments/` holding everything about
payment. Where a folder reflects such a meaningful grouping, mirror it: use a
subfolder of the same name within the matching collection, or a broader module
with its specialisations, and let it inform the creation hierarchy where the code
confirms it. Do not copy purely technical folders such as `src/`, `lib/`, `build/`
or `test/`, and do not let source folders override the collection structure below.

## Writing style

MeBasic is read by people who are new to the project, and many of them are not
programmers. Every description should make sense to such a reader the first time
they read it. This section explains how to write for them, and it applies to every
collection.

### Use plain words

Choose everyday words and keep sentences short. When a technical term cannot be
avoided, explain it in a few words the first time it appears in the file, even if
another description already explains it, because the reader may have arrived from
anywhere. A concrete example usually teaches more than an abstract definition, so
give one whenever it helps the reader picture what is happening.

### Write whole sentences

Write the way a patient colleague explains something out loud. Each sentence should
have a subject and a verb and should connect to the sentences around it, so that a
paragraph reads as one line of thought. Do not write in a clipped, note-taking style
that drops the words which join ideas together. For example, write "The store checks
that every order has a customer before it saves the order" rather than "Checks
customer. Saves order." Short sentences are welcome, but fragments and strings of
labels are not, and a reader should never have to guess how two statements relate.

The same applies inside lists. Each bullet or numbered step should be a complete
sentence that makes sense on its own. When a few ideas belong together, a short
paragraph usually explains them better than a list of fragments.

### Give the text a helpful shape

Use bullet points for facts that sit side by side, numbered steps for things that
happen one after another, and tables when they make inputs, attributes or
relationships easier to compare. Keep each explanation short and concrete. When a
section would have nothing useful to say, leave it out rather than filling it with an
empty heading or a sentence that only says there is nothing to add.

## Collections: meaning and generation

Each collection is a distinct view of the same project. Choose meaningful concepts
from a deep reading of the source, and link the views rather than repeating them.
Explain implemented behaviour faithfully; distinguish proposed structure and
unresolved questions from what exists today.

### Shared opening and reading pattern

Every object, module, UI page, reusable widget, data description, event and
algorithm has its own file. A widget used on only one page is described inside that
page instead (see Widgets). Constraints instead use the two numbered lists defined below,
and Flow uses its own fixed files (see Flow). Do not combine unrelated items into a catalogue
file. Begin every description with the same readable opening:

1. **Name:** a clear, human-readable title identifying the item, as a `#` heading.
2. **One-line summary:** one succinct sentence saying what it does or represents.
3. **One paragraph:** explain why it exists, its role in the project and how it
   relates to the larger picture. Assume the reader has not read the source code.
   Give enough context that the reader understands where the item fits in the
   whole application: which part of the user's work it serves, what uses it and
   what it relies on.

Write every title the same way, in PascalCase, the casing MeBasic uses for all
names: one word with each part starting with a capital and no spaces, dots or
underscores, such as `OrderProcessing` or `ProjectOverview`. Convert a code name to
this form: `order_store` becomes `OrderStore`, `report.export` becomes
`ReportExport`, and `InvoiceBuilder` stays `InvoiceBuilder`. Never start a title with a
lowercase letter or write it all in capitals; write acronyms as one capital
followed by lowercase letters inside a name, such as `HttpApp` or `ApiGateway`.
When the code name differs from the title, record it in the front matter as
`code_name` and mention it in the opening so readers can find it in the source.
Every description has a `title` in its front matter, so editors never fall back to
the lowercase filename.

Use Markdown headings for structure: one `#` heading for the title, `##` for each
section and `###` beneath a section. Do not use bold text, underlined text or
setext-style `===`/`---` lines as headings, and do not skip heading levels.

Follow this opening with the category sections below. Use the given section names
and order as reading templates; modules use the fixed sections and service-contract
guidance described under Modules. Write every section as the Writing style section
describes. Do not list functions, method signatures or incidental implementation
details; a module's API and Internal Functions sections, which name its entry points
and significant internal functions, are the one exception.

Link related descriptions where they are discussed so readers can move between
views naturally. Keep supporting source references at the bottom. Clearly label
proposed improvements and link relevant restructuring notes; do not present them
as implemented behaviour. The file-format section below defines the metadata.

### Creation hierarchy

The creation hierarchy records who creates what, and at what depth. A concept that
nothing else in the project creates is at hierarchy 1. A concept created by a
hierarchy 1 concept is at hierarchy 2, and so on. For example, if a `Car`
instantiates an `Engine` and the engine instantiates `Pistons`, then Car is 1,
Engine is 2 and Pistons is 3.

Record this in the front matter of modules and objects: `hierarchy` gives the depth,
and `created_by` names the description of the concept that creates it. Leave
`created_by` out at hierarchy 1. When several concepts create the same one, name
the creator that owns its lifetime, usually the one that keeps it, and give the
depth through that creator. Leave both fields out when the source does not create
the concept at runtime, such as a stateless helper or a static library.

The hierarchy is separate from folders. Folders group related concepts, such as a
broader module and its specialisations; the hierarchy follows creation, which can
cross folders and collections. A specialisation keeps its own place in the
hierarchy. Base both fields on construction actually found in the source, not on
names or on which file mentions which.

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
- **Sketch:** a small ASCII sketch, in a `text` code block, of how the page looks:
  its main regions and where each widget sits, labelled with the widget names used
  below. The page that frames the whole application sketches the overall app
  window. Draw the actual arrangement, not a proposal; keep it schematic, not
  pixel-accurate. It complements a screenshot and is still required when none exists.
- **Purpose:** the task the user comes here to accomplish.
- **Actions:** what the user can do and the result of each action; link events
  or flows where useful.
- **Feedback and states:** meaningful loading, empty, success and error states
  that actually exist, and how the user can respond.
- **Widgets:** a listing of every meaningful widget on the page, with a short note
  on its role here. Link reusable widgets to their files in `ui/widgets/`. Describe
  a page-specific widget directly in this listing: what it shows and what users can
  do with it. It gets no file of its own.

### Widgets

Widgets are the smaller parts of UI pages. Group by meaningful UI responsibility,
not every visual component or implementation class. Only REUSABLE widgets get their
own description: one that is used on more than one page, or that the implementation
deliberately provides as a shared component. These files live in the `ui/widgets/`
subfolder and link back to the pages that contain them.

A widget that is not reusable is simply listed, and described, in the page it is
created on; do not give it a file in `ui/widgets/`. When a page-specific widget
becomes reused, move its description into its own widget file and link it from each
page. When a reusable widget is left on only one page, fold it back into that page.

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

#### The App module

Application plumbing always belongs in one module, `modules/app_mod.md`, titled
App. Plumbing is the code that starts, wires and hosts the application rather
than providing its capabilities: `main` and other process entry points,
`Application` or `App` classes, HTTP or web app setup such as an `HttpApp`, server
or framework bootstrap, routing tables, dependency wiring, configuration loading,
and startup and shutdown. Describe all of it together in the App module, and never
split any of it into standalone modules, however many files or classes it spans.

The App module explains how the application starts, what it creates and connects,
how it is hosted and configured, and how it shuts down, linking the core modules it
starts or hands requests to. This fixed name is a deliberate exception to taking
module names from the project. The core capabilities themselves keep their own
modules; do not merge them into App because App starts them.

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

#### Module sections

Every module description has the same sections, in this order, so that a reader
always knows where to look:

1. **Overview.** This is the shared opening: the title, the one-line summary and the
   introductory paragraph. It has no heading of its own. The introduction says what
   the module does and how it fits into the whole application, in simple words and
   before any detail: which part of the application it serves, which modules or
   screens use it, and what it relies on.
2. **`## API`.** The entry points that other parts of the project call, as described
   below.
3. **`## Internal Functions`.** The module's own significant functions, as described
   below. Leave it out when nothing is worth naming.
4. **`## What it does`.** A list that walks through what the module actually does,
   step by step, in the order it happens. Write each step as one plain sentence that
   anyone could follow, such as "It checks that the order has at least one item, and
   refuses it with a message if it does not." Describe the real behaviour found in the
   source, not the names of the functions that carry it out. Number the steps when
   they always happen in that order; use plain bullets when the module offers several
   separate jobs, and give each job its own short run of steps.
5. **`## Vocabulary`.** An explanation of every word in this description that a newcomer
   might not know. This includes the project's own terms, such as a name the team
   invented for one of its techniques, as well as technical terms and the names of other
   concepts in the project. Explain each word again here, in a sentence or two, even
   when another description already defines it, so the reader never has to leave the
   page to understand it. Then link to the description that defines it or, when no
   description does, name the source file that deals with it most. For example, if the
   project calls one of its search steps "q-star", every module that mentions q-star
   says briefly what it is and links to where it is explained in full. Write each entry
   as a bullet that starts with the word in bold, followed by ` — ` and the
   explanation. Leave the section out only when every word in the description is
   everyday language.
6. **`## Where it happens`.** A short explanation of where this module sits in the
   running application. Say whether it is at the top, where the application starts,
   or several levels deep; what creates it and when, matching `hierarchy` and
   `created_by` in the front matter; what calls it and at which point in a user's
   journey or in the program's run; and how long it lives. Write this as prose, ground
   it in the construction and calls actually found in the source, and link the
   modules and screens involved.

After these sections, describe the rest of the module around the capabilities it
offers, in whatever layout makes its contract clearest. Short operation sections,
examples, prose or a table can all work, and there are no further mandatory
headings or column names.

#### API and Internal Functions sections

Every module has a section headed exactly `## API`, placed straight after the
introduction and before any other section. It lists the entry points
through which other parts of the project call into the module: the public
functions, methods, commands, routes, messages or events it accepts from outside.

Directly below it comes a section headed exactly `## Internal Functions`. It lists
the module's own significant functions that only the module itself calls, such as
helpers that do a meaningful step of its work. Leave out trivial helpers. A module
whose internal functions add nothing worth naming leaves this section out.

Never list the same function in both sections. Editors extract both lists, so each
entry keeps one fixed shape:

```markdown
## API

- `Evaluate` — Calculates the current expression and returns its value.
- `CreateVar` — Defines a named variable that later expressions can use.
- `POST /orders` — Accepts a new order and replies with its number.

## Internal Functions

- `Tokenize` — Splits an expression into numbers, names and operators.
```

Each entry is one top-level bullet. It starts with the entry point's name exactly as
callers use it in the source, in backticks, then ` — ` and one sentence saying what
calling it does. Use the name only, without parameter lists or types; explain inputs,
results and failures elsewhere in the description where they matter. Indented
bullets under an entry may add short notes. Order entries the way a caller meets
them, most important first. A module with no outside callers keeps the heading with
the line `No entry points: only this module calls its code.` and no bullets.

Name the module's general entry points in the front matter as `entry_methods`: the
one to three API entries that callers normally use first or most, through which
the rest of the module is usually reached. Use names exactly as they appear in the
API section. Editors list these first and mark them as entry points.

#### Application entry points

Identify the application's very first entry points: where the program starts
running or is first called from outside. Typically this is the App module holding
`main` or the web server start-up, plus any separately started process, command-line
command, scheduled job or service the outside world calls first. Mark each of these
modules with `entry_point: true` in the front matter. A small application usually
has one; do not mark ordinary modules that are only reached through others.

#### Base types and interfaces

When a module or object describes an interface, an abstract class, or a base class
that several others inherit from or implement, record that in the front matter as
`abstraction: interface`, `abstraction: abstract` or `abstraction: base`. Its
description explains the shared contract and links the types built on it. Editors
show these with a distinct icon.

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

These are questions to answer, not fields to fill. Outside the API section, describe
meaningful public capabilities rather than listing every function or method signature. Explain
internal processing only when needed to understand an observable result or rule;
link algorithms and flows for deeper detail. Brief statements can explain useful
steps inside the description without becoming a required processing inventory.

For example, a document service might offer Open, Save and Watch for changes.
Its Save description would explain the document and destination it accepts,
what successful saving means, how conflicts are reported, and any resulting
notification. This illustrates a contract; use the capabilities and behaviour
actually supported by the project.

#### Specialised modules

When several modules are more specific versions of one broader concept, describe
the broader concept as its own module and group the specialisations in a folder
beside it, named after it. For example, if the project has `Robot` with the
specialisations `MovingRobot` and `FlyingRobot`:

```text
modules/
  robot_mod.md
  robot/
    moving_robot_mod.md
    flying_robot_mod.md
```

Each specialisation keeps its own file and its code name, and its front matter
names the broader module in `parent`. The broader module explains what all its
specialisations share, such as the common contract, rules and responsibilities,
and links each specialisation. A specialisation describes what it adds or changes
and links back to the broader module rather than repeating it. Specialisations
of a specialisation nest the same way, one folder level per step.

Group modules this way only when the source supports the relationship, for example
through inheritance, implementations of a shared interface, or variants of one
role. A similar name alone is not enough, and a submodule that merely helps
another stays a sibling with links. Keep the broader module where existing links
already point; adding a specialisation must not move it.

`modules/project_mod.md` introduces the whole project's purpose, users, main
concepts and service responsibilities, linking to their descriptions. It is the
overview file and always comes first: its title is exactly `Overview - ` followed by
the project's name, such as `Overview - PaintLine`. This is the one title that
does not follow the PascalCase rule. Editors list it first among the modules.

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

Flow views the project as a factory: what data comes in, how it is changed, and
what comes out. When it is done, a reader has a clear picture of what data the
application receives and how that data moves through the system. Flow is the one
collection with a fixed set of files: `flow/boxes.json` for the kinds of data that
come in and the places data can pass through, `flow/data.json` for how one example
of each kind moves step by step, and `flow/inputs.md`, a Markdown list of the
discovered inputs that the user extends with requests and notes. Each kind of
data is shown on its own: choosing it in the editor draws only its pathway, and a
timeline walks that one piece of data through it, showing how it is transformed at
each step. Kinds of data can share boxes and then diverge; each pathway is still
drawn separately.

For example, a paint line takes a `Bucket` through BucketReader, ItemPositioner,
BucketPainter, ItemQC and Deposit, and a `Can` through ItemPositioner, CanPainter,
CanStencil, ItemQC and Deposit. Bucket and Can are the two inputs; both pass
through ItemPositioner, ItemQC and Deposit, but each is viewed as its own pathway.

Build the flow in this order:

0. **Read `flow/inputs.md` first.** Its `## Requests` section lists kinds of data the
   user wants shown as their own inputs, and `## Notes` holds the user's own thoughts
   about the flow. Treat both as instructions for this generation: for example, a
   request for `Color` means colour should become an input with its own pathway when
   the code processes colours. Never rewrite or remove the user's requests or notes.
1. **Find the independent data pieces.** Identify every distinct kind of data the
   code processes that comes from outside it, such as an uploaded document, a typed
   command, a sensor reading or a scheduled tick. Define each one by what the data
   is, not by how it arrives. When the same data can come in through two routes,
   such as an HTTP API and a command line, it is one input: ignore the plumbing and
   describe the data. These become the `inputs`.
2. **Trace each piece through the code and its configuration.** Follow the piece
   from where it enters to where its useful result or failure ends up, noting each
   place where it is processed separately and something distinct happens to it.
   Those places are the boxes. Follow the data into configuration as well as code:
   when stages are defined in data, such as a pipeline definition, a list of
   processing steps, a rule table or a chain of handlers read from a file, each configured stage the
   data passes through is a place of its own, even though one generic engine runs
   them all. Record only the big moves: leave out trivial functions, helpers, simple
   pass-throughs and plumbing. The flow documents the shape of the work, not every
   call; it is not exhaustive.
3. **Make boxes per step, not per class, module or component.** A box is one separate
   piece of processing that does something distinct, not a class, module, object
   type or grouping. If one class is created five times and the data passes through
   each instance separately for a different purpose, that is five boxes. A series of
   configured stages inside one component is one box per stage, not one box for the
   component. If the same code runs repeatedly doing the same thing, that is one
   box that the data reaches again.
4. **Show switched-off stages.** When the design includes stages the data would
   pass through but configuration or a setting currently switches them off, keep
   them in the pathway in their designed place and mark their boxes `off`, with a
   short `offReason` naming where they are switched off. Describe what the data
   does today faithfully: off stages are shown, never presented as running.
5. **Check every box before finishing.** Ask of each box: does it hide several
   distinct transformations, for example a note that needs "and" to say what
   happens, or a box whose module runs a list of configured stages? If so, split it.
   Then ask whether a reader could follow each input from entry to result without
   reading the source. If not, the flow is still too coarse.
6. **Adjust the modules when the flow shows they are wrong.** Tracing the data often
   reveals better boundaries: one module doing two distinct steps, or two modules
   doing one. When it does, adjust the module descriptions and carry the change into
   every affected description and link, as for any rename or move.

7. **Make a backwards pass over the modules.** Once the flow files are written,
   start from them and work back to the modules, asking whether the modules
   represent the project's organisation as well as the flow shows it could be:
   - a module named by several unrelated boxes may be doing several jobs and
     should be split;
   - a step that needs several modules, or several modules that always appear
     together, may be one responsibility and should be merged or grouped;
   - stages that are variants of one step may belong to a broader module with
     specialisations, or in one folder;
   - a module no input ever reaches is either plumbing that belongs in App, a
     helper too small to be a module, or a sign that an input is missing;
   - a module or box whose name does not say what the data experiences there
     should be renamed.
   Make the adjustments to the module descriptions that these questions support,
   carry them into every affected description and into the flow files, and run the
   checks in step 5 again. When a better organisation would need the source code
   itself to change, record it in `.mebrain/restructuring.md` instead of
   describing the code as if it already worked that way.

Trace the flow afresh from the source each time the flow is generated or
regenerated, and whenever this section changes. Do not build it by converting
older flow descriptions or by keeping earlier boxes unexamined: earlier versions
are drafts to check against the source, not a starting structure.

`flow/boxes.json` lists the inputs and every box any of them passes through:

```json
{
  "title": "PaintLine",
  "inputs": [
    {"id": "bucket", "title": "Bucket", "enters": "bucket-reader",
     "description": "A bucket to be painted and stored."}
  ],
  "boxes": [
    {"id": "bucket-reader", "kind": "External", "title": "BucketReader",
     "modules": ["modules/bucket_reader_mod.md"], "details": ["Waiting for buckets"]},
    {"id": "bucket-painter", "kind": "Effect", "title": "BucketPainter",
     "modules": ["modules/bucket_painter_mod.md", "modules/paint_store_mod.md"], "details": ["Paint ready"]},
    {"id": "bucket-varnish", "kind": "Effect", "title": "BucketVarnisher",
     "modules": ["modules/bucket_painter_mod.md"], "details": ["Clear varnish coat"],
     "off": true, "offReason": "Switched off in paint_line.json"}
  ]
}
```

- `inputs`: one per independent kind of data, titled by what the data is, with
  `enters` naming the first box it reaches and a short one-sentence `description`
  of what the data is, which editors show when the input is chosen.
- `boxes`: `id`, `kind`, `title`, `modules` (the descriptions of the modules that
  carry out this step, relative to `mebasic-src/`, the main one first) and `details`
  (up to three short lines about what the box holds when no data is there). Use `kind` `State` for where data
  waits, `Decision` where it is checked or routed, `Effect` where it changes
  something, and `External` for work done outside the project; combine two with
  `+`, such as `Decision + State`. Titles follow the PascalCase title rule. A stage
  that is designed but currently switched off has `"off": true` and an `offReason`.

`flow/data.json` follows one realistic example of each input through its boxes, so a
reader can watch that piece of data being transformed:

```json
{
  "pieces": [
    {"id": "red-bucket", "input": "bucket", "title": "A 10-litre bucket to paint red", "steps": [
      {"box": "bucket-reader", "note": "The bucket is read from the intake line",
       "details": ["Bucket: 10 L", "Colour: unpainted"]},
      {"box": "bucket-painter", "note": "Two coats of red paint are applied",
       "details": ["Colour: red", "Coats: 2"]}
    ]}
  ]
}
```

Give exactly one piece for each input. Its `steps` are in the order the data
reaches them: the `box`, a plain-language `note` of one or two short sentences
saying what happens to the data there (editors show it beside the box on the
diagram), and `details` describing the data as it is after that step, so the
reader sees it change. When the data goes back to an earlier box, for example after a
failed check, list that box again as a later step. Every step's box must exist in
`boxes.json`. List switched-off stages as steps in their designed place, with a
note saying the data would be changed there if the stage were on, and no details.
Editors lay each pathway out from these steps and draw a line for each move
between consecutive steps, so no positions or links are needed. They show off
stages greyed and dashed with an Off label; the timeline still steps into them, so
the reader sees where the data would go and that the stage is switched off.

`flow/inputs.md` is shared with the user and shown under the flow diagram:

```markdown
# Flow inputs

## Discovered inputs

- **Bucket** — A bucket to be painted and stored.
- **Can** — A can to be painted, stencilled and stored.

## Requests

- Color

## Notes

Paint mixing matters more than storage; show it in detail.
```

Generation owns `## Discovered inputs`: keep it matching the `inputs` in
`boxes.json`, one bullet each with the title in bold and the one-sentence
description. When a request cannot become an input, for example because the code
never handles that data, add a bullet under `## Discovered inputs` saying so in plain
words. The user owns `## Requests` and `## Notes`: preserve them exactly. Create the
file with all three headings when it does not exist.

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

Before every generation, check the published specification at
`https://raw.githubusercontent.com/mebrain/mebasic/main/mebasic.md`. If its
`version.revision` is higher than the project's copy in `mebasic-src/.mebasic/mebasic.md`,
replace that copy and generate with the newer revision. Never replace a copy with a
lower or equal revision, since an equal revision may hold the architect's local edits.
If the check fails, for example offline, continue with the newest local copy and say
which revision was used.

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
never makes its text human-authored. Attribution follows what is actually different
from the text before the edit, so when a person changes something by mistake and
then changes it back, or retypes the same wording, nothing becomes human-authored.
Editors also let the person unpin human attribution from text they do not want
protected, and pin text they do want protected; unpinned text is ordinary revisable
text again. Editors highlight the precise human ranges
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
- When a description is moved, renamed, merged or split, move the file itself
  and update every link to it. Never leave the old file behind as a stub, a
  redirect, or with a label such as "(moved)", "moved to", "deprecated" or
  "renamed" in its title or text. Carry any human-authored text and its
  attribution into the new file before removing the old one, and record the
  move in the change history rather than in the descriptions. Readers should
  only ever see current descriptions.
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
  ui/                       # one _ui.md per page
    widgets/                # reusable widgets only
    screenshots/
  modules/
  objects/
  flow/                     # boxes.json, data.json and inputs.md
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

Each description collection contains its own source-grounded content. The
`widgets/` and `screenshots/` subfolders are inside `ui/`; the other collections are
top-level siblings. `ui/widgets/` holds only reusable widgets; page-specific widgets
are described in their page's `_ui.md` file.
Subfolders may organise a collection's own descriptions without relocating another
collection, and should follow meaningful groupings in the project's own folders
(see Purpose). Subfolder names are lowercase like the collections. In `modules/`, a
subfolder with a module file of the same name beside it holds that module's
specialisations, as described under Specialised modules; any other subfolder is a
plain grouping.
These names and placements are required, not discretionary.

If a description collection genuinely does not apply, retain its folder with a
short Markdown explanation of why; never invent features to fill it. For constraints,
keep the two named lists and state when there are no supported entries. Screenshots
are real PNG/JPEG captures in `ui/screenshots/` and may be absent. `context/`
contains portable authoring memory, not user-facing descriptions. `.mebrain/`
contains restructuring notes, and `.mebasic/` contains private history and baselines.

Use one file per thing described, with a lowercase name and the primitive suffix:
`_ui.md`, `_widget.md` (reusable widgets in `ui/widgets/` only), `_mod.md`, `_obj.md`,
`_store.md`, `_event.md`, `_alg.md`. Constraints are the exception: use `architect_said.md` and `disallowed.md`.
Flow is the other exception: `flow/` holds only `boxes.json`, `data.json` and
`inputs.md`. Remove any older `_flow.md` descriptions; the flow is traced afresh
from the source rather than converted from them.
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
   - For a specialised module, `parent: modules/robot_mod.md`, relative to
     `mebasic-src/`, pointing to the broader module it specialises.
   - For a module or object in the creation hierarchy, `hierarchy: 2`, a positive
     integer, and below hierarchy 1, `created_by: modules/car_mod.md`, relative to
     `mebasic-src/`, pointing to the description of the concept that creates it.
   - `code_name`, the exact name in the source, when it differs from the title.
   - For a module, `entry_methods: [Evaluate]`, its general entry points, and
     `entry_point: true` when it is one of the application's first entry points.
   - For a module or object, `abstraction: interface`, `abstract` or `base` when
     it is a shared type that others implement or inherit from.
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
shown under UI according to their on-disk placement. A folder of specialised
modules is shown as its broader module: one sidebar entry with the broader
module's title, in that module's place, whose children are the broader module
followed by its specialisations. Within one folder, a description whose `created_by`
is in the same folder is shown nested under its creator as a tree, with connecting
lines and a control to collapse or expand what each concept creates. Expanding a
module also lists the entry points from its API section, each marked as a callable
entry, and choosing one opens the module at that entry. Its internal functions
follow the API entries, marked with a padlock. Hovering an entry shows its
one-sentence description. The overview file is listed first among the modules. Application entry points and a module's `entry_methods`
are listed first and labelled "Entry Point", and `abstraction` types get their own
icon. The flow collection has no entries of its own in navigation: choosing it opens
the flow diagram drawn from `flow/boxes.json` and `flow/data.json`, with `flow/inputs.md`
below it for the user to add requests and notes. Editors choose their own controls and layout.
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
