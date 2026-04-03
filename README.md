# dev-plan-skill

[![GitHub Repo](https://img.shields.io/badge/GitHub-coreline--ai%2Fdev--plan--skill-181717?logo=github)](https://github.com/coreline-ai/dev-plan-skill)
[![Codex Skill](https://img.shields.io/badge/Codex-skill-412991)](./dev-plan-generator)
[![Claude Code](https://img.shields.io/badge/Claude_Code-compatible-D97757)](./.claude)
[![skills-ref](https://img.shields.io/badge/skills--ref-validated-2ea44f)](./dev-plan-generator)

> Scope-first development plan tooling for **Codex** and **Claude Code**.
>
> 목적, 범위, 제외 범위, Phase, 테스트 게이트를 먼저 고정해서 구현이 목표 밖으로 확장되지 않게 만드는 개발 계획 스킬/프롬프트 모음입니다.

## Why this exists

LLM 기반 구현은 편리하지만, 다음 문제가 자주 발생합니다.

- 구현 중 범위가 조용히 확장됨
- 원래 목표보다 “추가로 더 좋아 보이는 것”까지 건드림
- 테스트 전에 다음 작업으로 넘어감
- 리팩토링 / 추가 개발 히스토리가 문서로 남지 않음

`dev-plan-skill`은 이 문제를 줄이기 위해 만들어졌습니다.

핵심 아이디어는 단순합니다.

- 작업 시작 전에 **개발 목적**을 고정한다.
- 이번 작업의 **개발 범위**와 **제외 범위**를 먼저 적는다.
- 작업을 **Phase 단위**로 나눈다.
- 각 Phase에 **체크박스**, **자체 테스트**, **이슈 기록**을 둔다.
- 새로운 작업이 시작될 때마다 `implement_YYYYMMDD_HHMMSS.md` 문서를 새로 만들어 **히스토리**를 쌓는다.

---

## What you get

이 저장소는 두 환경을 함께 지원합니다.

항목 | 용도
--- | ---
`dev-plan-generator/` | Codex용 skill 폴더
`.claude/commands/dev-plan.md` | Claude Code용 slash command
`.claude/agents/dev-plan-generator.md` | Claude Code용 custom subagent
`dev-plan-generator/scripts/new_dev_plan.py` | 새 개발 계획 문서 골격 생성 스크립트

---

## Core workflow

이 도구가 만드는 문서는 거대한 PRD/TRD가 아닙니다.

대신 아래 목적에 집중합니다.

1. 이번 작업의 **목표 고정**
2. 이번 작업의 **범위 제한**
3. **Phase 기반 구현 순서 정리**
4. **테스트 완료 후 다음 단계 진행** 강제
5. **작업별 히스토리 누적**

기본 흐름은 다음과 같습니다.

1. 관련 프로젝트 `.md` 문서를 먼저 읽음
2. 목적 / 범위 / 제외 범위 / 참조 문서를 정리함
3. `dev-plan/implement_YYYYMMDD_HHMMSS.md` 생성
4. 같은 작업을 진행하는 동안 같은 문서를 계속 업데이트함
5. 새로운 리팩토링/추가 개발이 시작되면 새 문서를 다시 생성함

---

## Generated document style

생성되는 문서는 기본적으로 아래 구조를 따릅니다.

```md
# implement_YYYYMMDD_HHMMSS.md

작성 일시: `YYYY-MM-DD HH:MM:SS TZ`

이 문서는 이번 개발의 범위를 고정하고, 구현이 목적 밖으로 확장되지 않도록 하기 위한 작업 문서다.

## 개발 목적
...

## 개발 범위
...

## 제외 범위
...

## 참조 문서
...

## 공통 진행 규칙
...

## Phase 상태 요약
- [ ] Phase 1 완료
- [ ] Phase 2 완료

## Phase 1. <이름>
### 목표
...

### 구현 태스크
- [ ] ...

### 자체 테스트
- [ ] ...

### 이슈 및 수정
- [ ] 발견 이슈 없음

### 완료 조건
- [ ] 구현 완료
- [ ] 자체 테스트 완료
- [ ] 다음 Phase 진행 가능
```

필요한 경우 아래 섹션도 추가할 수 있습니다.

- `문서 기반 제약 사항`
- `예상 영향 범위`
- `이전 개발 계획`
- `최종 결과 요약`
- `잔여 리스크 / 후속 과제`

---

## Repository layout

```text
dev-plan-skill/
├── README.md
├── dev-plan-generator/
│   ├── SKILL.md
│   ├── agents/
│   │   └── openai.yaml
│   └── scripts/
│       └── new_dev_plan.py
└── .claude/
    ├── commands/
    │   └── dev-plan.md
    └── agents/
        └── dev-plan-generator.md
```

---

## Use with Codex

### 1) Install the skill

Codex skill 디렉터리에 `dev-plan-generator/` 폴더를 넣습니다.

예시:

```bash
mkdir -p ~/.codex/skills
cp -R dev-plan-generator ~/.codex/skills/dev-plan-generator
```

설치 후 Codex를 재시작하면 인식이 더 안정적입니다.

### 2) When to use it

아래 요청에서 잘 맞습니다.

- “개발 계획 문서 만들어줘”
- “이번 작업 범위 고정하는 implement 문서 작성해줘”
- “체크박스 기반 phased dev plan 만들어줘”
- “리팩토링용 dev-plan 문서 하나 생성해줘”

### 3) What Codex should do

- 프로젝트 내 관련 `.md` 문서를 먼저 읽음
- 목적 / 범위 / 제외 범위를 상단에 고정
- 순차적인 Phase로 구현 태스크 생성
- 각 Phase마다 자체 테스트 추가
- 같은 작업은 같은 문서를 업데이트
- 새 작업은 새 `implement_*.md` 파일 생성

---

## Use with Claude Code

Claude Code에서는 두 방식으로 사용할 수 있습니다.

### Option A. Slash command

이 저장소에는 프로젝트용 slash command가 포함되어 있습니다.

경로:
- `.claude/commands/dev-plan.md`

사용 예시:

```text
/dev-plan 로그인 API 1차 구현 범위 문서 작성
```

이 command는 `dev-plan-generator/SKILL.md`와 `scripts/new_dev_plan.py`를 참조해 같은 워크플로우를 따르도록 구성되어 있습니다.

### Option B. Custom subagent

경로:
- `.claude/agents/dev-plan-generator.md`

이 subagent는 아래 상황에서 쓰기 좋습니다.

- 구현 전 계획 문서를 먼저 정리하고 싶을 때
- 여러 작업 중 scope 고정 역할을 분리하고 싶을 때
- 개발 계획 문서를 반복적으로 생성/업데이트할 때

### Claude Code에 프로젝트 로컬로 넣기

원하는 프로젝트 루트에 아래를 복사하면 됩니다.

- `.claude/commands/dev-plan.md`
- `.claude/agents/dev-plan-generator.md`
- `dev-plan-generator/`

---

## Script usage

`new_dev_plan.py`는 **문서를 똑똑하게 작성하는 도구**가 아니라, **문서 골격을 빠르게 생성하는 도구**입니다.

즉:
- 문서 해석 / 범위 정의 / 제약 추출 → 에이전트가 담당
- 파일 생성 / 기본 섹션 생성 → 스크립트가 담당

### Example

```bash
python3 dev-plan-generator/scripts/new_dev_plan.py \
  --root /path/to/project \
  --purpose "로그 경로 정리와 회귀 테스트 추가" \
  --scope "기존 로그 집계 리팩토링과 테스트 보강에 한정" \
  --reference "README.md" \
  --reference "docs/logging.md" \
  --exclude "신규 기능 추가" \
  --exclude "무관한 구조 변경" \
  --phase "로그 수집 경로 정리" \
  --phase "회귀 테스트 보강" \
  --phase "문서 동기화"
```

### Supported options

옵션 | 설명
--- | ---
`--root` | `dev-plan/` 폴더를 생성할 프로젝트 루트
`--purpose` | 이번 개발의 목적
`--scope` | 이번 개발 범위
`--exclude` | 제외 범위 항목, 반복 가능
`--reference` | 참조 문서 경로/링크, 반복 가능
`--phase` | Phase 이름, 반복 가능
`--previous-plan` | 이전 개발 계획 문서 경로

### Output

기본 출력 위치:

```text
dev-plan/implement_YYYYMMDD_HHMMSS.md
```

---

## Recommended usage pattern

이 도구는 아래 패턴으로 쓸 때 가장 효과적입니다.

### 새 작업 시작 시
- 새 `implement_*.md` 생성
- 목적 / 범위 / 제외 범위 먼저 고정

### 같은 작업 계속 진행 시
- 같은 문서 업데이트
- 체크박스 상태와 이슈 기록 갱신

### 추가 리팩토링 / 새 작업 시작 시
- 새 `implement_*.md` 생성
- 이전 문서를 `참조 문서`에 연결

이렇게 하면 개발 계획 문서가 자연스럽게 **작업 히스토리**가 됩니다.

---

## Design principles

이 저장소는 아래 원칙을 따릅니다.

- **Scope-first**: 구현보다 범위 고정이 먼저
- **Lightweight**: 거대한 설계 문서가 아니라 작업 문서
- **Phased**: 큰 작업을 순차 Phase로 분해
- **Test-gated**: 테스트 완료 전 다음 Phase 금지
- **History-friendly**: 작업 단위별 문서 누적
- **Tool-agnostic workflow**: Codex와 Claude Code 모두 같은 사고방식 사용

---

## Compatibility notes

### Codex
- `dev-plan-generator/`는 Codex skill 구조를 따릅니다.
- `SKILL.md` + `agents/openai.yaml` + `scripts/` 구성을 포함합니다.

### Claude Code
- `.claude/commands/` 기반 custom slash command 포함
- `.claude/agents/` 기반 custom subagent 포함
- 둘 다 같은 workflow를 공유하도록 작성됨

---

## Validation

현재 저장소의 Codex skill은 `skills-ref validate` 기준으로 검증할 수 있습니다.

예시:

```bash
skills-ref validate ./dev-plan-generator
```

검증 포인트:
- skill 이름과 폴더명 일치
- `SKILL.md` frontmatter 정합성
- 스크립트 포함 구조 유지
- Claude Code용 command / agent 파일 존재
- 문서 구조와 참조 경로 일관성

---

## Example use cases

- 기능 구현 전에 범위 고정 문서 만들기
- 리팩토링 시작 전에 목표와 금지선 고정하기
- 장기 작업을 Phase별로 나눠 체크박스로 추적하기
- 테스트 완료 전 다음 단계로 넘어가지 않도록 문서 게이트 만들기
- 추가 개발이 생길 때마다 새 문서를 생성해 히스토리 남기기

---

## References

- Anthropic Claude Code tutorials: https://docs.anthropic.com/en/docs/claude-code/tutorials
- Anthropic Claude Code slash commands: https://docs.anthropic.com/en/docs/claude-code/slash-commands
- Anthropic Claude Code subagents: https://docs.anthropic.com/en/docs/claude-code/sub-agents

---

## Summary

`dev-plan-skill`은

- **Codex에서는 skill로**
- **Claude Code에서는 command / subagent로**

같은 개발 계획 워크플로우를 재사용할 수 있게 해줍니다.

핵심 목적은 하나입니다.

> 구현을 더 많이 하게 만드는 것이 아니라,
> **이번 작업이 어디까지인지 먼저 고정하고 일관되게 끝까지 가게 만드는 것**.
