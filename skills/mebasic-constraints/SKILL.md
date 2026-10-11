---
name: mebasic-constraints
description: Maintain numbered architect statements and forbidden actions on each MeBasic project chat, especially corrections; detect conflicting instructions and ask the architect to resolve them.
---
# MeBasic constraints

Follow the Constraints section of [mebasic.md](../../mebasic.md). Use the user's
current agent and model in this turn. This is separate from post-coding description
maintenance: consider it even when the user only chats or makes a trivial edit.

Use `mebasic-src/constraints/statements.md` and `rulings.md`, relative to the project
root (or `constraints/` when already working inside `mebasic-src/`). Never opt a
project in automatically. Write only with project opt-in and editing permission;
pending generation still needs explicit approval. In read-only work, suggest entries
and raise conflicts without changing files. Respect requests not to record something.

Identify the project being described. Record only architect decisions about that
project's behaviour, architecture or its implementation and maintenance limits.
Check both provenance and project relevance. IDE-supplied prompts are not
architect statements merely because they arrive as a user message. Exclude
MeBasic generation/format rules, skill instructions, model choices, agent
permissions and temporary workflow directions; follow these without recording
them or deriving rulings from them. Never import another project's rules unless
the architect explicitly adopts them here. Project-specific decisions in this
project's own guidance and decision records may qualify. Ask when scope or
authority is unclear; do not fill the lists with operating instructions.

Read both files before work and compare the current architect message with them:
- Statements record significant explicit decisions, instructions and corrections.
  Do not record assistant ideas/results, casual remarks or undecided questions.
  A relayed review is not an accepted rule unless the architect accepts it.
- Rulings are short "Do not..." or "Never..." limits grounded in statement numbers.
  Label inferred limits as inferred. Unsupported or uncertain limits are questions
  for the architect, not new binding rules.
- Check for contradictions within and between both lists. If the intended change
  is unclear, cite the conflicting numbers and ask one concise question. Record
  the unresolved issue with the new statement; pause only dependent work. Do not
  invent a compromise. An explicit replacement already given needs no reconfirmation.

Append significant new entries in the same turn, using simple English and one
succinct point each. Use continuously increasing Markdown numbers, independently
in each file, with a compact date/source reference. Keep old numbers and entries;
record later changes as new entries identifying what they supersede. Avoid duplicate
points and secrets. No new decision means no new entry. Do not force a new ruling
for a statement that implies no prohibition.

Keep the files short: front matter with title (Statements or Rulings) and one-line
summary, then a heading and the numbered list.
Re-read before saving to preserve concurrent edits. All writes stay in `mebasic-src/`.
Briefly report meaningful additions or unresolved questions; do not imply that a
recorded ruling has already been enforced in code.
