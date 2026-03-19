# Rule: Fault Localization

## What It Is

Given a failing test, rank the most likely buggy code locations by tracing the execution path and identifying where the implementation diverges from expected behavior — without running the test.

## When to Use

- "Where is this bug?"
- "This test is failing, help me find the cause"
- Automated bug triage or ranking of suspicious locations

## Template

```
PHASE 1 — TEST SEMANTICS (PREMISES):
  Test file: [file:line]
  PREMISE: The test asserts [exact assertion text] at [file:line]
  PREMISE: Expected behavior: [what must be true for the test to PASS]
  PREMISE: Apparent failure: [what the test message / signature suggests is wrong]
  PREMISE: Test entry point calls [method] at [file:line]

PHASE 2 — EXECUTION PATH TRACE:
  For each method in the call chain from test entry to the likely failure:

  METHOD: [name]
  LOCATION: [file:line]
  PARAMETERS: [types and expected values given by test]
  BEHAVIOR: [what this method does — read the source, do not assume]
  CALLS: [callees at file:line that need further tracing]

  HYPOTHESIS: [what you expect to find at the next level]
  EVIDENCE: [what you actually found — specific lines, values, logic]
  CONFIDENCE: [HIGH / MEDIUM / LOW]
  HYPOTHESIS UPDATE: [revised understanding]

  [Repeat for each level until divergence found or paths exhausted]

PHASE 3 — DIVERGENCE IDENTIFICATION (CLAIMS):
  CLAIM 1: Implementation at [file:line] does [X]
  CLAIM 2: Test expects [Y] (evidence: [file:line])
  CLAIM 3: X != Y because [reason] — candidate bug location

  [Add one CLAIM set per candidate location]

PHASE 4 — RANKED PREDICTIONS:
  Rank 1: [file:line] — [reason, citing CLAIM N above]
  Rank 2: [file:line] — [reason, citing CLAIM N above]
  Rank 3: [file:line] — [reason, citing CLAIM N above]
  ...

  Rule: every rank must cite at least one CLAIM from Phase 3.
```

## Exploration Protocol (use during Phase 2)

When uncertain which path to follow:

```
HYPOTHESIS: [what you expect to find]
EVIDENCE: [what you actually found at file:line]
CONFIDENCE: HIGH / MEDIUM / LOW
OBSERVATIONS:
  file:line -> [what the code does]
  file:line -> [what the code does]
HYPOTHESIS UPDATE: [how your model changed]
```

Only advance to CLAIMS when confidence is HIGH on the relevant path.

## Common Pitfalls

- **Staying shallow** — tracing only one level deep. Follow calls interprocedurally.
- **Assuming correct behavior** — read every method in the chain; don't assume it works as named.
- **Single-candidate bias** — always look for at least 2–3 candidate locations before ranking.
- **Ignoring setup/teardown** — test fixtures and before/after hooks are frequent bug sites.
