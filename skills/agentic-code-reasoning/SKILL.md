---
name: agentic-code-reasoning
description: Semi-formal structured reasoning for code analysis tasks — patch equivalence, fault localization, and code question answering. Based on "Agentic Code Reasoning" (Ugare & Chandra, 2026). Use when asked to analyze code semantics, compare patches, find bugs, or answer questions about program behavior without executing the code.
license: MIT
metadata:
  author: anthropic
  version: "1.0.0"
  source: https://arxiv.org/abs/2603.01896
---

# Agentic Code Reasoning

A structured, semi-formal reasoning methodology for deep code analysis without execution. Based on the paper "Agentic Code Reasoning" by Shubham Ugare and Satish Chandra (arXiv:2603.01896).

## Core Insight

Standard LLM code reasoning fails because agents skip edge cases and make unsupported logical leaps. Semi-formal reasoning forces the agent to:
1. State explicit **premises** from gathered evidence
2. **Trace execution paths** interprocedurally (following function calls rather than guessing)
3. Document every **claim** with a specific `file:line` citation
4. Derive **formal conclusions** only from documented premises

The structured template makes it impossible to skip cases — every field must be filled with real evidence before a conclusion can be reached.

## When to Apply

- Comparing two patches or code versions for behavioral equivalence
- Localizing bugs from failing test descriptions
- Answering questions about program behavior, data flow, or invariants
- Code review requiring semantic (not just syntactic) analysis
- Any task requiring reasoning about code without running it

## Task Templates

See individual rule files for complete templates:
- `rules/patch-equivalence.md` — Compare two patches for test-observable equivalence
- `rules/fault-localization.md` — Rank likely bug locations from a failing test
- `rules/code-qa.md` — Answer semantic questions about a codebase

## Full Reference

See `AGENTS.md` for the complete methodology guide.
