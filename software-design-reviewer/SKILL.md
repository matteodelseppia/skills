---
name: software-design-reviewer
description:
  Review software design documents, technical proposals, RFCs, ADRs, and
  architecture diagrams for completeness, correctness, trade-offs, security,
  reliability, scalability, operability, and implementation fit. Use when asked
  to review, critique, approve, or prepare a design document for implementation.
---

# Software Design Reviewer

## Overview

Review a design document as a skeptical senior software architect and downstream
implementation owner. Ground every finding in the document, repository, or cited
source; distinguish a missing decision from a wrong decision and a material risk
from a stylistic preference.

## Review workflow

### 1. Establish scope and evidence

1. Identify the target document from the user request or the repository. If the
   skill is explicitly invoked with additional prompt text, treat that text as
   part of the request. If there are multiple candidates, state which one you
   selected and why.
2. Read the complete document before judging individual sections. Inspect linked
   diagrams, schemas, ADRs, API definitions, configuration, tests, and relevant
   implementation files when available.
3. Determine the review mode:
   - **Document-only:** assess the proposal and explicitly state that repository
     conformance was not checked.
   - **Repository-aware:** compare the proposal with current code, dependencies,
     deployment files, ADRs, and established patterns.
   - **Targeted:** focus on the requested concern, but still flag blocking
     cross-cutting risks.
4. Extract the system context, actors, components, data flows, invariants,
   decision drivers, assumptions, and open questions before forming conclusions.

### 2. Review in passes

Apply the detailed checklist in
[references/review-rubric.md](references/review-rubric.md). Use only the
sections relevant to the system.

Review these passes in order:

1. **Intent and requirements:** problem, goals, non-goals, constraints,
   functional requirements, non-functional requirements, success metrics, and
   acceptance criteria.
2. **Architecture and behavior:** boundaries, ownership, dependencies, data
   flow, state transitions, failure modes, consistency, and diagram/document
   agreement.
3. **Interfaces and data:** APIs, events, schemas, versioning, compatibility,
   idempotency, authorization, migrations, and lifecycle ownership.
4. **Quality attributes:** security and privacy, reliability and recovery,
   performance and capacity, cost, observability, operability, and
   sustainability where relevant.
5. **Delivery and evolution:** alternatives, trade-offs, ADRs, rollout, feature
   flags, migration, rollback, testing, monitoring, and deprecation.
6. **Implementation fit:** compare proposed boundaries and assumptions with the
   repository when repository-aware review is possible.

### 3. Classify and prioritize findings

Classify each finding as one of:

- **Contradiction:** the document conflicts with itself, an interface, a cited
  constraint, or verified repository behavior.
- **Omission:** an important decision, requirement, failure mode, owner, or
  operational detail is absent.
- **Risk:** the design may work but has a material security, reliability,
  scalability, cost, privacy, or delivery risk.
- **Trade-off:** a deliberate choice that should be made explicit or accepted by
  the right stakeholder.
- **Preference:** an optional improvement; do not present it as a blocker.

Use severity consistently:

- **P0 — Blocking:** unsafe, infeasible, or likely to cause severe production or
  business harm.
- **P1 — High:** must be resolved before implementation or launch.
- **P2 — Medium:** should be resolved before launch or captured as a follow-up.
- **P3 — Low:** useful clarification, polish, or optional improvement.

Do not invent traffic numbers, SLOs, compliance obligations, repository facts,
or platform capabilities. Mark them as unknowns and ask focused questions.

### 4. Resolve doubts and gaps interactively

After the initial review, do not stop at a static list of open questions. Treat
every material doubt, ambiguity, omission, or unresolved trade-off as a decision
to work through with the user.

1. Build a deduplicated queue of all material doubts and gaps discovered during
   the review. Include P0-P2 findings that require user intent or a
   product/architecture decision, plus any P3 item that would otherwise force an
   implementation assumption.
2. Ask about them **one at a time**. For each item:
   - explain the concrete gap and why it matters;
   - state the relevant evidence from the design or repository;
   - present the main viable options and their trade-offs when useful;
   - ask one focused question that lets the user make or clarify the decision.
3. After each answer, reason through it with the user. If the answer exposes
   another material ambiguity, resolve that before moving on. Do not silently
   choose among materially different interpretations.
