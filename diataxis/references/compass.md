# The Diataxis compass — classifying content

Every piece of documentation answers one of four questions. Work through these
in order; stop at the first one that fits.

## 1. Is the reader trying to learn, with you guiding every step?

If yes → **Tutorial**. Signal phrases: "first time", "getting started",
"onboarding", "follow along", "by the end of this you'll have built...". The
reader is not yet capable of judging whether a step succeeded — the tutorial
must tell them what to expect at each step so they know they're on track.
Internally this is almost always a new-hire onboarding doc, a "set up your
local dev environment" guide, or a "make your first PR" walkthrough.

If the reader already has some competence and is not being walked through
step-by-step → not a tutorial, go to 2.

## 2. Is the reader trying to accomplish a specific real-world task right now?

If yes → **How-to guide**. Signal phrases: "how do I...", "configure X for Y",
"set up... for production", "migrate from... to...", "what do I do if the
service is down". The reader has a goal external to the documentation itself
(deploy this, connect that, roll back that, fix this alert) and arrived
competent enough to follow instructions without hand-holding. Internally this
covers runbooks, deploy/rollback procedures, on-call playbooks, and
"how to add a new endpoint to service X" guides.

If there's no specific task, and the reader instead wants a fact while doing
something else → go to 3. If there's no task and no fact needed, just an
understanding they're after → go to 4.

## 3. Is the reader looking something up mid-task?

If yes → **Reference**. Signal phrases: "what are the parameters for...",
"what does this config value do", "internal API for...", "full list of...",
"who owns service X". The reader already knows what they're trying to do;
they need one specific, authoritative fact, fast. Structure for scanning, not
reading start to finish. Internally this covers internal API/config
reference, service catalog entries, CLI reference, schema/data dictionaries
— and should be generated from source wherever possible (see
`company-context.md`).

## 4. Is the reader trying to understand why or how something works, with no immediate task?

If yes → **Explanation**. Signal phrases: "why does...", "why did we
choose...", "how does... work internally", "what's the difference between...
and...", "the architecture of...", "what happened during the incident".
This is the only mode where discursive, exploratory prose is appropriate —
connecting ideas, discussing alternatives and trade-offs, providing context.
Internally this covers architecture decision records (ADRs), design docs,
architecture overviews, and the analysis portion of postmortems.

## Boundary cases

**Tutorial vs. how-to guide** — the same task ("install and configure X") can
be written either way, and the difference is *who it's for* and *what it
guarantees*, not the content:
- Tutorial: guarantees a specific, working outcome for a beginner. No
  branching. No "if your case is different...". If the tutorial doesn't
  produce the promised result, it has failed.
- How-to guide: assumes the reader can adapt the steps to their situation. Can
  branch ("if you're using Docker, do X; otherwise Y"). Success is on the
  reader, not guaranteed by the document.

Test: could a total beginner follow this with zero judgment calls and reach
a working result? If yes, it's tutorial material. If it requires the reader to
already know things or make choices, it's a how-to guide.

**Reference vs. explanation** — both are "no immediate task", but:
- Reference states facts (this parameter accepts these values). No argument,
  no narrative, no "because".
- Explanation discusses reasoning (why this parameter exists, why it defaults
  to this, what trade-off it represents).

Test: does removing all the "because" and "the reason is" language leave the
document intact? If yes, it was reference all along. If the document falls
apart without its reasoning, it's explanation.

**A single request spanning multiple modes** — very common, especially for
"document this service" style requests. Example: "document our new caching
layer." This is not one document. It likely needs:
- a how-to guide ("how to enable caching for your service")
- a reference page (cache config parameters, TTL defaults, eviction policy
  values) — ideally generated from source
- an explanation, likely an ADR ("why we chose write-through caching",
  trade-offs considered)
- possibly a tutorial if onboarding a new engineer to the codebase requires it

Say this explicitly to the user rather than writing one sprawling document.
"Document service X for the team" almost always decomposes into a service
README (how-to, in-repo), generated reference docs (in-repo), and one or more
ADRs (wiki or in-repo `/docs/adr/`) — not a single all-purpose page.
