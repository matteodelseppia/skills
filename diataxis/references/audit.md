# Auditing and restructuring existing documentation

Use this when the user has existing docs that feel disorganized, bloated, or
"trying to do too much" — the goal is to diagnose and propose a restructure,
not necessarily rewrite every sentence.

## Process

1. **Inventory.** List every existing doc/page/section. For each, note its
   current title and a one-line summary of what it actually contains (not
   what its title claims).

2. **Tag every section by mode**, using `compass.md`'s decision tree. Do this
   at the section level, not just the document level — a single page
   frequently contains two or three modes stacked on top of each other (a
   "Getting Started" page that's 30% tutorial, 40% reference-style config
   dump, 30% architecture explanation is extremely common).

3. **Flag mixed-mode documents explicitly.** For each one, list which
   sections belong to which mode. This list is the direct input to the
   restructuring proposal — don't skip straight to a rewrite without showing
   this breakdown, since it's often the most useful output on its own.

4. **Propose a new structure**, not just a diagnosis:
   - Which sections merge into which new document, under which mode
   - What gets cut because it's redundant across documents
   - What's missing entirely (e.g., there's reference material scattered
     across three how-to guides but no dedicated reference page — consolidate
     it into one)
   - How the new documents cross-link (a how-to guide linking to the
     relevant reference entries and to an explanation doc for background)

5. **Sequence the work.** Restructuring is often bigger than one sitting.
   Propose an order — usually: split out reference material first (it's
   the most mechanical extraction), then tutorials (highest-leverage for new
   users), then how-to guides, then explanation (lowest urgency, often
   already scattered as ad hoc comments/wiki pages that just need
   consolidating).

## What NOT to do

- Don't rewrite prose you don't have to — if a paragraph is factually correct
  and just in the wrong document, move it, don't rewrite it, unless its
  register also needs to change (e.g., narrative reference prose needs to
  become terse when moved to a reference page).
- Don't merge everything into one document "for convenience" — the whole
  point of the restructure is that one document per mode is what makes
  documentation navigable.
- Don't propose a full rewrite when a re-file-and-trim will do. Diataxis is a
  structural fix as much as a writing-quality fix; often the words are mostly
  fine and just in the wrong place.

## Output format

When presenting an audit, a table works well:

| Current location | Content | Belongs in (mode) | Proposed destination |
|---|---|---|---|
| `getting-started.md` §1–2 | Install + first request walkthrough | Tutorial | `tutorials/first-request.md` |
| `getting-started.md` §3 | Full list of client config options | Reference | `reference/client-config.md` |
| `getting-started.md` §4 | Why we chose polling over webhooks | Explanation | `explanation/polling-vs-webhooks.md` |
