---
name: harness-new
description: |
  Workshop-friendly Harness intake and project audit for Claude Code.
  Use when the user wants a first-pass harness diagnosis, asks "/harness-new",
  "harness new", "하네스 신규 점검", "워크숍 하네스 점검",
  "내 프로젝트 하네스 봐줘", or wants to prepare a project for a Harness
  Engineering workshop.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Write
  - AskUserQuestion
validate_prompt: |
  Must ask the 8 intake questions one by one with AskUserQuestion, inspect the
  current project when available, write one markdown report under
  .harness/harness-new-reports/, and recommend exactly one workshop task.
---

# /harness-new — Workshop Harness Intake

`harness-new` is the lightweight entry point before a Harness Engineering
workshop. It helps a participant describe how they currently use agents, scans
the current project, then produces a short report they can use for pair review
and hands-on improvement.

In this repo, `/check-harness` is the main workshop starter. Use
`/harness-new` only when you specifically want this copied intake/report
variant from the original `harness-session` workflow.

## Output

Write one markdown report:

```text
.harness/harness-new-reports/harness-new-{YYYY-MM-DD}-{project-slug}/report.md
```

Also render a compact chat summary:

```markdown
## Harness New TL;DR
- Project: ...
- Biggest bottleneck: ...
- Today’s workshop task: ...
- Report: `.harness/harness-new-reports/.../report.md`
```

Do not create HTML, open files, install hooks, or edit project code/config. The
only file write is this skill's markdown report.

## Phase 1 — Intake Questions

Ask the following 8 questions **one by one** with `AskUserQuestion`. If the user
already answered one in the prompt, record it and skip asking that exact
question. Keep answers concise in memory for the report.

1. **Problem**
   - question: `현재 에이전트를 사용하면서 가장 크게 느끼는 문제의식이 있다면?`
   - options:
     - `결과 품질` — 원하는 결과가 안정적으로 안 나온다
     - `시간/통제` — 계속 지켜봐야 해서 오래 맡기기 어렵다
     - `워크플로우 부재` — 매번 어떻게 시킬지 새로 고민한다

2. **Starting Work**
   - question: `처음에 AI에게 일을 어떻게 시키고 계신가요?`
   - options:
     - `명확히 지시` — 배경, 목적, 제약을 어느 정도 준다
     - `대충 해줘` — 일단 시켜보고 결과를 보며 고친다
     - `체계적 프로세스` — 인터뷰나 별도 스킬로 시작한다

3. **Skill Usage**
   - question: `자주 쓰는 스킬은 몇 개 정도 있으신가요?`
   - options:
     - `1-2개` — 목적에 맞는 소수만 쓴다
     - `없음` — 따로 쓰는 스킬이 없다
     - `여러 개` — 작업별로 여러 스킬을 구분해 쓴다

4. **Harness Maintenance**
   - question: `CLAUDE.md나 만들어둔 SKILL을 얼마나 자주 변경하고 계신가요?`
   - options:
     - `문제 생기면 수정` — 결과가 안 나오면 적극적으로 고친다
     - `거의 안 함` — 만든 뒤 거의 손대지 않는다
     - `정기적으로 정리` — 세션/프로젝트 후 규칙과 스킬을 갱신한다

5. **Long-running Work**
   - question: `AI 작업을 얼마나 길게 맡길 수 있으신가요?`
   - options:
     - `1-5분` — 짧게 시키고 바로 확인한다
     - `30분 이상` — 오래 맡길 수 있는 작업이 있다
     - `작업별로 다름` — 위험도에 따라 길이가 크게 달라진다

6. **Context Management**
   - question: `작업하실 때 컨텍스트 관리를 어떻게 하시나요?`
   - options:
     - `작업별 분리` — clear/compact/새 세션을 구분해 쓴다
     - `한 세션에 계속` — 대부분 하나의 세션에서 이어간다
     - `handoff 사용` — 인수인계 문서로 세션을 넘긴다

7. **Verification**
   - question: `AI가 만든 결과물을 어떻게 검증하고 계신가요?`
   - options:
     - `직접 확인` — 내가 읽고 클릭해본다
     - `AI에 위임 시도` — 리뷰/테스트/QA를 AI에게 맡긴다
     - `자동 검증` — 테스트, 빌드, 브라우저 QA 같은 루틴이 있다

