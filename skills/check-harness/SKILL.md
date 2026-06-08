---
name: check-harness
description: |
  Ask one free-form problem question plus 7 structured workshop intake
  questions, inspect the current Claude Code project and user-input session
  patterns, then write a simple harness report.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - AskUserQuestion
---

# /check-harness

Read `references/questionnaire.md` and follow it: ask Q1 as free text with examples, ask Q2-Q8 with AskUserQuestion, inspect project surfaces and user-input session patterns, write one markdown report; no subagents, no HTML, no project mutation.
