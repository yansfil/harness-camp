# harness-camp

Harness Engineering 워크숍을 위한 Claude Code 스킬/에이전트 묶음.

현재는 `check-harness` 스킬을 중심으로, 사용자의 Claude Code 하네스를 6가지 축으로 점검하고 리포트를 생성한다.

## Included

| Path | Purpose |
|---|---|
| `skills/check-harness/` | 하네스 성숙도 진단 스킬 |
| `agents/skill-portfolio-analyzer.md` | 설치된 스킬/플러그인/MCP 포트폴리오 분석 |
| `agents/session-pattern-analyzer.md` | Claude Code 세션 사용 패턴 분석 |
| `agents/context-quality-reviewer.md` | 프로젝트 `CLAUDE.md`와 rules 품질 점검 |
| `agents/project-automation-auditor.md` | 테스트, 훅, 자동화, 검증 루틴 점검 |

## check-harness

`/check-harness`는 하네스를 다음 6축으로 평가한다.

1. 구조
2. 맥락
3. 계획
4. 실행
5. 검증
6. 개선

결과는 대화창에 요약되고, 프로젝트 안의 `.harness/check-reports/` 아래에 Markdown/HTML 리포트로 저장된다.

## Usage

Claude Code에서 이 repo를 플러그인으로 설치한 뒤, 점검하고 싶은 프로젝트에서 실행한다.

```text
/check-harness
```

스코프를 명시할 수도 있다.

```text
/check-harness all
/check-harness project
/check-harness overall
```

## Workshop Flow

1. 참가자가 본인 프로젝트에서 `/check-harness`를 실행한다.
2. 리포트의 가장 약한 축과 Quick Wins를 확인한다.
3. 2인 페어로 서로의 리포트를 리뷰한다.
4. 오늘 고칠 개선 과제 1개를 고른다.
5. 개선 후 Before/After를 공유한다.

## Development

- 스킬은 `skills/{name}/SKILL.md`에 둔다.
- 서브에이전트는 `agents/*.md`에 둔다.
- `check-harness`는 `skills/check-harness/references/checklist.md`와 `html-template.html`을 참조한다.
- 스킬은 프로젝트 파일을 점검하지만, 사용자의 프로젝트를 직접 수정하지 않는 진단 도구로 유지한다.
