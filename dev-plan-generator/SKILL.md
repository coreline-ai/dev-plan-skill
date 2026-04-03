---
name: dev-plan-generator
description: Create or update lightweight phased development plan markdown documents for implementation work. Use when the user asks for a 개발 계획 문서, 구현 계획, dev plan, phased implementation plan, implement_*.md, or a checkbox-based task plan before or during coding. Before drafting, read relevant project `.md` files to clarify the goal, scope, and documented rules. Generate or maintain `dev-plan/implement_YYYYMMDD_HHMMSS.md` with a clear purpose, scope limits, phased tasks, per-phase self-tests, and progress checkboxes.
---

# Dev Plan Generator

## Overview

Create a lightweight development scope document that keeps implementation focused on the current goal.
This document is for fixing scope, tracking phased work, and preserving development history across multiple implementation efforts.

## Core intent

- Fix the goal and scope of the current work.
- Prevent implementation from expanding in a different direction.
- Track progress with phase-based checkboxes.
- Keep a history by creating a new plan file for each new workstream.

## Workflow

1. Read relevant project `.md` files before drafting.
2. Clarify the goal, scope, exclusions, and any documented rules.
3. Create a new `dev-plan/implement_YYYYMMDD_HHMMSS.md` for a new workstream.
4. Update the same file while the same workstream is in progress.
5. Create a new file again when a new refactor or new development effort starts.

## Document rules

- Put `개발 목적`, `개발 범위`, `제외 범위`, `참조 문서`, and `공통 진행 규칙` at the top.
- Keep the document lightweight and implementation-oriented.
- Do not turn the document into a PRD, TRD, meeting log, or large design spec.
- Break work into ordered phases.
- Use markdown checkboxes for all implementation and test tasks.
- Each Phase must include self-tests.
- Do not move to the next Phase until the current Phase tests are complete.
- Record implementation issues inside the current Phase and fix them there.
- Update checkbox state to match real progress.
- Do not add undocumented new features or unrelated refactors.

## History rules

- Create files under `dev-plan/`.
- Name files as `implement_YYYYMMDD_HHMMSS.md`.
- Set the first H1 to the exact filename including `.md`.
- Add `작성 일시` near the top.
- If this work follows a previous plan, add the previous plan to `참조 문서`.
- Do not overwrite old plan files.

## Required structure

Use this structure by default:

```md
# implement_YYYYMMDD_HHMMSS.md

작성 일시: `YYYY-MM-DD HH:MM:SS TZ`

이 문서는 이번 개발의 범위를 고정하고, 구현이 목적 밖으로 확장되지 않도록 하기 위한 작업 문서다.

## 개발 목적
...

## 개발 범위
...

## 제외 범위
- ...

## 참조 문서
- [문서명](경로 또는 링크)
- 없음

## 공통 진행 규칙
- 각 Phase는 앞선 Phase의 자체 테스트 완료 후에만 시작한다.
- 구현 중 발생한 이슈는 해당 Phase에서 수정하고 기록한다.
- 체크박스 상태를 실제 진행 상태와 맞게 업데이트한다.
- 문서에 없는 범위 확장은 하지 않는다.

## Phase 상태 요약
- [ ] Phase 1 완료
- [ ] Phase 2 완료

## Phase 1. <이름>
### 목표
- ...

### 구현 태스크
- [ ] ...
- [ ] ...

### 자체 테스트
- [ ] ...
- [ ] ...

### 이슈 및 수정
- [ ] 발견 이슈 없음

### 완료 조건
- [ ] 구현 완료
- [ ] 자체 테스트 완료
- [ ] 다음 Phase 진행 가능
```

## Optional sections

Add only when needed:

- `문서 기반 제약 사항`
- `예상 영향 범위`
- `최종 결과 요약`
- `잔여 리스크 / 후속 과제`
- `이전 개발 계획`

## Script

Use `scripts/new_dev_plan.py` to scaffold a new file quickly.
The script should stay minimal. It creates the document skeleton; the actual scope clarification comes from reading project docs and the user request.
