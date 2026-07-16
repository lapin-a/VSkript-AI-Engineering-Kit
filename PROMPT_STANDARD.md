# PROMPT_STANDARD.md

# VSkript AI Engineering Kit

> Standard Prompt Specification

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 사용하는 모든 Prompt의 표준 구조를 정의한다.

모든 Prompt는 본 문서를 기반으로 작성되어야 하며, 프로젝트 전체에서 동일한 품질과 일관성을 유지하는 것을 목표로 한다.

---

# 2. Objectives

Prompt는 단순한 질문이 아니라 AI가 수행해야 하는 작업 명세서(Task Specification)이다.

모든 Prompt는 다음 목표를 만족해야 한다.

- 프로젝트를 올바르게 이해한다.
- 필요한 파일을 스스로 탐색한다.
- 추측하지 않는다.
- 근거를 제시한다.
- 일관된 결과를 생성한다.

---

# 3. Prompt Architecture

모든 Prompt는 아래 구조를 따른다.

```text
Purpose

Scope

Role

Required Skills

Context

Inputs

Expected Outputs

Constraints

Rules

Workflow

Evidence Policy

Verification Policy

Quality Checklist

Completion Criteria
```

---

# 4. Purpose

Prompt의 목적을 설명한다.

예시

```text
Analyze the project architecture and identify potential scalability issues.
```

---

# 5. Scope

Prompt의 적용 범위를 정의한다.

예시

```text
Entire Repository

Current Module

Database Layer

Provider Layer

Build System
```

---

# 6. Role

AI의 역할을 정의한다.

예시

```text
You are a senior software engineer specializing in

TypeScript

VSCode Extension

Language Server Protocol

Software Architecture

Static Analysis

Code Review
```

---

# 7. Required Skills

Prompt 수행에 필요한 전문 분야를 명시한다.

예시

- TypeScript
- Node.js
- VSCode API
- JSON
- Build System
- LSP
- Performance
- Database

---

# 8. Context

현재 작업의 배경을 설명한다.

예시

```text
The project has recently introduced a modular DataPack system.

Analyze how this affects the existing architecture.
```

---

# 9. Inputs

AI가 사용할 입력을 정의한다.

예시

```text
Repository

Before ZIP

After ZIP

README

Architecture Documents

Requirements
```

---

# 10. Expected Outputs

Prompt가 생성해야 하는 결과를 정의한다.

예시

```text
Architecture Report

Dependency Graph

Verification Report

Performance Analysis
```

---

# 11. Constraints

AI가 반드시 지켜야 하는 제한사항이다.

예시

- 추측 금지
- 필요한 파일 직접 탐색
- 프로젝트 전체를 먼저 분석
- Runtime Test와 정적 검증 구분
- 변경 이유 설명

---

# 12. Mandatory Rules

모든 Prompt는 아래 규칙을 포함해야 한다.

## Rule 1

프로젝트 전체를 먼저 이해한다.

---

## Rule 2

필요한 파일은 AI가 직접 탐색한다.

---

## Rule 3

모든 결론에는 근거를 제시한다.

---

## Rule 4

추측하지 않는다.

---

## Rule 5

프로젝트 구조를 유지한다.

---

## Rule 6

기존 기능을 최대한 유지한다.

---

# 13. Workflow

Prompt 내부 작업 절차를 정의한다.

예시

```text
Analyze

↓

Collect Evidence

↓

Evaluate

↓

Verify

↓

Generate Report
```

---

# 14. Evidence Policy

모든 결과는 반드시 근거를 포함해야 한다.

허용되는 근거

- Source Code
- Configuration
- Build Files
- Documentation
- Runtime Result

허용되지 않는 근거

- 추측
- 일반적인 경험
- 프로젝트 외부 정보

---

# 15. Verification Policy

모든 검증은 다음 상태를 사용한다.

| Status | Meaning |
|----------|----------|
| VERIFIED | 코드로 확인됨 |
| PARTIALLY VERIFIED | 일부 확인 |
| NEEDS RUNTIME TEST | 실행 필요 |
| ISSUE | 문제 발견 |
| NOT APPLICABLE | 해당 없음 |

---

# 16. Quality Checklist

Prompt 실행 후 반드시 확인한다.

□ 프로젝트 전체를 분석했는가

□ 필요한 파일을 탐색했는가

□ 근거를 제시했는가

□ 추측하지 않았는가

□ Runtime Test를 분리했는가

□ 영향 범위를 분석했는가

□ 보고서를 작성했는가

---

# 17. Common Mistakes

금지

- 일부 파일만 분석

- 추측으로 결론

- Runtime Test 생략

- 코드 근거 없는 판단

- 구조를 이해하지 않은 구현

---

# 18. Prompt Template

모든 Prompt는 아래 템플릿을 기반으로 작성한다.

```markdown
# Prompt Name

## Purpose
## Scope
## Role
## Required Skills
## Context
## Inputs
## Expected Outputs
## Constraints
## Rules
## Workflow
## Evidence Policy
## Verification Policy
## Quality Checklist
## Completion Criteria
```

---

# 19. Prompt Categories

Prompt는 다음 카테고리 중 하나에 속한다.

| Category | Description |
|-----------|-------------|
| Analysis | 프로젝트 분석 |
| Development | 기능 구현 |
| Verification | 검증 |
| Audit | 코드 감사 |
| Documentation | 문서 생성 |
| Reporting | 보고서 생성 |

---

# 20. Naming Convention

Prompt 파일은 다음 형식을 따른다.

```text
01_Project_Analysis.md

02_Architecture_Review.md

03_Dependency_Analysis.md
```

---

# 21. Versioning

Prompt 변경 시 Semantic Versioning을 사용한다.

예시

```text
v1.0

v1.1

v2.0
```

---

# 22. Definition of Done

Prompt는 다음 조건을 만족하면 완료로 간주한다.

- 표준 구조 준수
- 목적 명확
- 규칙 포함
- Evidence Policy 포함
- Verification Policy 포함
- Output 정의
- Checklist 포함

---

# 23. Document Information

| Item | Value |
|------|-------|
| Document | PROMPT_STANDARD.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

PROMPT_STANDARD.md는 VSkript AI Engineering Kit의 모든 Prompt가 따라야 하는 공통 명세를 정의한다.

모든 Prompt는 본 문서의 구조와 규칙을 기반으로 작성되며, 이를 통해 AI 간 일관된 결과와 높은 품질을 보장한다.
