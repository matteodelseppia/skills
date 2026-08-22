---
name: bug-solver
description: >
  Investigate, diagnose, and fix software bugs by reproducing failures,
  identifying root causes, implementing minimal fixes, and verifying regressions.
license: MIT
---

# Bug Solver

## Purpose

Use this skill when an agent is asked to investigate, diagnose, or fix a software
bug.

The goal is not merely to make the reported symptom disappear. The goal is to
understand the failure, reproduce it when practical, identify the smallest
correct fix, verify that the fix does not introduce regressions, and leave behind
enough evidence and documentation that another engineer can understand why the
change is correct.

Prefer correctness, reproducibility, minimal behavioral change, and
maintainability over expedient patches.

## Core principles

A bug fix should normally satisfy four properties:

1. **The failure is understood.** The agent should be able to explain what
   behavior is wrong, what behavior is expected, and why the current
   implementation violates that expectation.
2. **The failure is reproducible.** Whenever practical, there should be a
   deterministic way to demonstrate the bug.
3. **The fix is minimal and compatible.** Change the smallest appropriate
   surface while preserving contracts and unrelated behavior.
4. **The fix is verified.** Evidence should show both that the original bug is
   gone and that the surrounding system still works.

Do not start editing code merely because a suspicious line has been found. First
establish enough context to know whether changing that line is actually correct.

---

## Workflow

### 1. Read the context file, or create it if it doesn't exist

Before investigating the defect, check whether a context file for this
investigation already exists, for example left behind by a previous session
working on the same bug. If it exists, read it fully to recover prior
findings, hypotheses, and decisions before doing anything else. If it does
not exist, create one and use it to record the investigation as it
progresses, so the work can be resumed even if the session is interrupted or
compacted.

Use the context file to establish, and keep updated, a concise mental model
of the software. Determine:

* what the software or affected subsystem does;
* who or what consumes it;
* the important architectural boundaries;
* the relevant data flow;
* the public interfaces involved;
* the expected invariants;
* where tests live and how they are run;
* whether compatibility constraints exist.

Read the smallest useful amount of surrounding documentation and code. Avoid
exploring the repository indiscriminately.

For the affected execution path, identify roughly:

`input → validation → transformation → state/business logic → output`

When the software exposes APIs, libraries, file formats, command-line interfaces,
schemas, persistent data, network protocols, or observable UI behavior, treat
these as compatibility-sensitive surfaces.

### 2. Precisely define the bug

Translate the bug report into an explicit behavioral statement.

Identify:

**Expected behavior:** what should happen.

**Observed behavior:** what actually happens.

**Trigger:** the smallest known input, state, sequence of actions, or environment
that causes the discrepancy.

**Scope:** which users, configurations, code paths, or versions appear affected.

Separate facts from hypotheses.

For example:

> Fact: parsing `foo=` produces `None`.
>
> Expected: it should produce an empty string.
>
> Hypothesis: the parser incorrectly treats empty values as missing values.

Do not silently reinterpret an ambiguous report. If several interpretations are
possible, use code, tests, specifications, history, or neighboring behavior to
infer the most consistent contract.

### 3. Reproduce the issue locally

Whenever practical, reproduce the reported behavior before changing the
implementation.

Prefer the smallest reproduction that still exercises the defective behavior.

Possible reproduction mechanisms include:

* a focused unit test;
* an integration test;
* a system or end-to-end test;
* a small executable example;
* a CLI invocation;
* a minimal HTTP request;
* a controlled dataset or fixture.

Record the exact conditions required to reproduce the issue.

If the issue cannot be reproduced, investigate why. Possible causes include:

* environment differences;
* timing or concurrency;
* stale or malformed data;
* version differences;
* configuration or feature flags;
* platform-specific behavior;
* hidden state;
* nondeterminism.

Do not fabricate a reproduction.

If reproduction is impossible but the defect is strongly supported by code
reasoning or production evidence, explicitly state that limitation.

### 4. Encode the bug as a regression test

Whenever reasonably possible, convert the reproduction into an automated test
before fixing the implementation.

This is the **red phase**.

The test should:

* fail on the current implementation;
* fail for the reason described in the bug report;
* express the intended behavior rather than the current implementation;
* be as small and deterministic as practical;
* avoid depending on irrelevant implementation details.

Run the test and confirm that it fails.

A test that unexpectedly passes is evidence that the assumed reproduction is
incomplete or incorrect. Investigate before proceeding.

Prefer the lowest test level capable of expressing the contract:

* unit test for isolated logic;
* integration test for component boundaries;
* system test when the defect depends on full-system behavior.

For important bugs, it can be appropriate to have both a focused regression test
and broader system coverage.

### 5. Determine the root cause

Trace the failing behavior to its cause.

Distinguish between:

**Symptom:** where incorrect behavior becomes visible.

**Proximate cause:** the operation that directly produces the incorrect result.

**Root cause:** the violated assumption, invariant, lifecycle rule, data contract,
or design expectation that allowed the defect to occur.

For example:

