# Rule: Code Question Answering

## What It Is

Answer semantic questions about code behavior, data flow, or program invariants by building a verified evidence base before concluding — never guessing.

## When to Use

- "What does this function return when X?"
- "Can this value ever be null here?"
- "What's the data flow for this variable?"
- "Does this code handle edge case Y?"
- Any question about program semantics requiring more than surface reading

## Template

```
DEFINITIONS:
  Question: [restate precisely in one sentence]
  Answer form: [yes/no | a value | a description | a list]
  Scope: [files/modules relevant to answering]

FUNCTION TRACE TABLE:
  Build this table by reading each function before adding a row.
  Never mark "Verified" without opening the implementation.

  | Function   | file:line | Parameters        | Returns | Verified Behavior          |
  |------------|-----------|-------------------|---------|----------------------------|
  | [name]     | [loc]     | [name: type, ...] | [type]  | [what it actually does]    |
  | [callee]   | [loc]     | [name: type, ...] | [type]  | [what it actually does]    |

DATA FLOW ANALYSIS:
  Key variable: [name]
  Created:  [file:line] — initial value / type: [value]
  Modified: [file:line] — transformation: [how value changes]
  Modified: [file:line] — transformation: [how value changes]
  Consumed: [file:line] — used as: [how it's used]

  [Repeat block for each variable relevant to the question]

SEMANTIC PROPERTIES:
  INVARIANT 1: [property that always holds] — evidence: [file:line]
  INVARIANT 2: [property that always holds] — evidence: [file:line]
  BOUNDARY 1: [edge case behavior] — evidence: [file:line]

ALTERNATIVE HYPOTHESIS CHECK:
  If the answer is [X], the opposite answer [not-X] would require:
    [what would need to be true in the code]
  Evidence against [not-X]:
    [file:line] — [why this rules out the alternative]
  Conclusion: [not-X] is refuted / unsupported because [reason]

FORMAL CONCLUSION:
  Answer: [direct answer to the question]
  Because: [derive from INVARIANTS, DATA FLOW, and FUNCTION TRACE above — no new reasoning]
  Confidence: HIGH / MEDIUM / LOW
  If MEDIUM or LOW: [what additional evidence would raise confidence]
```

## Exploration Protocol

When you don't know where to look:

```
HYPOTHESIS: [what you expect to find]
EVIDENCE: [what you actually found — file:line]
CONFIDENCE: HIGH / MEDIUM / LOW
OBSERVATIONS:
  file:line -> [what the code does]
HYPOTHESIS UPDATE: [how your model changed]
```

Iterate until you have HIGH confidence before adding a row to the Function Trace Table.

## Common Pitfalls

- **Skipping the alternative hypothesis check** — this is mandatory, not optional. Actively try to disprove your answer.
- **Shallow function trace** — if a function calls others, trace those too.
- **Assuming parameter types** — read call sites to determine actual types passed.
- **Ignoring null/undefined paths** — check every conditional that guards against null.
- **Conflating "usually" with "always"** — INVARIANT must hold on all paths; use BOUNDARY for conditional behavior.
