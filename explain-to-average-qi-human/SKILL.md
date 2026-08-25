---
name: explain-to-average-qi-human
description: >
  Explain complex technical or conceptual systems from first principles to a
  reader with ordinary general intelligence and some coding literacy. Use when
  the user asks for a deep explanation without assuming hidden background
  knowledge, especially for software architecture, distributed systems,
  AI/agents, protocols, authentication, state, concurrency, storage,
  networking, or other layered systems.
license: MIT
---

# Explain to Average-QI Human

Explain the subject deeply without assuming that familiarity with terminology
implies understanding of the underlying mechanism.

The target reader is intelligent and can follow code, abstractions, and simple
diagrams, but may not know the domain's implicit conventions. Do not dumb the
topic down. Remove hidden assumptions instead.

## Core rule

Build the explanation from the smallest useful mental model upward.

Before explaining implementation details, answer:

1. What are the entities involved?
2. Which entity owns which responsibility?
3. What information moves between them?
4. What state exists, where does it live, and how long does it live?
5. What causes the system to move from one state to another?
6. What happens when something fails, restarts, or is repeated?

Prefer causal explanations over definitions.

Bad:

> A session is persisted in SQLite and restored on startup.

Better:

> The model itself does not need to stay alive. The application stores the
> conversation locally. When the application starts again, it reads that stored
> history and sends enough of it back to a new model invocation. The continuity
> comes from reconstructing context, not from waking up the old model.

## Explanation method

Start with a one- or two-sentence thesis that reveals the central mechanism.

Then construct a concrete minimal example and follow it end to end. Use names
and values rather than abstract placeholders when possible.

For example, instead of:

> The client invokes a tool and returns its result to the model.

use:

```text
User: "Why are my tests failing?"

Model: "Run npm test."

Agent harness: executes npm test.

Tool result: "parser.test.ts failed: expected 42, got 41"

Agent harness: sends that result back to the model.

Model: "Read parser.ts next."
```

Only introduce terminology after the reader has seen the mechanism it names.

## Separate layers aggressively

When multiple concepts are commonly conflated, explicitly distinguish them.

Examples:

```text
LLM             = reasoning engine
Agent harness   = program controlling the reasoning engine
Coding agent    = harness + LLM + tools + state + environment
```

```text
Session = long-lived conversation/history
Message = one item in that history
Run     = one execution triggered by one user prompt
```

```text
Persistent state = survives restart
Model context    = information sent in one model request
Live state       = work currently happening in the running process
```

Do not proceed until these boundaries are conceptually clear.

## Never give hidden machinery for granted

When using a term such as authentication, context, tool call, streaming,
session, retry, queue, event, process, API, state machine, token, credential,
protocol, or idempotency, explain what physically or logically happens unless
the user has already demonstrated that understanding.

Prefer formulations like:

> "Authentication" here means that the program attaches a secret credential to
> an outgoing request so the receiving service can decide whether to accept it.

rather than:

> The client authenticates with bearer auth.

Technical vocabulary may follow after the mechanism is clear.

## Use diagrams when topology or control flow matters

Prefer small ASCII diagrams over verbose prose when relationships are easier to
see spatially.

Example:

```text
User -> Harness -> Model
          ^          |
          |          | tool request
          +-- Tool <-+
              result
```

For state transitions:

```text
Requesting -> Streaming -> Completed
                  |
                  v
           AwaitingApproval
                  |
                  v
            ExecutingTool
                  |
                  +----> Requesting
```

Every diagram must be explained in prose. Do not make the reader
reverse-engineer it.

## Explain state explicitly

For any stateful system, identify at least:

- authoritative state;
- cached or derived state;
- transient/live state;
- persistent state;
- what survives restart;
- what is deliberately not resumed.

Use failure or restart scenarios to expose the distinction.

Example:

> If the application crashes after a command executes but before it records
> completion, blindly replaying the command might perform the side effect twice.
> Therefore a safe MVP may preserve the conversation but mark the old run
> interrupted instead of resuming execution automatically.

This kind of counterexample is preferred because it explains why the
architecture exists.

## Explain communication as messages

When explaining APIs, protocols, agents, frontends/backends, or distributed
systems, show representative messages.

Example:

```text
send_prompt({
  session_id: "51",
  prompt: "Fix the parser"
})
```

Then explain what happens after that message is received.

Do not focus on serialization syntax, framework names, transport libraries, or
language-specific code unless they materially affect the mechanism the user
asked about.

## Explain tools as capability requests

When explaining AI agents, emphasize:

> The model does not execute tools. It emits a structured request asking the
> harness to execute a capability.

Then show the loop:

```text
model reasoning
    -> tool request
    -> harness validates
    -> optional human approval
    -> harness executes
    -> tool result
    -> model reasons again
```

Explain that the harness, not the model, is the authority for permissions,
validation, execution, cancellation, and limits.

## Explain memory without anthropomorphism

Avoid saying that a stateless model "remembers" unless actual provider-side
state is specifically established.

Prefer:

> The application stores earlier messages and includes relevant ones in a later
> request, making the new model invocation behave as if the conversation
> continued.

Explicitly distinguish stored history from active context. A system may store
far more history than it sends to the model on any one request.

## Use comparisons carefully

Analogies are useful only after mapping their parts.

Good:

> The LLM/harness relationship is somewhat like a userspace program and an
> operating system: the program requests operations, but the OS decides what is
> legal and performs the privileged work. Here the LLM proposes a `read_file`
> action; the harness validates the path and actually reads the disk.

Bad:

> It's basically an operating system.

State where the analogy stops being exact when that matters.

## Depth progression

Prefer this order:

1. Central mental model.
2. Concrete end-to-end example.
3. Components and ownership boundaries.
4. Communication flow.
5. State and persistence.
6. Failure/restart/cancellation behavior.
7. Subtleties and edge cases.
8. Compact summary mental model.

Do not start with an inventory of classes, modules, libraries, frameworks, or
interfaces.

## Style

Write for an adult with some coding expertise.

Be direct, dense, and patient without sounding patronizing.

Do not use phrases such as "it's simple", "obviously", "just", or "as you know"
when they hide complexity.

Avoid excessive bullet lists. Prefer short explanatory sections, compact prose,
small examples, and diagrams.

Do not equate accessibility with shallowness. The explanation should remain
technically accurate and may go deep into concurrency, security, state, or
protocol semantics when needed.

When the user asks for implementation-independent understanding, strip away
framework and language names unless a choice changes the conceptual behavior.

## Final check

Before answering, verify:

- Could a reader explain who actually performs each action?
- Could they say where each important piece of state lives?
- Could they trace one request from user input to final result?
- Could they explain what survives a restart?
- Could they distinguish the model's responsibilities from the surrounding
  application's?
- Were important terms explained through mechanism rather than renamed with
  jargon?
- Did examples make the invisible message flow visible?

If not, revise the explanation before responding.
