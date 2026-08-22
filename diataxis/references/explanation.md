# Explanation — understanding-oriented

**Purpose:** deepen and broaden the reader's understanding of a topic — the
why, the context, the trade-offs, the history, the alternatives considered.
This is the one mode where discursive prose is not just allowed but the point.

Inside a software company this covers architecture decision records (ADRs),
design docs, architecture overviews, and the analysis portion of
postmortems/incident reviews. Two things are specific to this context:

- **ADRs are typically append-only.** A decision that's superseded gets a new
  ADR that references and supersedes the old one — the old one isn't edited
  or deleted, since the record of "we used to think X, then learned Y" is
  itself valuable. If asked to "update" an old ADR, propose a new one instead
  and say why.
- **Postmortems mix explanation with action items.** The narrative (what
  happened, why, contributing factors) is explanation; the resulting action
  items are more how-to-guide shaped. Keep them visibly distinct sections
  rather than interleaving them — a reader six months later may want the
  narrative for context without wading through a checklist, or vice versa.

## Rules

- **No task, no steps required.** The reader isn't doing anything right now;
  they want to understand. Don't structure this as instructions.
- **Discuss, don't just state.** This is where "because," "the reason is,"
  "an alternative approach would be," and "the trade-off here is" belong —
  content a tutorial, how-to guide, or reference page should have been
  cutting out.
- **Connect ideas.** Relate the topic to other parts of the system, to
  history, to design principles, to alternatives that were rejected and why.
  Explanation is where the bigger picture lives.
- **It's fine to be opinionated and to discuss trade-offs**, as long as it's
  clearly presented as such — this isn't reference, so "we chose X over Y
  because Z, though Z has this downside" is appropriate here in a way it
  wouldn't be in reference.
- **Bound the topic.** "Understanding our caching layer" is fine;
  "understanding our entire architecture" usually should be split into
  several explanation documents by topic so each stays coherent.
- **Link to reference and how-to guides rather than duplicating them.** If a
  reader needs to actually do something as a result of understanding this,
  point them to the how-to guide — don't turn this into one.

## Common mistakes (mode leakage)

- Sliding into step-by-step instructions — that's a how-to guide; link to it
  instead of writing it here.
- Padding with an exhaustive parameter list that belongs in reference.
- Trying to cover an entire system in one document instead of splitting by
  topic, leaving the reader unable to find the part they care about.
- Being reference-neutral when the topic actually calls for discussing
  trade-offs and reasoning — this is the one mode that should have a point of
  view about design decisions.

## Self-review checklist

- [ ] Does this avoid turning into a set of instructions?
- [ ] Does it actually explain reasoning, trade-offs, or context — not just
      restate facts a reference page would cover?
- [ ] Is the topic scoped narrowly enough to stay coherent?
- [ ] Does it link out to how-to guides / reference rather than duplicating
      their content?
- [ ] If this is an ADR: is it framed as a new, standalone record (linking to
      any decision it supersedes) rather than an edit to a past one?
- [ ] If this is a postmortem: are the narrative and the action items kept in
      visually distinct sections?