> Symptom: users receive duplicate notifications.
>
> Proximate cause: the notification worker sends the same job twice.
>
> Root cause: retry handling does not make notification delivery idempotent.

Prefer fixing the root cause when doing so is safe and proportionate.

Do not over-generalize from one example. Check neighboring cases and boundary
conditions that may reveal the true domain of the defect.

Useful questions include:

* Which invariant is being violated?
* Is an invalid state being created earlier than where the crash occurs?
* Is the defect caused by missing validation or incorrect transformation?
* Is state being shared unexpectedly?
* Is ordering assumed where none is guaranteed?
* Is there an off-by-one or boundary condition?
* Is a value being confused with its absence?
* Is the code violating a documented external contract?
* Is the system behaving incorrectly only under retries, concurrency, caching, or
  partial failure?

### 6. Inspect blast radius before modifying behavior

Before implementing a fix, identify what could depend on the current behavior.

Pay particular attention to:

* public APIs;
* method signatures;
* return types;
* exceptions and error codes;
* serialization formats;
* database schemas;
* command-line arguments;
* configuration;
* ordering guarantees;
* timing behavior;
* side effects;
* persisted state;
* network protocols;
* library consumers;
* extension points;
* mocks and test fixtures.

Incorrect behavior may still have become de facto behavior relied upon by
consumers.

When possible, preserve compatibility and repair the internal implementation.

If a contract must change, make that change explicit rather than allowing it to
emerge accidentally from the patch.

### 7. Design the smallest correct fix

Prefer the smallest change that restores the intended invariant.

A good fix usually:

* addresses the root cause;
* touches a limited area of the codebase;
* preserves unrelated behavior;
* avoids unnecessary refactoring;
* does not broaden public interfaces without need;
* remains understandable to future maintainers.

Avoid "while I am here" changes unless they are directly necessary for
correctness.

Do not hide bugs by:

* swallowing unexpected exceptions;
* returning placeholder values;
* adding arbitrary retries;
* disabling validation;
* weakening assertions;
* changing tests to accept the broken behavior;
* catching excessively broad exceptions;
* adding special cases without understanding why they are needed.

Sometimes the smallest *diff* is not the smallest *safe fix*. A slightly larger
change that restores an invariant may be preferable to a one-line workaround that
leaves the system inconsistent.

### 8. Consider edge cases before implementation

Before modifying the code, inspect nearby behavioral boundaries.

Depending on the domain, consider:

* empty values;
* null or missing values;
* zero;
* negative values;
* minimum and maximum limits;
* duplicated values;
* malformed inputs;
* Unicode or encoding;
* time zones and daylight-saving transitions;
* ordering;
* concurrency;
* retries;
* partial failures;
* stale state;
* backwards compatibility;
* overflow or precision;
* resource cleanup.

Add tests for additional cases only when they materially clarify the bug's
contract or prevent likely regressions.

Do not turn a focused bug fix into an exhaustive test expansion.

### 9. Implement the fix

Modify the implementation according to the chosen design.

Keep the patch focused.

Follow the repository's existing:

* coding conventions;
* abstractions;
* error-handling patterns;
* dependency boundaries;
* logging conventions;
* testing style.

Avoid introducing a new abstraction when an existing one already expresses the
needed behavior.

If the root cause indicates a missing invariant, enforce that invariant at the
most appropriate layer.

### 10. Run the regression test

Run the previously failing test.

This is the **green phase**.

Confirm that:

* the regression test now passes;
* it passes because the bug was fixed rather than because the assertion or
  fixture was weakened.

If the test still fails, revise the diagnosis instead of repeatedly patching
symptoms.

### 11. Run nearby tests

Run tests for the affected component, module, package, or subsystem.

This catches regressions that the focused test may not detect.

If the change affects shared infrastructure, expand the test scope accordingly.

Examples include changes to:

* serialization;
* authentication;
* database utilities;
* common parsers;
* concurrency primitives;
* shared domain models.

### 12. Run the full system test suite

When practical, run the project's complete automated test suite or the broadest
equivalent validation available.

The objective is to verify that the bug fix has not changed unrelated system
behavior.

Also run relevant static checks when the project uses them:

* type checking;
* linting;
* formatting validation;
* compilation;
* schema validation;
* security checks.

Do not claim that "all tests pass" unless the corresponding tests were actually
run.

If some tests cannot be run, state exactly which verification was performed and
what remains unverified.

### 13. Reproduce the original scenario again

After automated verification, repeat the original reproduction when practical.

A passing unit test does not necessarily prove that the real user-visible path
works.

For bugs originally observed through an API, CLI, UI, integration, or deployed
workflow, exercise that path again and verify that the original symptom is gone.

### 14. Review the patch as a consumer

Before declaring the work complete, inspect the diff from the perspective of users
and downstream code.

Ask:

* Did any public interface change?
* Did error semantics change?
* Did output ordering change?
* Did serialization change?
* Did new side effects appear?
* Did performance characteristics change materially?
* Did concurrency behavior change?
* Could persisted data now be interpreted differently?
* Does the fix require migration or configuration changes?
* Could consumers have relied on the previous behavior?

