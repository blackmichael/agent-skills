# Rule: Patch Equivalence Verification

## What It Is

Determine whether two code patches produce identical observable behavior on all relevant tests — without executing them.

## When to Use

- "Are these two fixes equivalent?"
- "Does my patch match the reference solution?"
- "Will this refactor change any behavior?"
- Automated code review comparing candidate patches

## Template

```
DEFINITIONS:
  - Patch A: [brief description — what files/lines it changes and how]
  - Patch B: [brief description — what files/lines it changes and how]
  - Equivalence: EQUIVALENT iff every test in scope produces the same PASS/FAIL under A and B
  - Test scope: [list of tests or "full suite"]

PREMISES:
  P1: Patch A modifies [file:line] — [description of change]
  P2: Patch B modifies [file:line] — [description of change]
  P3: Both patches share [shared unchanged code at file:line] — [description]
  P4: Test T1 at [file:line] asserts [assertion]
  P5: Test T2 at [file:line] asserts [assertion]
  ... (one premise per test, verified by reading)

ANALYSIS:
  === Test T1 ===
  Patch A execution trace:
    [test entry at file:line] -> [callee at file:line] -> ... -> [assertion at file:line]
    Result: PASS/FAIL — because [specific line that determines outcome]

  Patch B execution trace:
    [test entry at file:line] -> [callee at file:line] -> ... -> [assertion at file:line]
    Result: PASS/FAIL — because [specific line that determines outcome]

  CLAIM 1: T1 produces [same/different] outcome under A vs B
    Evidence: A->file:line, B->file:line

  === Test T2 ===
  [repeat above structure]

  [If non-equivalence found:]
  COUNTEREXAMPLE: Test [T], Patch A -> [outcome], Patch B -> [outcome]
    Divergence point: [file:line] — A does [X], B does [Y]

FORMAL CONCLUSION:
  [EQUIVALENT / NOT EQUIVALENT]
  Because: [cite claims above]
  [If not equivalent: state the counterexample test and diverging behavior]
```

## Common Pitfalls

- **Assuming shared code paths are identical** — verify that both patches call into the same code for non-modified paths
- **Stopping at first equivalent test** — must check all tests in scope
- **Ignoring indirect effects** — a change in one method may affect callers
- **Skipping edge cases** — null inputs, empty collections, boundary values often expose divergence
