# harness-camp

Claude Code용 Harness Engineering 워크숍 스킬 repo.

## Project Structure

```text
.claude-plugin/plugin.json  # Claude Code plugin manifest
skills/
  check-harness/            # 워크숍 starter. 8개 질문 + 프로젝트/세션 패턴 점검
  harness-new/              # harness-session에서 가져온 intake/project audit 변형
  scaffold/                 # 새 프로젝트 구조 + Harness scaffold
  specify/                  # spec.md 생성
  doc-drift/                # context/docs drift 점검
  qa/                       # QA/test/fix workflow
  agent-orchestrate/        # agent orchestration pattern 선택
  deep-interview/           # 요구사항 인터뷰
```

## Development Guidelines

- `skills/check-harness/SKILL.md`가 메인 실행 계약이다.
- `skills/check-harness/references/questionnaire.md`는 질문지와 리포트 형식의 원본이다.
- `check-harness`는 의도적으로 단순한 워크숍 진입점이다. 외부 repo에서 복사한 더 무거운 버전으로 덮어쓰지 않는다.
- `harness-session`에서 가져온 스킬들은 참고용 성격이 강하다. 워크숍 first-flow에 넣기 전에는 별도로 단순화한다.
- 서브에이전트 의존성을 추가하지 않는다. 워크숍 진입점은 단순해야 한다.
- 참가자 프로젝트를 수정하는 기능을 추가하지 않는다. 이 repo의 기본 역할은 진단과 리포트 생성이다.
- 리포트 출력은 `.harness/check-reports/` 아래에 둔다.
- 스킬 변경 후에는 YAML/JSON 파싱과 `git diff --check`를 실행한다.
