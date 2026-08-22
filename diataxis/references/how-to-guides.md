# How-to guides — task-oriented

**Purpose:** help a competent user accomplish a specific real-world goal. A
how-to guide assumes the reader already knows the basics and arrived with a
concrete task in mind — it is not there to teach them the subject.

Inside a software company, the sharpest version of this is the **runbook** — a
how-to guide read by someone on-call, often under time pressure or during an
incident. Runbooks earn extra rules on top of the ones below (see the "Runbooks
specifically" section).

## Rules

- **Start with the goal, named plainly.** Title and opening line should state
  exactly what the reader will accomplish ("How to rotate API credentials
  without downtime"), not a topic area.
- **Assume competence.** Don't re-explain concepts a how-to guide's audience
  already has from using the product. Link to reference or explanation for
  background instead of inlining it.
- **Address the real-world mess.** Unlike a tutorial, a how-to guide can and
  should branch for realistic variations ("if you're behind a proxy...", "for a
  multi-region setup..."). The goal is usefulness in practice, not a single
  clean path.
- **Sequence of actions, not concepts.** Structure as ordered steps toward the
  goal. Each step is an action, not a discussion.
- **State the precondition and the end state.** What must be true before
  starting (a precondition), and what's true once done — so the reader can
  verify they actually achieved the goal, not just followed steps.
- **Don't explain, don't teach.** If you notice yourself writing a paragraph
  about _why_ something works this way, that's an explanation document — cut it
  and link out.
- **Prefer several focused guides over one sprawling one.** "How to deploy" is
  too broad if it secretly covers five different scenarios; split into separate
  guides addressing each named situation.

## Runbooks specifically

A runbook is a how-to guide for a reader who may be paged at 3am, mid-incident,
with limited attention and no time to interpret ambiguity. Adjust accordingly:

- **Shorter steps, more explicit verification** than a normal how-to guide —
  after every step, state exactly what confirms success, since a stressed reader
  shouldn't have to infer it.
- **Explicit escalation paths.** "If step 3 doesn't resolve it, page
  [team/rotation]" — don't leave the reader to decide when to escalate.
- **State blast radius and rollback up front.** Before the steps, note what this
  procedure affects and how to undo it if something goes wrong.
- **Prefer copy-pasteable commands over described actions** — "run `X`" beats
  "navigate to the dashboard and click the button that says Y", since the former
  is faster and less ambiguous under stress.
- **Treat "untested" as a known defect.** A runbook nobody has followed since
  the last infra change may be wrong. If you can't confirm it's current, flag it
  for the owning team rather than presenting it as reliable.

## Common mistakes (mode leakage)

- Teaching fundamentals the audience should already have — that's tutorial
  territory, and it clutters the guide.
- Long "background" sections before the steps start — link to explanation
  instead.
- Treating it like reference: dumping every possible flag/option instead of the
  ones relevant to this specific goal.
- No real branching for realistic variation, forcing readers whose situation
  doesn't match to guess.
- A vague or missing "how you'll know this worked" — leaves the reader unsure if
  they're actually done.

## Self-review checklist

- [ ] Is the goal stated plainly in the title and first line?
- [ ] Does it skip re-teaching basics the audience should already have?
- [ ] Does it branch for realistic real-world variation where needed?
- [ ] Is there a clear way to verify the goal was achieved?
- [ ] Is all "why" content removed or reduced to a link-out?
- [ ] Does it have a named owner (person or team)?
- [ ] If this is a runbook: are escalation paths explicit, is blast
      radius/rollback stated up front, and is there reason to believe it's still
      accurate against the current system?
