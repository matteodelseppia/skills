---
name: diataxis
description:
  Plans, writes, restructures, and reviews internal software-engineering
  documentation using the Diataxis framework — the four-quadrant model of
  tutorials, how-to guides, reference, and explanation. Use this whenever
  someone asks to write, organize, restructure, audit, or improve engineering
  docs at a software company — service READMEs, onboarding guides, runbooks,
  on-call/incident docs, internal API or config reference, architecture decision
  records (ADRs), design docs, or a team/repo docs folder — even if they never
  say the word "Diataxis". Also use when a user complains that internal docs are
  "confusing", "out of date", "scattered across the wiki", "trying to do too
  much", or when they ask you to classify a piece of content, split a bloated
  doc into focused pages, set up a docs/ folder for a repo, or figure out why
  nobody can find the right doc.
license: MIT
---

# Diataxis

Diataxis is a way of thinking about documentation that organizes content into
four distinct modes based on what the reader needs _right now_: to learn, to
accomplish a task, to look something up, or to understand. Mixing these modes in
one document is the single most common cause of bad internal documentation — an
onboarding guide that stops to explain architecture, or a runbook that assumes
context a sleep-deprived on-call engineer doesn't have at 3am.

Use this skill any time you are asked to write, restructure, or evaluate
engineering documentation inside a software company — for a service, a team, a
repo, or an internal platform. The job is almost never "write good docs" in the
abstract — it is always "identify which mode(s) this content needs, then apply
that mode's rules strictly." In a company setting the four modes map onto
familiar artifacts:

- **Tutorial:** New-hire onboarding walkthrough, "your first PR" guide, and
  local development setup.
- **How-to guide:** Runbook, deploy/rollback procedure, on-call playbook, and
  instructions for adding a service endpoint.
- **Reference:** Internal API/config reference, service catalog entry, CLI
  reference, and schema/data dictionary.
- **Explanation:** Architecture decision record (ADR), design doc, postmortem
  analysis, and an explanation of why a choice was made.

See `references/company-context.md` for what's different about writing docs for
colleagues instead of external users — ownership, staleness, discoverability,
and where each artifact type should actually live (repo vs. wiki).

## The compass

Diataxis maps every kind of documentation on two axes:

- **Study (acquiring):**
  - **Tutorial:** learning by doing.
  - **Explanation:** understanding.
- **Work (applying):**
  - **How-to guide:** achieving a goal.
  - **Reference:** looking up information.

Read `references/compass.md` for the full decision tree before classifying
ambiguous content — most misclassifications happen at the tutorial/how-to
boundary or the reference/explanation boundary, and that file has the specific
tests to resolve both.

## Workflow

1. **Figure out what's actually being requested.** Ask (or infer from context):
   is the reader a total beginner who needs to be walked through something
   (tutorial), someone who already knows the basics and wants to accomplish a
   specific task (how-to guide), someone who needs to look up a fact while
   working (reference), or someone who wants to understand why something works
   the way it does (explanation)? A single request often maps to more than one
   piece of documentation — say so, and propose the split, rather than writing
   one document that tries to serve every need.

2. **Load the reference file for the relevant mode(s)** before writing or
   reviewing:
   - `references/tutorials.md`
   - `references/how-to-guides.md`
   - `references/reference-docs.md`
   - `references/explanation.md`

   Each covers that mode's purpose, the rules that define it, common mistakes
   that leak in from other modes, and a checklist to self-review against.

3. **When restructuring existing docs**, use `references/audit.md` — it's a
   guide for triaging an existing docs set: reading through content, tagging
   each section by mode, flagging mixed-mode documents, and proposing a new
   structure without necessarily rewriting the prose from scratch.

4. **When scaffolding docs for a service, repo, or team**, use
   `references/site-structure.md` for how to lay out the four modes (folder
   structure, landing page pattern, cross-linking conventions, and what goes
   in-repo vs. in the internal wiki).

5. **Draft using the templates in `assets/`** as starting skeletons
   (`tutorial-template.md`, `how-to-template.md`, `reference-template.md`,
   `explanation-template.md`) rather than starting from a blank page — they
   encode the structural conventions for each mode so you don't have to hold
   them all in working memory while writing.

6. **Before finishing, re-read what you wrote against the checklist** in the
   relevant reference file. The most common failure mode is a tutorial that
   drifts into explaining _why_, or a how-to guide that drifts into teaching —
   catch this before presenting the result, not after.

7. **Check ownership and staleness risk** using `references/company-context.md`
   before calling internal docs done — every internal doc needs a named owner or
   owning team and, for anything operational (runbooks especially), a plan for
   staying accurate as the system changes.

## Core rules that apply everywhere

- **One document, one mode.** If content genuinely serves two purposes, it
  belongs in two documents, cross-linked — not merged into one that switches
  registers halfway through.
- **Don't explain in tutorials or how-to guides.** Link out to an explanation
  document instead of a paragraph of "the reason this works is...". The reader
  came to _do_ something; respect that.
- **Don't teach in reference docs.** Reference is for looking things up mid-task
  — it should be structured for scanning (consistent headings, tables, parameter
  lists), not narrative.
- **Tutorials guarantee success; how-to guides assume competence.** A tutorial
  never asks the reader to make a judgment call the tutorial hasn't prepared
  them for. A how-to guide can say "choose the option appropriate to your case"
  because it's written for someone already capable.
- **Name things by what they are**, not by audience or vibe — "Tutorial:
  Building your first X", "How to configure Y for Z", "X reference",
  "Understanding how X works" — so readers self-select correctly before they
  even open the page.
- **Every internal doc has an owner.** Unlike public docs written once for a
  broad audience, internal docs go stale as the system changes underneath them.
  A doc with no owner is a doc nobody will fix when it drifts — flag this rather
  than leaving it implicit.
- **Runbooks and how-to guides must be tested against reality, not just
  written.** A runbook that was accurate when written but never re-run during an
  incident or a drill is a liability — say so if you can't verify a procedure
  still works.
