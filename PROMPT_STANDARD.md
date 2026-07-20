# PROMPT_STANDARD.md

# VSkript AI Engineering Kit
## Prompt Standard

Version: 2.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 사용하는 모든 Prompt의 표준 구조를 정의한다.

Prompt는 AI가 일관된 품질과 절차를 유지하며 작업하도록 하는 공식 인터페이스이다.

모든 Prompt는 본 문서를 기준으로 작성한다.

---

# 2. Goals

Prompt Standard의 목적은 다음과 같다.

- AI 모델에 관계없이 동일한 품질 확보
- 프로젝트 전체를 고려한 작업 수행
- Evidence 기반 분석
- 재현 가능한 결과 생성
- Prompt 재사용성 확보

---

# 3. Prompt Philosophy

모든 Prompt는 다음 원칙을 따른다.

- Specification First
- Evidence First
- No Guessing
- Project First
- Consistency
- Reproducibility

---

# 4. Prompt Categories

Prompt는 다음과 같이 분류한다.

## Analysis Prompt

프로젝트를 분석하기 위한 Prompt

예)

- Project Analysis
- Architecture Review
- Dependency Analysis
- Performance Analysis

---

## Development Prompt

기능 구현을 위한 Prompt

예)

- Feature Development
- Refactoring
- DataPack Builder
- Registry Generator

---

## Verification Prompt

결과를 검증하기 위한 Prompt

예)

- Static Verification
- Runtime Verification
- Regression Test
- Security Review

---

## Documentation Prompt

문서를 생성하기 위한 Prompt

예)

- README
- Specification
- Report
- API Documentation

---

## Maintenance Prompt

유지보수를 위한 Prompt

예)

- Changelog
- Migration
- Version Upgrade

---

# 5. Standard Prompt Structure

모든 Prompt는 아래 순서를 따른다.

```text
Title

↓

Purpose

↓

Role

↓

Context

↓

Scope

↓

Inputs

↓

Constraints

↓

Tasks

↓

Expected Output

↓

Quality Criteria
```

---

# 6. Required Sections

모든 Prompt에는 반드시 다음 항목이 존재해야 한다.

| Section | Required |
|----------|----------|
| Purpose | YES |
| Role | YES |
| Context | YES |
| Scope | YES |
| Inputs | YES |
| Constraints | YES |
| Tasks | YES |
| Expected Output | YES |
| Quality Criteria | YES |

---

# 7. Prompt Inputs

Prompt는 필요한 입력을 명시해야 한다.

예)

- Repository
- Source Code
- Specification
- Requirements
- Previous Report
- Runtime Logs

필요한 입력이 없으면 AI는 먼저 요청해야 한다.

---

# 8. Prompt Constraints

Prompt는 반드시 제약사항을 정의한다.

예)

- 추측 금지
- 실제 코드만 사용
- Specification 준수
- Runtime 결과와 Static 결과 구분
- Evidence 제시

---

# 9. Prompt Role

Prompt는 AI의 역할을 명확히 정의한다.

예)

Software Architect

TypeScript Expert

VSCode Extension Developer

Technical Writer

Code Auditor

Performance Engineer

Prompt Engineer

---

# 10. Expected Outputs

Prompt는 산출물을 명확히 정의한다.

예)

Markdown Report

Checklist

Specification

Source Code

Review Report

JSON

DataPack

---

# 11. Quality Criteria

Prompt는 최소 품질 기준을 포함한다.

□ Evidence 존재

□ 추측 없음

□ 구조적 일관성

□ Specification 준수

□ 프로젝트 전체 고려

□ 재현 가능

---

# 12. Prompt Lifecycle

Prompt는 다음 생명주기를 가진다.

```text
Draft

↓

Review

↓

Approved

↓

Published

↓

Deprecated
```

---

# 13. Prompt Naming

Prompt 파일명은 다음 규칙을 따른다.

```
analysis_project.md

analysis_architecture.md

development_feature.md

verification_runtime.md

documentation_report.md
```

형식

```
category_name.md
```

---

# 14. Prompt Directory

Prompt는 다음 구조를 따른다.

```text
prompts/

analysis/

development/

verification/

documentation/

maintenance/

shared/
```

---

# 15. Prompt Composition

모든 Prompt는 재사용 가능한 모듈로 구성한다.

```text
Prompt

├── Purpose
├── Role
├── Context
├── Inputs
├── Constraints
├── Tasks
├── Outputs
└── Quality
```

---

# 16. Prompt Dependencies

Prompt는 다음 문서를 참조할 수 있다.

RULES.md

PROJECT.md

WORKFLOW.md

REQUIREMENTS.md

STYLE_GUIDE.md

QUALITY_GATE.md

REPORT_STANDARD.md

---

# 17. AI Independence

Prompt는 특정 AI 모델에 의존하지 않는다.

지원 대상 예시

- ChatGPT
- Claude
- Gemini
- Codex
- Cursor
- Windsurf
- GitHub Copilot

---

# 18. Prompt Review

Prompt 변경 시 다음 항목을 검토한다.

- 목적
- 입력
- 출력
- Workflow
- 품질 기준

---

# 19. Definition of Done

Prompt는 다음 조건을 만족해야 완료된다.

- 표준 구조 준수
- Quality Criteria 포함
- 프로젝트 문서와 일관성 유지
- 재사용 가능
- 모델 독립성 확보

---

# 20. Summary

Prompt Standard는 VSkript AI Engineering Kit에서 사용하는 모든 Prompt의 공식 작성 기준이다.

모든 Prompt는 동일한 구조와 품질 기준을 따르며, 프로젝트 전체를 고려한 Evidence 기반 작업을 수행할 수 있도록 설계한다.