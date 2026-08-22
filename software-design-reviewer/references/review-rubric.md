# Software design review rubric

Use this as a question bank. Select questions based on the system and review scope; do not produce a checklist dump.

## Intent and requirements

- Is the user or business problem concrete and measurable?
- Are goals, non-goals, assumptions, constraints, and dependencies explicit?
- Are functional requirements testable and unambiguous?
- Are non-functional requirements quantified where possible: availability, latency, throughput, durability, recovery time, recovery point, privacy, and cost?
- Are success metrics, acceptance criteria, and launch criteria defined?
- Does every major requirement map to a design element and a validation method?
- Are unknowns and decisions still needed clearly separated from settled facts?

## Architecture and boundaries

- Is the system context clear: users, external systems, ownership, and trust boundaries?
- Are component boundaries aligned with responsibilities, data ownership, team ownership, and change cadence?
- Are dependencies directional and justified?
- Are synchronous and asynchronous interactions distinguished?
- Are data flows, state transitions, and critical sequences understandable?
- Do diagrams, prose, APIs, and schemas agree on names, relationships, and behavior?
- Are abstraction levels separated rather than mixed in one diagram?
- Does the design introduce unnecessary services, queues, databases, or frameworks?
- Are there hidden shared databases, circular dependencies, or single points of failure?

## Alternatives and decisions

- Are credible alternatives considered, including keeping the current design?
- Are decision drivers explicit: latency, reliability, cost, team expertise, time to market, compliance, or reversibility?
- Are important trade-offs and rejected alternatives recorded?
- Is the decision reversible, and what is the cost of changing it later?
- Does each major decision have a clear owner and an ADR or equivalent record?

## API, events, and data

- Are interfaces defined with request/response or event schemas, errors, authentication, authorization, and rate limits?
- Are idempotency, ordering, retries, timeouts, pagination, and partial failure behavior defined?
- Is compatibility policy clear for clients, services, events, and stored data?
- Are data owners, retention, classification, residency, encryption, and deletion requirements addressed?
- Are transactions, consistency expectations, concurrency, and conflict resolution explicit?
- Is the migration/backfill strategy safe, observable, resumable, and reversible?
- Are schema evolution and deprecation paths defined?

## Security and privacy

- Are assets, actors, trust boundaries, threats, and abuse cases identified?
- Is least privilege applied to users, services, agents, tools, databases, and deployment identities?
- Are authentication, authorization, tenant isolation, secrets, key rotation, and audit logging specified?
- Could untrusted input influence queries, commands, templates, prompts, tools, or generated output?
- Are sensitive data flows, redaction, retention, deletion, and access review addressed?
- Are supply-chain, dependency, artifact, and deployment integrity risks considered?
- For LLM/agent systems: are prompt injection, data poisoning, insecure output handling, excessive agency, tool misuse, model/provider failure, and human approval boundaries addressed?

## Reliability and recovery

- What happens when each dependency is slow, unavailable, inconsistent, or returns malformed data?
- Are timeouts, retries, backoff, circuit breaking, deduplication, rate limits, and load shedding bounded?
- Are failure domains, redundancy, health checks, and graceful degradation described?
- Are SLOs, error budgets, durability, RTO, RPO, backup, restore, and disaster recovery defined?
- Can operators detect, diagnose, contain, and recover from the important failure modes?
- Is recovery tested rather than merely documented?

## Performance, capacity, and cost

- What workload model supports the design: users, requests, payload sizes, read/write mix, peaks, growth, and tenancy?
- Are latency budgets and tail latency targets defined?
- Where are the likely bottlenecks: network, CPU, memory, storage, locks, queues, rate limits, or third-party APIs?
- Does the design use appropriate caching, batching, partitioning, indexing, streaming, and backpressure?
- What is the capacity plan and scaling trigger?
- Is the cost model explicit for normal, peak, idle, and failure scenarios?
- For LLM/agent systems: are token usage, model latency, tool calls, concurrency, fallback models, and evaluation costs estimated?

## Operations and observability

- Who owns the service and its dependencies in production?
- Are logs, metrics, traces, dashboards, alerts, and correlation identifiers defined?
- Can operators distinguish user errors, dependency failures, capacity issues, data defects, and model failures?
- Are alerts actionable and tied to SLOs or operational symptoms?
- Are runbooks, escalation paths, support procedures, and audit trails available?
- Are configuration, feature flags, secrets, model versions, prompts, and policies versioned?

## Delivery, migration, and lifecycle

- Is the rollout staged with an explicit blast radius and success criteria?
- Are feature flags, canaries, shadow traffic, dark launches, and rollback paths appropriate?
- Can old and new versions coexist safely during migration?
- Are test levels specified: unit, contract, integration, load, resilience, security, migration, and end-to-end?
- What data, API, or operational cleanup is required after rollout?
- Is deprecation, ownership transfer, and long-term maintenance addressed?

## Repository and implementation conformance

When repository access is available:

- Does the proposal match existing service boundaries, libraries, deployment model, and coding conventions?
- Are referenced modules, endpoints, events, schemas, and configuration present and used as described?
- Does it duplicate an existing capability or contradict an accepted ADR?
- Are the proposed changes reflected in tests, CI/CD, observability, infrastructure, and documentation?
- Are migration and compatibility assumptions supported by actual data and usage patterns?

When repository access is unavailable, state that these checks remain unverified.
