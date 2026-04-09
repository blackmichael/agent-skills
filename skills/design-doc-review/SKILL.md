---
name: design-doc-review
description: Review software design documents for clarity, organization, rigor, and persuasive quality. Based on Grant Slatton's "How to write a good design document". Use when asked to critique a technical design doc, architecture proposal, RFC, or implementation plan and identify where the writing fails to convince the reader the design is optimal given constraints and trade-offs.
license: MIT
metadata:
  author: blackmichael
  version: "1.0.0"
  source: https://grantslatton.com/how-to-design-document
---

# Design Doc Review

Review a design document as a technical argument, not just a bundle of sections. The standard is whether the document leads a skeptical reader from the problem statement to the conclusion without surprise.

## Core Review Lens

Check whether the doc:
1. Defines the problem, constraints, and success criteria clearly
2. Makes the proposed design feel inevitable by the time it appears
3. Anticipates and answers obvious objections
4. Uses structure that matches the reader's order of understanding
5. Respects reader attention through concise paragraphs and trimmed prose
6. Moves complex supporting detail into appendices instead of clogging the main line of reasoning

## How to Review

1. Read once for the argument:
   Track the intended claim, the target reader, and the decisions the doc wants approved.
2. Read again for flow:
   Check whether each paragraph contributes one clear idea and whether each section logically enables the next.
3. Read again for objections:
   Look for unanswered trade-offs, hidden assumptions, unsupported claims, and places where a reviewer would stop and ask "why?"
4. Read again for compression:
   Mark repetition, throat-clearing, long paragraphs carrying multiple ideas, and body content that belongs in an appendix.

## Output Format

Use this structure unless the user asks for something else:

```markdown
Verdict: <one-sentence assessment>

Major findings
- <issue>: <why it weakens the document>

Flow and organization
- <issue>: <missing transition, bad ordering, reader surprise, or overloaded section>

Objections and trade-offs
- <issue>: <unanswered reviewer question, hidden assumption, or missing alternative analysis>

Conciseness and paragraph quality
- <issue>: <where the prose can be cut or split>

Suggested fixes
1. <highest-leverage revision>
2. <next revision>
3. <next revision>
```

Prefer specific comments tied to exact sections or paragraphs from the doc.

## Review Priorities

- Prioritize argument quality over template compliance
- Prioritize reader confusion over stylistic preference
- Prioritize missing trade-off analysis over minor wording issues
- Flag when the author seems to know the answer but has not brought the reader there

## Full Reference

See `AGENTS.md` for the detailed rubric and review checklist.