8. **Authority**
   - question: `AI에게 어디까지 권한을 주고 계신가요?`
   - options:
     - `터미널/도구까지` — 실행, 브라우저, 외부 도구를 쓴다
     - `읽기/작성 정도` — 파일 읽기와 수정 중심이다
     - `민감 작업까지` — 배포, DB, 결제/고객 데이터도 맡긴 경험이 있다

## Phase 2 — Project Scan

Find `PROJECT_ROOT` by walking up from cwd until `.claude` or `CLAUDE.md`
exists. If none exists, set project scan status to `Unknown` and continue with
intake-only analysis.

When a project root exists, inspect only these surfaces:

- `CLAUDE.md`
- `.claude/settings.json`
- `.claude/rules/`
- `.claude/skills/`
- `skills/`
- `hooks/`
- `.gitignore`
- common build/test manifests: `package.json`, `pyproject.toml`, `Makefile`,
  `justfile`, `Taskfile.yml`

Do not read secrets or `.env` files. Do not inspect full session transcripts.

Map evidence into the 6 axes:

| Axis | Look For |
|---|---|
| Structure | project guide, skill/hook layout, clear folders |
| Context | concise rules, domain docs, progressive disclosure |
| Planning | interview/spec/plan workflow or instructions |
| Execution | delegation, subagents, commands, handoff structure |
| Verification | tests, build checks, QA/review routines, guardrails |
| Improvement | rules updated from mistakes, docs drift, cleanup loops |

Use simple statuses only:

- `Strong` — evidence is explicit and usable
- `Partial` — some evidence exists but it is thin or inconsistent
- `Missing` — expected harness support is absent
- `Unknown` — not enough evidence or no project root

## Phase 3 — Recommend One Workshop Task

Pick exactly one task the participant should attempt during the workshop. Choose
the smallest task that will improve the weakest important axis.

Allowed task types:

- `CLAUDE.md 정리`
- `검증 체크리스트 추가`
- `간단한 Skill 초안`
- `Hook 후보 설계`
- `handoff/output 폴더 구조 만들기`
- `권한/보안 경계 정리`

If several are tied, prefer in this order:

1. Verification task when output quality or safety is weak.
2. Context task when the participant repeats the same instructions.
3. Skill task when the participant repeats the same workflow.
4. Hook task only when an existing manual habit is already clear.

## Report Template

```markdown
# Harness New Report

- Project: {project_name}
- Project root: `{project_root or not found}`
- Created: {YYYY-MM-DD}
- Audience: workshop participant

## Intake Summary

| Question | Answer | Signal |
|---|---|---|
| Problem | ... | ... |
| Starting work | ... | ... |
| Skill usage | ... | ... |
| Maintenance | ... | ... |
| Long-running work | ... | ... |
| Context management | ... | ... |
| Verification | ... | ... |
| Authority | ... | ... |

## 6-Axis Snapshot

| Axis | Status | Evidence | What it means |
|---|---|---|---|
| Structure | Strong/Partial/Missing/Unknown | ... | ... |
| Context | Strong/Partial/Missing/Unknown | ... | ... |
| Planning | Strong/Partial/Missing/Unknown | ... | ... |
| Execution | Strong/Partial/Missing/Unknown | ... | ... |
| Verification | Strong/Partial/Missing/Unknown | ... | ... |
| Improvement | Strong/Partial/Missing/Unknown | ... | ... |

## Biggest Bottleneck

{One paragraph. Explain the gap in plain Korean.}

## Workshop Task Recommendation

- Task: {exactly one allowed task type}
- Why this one: ...
- Done when: ...
- Expected effect: ...

## Pair Discussion Prompts

1. 이 리포트에서 내가 과소평가한 축은 무엇인가?
2. 상대가 보기엔 오늘 가장 먼저 고칠 지점이 무엇인가?
3. 이 개선이 다음 AI 작업에서 어떻게 체감될까?

## Evidence

- Intake answers: ...
- Files inspected: ...
- Missing surfaces: ...
```

## Tone Rules

- Write in Korean by default.
- Avoid expert-only jargon in the report body.
- Explain `Hook`, `Skill`, `MCP`, and `handoff` only when recommending them.
- Do not shame missing setup. Phrase gaps as practical next steps.
