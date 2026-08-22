# Tutorials — learning-oriented

**Purpose:** take a beginner from zero to a working result, building confidence
and orientation in the subject as they go. A tutorial is a lesson, not a
reference and not a task list for someone who already knows what they're doing.

Inside a software company, this is almost always one of: a new-hire onboarding
walkthrough, a "set up your local dev environment" guide, or a "make your first
PR" guide. Treat these as some of the highest-leverage docs a team owns — every
new engineer runs into them, and every hour they save (or cost) is multiplied by
headcount growth.

## Rules

- **Guarantee the outcome.** Every step must work, exactly as written, on a
  clean environment. Test it, don't assume it. If a step can fail depending on
  the reader's setup, the tutorial needs to either eliminate that variable (pin
  versions, specify the exact environment) or it isn't ready.
- **No decisions for the reader to make.** Never write "choose the option that
  fits your case." If there's a choice, the tutorial makes it for them, and can
  mention alternatives exist without asking the reader to pick one.
- **Narrate what they should see.** After a step, tell the reader what output or
  state confirms success ("you should now see... in your terminal"). This is
  what lets a beginner tell the difference between progress and failure.
- **Concrete, not abstract.** Use a real, specific example throughout — a single
  running example the reader builds incrementally — never pseudo-code or
  "your-value-here" placeholders that force the reader to improvise.
- **Minimal explanation, maximal action.** If you feel the urge to explain _why_
  a step works, cut it to one sentence or a link to an explanation document. The
  tutorial's job is the doing; understanding can come later, once they have a
  working result to anchor it to.
- **Start from a known, minimal state.** State the prerequisites precisely
  (versions, accounts, installed tools) so "start here" actually means the same
  thing for every reader.
- **End with a working thing and a clear next step.** Close with what the reader
  now has, and point to a how-to guide or explanation for going further — don't
  leave them wondering what to do with what they just built.

## Common mistakes (mode leakage)

- Stopping mid-tutorial to explain internals ("this works because...") — cut it
  or link out.
- Branching logic ("if you're on Mac... if you're on Windows...") beyond what's
  unavoidable — this is how-to guide territory; keep tutorials on rails.
- Reference-style tables of every available option when only one is needed for
  this tutorial — link to reference instead of inlining it.
- Vague success criteria ("it should work now") instead of a concrete, checkable
  result.

## Self-review checklist

- [ ] Could someone with the stated prerequisites — and nothing else — follow
      this and succeed, with zero judgment calls?
- [ ] Is there a single concrete example running through the whole tutorial?
- [ ] Does every step say what the reader should observe if it worked?
- [ ] Is all "why" content removed or reduced to a one-line link-out?
- [ ] Does it end with a working result and a pointer to what's next?
- [ ] Does it have a named owner (person or team) responsible for keeping it
      accurate as the toolchain/stack changes?
- [ ] If this is onboarding material, has it actually been run recently by
      someone who wasn't the author (e.g. the last new hire) — or is it running
      on assumed-still-true steps?
