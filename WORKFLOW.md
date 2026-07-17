# WORKFLOW.md

# VSkript AI Engineering Kit

> Standard Workflow for AI-Assisted Development, Analysis, Verification, and Maintenance

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 사용하는 표준 작업 절차(Standard Workflow)를 정의한다.

모든 AI는 본 문서의 절차를 따라 프로젝트를 분석하고, 구현하며, 검증해야 한다.

Workflow는 프로젝트 규모와 관계없이 동일한 순서를 유지하는 것을 원칙으로 한다.

---

# 2. Workflow Philosophy

모든 작업은 다음 원칙을 따른다.

- 분석 없이 구현하지 않는다.
- 구조를 이해하기 전에 수정하지 않는다.
- 구현 후 반드시 검증한다.
- 검증 후 반드시 보고서를 작성한다.
- 모든 단계는 이전 단계의 결과를 기반으로 진행한다.

---

# 3. Standard Workflow

```text
Project Request
      │
      ▼
Project Analysis
      │
      ▼
Architecture Review
      │
      ▼
Dependency Analysis
      │
      ▼
Performance Analysis
      │
      ▼
Implementation Planning
      │
      ▼
Feature Implementation
      │
      ▼
Refactoring
      │
      ▼
Static Verification
      │
      ▼
Functional Verification
      │
      ▼
Runtime Test
      │
      ▼
Regression Test
      │
      ▼
Code Audit
      │
      ▼
Security Review
      │
      ▼
Final Report
```

---

# 4. Workflow Phases

## Phase 1 - Project Analysis

### Purpose

프로젝트 전체를 이해한다.

### Input

- Source Code
- Documentation
- Configuration
- Build Files

### Tasks

- 프로젝트 구조 분석
- 실행 흐름 분석
- 데이터 흐름 분석
- 핵심 모듈 식별
- Provider 구조 분석

### Output

- Project Summary
- Architecture Overview
- Directory Structure
- Component List

### Prompt

```
01_Project_Analysis.md
```

---

## Phase 2 - Architecture Review

### Purpose

현재 구조가 적절한지 검토한다.

### Tasks

- Layer 분석
- Module 분석
- SOLID 평가
- 확장성 평가
- 책임 분리

### Output

- Architecture Report
- Improvement List

### Prompt

```
02_Architecture_Review.md
```

---

## Phase 3 - Dependency Analysis

### Purpose

프로젝트의 의존성을 분석한다.

### Tasks

- Internal Dependency
- External Dependency
- Circular Dependency
- Module Coupling

### Output

- Dependency Report

### Prompt

```
03_Dependency_Analysis.md
```

---

## Phase 4 - Performance Analysis

### Purpose

성능 병목을 찾는다.

### Tasks

- Startup Cost
- Memory Usage
- Cache
- Lazy Loading
- JSON Parsing

### Output

- Performance Report
- Optimization Candidates

### Prompt

```
04_Performance_Analysis.md
```

---

## Phase 5 - Implementation Planning

### Purpose

기능 구현 계획을 수립한다.

### Tasks

- 영향도 분석
- 수정 대상 식별
- 신규 파일 정의
- 기존 구조 유지 여부 검토

### Output

- Implementation Plan

---

## Phase 6 - Feature Implementation

### Purpose

기능을 구현한다.

### Rules

- 기존 구조를 우선 유지한다.
- 변경 이유를 기록한다.
- 필요한 경우 Adapter Layer를 사용한다.

### Prompt

```
10_Implementation.md
```

---

## Phase 7 - Refactoring

### Purpose

구조를 개선한다.

### Tasks

- 중복 제거
- 가독성 향상
- 구조 개선
- 코드 단순화

### Prompt

```
11_Refactoring.md
```

## Development Prompts (Conditional)

### Purpose

프로젝트 특성에 따라 선택적으로 수행하는 개발 Prompt를 정의한다.

### Prompt

12_Database_Architecture.md — Database 설계
13_DataPack_System.md — DataPack 시스템
14_Performance_Optimization.md — 성능 최적화

### Usage

`# 5. Workflow Selection Guide`에서 지정하는 상황(데이터베이스 개편, 성능 개선)에 한해 Phase 6(Feature Implementation) 또는 Phase 7(Refactoring) 이후에 수행한다.

