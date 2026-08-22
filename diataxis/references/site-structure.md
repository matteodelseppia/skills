# Structuring docs by mode inside a software company

When scaffolding docs for a service, repo, or team (or reorganizing during a
restructure), lay the four modes out as top-level siblings rather than
nesting or mixing them — but also decide deliberately which live in the repo
and which live in the internal wiki/docs platform, per `company-context.md`.

## In the repo (source of truth, changes land with the code)

```
service-name/
├── README.md              # short how-to: run/test/deploy this service,
│                          #   links out to everything below
├── docs/
│   ├── reference/         # generated where possible: API, config, schema
│   ├── runbooks/          # how-to guides for on-call — deploy, rollback,
│   │                      #   common alerts and their response
│   └── adr/               # architecture decision records, append-only
```

A minimal service README is itself a how-to guide ("how to build/test/run
this service locally") plus links — it is not the place for onboarding
tutorials or deep architecture explanation.

## In the internal wiki / docs platform (cross-repo, discoverability-first)

```
Team space
├── Onboarding/             # tutorials: local dev setup, first PR
├── How-to guides/          # cross-service procedures, not tied to one repo
├── Architecture/           # explanation: design docs, architecture overviews
└── Postmortems/            # explanation + action items, per incident
```

Cross-team or onboarding content belongs here rather than buried in one
repo's README, since new hires and other teams need to find it without
already knowing which repo to look in.

## In an internal service/API catalog (if your company has one)

Service ownership, API surface, and dependency information is reference
material — treat catalog entries as reference docs subject to the same rules
(structured for scanning, generated from source metadata where the catalog
supports it, one named owner per entry).

## Landing page pattern

The docs homepage (or README) should not itself be one of the four modes — it
should be a short index that routes readers to the right quadrant based on
what they need right now:

- "New here? Start with the tutorial." → tutorials/
- "Trying to do something specific?" → how-to-guides/
- "Looking up a detail?" → reference/
- "Want to understand how this works?" → explanation/

Keep this landing page terse — a sentence or two per quadrant plus the link,
not a summary of the content within.

## Cross-linking conventions

- How-to guides link to reference for parameter details rather than
  inlining them, and to explanation for background rather than justifying
  inline.
- Reference pages can link to explanation for "why is it designed this way"
  but shouldn't discuss it themselves.
- Explanation pages link to the relevant how-to guide when a reader would
  plausibly want to act on what they just learned.
- Tutorials link forward to how-to guides and explanation as "next steps"
  at the end, not mid-tutorial.

## Naming conventions

Prefix or title pages by their mode so readers self-select before opening:
"Tutorial: ...", "How to ...", "... reference", "Understanding ...". This
matters more than folder structure alone, since readers often land on a page
via search rather than by browsing the site.
