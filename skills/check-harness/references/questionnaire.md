# check-harness questionnaire

This is the source of truth for `/check-harness`.

Goal: help a workshop participant see how they currently use AI agents, compare
that self-assessment with lightweight project/session evidence, and identify a
short list of concrete improvements to discuss or implement during the
workshop.

## Rules

- Ask Q1 as a free-form question with examples only.
- Ask Q2-Q8 one by one with `AskUserQuestion`.
- Prefer Korean unless the user is using English.
- If the user already answered a question in the prompt, record the answer and
  skip asking that question.
- Do not use subagents.
- For session analysis, inspect user message text when available. Do not read
  assistant prose, tool outputs, or full file contents from session logs.
- Summarize user-message patterns; do not quote long raw prompts in the report.
- Do not modify the target project except writing the final report under
  `.harness/check-reports/`.
- Keep the final diagnosis practical, not judgmental.

## 8 Questions

### Q1. Problem

Question: `현재 에이전트를 사용하면서 가장 크게 느끼는 문제의식이 있다면?`

Ask this as free text, not a multiple-choice question. Show examples only:

- `예: 결과 품질이 들쭉날쭉하다.`
- `예: 오래 맡기기 어렵고 계속 지켜봐야 한다.`
- `예: 매번 어떻게 시킬지 새로 고민한다.`
- `예: 검증을 내가 다 해야 해서 시간이 줄지 않는다.`

Record the user's answer as-is and summarize the signal in the report.

### Q2. Starting Work

Question: `처음에 AI에게 일을 어떻게 시키고 계신가요?`

Use options:

- `대충 해줘` — 일단 시켜보고 결과를 보며 고친다.
- `명확하게 지시` — 배경, 목적, 제약을 어느 정도 준다.
- `체계적 프로세스` — 인터뷰나 별도 스킬로 시작한다.

### Q3. Skill Usage

Question: `자주 쓰는 스킬이 몇 개 정도 있으신가요?`

Use options:

- `없음` — 따로 쓰는 스킬이 없다.
- `1-2개` — 목적에 맞게 소수만 쓴다.
- `여러 개` — 작업별로 여러 스킬을 구분해 쓴다.

### Q4. Harness Maintenance

Question: `CLAUDE.md나 만들어둔 SKILL을 얼마나 자주 변경하고 계신가요?`

Use options:

- `거의 안 함` — 만든 뒤 거의 손대지 않는다.
- `문제 생기면 수정` — 원하는 워크플로우나 결과가 안 나오면 수정한다.
- `정기적으로 정리` — 세션/프로젝트 후 규칙과 스킬을 갱신한다.

### Q5. Long-running Work

Question: `AI 작업을 얼마나 길게 맡길 수 있으신가요?`

Use options:

- `1-5분` — 짧게 시키고 바로 확인한다.
- `30분 이상` — 오래 맡길 수 있는 작업이 있다.
- `작업별로 다름` — 위험도나 작업 종류에 따라 다르다.

### Q6. Context Management

Question: `작업하실 때 컨텍스트 관리를 어떻게 하시나요?`

Use options:

- `한 세션에 계속` — 대부분 하나의 세션에서 이어간다.
- `작업별 분리` — 작업 단위로 clear/compact/새 세션을 구분한다.
- `handoff 사용` — 인수인계 문서로 세션을 넘긴다.

### Q7. Verification

Question: `AI가 만든 결과물을 어떻게 검증하고 계신가요?`

Use options:

- `직접 확인` — 내가 직접 읽고/클릭해본다.
- `AI에게 위임 시도` — 리뷰, 테스트, QA를 AI에게 맡겨보려 한다.
- `자동 검증 루틴` — 테스트, 빌드, 브라우저 QA 같은 루틴이 있다.

### Q8. Authority

Question: `AI에게 어디까지 권한을 주고 계신가요?`

Use options:

- `읽기/작성 정도` — 파일 읽기와 수정 중심이다.
- `터미널/외부 도구` — 터미널 실행, 브라우저, 외부 도구까지 사용한다.
- `민감 작업까지` — 배포, DB, 결제/고객 데이터 등도 맡긴 경험이 있다.

