# Agentic Code Reasoning — Complete Reference

Based on: "Agentic Code Reasoning" by Shubham Ugare & Satish Chandra (arXiv:2603.01896, 2026)

---

## Why Semi-Formal Reasoning Works

Standard "chain of thought" prompting for code tasks allows agents to:
- Make claims without citing evidence
- Skip edge cases silently
- Guess function behavior instead of tracing it
- Jump to conclusions without interprocedural analysis

Semi-formal reasoning uses structured templates that **require** filling every field with gathered evidence. The template acts as a verification mechanism: you cannot reach FORMAL CONCLUSION without having stated explicit PREMISES and traced specific code paths with file:line citations.

**Results from the paper:**
| Task | Standard Agentic | Semi-Formal |
|------|-----------------|-------------|
| Patch Equivalence (curated) | 78% | 88% |
| Patch Equivalence (real-world) | — | 93% |
| Code QA (RubberDuckBench) | 78% | 87% |
| Fault Localization Top-5 | baseline | +5–12 pp |

---

## Universal Template Structure

All three task types share this four-phase skeleton:

```
DEFINITIONS: [Formal problem statement — define the success criteria precisely]

PREMISES: [Explicit, evidence-backed declarations about what the code does]
  - Each premise must cite file:line
  - No guessing — only what you verified by reading the code

ANALYSIS / CLAIMS: [Trace execution paths with evidence]
  - Follow function calls interprocedurally (read callees, don't assume)
  - CLAIM [N]: <statement> (evidence: file:line, file:line)
  - COUNTEREXAMPLE or PROOF as applicable

FORMAL CONCLUSION: [Derive answer solely from documented premises above]
  - If premises are insufficient, go back and gather more evidence
  - Never introduce new reasoning at conclusion time
```

---

## Exploration Protocol

When exploring a codebase, use this hypothesis-driven loop:

```
HYPOTHESIS: [What you expect to find at this location]
EVIDENCE: [What you actually found — specific lines, values, types]
CONFIDENCE: [HIGH / MEDIUM / LOW — based on evidence quality]
OBSERVATIONS: [file:line → what the code does]
HYPOTHESIS UPDATE: [Revised understanding after seeing evidence]
```

Iterate until confidence is HIGH before advancing to PREMISES.

---

## Task 1: Patch Equivalence

**Goal:** Determine whether two patches produce identical observable behavior on all relevant tests.

**Definition used in paper:**
Two patches are EQUIVALENT MODULO TESTS iff for every test in the test suite, both patches produce the same PASS/FAIL outcome.

### Template

```
DEFINITIONS:
  - Patch A: [brief description of modification]
  - Patch B: [brief description of modification]
  - Equivalence criterion: identical PASS/FAIL on all tests in scope

PREMISES:
  P1: Patch A modifies [file:line] — [what it changes]
  P2: Patch B modifies [file:line] — [what it changes]
  P3: Test suite contains [N] relevant tests: [list]
  P4: [Any shared behavior both patches preserve]

ANALYSIS:
  For each test T:
    TRACE (Patch A): entry → [call chain] → assertion [file:line]
      Result: PASS / FAIL — reason: [specific line that causes it]
    TRACE (Patch B): entry → [call chain] → assertion [file:line]
      Result: PASS / FAIL — reason: [specific line that causes it]
    CLAIM: T produces [same / different] outcome under A vs B
      Evidence: [file:line for each]

  If different outcome found:
    COUNTEREXAMPLE: Test [T], Patch A → [outcome], Patch B → [outcome]
    Evidence: [file:line]

FORMAL CONCLUSION:
  [EQUIVALENT / NOT EQUIVALENT] because [cite specific claims above]
  [If not equivalent: counterexample test and the diverging lines]
```

---

## Task 2: Fault Localization

**Goal:** Given a failing test, rank the most likely buggy code locations.

### Template

