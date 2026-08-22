# Writing docs for colleagues, not the public

Diataxis was designed around public/product documentation, but the same four
modes hold for internal engineering docs. What changes inside a company is
the audience (colleagues, not strangers), the failure mode (docs quietly rot
instead of being visibly missing), and where things should live. This file
covers what to adjust.

## The audience is colleagues, not strangers

- You can (and should) assume more shared context: internal tool names,
  standard stack, team conventions — you don't need to explain what your
  company's deploy pipeline is called every time you reference it. But don't
  assume knowledge specific to one team when writing for a company-wide
  audience (a new hire on a different team is effectively a stranger to your
  service).
- Internal docs are read under time pressure far more often than public docs
  — someone is on-call, mid-incident, or blocked on a PR. This makes the
  tutorial/how-to-guide distinction matter *more*, not less: a runbook with
  tutorial-style hand-holding wastes a responder's time; a runbook that
  assumes undocumented tribal knowledge fails them entirely.
- Write for the person who joins the team in six months, not just the person
  asking right now. A Slack thread or a verbal explanation answers today's
  question; only a doc answers it for the next person too.

## Staleness is the main enemy, not absence

Public docs that go stale get noticed (users complain, support tickets pile
up). Internal docs go stale silently — the service moved on, the doc didn't,
and nobody outside the doc's original author necessarily notices until it
actively misleads someone.

- **Every doc needs an owner** — a person or, better, a team (people leave;
  teams persist). State it at the top of the doc or in its metadata, not just
  in institutional memory.
- **Runbooks and how-to guides for operational tasks need to be exercised**,
  not just written once. A deploy/rollback runbook that hasn't been followed
  since the last infra migration may no longer be true. Prefer runbooks that
  are tested during on-call handoffs, game days, or incident drills over ones
  that exist only as prose.
- **Tie docs to the thing they describe** wherever possible so they're more
  likely to be updated in the same change: a service's README and its
  reference docs belong in that service's repo, updated in the same PR that
  changes the behavior they document. Don't let source-of-truth
  documentation live somewhere structurally disconnected from the code.
- **Prefer "generated where possible."** Reference documentation for an
  internal API, CLI, or config schema should be generated from source
  (docstrings, OpenAPI specs, `--help` output) rather than hand-maintained
  wherever tooling allows it — hand-maintained reference is the likeliest of
  the four modes to silently drift from reality.
- When you can't verify a procedure is still accurate, say so explicitly
  rather than presenting stale content with false confidence — flag it for
  the owning team to confirm.

## Where things should live

Not every mode belongs in the same place. A common internal-docs failure is
treating "the wiki" or "the repo README" as the answer for everything.

- **In the repo, next to the code:** service README (a short how-to-guide-
  style "how to run/test/deploy this service" plus links out), reference docs
  generated from source, and anything that must change in lockstep with the
  code. If it's in the repo, it's far more likely to get updated in the PR
  that invalidates it.
- **In an internal wiki or docs platform:** onboarding tutorials, cross-team
  how-to guides, ADRs/design docs, and postmortems — content that spans
  multiple repos/services or needs to be discoverable without knowing which
  repo to look in.
- **In a service/API catalog (e.g. an internal developer portal):** the
  reference layer for "what services exist, what do they own, who owns them,
  what's their API" — this is reference material and should be structured
  and generated the same way any other reference doc is.
- Whichever you pick, cross-link relentlessly: a service README (how-to)
  should link to its ADRs (explanation) and its generated API reference
  (reference); an onboarding tutorial should link out to the how-to guides
  and reference material the new hire will need next, not attempt to contain
  them.

## Company-specific artifact notes

- **Architecture decision records (ADRs)** are explanation documents with one
  extra property: they're typically append-only / immutable once accepted —
  a new decision that supersedes an old one gets its own new ADR that
  references the old one, rather than editing history. Say this explicitly
  if a user asks you to "update" an old ADR.
- **Postmortems / incident reviews** are explanation documents (what
  happened and why), but often need a short how-to-guide-shaped action-items
  section attached (what will change as a result). Keep the narrative
  analysis and the action items visually distinct — don't let the action list
  fragment the narrative or vice versa.
- **Onboarding guides** are tutorials in the strictest sense: they must
  guarantee a working outcome (a new hire with a running local environment
  and a merged first PR) with zero judgment calls. If the onboarding guide
  breaks because the toolchain changed, it needs to be fixed with the same
  urgency as a broken build — treat onboarding-doc rot as a real cost, since
  it compounds every time someone new joins.
- **Runbooks** are how-to guides written for a reader under stress with
  limited attention — prefer even shorter steps, even more explicit
  verification points, and explicit escalation paths ("if step 3 doesn't
  work, page X") over the branching flexibility a normal how-to guide can
  afford.
