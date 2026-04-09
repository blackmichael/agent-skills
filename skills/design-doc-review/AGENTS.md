# Design Doc Review — Complete Reference

Based on: Grant Slatton, "How to write a good design document" (2025-02-01)

---

## Standard

Treat a design document as a technical report whose job is to justify an implementation strategy in the context of constraints and trade-offs.

The question is not "does this look like a design doc?" The question is "would a skeptical engineer finish this document convinced that this is the right design for this situation?"

Review against that standard.

---

## Primary Failure Modes

### 1. The doc does not define the problem precisely enough

Symptoms:
- The reader cannot tell what decision is being requested
- Constraints, assumptions, or success criteria are implicit
- The proposal is described before the problem is pinned down

Review comments should call out:
- Missing problem statement
- Missing scope boundaries
- Missing constraints, trade-offs, or decision criteria

### 2. The doc is "spaghetti prose"

Symptoms:
- Sentences contain the right ingredients but appear in the wrong order
- The doc introduces solutions before readers have the mental model to evaluate them
- The reader must do reconstruction work to understand why a section exists

Review comments should call out:
- Reordering needed to make later claims feel obvious
- Missing transitions between observations and conclusions
- Sections that mix background, proposal, and evaluation in one block

### 3. The proposed design appears before it feels earned

Symptoms:
- The solution shows up as an assertion instead of a conclusion
- Alternatives are dismissed quickly or not at all
- Key trade-offs are not surfaced before the recommendation

Review comments should call out:
- Where the doc should first establish facts or constraints
- Which objections a reasonable reviewer would raise
- Which alternatives deserve explicit analysis

### 4. The author has not modeled the reader

Symptoms:
- Terms, systems, or assumptions are introduced as if already shared
- The doc answers the author's private questions instead of the audience's likely questions
- Background is too thin for unfamiliar readers or too bloated for familiar readers

Review comments should call out:
- Missing context the intended audience needs
- Explanations that solve the wrong confusion
- Places where the reader's likely objection is left unstated

### 5. Paragraphs carry too many ideas

Symptoms:
- Long paragraphs cannot be summarized in one sentence
- Multiple claims and caveats are packed together
- Reviewers must re-read to understand the point

Review comments should call out:
- Paragraphs that should be split
- Single ideas that should become their own paragraphs
- Topic sentences that do not match the body

### 6. The prose wastes reader attention

Symptoms:
- Repetition, throat-clearing, and filler
- Verbose phrasing where a shorter version says the same thing
- Detailed calculations, simulations, or data dumps in the body

Review comments should call out:
- Content that can be cut without losing meaning
- Supporting detail that should move to an appendix
- Places where the body should cite a result and defer proof

---

## Review Procedure

### Pass 1: Extract the argument

Write down:
- What problem is being solved
- What decision the author wants approved
- What constraints matter
- What the proposed design is
- Why the author thinks it is the right choice

If you cannot extract these quickly, that is already a top-level finding.

### Pass 2: Check logical order

For each section, ask:
- Why is this section here?
- What belief should the reader hold after reading it?
- Does the next section depend on that belief?

Flag any section that:
- Arrives too early
- Repeats prior content
- Depends on unstated context
- Forces the reader to hold too many unresolved ideas at once

### Pass 3: Simulate a skeptical reviewer

Ask:
- What obvious objection would I raise here?
- Did the doc answer it before I had to ask?
- What alternative would I want compared?
- What evidence or trade-off analysis is missing?

If the objection is foreseeable and unanswered, treat that as a major issue.

### Pass 4: Compress the prose

For each paragraph, ask:
- Can this be summarized in one sentence?
- Is that sentence worth keeping?
- Can 20-30% be cut with no loss of information?
- Should this detail move to an appendix?

Prefer direct edits such as:
- Split this paragraph after the first claim
- Move the derivation to an appendix and keep only the conclusion here
- Replace three sentences of setup with one sentence naming the constraint

---

## Review Rubric

Score each dimension informally as `strong`, `mixed`, or `weak`.

### Problem framing

Strong:
- Problem, goals, constraints, and non-goals are explicit
- The document makes clear what decision is under review

Weak:
- The proposal exists in search of a problem
- Key constraints only appear later or not at all

### Persuasive flow

Strong:
- The reader is prepared for the conclusion
- Each section naturally enables the next

Weak:
- The recommendation feels dropped in
- Trade-offs are discussed after the conclusion, if at all

### Objection handling

Strong:
- Obvious alternatives and risks are addressed
- Claims are supported by reasoning or evidence

Weak:
- Counterarguments are absent, shallow, or hand-waved

### Reader modeling

Strong:
- Background matches the audience
- The doc teaches the right mental model before using it

Weak:
- The doc assumes context the reader may not have
- The doc over-explains trivia while skipping critical setup

### Paragraph quality

Strong:
- One idea per paragraph
- Short paragraphs with clear purpose

Weak:
- Dense blocks mixing observations, conclusions, and caveats

### Concision

Strong:
- Minimal filler
- Support detail is pushed out of the body when possible

Weak:
- Repetition, longwinded phrasing, or unnecessary derivations in the core argument

---

## Response Template

Use this format when the user wants a review:

```markdown
Verdict: <one-sentence judgment on whether the doc currently convinces the reader>

Major findings
- <finding>: <why it matters to persuasion or correctness>
- <finding>: <why it matters to persuasion or correctness>

Flow and organization
- <specific ordering or transition issue>

Objections and trade-offs
- <unanswered reviewer question>

Conciseness and paragraph quality
- <where to cut, split, or move detail to appendix>

Suggested rewrite plan
1. <restructure the argument>
2. <add missing trade-off or alternative analysis>
3. <tighten prose and paragraph boundaries>
```

For line-by-line review, attach comments to the exact quoted passage or section heading.

---

## Rewrite Guidance

When asked to improve a doc, do not only critique it. Rebuild the argument in this order unless the document's context demands otherwise:

1. Problem and constraints
2. Relevant observations or system facts
3. Derived requirements
4. Candidate approaches and trade-offs
5. Recommended design
6. Risks, mitigations, and open questions
7. Appendix for proofs, derivations, simulations, and bulky supporting detail

The proposal should feel like the consequence of the earlier sections, not a surprise.

---

## What Not to Reward

Do not over-credit:
- Fancy formatting
- Long docs
- Exhaustive appendices when the main argument is weak
- Section headers that exist without a coherent argument beneath them

The document succeeds only if it carries the reader from current beliefs to the intended conclusion with minimal friction.