```
PHASE 1 — TEST SEMANTICS (PREMISES):
  Test: [file:line of test]
  PREMISE: The test asserts [exact assertion, file:line]
  PREMISE: Expected behavior: [what must be true for PASS]
  PREMISE: Actual behavior: [what appears to happen, from test failure]
  PREMISE: Test entry point calls [method at file:line]

PHASE 2 — EXECUTION PATH TRACE:
  Starting from test entry, follow the call chain:
    METHOD: [name] at [file:line]
    PARAMETERS: [types and expected values]
    BEHAVIOR: [what this method does — read it, don't assume]
    CALLS: [callees at file:line]

  Continue until reaching the divergence point or exhausting paths.
  Use HYPOTHESIS/EVIDENCE loop (see Exploration Protocol) at each step.

PHASE 3 — DIVERGENCE IDENTIFICATION (CLAIMS):
  CLAIM 1: Implementation at [file:line] does [X]
  CLAIM 2: Test expects [Y] (evidence: [file:line])
  CLAIM 3: X ≠ Y because [reason] — this is a candidate bug location

  Repeat for each candidate location found.

PHASE 4 — RANKED PREDICTIONS:
  Rank 1: [file:line] — [reason, citing CLAIM N]
  Rank 2: [file:line] — [reason, citing CLAIM N]
  Rank 3: [file:line] — [reason, citing CLAIM N]
  ...

  Each rank must cite at least one CLAIM from Phase 3.
```

---

## Task 3: Code Question Answering

**Goal:** Answer a semantic question about code behavior, invariants, or data flow.

### Template

```
DEFINITIONS:
  Question: [restate precisely]
  Answer form: [what kind of answer is expected — yes/no, a value, a description]

FUNCTION TRACE TABLE:
  For every function examined:
  | Function | file:line | Parameters | Returns | Verified Behavior |
  |----------|-----------|------------|---------|-------------------|
  | [name]   | [loc]     | [types]    | [type]  | [what it does]    |

  Rule: only mark "Verified" after reading the implementation.

DATA FLOW ANALYSIS:
  Key variable: [name]
  Created at: [file:line] — initial value: [value/type]
  Modified at: [file:line] — new value: [transformation]
  Consumed at: [file:line] — used as: [how]

  Repeat for each variable relevant to the question.

SEMANTIC PROPERTIES:
  INVARIANT 1: [property that always holds] — evidence: [file:line]
  INVARIANT 2: [property that always holds] — evidence: [file:line]
  ...

ALTERNATIVE HYPOTHESIS CHECK:
  Opposing answer would require: [what would need to be true]
  Evidence against: [file:line citations that rule it out]
  Conclusion: alternative is [supported / refuted] because [reason]

FORMAL CONCLUSION:
  Answer: [direct answer to question]
  Because: [derive from INVARIANTS and DATA FLOW above — no new reasoning]
  Confidence: [HIGH / MEDIUM / LOW] — [what would increase confidence]
```

---

## Key Rules

1. **Read callees, don't assume.** Never write "this function probably does X." Open the file and verify.
2. **Every claim needs file:line.** If you can't cite a line, you don't have evidence.
3. **Fill all fields.** If a template section is empty, you haven't done the work yet.
4. **Alternative hypothesis check is mandatory** for QA tasks. Actively try to disprove your answer.
5. **Interprocedural by default.** Tracing a path means following every function call in that path.
6. **Confidence gates conclusions.** Only write FORMAL CONCLUSION when all PREMISES are HIGH confidence.
7. **Python probes are allowed.** For language behavior questions (e.g., "does Python's `int()` truncate or round?"), a small standalone script is acceptable evidence.

---

## Agent Setup (from paper)

The paper uses a minimal SWE-agent configuration:
- Tools: bash (file navigation, grep, git log — **no test execution**)
- Max steps: 100
- No sandbox, no test running
- Static analysis only
- Python probes allowed for language-level behavior (no I/O, no imports from the repo)

This constraint is intentional: the methodology proves that structured reasoning alone — without running code — achieves strong results.