---

## Phase 8 - Static Verification

### Purpose

Before / After를 비교한다.

### Tasks

- 수정 파일
- 신규 파일
- 삭제 파일
- 영향 범위
- API 변경

### Output

- Static Verification Report

### Prompt

```
20_Static_Verification.md
```

---

## Phase 9 - Functional Verification

### Purpose

기능이 정상 동작하는지 확인한다.

### Verification Targets

- Completion
- Hover
- Signature Help
- Definition
- References
- Diagnostics
- Commands
- Registry
- DataPack
- Manifest

### Prompt

```
21_Functional_Verification.md
```

---

## Phase 10 - Runtime Test

### Purpose

실행 환경에서 확인이 필요한 항목을 정리한다.

### Examples

- VSCode 실행
- Extension 활성화
- Hover 동작
- Completion 동작
- Plugin Detection
- DataPack Loading

### Output

Runtime Test Checklist

### Prompt

```
22_Runtime_Test.md
```

---

## Phase 11 - Regression Test

### Purpose

기존 기능이 손상되지 않았는지 확인한다.

### Verification Targets

- Completion
- Hover
- Signature Help
- Definition
- References
- Build

### Prompt

```
23_Regression_Test.md
```

---

## Phase 12 - Code Audit

### Purpose

코드 품질을 평가한다.

### Review Items

- SOLID
- Maintainability
- Scalability
- Complexity
- Error Handling
- Testability

### Prompt

```
24_Code_Audit.md
```

---

## Phase 13 - Security Review

### Purpose

잠재적인 보안 문제를 검토한다.

### Review Items

- Input Validation
- File Access
- Dependency Risk
- Configuration
- Data Integrity

### Prompt

```
25_Security_Review.md
```

---

## Phase 14 - Final Report

### Purpose

모든 작업을 종합하여 최종 보고서를 작성한다.

### Report Sections

- Executive Summary
- Completed Work
- Evidence
- Risks
- Remaining Tasks
- Recommendations
- Next Steps

### Prompt

```
26_Final_Report.md
```

---

# 5. Workflow Selection Guide

| Situation | Recommended Workflow |
|-----------|----------------------|
| 새로운 프로젝트 분석 | 01 → 02 → 03 → 04 |
| 신규 기능 구현 | 01 → 02 → 10 → 20 → 21 → 26 |
| 리팩터링 | 02 → 03 → 11 → 20 → 24 → 26 |
| 버그 수정 | 01 → 20 → 21 → 23 → 26 |
| 성능 개선 | 04 → 14 → 20 → 22 → 26 |
| 데이터베이스 개편 | 01 → 12 → 13 → 20 → 21 → 26 |
| 릴리즈 전 점검 | 20 → 21 → 22 → 23 → 24 → 25 → 26 |

---

# 6. Workflow Rules

모든 Workflow는 다음 규칙을 따른다.

- 이전 단계를 완료한 후 다음 단계를 수행한다.
- 프로젝트 분석 없이 구현하지 않는다.
- 구현 후 검증을 생략하지 않는다.
- Runtime Test가 필요한 항목은 반드시 분리한다.
- 모든 결과에는 코드 근거(Evidence)를 포함한다.


Database Architecture(12_Database_Architecture.md), DataPack System(13_DataPack_System.md),
Performance Optimization(14_Performance_Optimization.md)은
본 Phase와 동일한 절차(기존 구조 우선 유지 → 변경 이유 기록 → Adapter Layer 필요 시 적용)를 따른다.

---

# 7. Exit Criteria

Workflow는 다음 조건을 모두 만족하면 종료된다.

- 모든 요구사항 충족
- 정적 검증 완료
- 기능 검증 완료
- Runtime Test Checklist 작성
- 코드 감사 완료
- 최종 보고서 작성 완료

---

# 8. Document Information

| Item | Value |
|------|-------|
| Document | WORKFLOW.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

WORKFLOW.md는 VSkript AI Engineering Kit의 표준 작업 절차를 정의하는 문서이다. 프로젝트 분석부터 최종 보고서까지의 전체 흐름을 일관되게 관리하여 AI가 항상 동일한 절차와 품질 기준에 따라 작업하도록 하는 것을 목표로 한다.
