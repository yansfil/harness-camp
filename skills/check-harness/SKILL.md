---
name: check-harness
description: |
  Ask 8 workshop intake questions, inspect the current Claude Code project and
  session/tool-use patterns, then write a simple harness report.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - AskUserQuestion
---

# /check-harness

Read `references/questionnaire.md` and follow it: ask 8 questions, inspect project/session metadata, write one markdown report; no subagents, no HTML, no project mutation.