## Lightweight Project Inspection

Find the project root by walking upward from cwd until one of these exists:

- `CLAUDE.md`
- `.claude/`
- `.git/`

Inspect only likely harness surfaces:

- `CLAUDE.md`
- `.claude/settings.json`
- `.claude/rules/`
- `.claude/skills/`
- `skills/`
- `hooks/`
- `.gitignore`
- `README.md`
- `package.json`, `pyproject.toml`, `Makefile`, `justfile`, `Taskfile.yml`

Collect simple evidence:

- Does the project tell AI what it is?
- Are repeated instructions captured in files?
- Are there skills or commands?
- Are there hooks or guardrails?
- Are there tests/build/check commands?
- Are sensitive files ignored?
- Is there a handoff, report, or lesson-learned place?

## User-Input Session Pattern Analysis

If Claude Code session logs are available, focus on the user's own messages.
The goal is to understand how the participant actually asks agents to work.
Read only user message text plus minimal metadata such as timestamps and tool
names. Do not read assistant prose, tool outputs, or full file contents stored
inside session logs.

Useful signals:

- recent session count for this project
- common request types and repeated workflows
- whether initial asks are vague, clear, or process-driven
- whether the user gives background, constraints, acceptance criteria, or examples
- whether the user asks the agent to clarify first
- whether the user asks for verification, QA, commit, push, deploy, or browser checks
- whether the user delegates longer work or keeps asking for small step-by-step changes
- repeated user requests that look like candidates for a skill, command, hook, or doc rule
- optional supporting metadata: common tool names, session count, and whether sessions are mostly short or long

Prefer quick shell commands over large parsing. If session paths are hard to
map, say `session evidence unavailable` and continue.

## Interpretation

Use these 6 plain-language axes:

| Axis | Meaning |
|---|---|
| 구조 | AI가 일할 자리가 정리되어 있는가 |
| 맥락 | 반복 설명이 문서/규칙으로 남아 있는가 |
| 계획 | 바로 시키기 전에 모호성을 줄이는가 |
| 실행 | 긴 작업, 위임, 도구 사용을 다룰 수 있는가 |
| 검증 | 결과를 확인하는 루틴이 있는가 |
| 개선 | 배운 점이 다음 작업에 남는가 |

Use statuses:

- `좋음`
- `부분적`
- `비어있음`
- `증거 부족`

## Report Template

Write:

```text
.harness/check-reports/check-harness-{YYYY-MM-DD}/report.md
```

Template:

```markdown
# Check Harness Report

- Project: {project name}
- Project root: `{project root}`
- Created: {date}

## 1. Self Check

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

## 2. Project Evidence

- Project guide:
- Rules/context:
- Skills/commands:
- Hooks/guardrails:
- Tests/checks:
- Sensitive-file boundary:
- Handoff/improvement traces:

## 3. Session Pattern Evidence

- Recent sessions:
- User request patterns:
- Initial instruction clarity:
- Context/constraint signal:
- Clarification signal:
- Verification/delegation signal:
- Automation candidates:

## 4. 6-Axis Snapshot

| Axis | Status | Why |
|---|---|---|
| 구조 | ... | ... |
| 맥락 | ... | ... |
| 계획 | ... | ... |
| 실행 | ... | ... |
| 검증 | ... | ... |
| 개선 | ... | ... |

## 5. Biggest Bottleneck

{One short paragraph.}

## 6. Recommended Improvements

List 3-5 improvements. Keep them concrete enough to start during the workshop.

| Priority | Improvement | Axis | Why | First action | Done when |
|---|---|---|---|---|---|
| 1 | ... | ... | ... | ... | ... |
| 2 | ... | ... | ... | ... | ... |
| 3 | ... | ... | ... | ... | ... |

## 7. Pair Discussion

1. 상대가 보기엔 가장 먼저 고칠 축이 무엇인가?
2. 내가 과소평가한 위험이나 병목은 무엇인가?
3. 이 개선이 다음 AI 작업에서 어떻게 체감될까?
```

In chat, show only TL;DR, report path, and the top recommended improvements.