4. Record every resolved decision immediately in a durable Markdown decision
   log. By default, create or update a sibling file next to the design document
   named `<design-filename-without-extension>.review-decisions.md`. If the
   repository already has an established location or convention for design
   decisions, ADRs, or review notes, follow that convention instead.
5. Each recorded decision should include:
   - the question/gap;
   - the user's answer or chosen option;
   - the rationale discussed;
   - consequences, constraints, or follow-up actions;
   - the design section(s) affected.
6. Continue until all material doubts/gaps are resolved, explicitly deferred, or
   marked as requiring an external owner. Do not bundle unresolved questions
   into a single final prompt when they can be discussed individually.

### 5. Apply resolved decisions to the design

Once the interactive resolution loop is complete, update the design document
itself whenever the user's answers change, clarify, or complete the intended
design.

1. Work directly on the repository's `main` branch when repository access is
   available and the user has not instructed otherwise. If not currently on
   `main`, switch to `main` only when doing so is safe and will not discard or
   overwrite unrelated work. Never destroy, reset, stash, or overwrite unrelated
   user changes merely to switch branches.
2. Edit the design document so the resolved decisions are incorporated into the
   authoritative sections rather than merely appended as review notes. Preserve
   the document's existing structure and style unless a structural change is
   needed for clarity.
3. Keep the decision log as the audit trail even after incorporating the answers
   into the design.
4. Re-read the modified design and run a focused consistency pass to ensure the
   new decisions do not create contradictions, stale diagrams, mismatched
   interfaces, or new unanswered gaps.
5. Make the edits in the working tree; do not create a commit, push, open a pull
   request, or modify implementation code unless the user explicitly asks for
   that separately.
6. If a requested design change cannot safely be applied (for example because
   the working tree has conflicting unrelated edits), explain the blocker and
   preserve the recorded decision rather than risking data loss.

### 6. Produce the review

Return the following structure:

```markdown
# Design review: <title>

## Verdict

<Ready | Ready with conditions | Not ready> — <one-sentence rationale>

## Scope and confidence

- Review mode: <document-only | repository-aware | targeted>
- Evidence inspected: <files, sections, diagrams, or interfaces>
- Confidence: <high | medium | low>, with limitations

## Findings

### [P1] <short title> — <omission | contradiction | risk | trade-off | preference>

- Evidence: <section, heading, file, or diagram element>
- Risk: <what can go wrong and who is affected>
- Recommendation: <specific change or decision>
- Trade-off / validation: <cost, downside, experiment, or question>

## Decision-resolution status

- Resolved: <count and brief summary>
- Deferred/external: <remaining items, if any>
- Decision log: <path>

## Strengths

- <specific design strength>

## Suggested document changes

- <small, actionable edits; do not rewrite the whole document unless asked>
```

Sort findings by severity, then by implementation or business impact. Include
only actionable findings. Cite exact headings, line ranges, file paths, diagram
nodes, or interface names whenever possible.

## Review principles

- Review the design against its stated goals and constraints before applying
  generic best practices.
- Prefer the simplest design that satisfies the requirements and operational
  reality.
- Make alternatives and trade-offs explicit; do not declare one architecture
  universally correct.
- Treat security, privacy, data loss, backwards compatibility, and rollback as
  first-class design concerns.
- Check that every critical component has an owner, an observable health signal,
  a failure behavior, and a recovery path.
- For AI or agentic systems, additionally examine prompt/data trust boundaries,
  tool permissions, human approval, model failure handling, evaluation, and
  auditability.
- Never approve a design solely because it sounds plausible. State what evidence
  or validation is still required.
- Prefer explicit user decisions over inferred intent: when a material gap can
  be resolved by asking, ask and record the answer before finalizing the design.

## Optional framework grounding

Use the framework best aligned with the project, not all of them mechanically:

- C4 for architecture views and diagram consistency.
- ADRs for important decisions, rationale, trade-offs, and consequences.
- AWS, Azure, or Google Cloud Well-Architected guidance for quality attributes.
- OWASP guidance for application and LLM/agent security.

## Additional resources

- Detailed checklist: [references/review-rubric.md](references/review-rubric.md)
