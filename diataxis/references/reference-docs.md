# Reference — information-oriented

**Purpose:** describe the machinery accurately and completely so a reader who
already knows what they're doing can look up one specific fact fast, mid-task.
Reference documentation is consulted, not read cover to cover.

Inside a software company this covers internal API/config reference,
service catalog entries, CLI reference, and schema/data dictionaries. This is
also the mode most worth generating from source (docstrings, OpenAPI specs,
`--help` output, schema definitions) rather than hand-maintaining — hand-
written reference is the likeliest of the four modes to silently drift from
what the code actually does. Prefer "generate it" over "write it" whenever
tooling makes that possible, and say so if a user asks you to hand-write
something that should instead be generated.

## Rules

- **Structure for scanning, not reading.** Consistent headings, tables,
  definition lists. A reader should be able to jump straight to the one
  section they need via a heading, a table of contents, or search — never
  need to read from the top.
- **Mirror the structure of the thing it describes.** If it's an API, one
  section per endpoint/method, in a consistent order (name, parameters,
  return value, errors, example). If it's a config file, one entry per key,
  same order every time. Consistency is what makes it scannable.
- **State facts, not reasoning.** No "because," no "the idea behind this is."
  If you're explaining *why* something is the way it is, that content belongs
  in an explanation document — link to it, don't inline it.
- **Complete and accurate above all.** Reference is judged on correctness and
  completeness, not on how enjoyable it is to read. Missing an edge case, an
  optional parameter, or an error code is a real defect here in a way it
  usually isn't for a how-to guide.
- **Neutral, terse register.** Description of behavior, not instructions on
  what to do with it — "Returns a 404 if the resource does not exist," not
  "You should handle the 404 case by...".
- **Include a minimal example per entry where useful**, but keep it
  illustrative, not a tutorial — a single line showing usage, not a
  walkthrough.

## Common mistakes (mode leakage)

- Narrative prose explaining design rationale inline instead of linking to
  explanation.
- Step-by-step instructions embedded in what should be a parameter table —
  that content belongs in a how-to guide.
- Inconsistent structure across entries (one endpoint documented with
  examples and error codes, another missing them) — breaks scannability.
- Omitting edge cases or defaults because they seemed obvious — reference is
  exactly where "obvious" things need to be stated.

## Self-review checklist

- [ ] Can a reader find one specific fact without reading the whole page?
- [ ] Is every entry structured identically to its siblings?
- [ ] Is all "why"/rationale content removed or linked out to explanation?
- [ ] Is it complete — every parameter, option, error case, default covered?
- [ ] Is the tone neutral and descriptive, not instructional or narrative?
- [ ] Could this be generated from source instead of hand-maintained? If so,
      flag that rather than settling for a hand-written copy that will drift.
- [ ] Does it have a named owner if it can't be generated?