If the answer to any of these is yes, document it explicitly.

### 15. Document the bug and the fix

Leave a concise explanation covering:

**Root cause:** why the bug existed.

**Fix:** what changed and why that change corrects the problem.

**Verification:** how the failure was reproduced and which tests demonstrate the
fix.

**Consumer impact:** whether APIs, outputs, behavior, performance, data, or
operational requirements changed.

**Residual risk:** anything relevant that remains uncertain or untested.

A useful summary might look like:

> The parser treated empty values and missing values identically because both paths
> were normalized to `None`. The fix preserves empty strings during parsing while
> leaving missing keys unchanged. A regression test reproduces the original case,
> and the parser and system test suites pass. The public API is unchanged;
> consumers that previously worked around empty values being converted to `None`
> may observe the corrected behavior.

---

## Investigation heuristics

When the cause is unclear, prefer narrowing the search systematically rather than
reading large quantities of code.

Start from the observable failure and trace backwards.

Useful techniques include:

* inspect the failing call stack;
* search for the produced value or error;
* trace data mutations;
* compare working and failing inputs;
* inspect relevant tests;
* inspect recent changes when regression is suspected;
* temporarily add targeted logging or assertions;
* bisect behavior when version history is available;
* reduce the failing input until removing any remaining element makes the bug
  disappear.

Temporary diagnostic changes should normally be removed before completing the fix
unless they provide lasting operational value.

---

## Regression-specific workflow

If the behavior previously worked, determine when it changed if practical.

Inspect recent changes around the relevant subsystem and ask what assumption
changed.

Do not automatically revert the commit that introduced the regression. The earlier
change may itself be necessary.

Instead determine whether:

* the new behavior exposed a pre-existing bug;
* an invariant was accidentally removed;
* a compatibility case was missed;
* a test failed to cover an important interaction.

Use version-control history as evidence, not as a substitute for understanding the
current code.

---

## Concurrency and nondeterministic bugs

For race conditions, flaky tests, timing problems, and distributed failures,
avoid "fixes" based purely on increasing delays or retry counts.

Identify the missing synchronization, ordering guarantee, idempotency rule,
ownership model, or state transition.

Try to make the reproduction deterministic by controlling:

* scheduling;
* clocks;
* randomness;
* network behavior;
* retry timing;
* dependency responses.

Prefer fixing synchronization or protocol semantics over adjusting timing
constants.

---

## Production-only bugs

When a bug cannot safely or realistically be reproduced locally, work from
available evidence such as:

* logs;
* traces;
* metrics;
* crash reports;
* sanitized production inputs;
* database state;
* request metadata.

Construct the closest deterministic local model possible.

Never introduce logging that exposes secrets, credentials, personal data, or
sensitive payloads merely to investigate a bug.

When confidence remains limited, favor defensive fixes with narrow blast
radius and explicitly document the uncertainty.

---

## Test quality rules

Regression tests are part of the fix, not merely evidence for it.

Prefer tests that describe behavior:

> `empty_query_parameter_is_preserved`

over tests tied to implementation:

> `parser_calls_normalize_with_false`

A strong regression test should continue to be useful even if the internal
implementation is later rewritten.

Avoid tests that are:

* overly coupled to private implementation details;
* nondeterministic;
* dependent on external services without need;
* much broader than the behavior under investigation.

---

## Scope control

Bug fixing frequently reveals unrelated defects or cleanup opportunities.

Do not expand the current patch automatically.

Classify discoveries as:

**Required:** necessary for this bug to be fixed correctly.

**Closely related:** small change that significantly reduces regression risk.

**Unrelated:** should normally be left for separate work.

Prefer a reviewable, explainable patch over a large cleanup mixed with a
behavioral fix.

---

## Completion criteria

A bug should normally be considered fixed only when:

* the expected behavior is understood;
* the failure was reproduced, or the inability to reproduce it is documented;
* the root cause is understood with reasonable confidence;
* a regression test exists when practical;
* the regression test failed before the change;
* the implementation has been corrected;
* the regression test now passes;
* relevant surrounding tests pass;
* the broadest practical system validation passes;
* the original scenario was rechecked when practical;
* compatibility and consumer impact were considered;
* the root cause, fix, verification, and impact are documented;
* the context file is updated.

The final report must distinguish between what was **verified** and what was
merely **reasoned about**.

---

## Expected final report

When completing a bug fix, report the result concisely using this structure:

### Root cause

Explain the underlying reason for the defect.

### Fix

Explain the implementation change and why it is the appropriate scope.

### Tests

State:

* how the bug was reproduced;
* which regression test was added or changed;
* that the test failed before the fix, if verified;
* which focused and system tests were run.

### Compatibility and impact

Explain any observable behavior changes and whether existing consumers could be
affected.

Explicitly state when public interfaces are unchanged.

### Remaining risks

Mention unresolved uncertainty, untested environments, migration requirements, or
follow-up work.

Do not claim verification that was not actually performed.
