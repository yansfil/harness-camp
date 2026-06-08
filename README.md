# harness-camp

Harness Engineering 워크숍을 위한 Claude Code 스킬 묶음.

현재는 `check-harness` 하나로 시작한다. 8개 질문으로 사용자의 현재 AI 활용 방식을 먼저 묻고, 현재 프로젝트와 세션 패턴을 가볍게 확인한 뒤 워크숍용 리포트를 만든다.

## Included

| Path | Purpose |
|---|---|
| `skills/check-harness/` | 8개 질문 + 프로젝트/세션 패턴 점검 스킬 |
| `skills/check-harness/references/questionnaire.md` | 질문지와 리포트 형식 |

## check-harness

`/check-harness`는 먼저 아래 8개 질문을 한 번에 하나씩 묻는다.

1. 현재 가장 크게 느끼는 문제의식
2. AI에게 처음 일을 시키는 방식
3. 자주 쓰는 스킬 수
4. `CLAUDE.md`나 Skill을 고치는 빈도
5. AI에게 맡길 수 있는 작업 길이
6. 컨텍스트 관리 방식
7. 결과물 검증 방식
8. AI에게 주는 권한 범위

그다음 현재 프로젝트와 Claude Code 세션 메타데이터를 가볍게 확인해서 다음 6축으로 정리한다.

1. 구조
2. 맥락
3. 계획
4. 실행
5. 검증
6. 개선

결과는 대화창에 짧게 요약되고, 프로젝트 안의 `.harness/check-reports/` 아래에 Markdown 리포트로 저장된다.

## Usage

Claude Code에서 이 repo를 플러그인으로 설치한 뒤, 점검하고 싶은 프로젝트에서 실행한다.

```text
/check-harness
```

## Workshop Flow

1. 참가자가 본인 프로젝트에서 `/check-harness`를 실행한다.
2. 리포트의 가장 약한 축과 추천 워크숍 과제를 확인한다.
3. 2인 페어로 서로의 리포트를 리뷰한다.
4. 오늘 고칠 개선 과제 1개를 고른다.
5. 개선 후 Before/After를 공유한다.

## Development

- 스킬은 `skills/{name}/SKILL.md`에 둔다.
- `check-harness`는 `skills/check-harness/references/questionnaire.md`를 참조한다.
- 스킬은 프로젝트 파일을 점검하지만, 사용자의 프로젝트를 직접 수정하지 않는 진단 도구로 유지한다.
