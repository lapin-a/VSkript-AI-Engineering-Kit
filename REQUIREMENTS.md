# REQUIREMENTS.md

# VSkript AI Engineering Kit

> Functional and Non-Functional Requirements

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit가 반드시 충족해야 하는 기능적 요구사항과 비기능적 요구사항을 정의한다.

모든 Prompt, Template, Workflow 및 문서는 본 요구사항을 기준으로 작성되어야 한다.

---

# 2. Objectives

본 프로젝트는 다음 목표를 달성해야 한다.

- AI가 프로젝트를 정확하게 분석할 수 있도록 한다.
- AI의 구현 품질을 표준화한다.
- 검증 절차를 자동화한다.
- 코드 감사(Code Audit)를 체계화한다.
- 보고서 품질을 향상시킨다.
- AI 결과물의 재현성을 확보한다.

---

# 3. Functional Requirements

## FR-001 Project Analysis

### Description

AI는 프로젝트 전체를 분석해야 한다.

### Requirements

- 프로젝트 전체 구조 분석
- 주요 컴포넌트 분석
- 데이터 흐름 분석
- 실행 흐름 분석
- Provider 구조 분석
- Build 구조 분석

### Output

- Project Summary
- Directory Tree
- Component List
- Architecture Diagram (텍스트)

---

## FR-002 Architecture Review

### Description

프로젝트의 설계를 검토한다.

### Requirements

- Layer 분석
- Module 분석
- SOLID 평가
- 책임 분리
- 의존성 방향
- 확장성 평가

---

## FR-003 Dependency Analysis

### Description

프로젝트의 의존성을 분석한다.

### Requirements

- Internal Dependency
- External Dependency
- Circular Dependency
- Coupling
- Cohesion

---

## FR-004 Performance Analysis

### Description

성능을 분석한다.

### Requirements

- Startup Cost
- Memory Usage
- Lazy Loading
- Cache
- Search Cost
- JSON Parsing
- File Loading

---

## FR-005 Feature Implementation

### Description

새로운 기능을 구현한다.

### Requirements

- 기존 구조 유지
- 영향 범위 분석
- 변경 이유 설명
- Adapter 적용 여부 판단

---

## FR-006 Refactoring

### Requirements

- 코드 중복 제거
- 구조 개선
- 가독성 향상
- 유지보수성 향상

---

## FR-007 Static Verification

### Description

Before / After 프로젝트를 비교한다.

### Requirements

- 수정 파일
- 신규 파일
- 삭제 파일
- 영향 범위
- API 변경
- 데이터 흐름 변경

---

## FR-008 Functional Verification

### Requirements

다음을 검증한다.

- Completion
- Hover
- Signature
- Definition
- References
- Diagnostics
- Commands
- DataPack
- Manifest
- Registry

---

## FR-009 Runtime Verification

### Requirements

AI는 실제 테스트가 필요한 항목을 구분해야 한다.

예시

- VSCode 실행
- Extension 활성화
- Hover 테스트
- Completion 테스트
- Plugin Detection
- DataPack Loading

---

## FR-010 Code Audit

### Requirements

- SOLID
- Maintainability
- Scalability
- Testability
- Readability
- Complexity
- Error Handling

---

## FR-011 Documentation

AI는 문서를 생성해야 한다.

### Examples

- README
- Prompt
- Guide
- Workflow
- Template
- Checklist

---

## FR-012 Report Generation

AI는 표준 보고서를 생성해야 한다.

보고서는 다음 구조를 따른다.

- Executive Summary
- Scope
- Inputs
- Methodology
- Findings
- Evidence
- Impact Analysis
- Risk Assessment
- Recommendations
- Verification Result
- Next Steps
- Appendix

---

# 4. Non-Functional Requirements

## NFR-001 Evidence-Based Analysis

모든 결론은 실제 코드와 프로젝트 구조를 근거로 작성해야 한다.

추측은 허용되지 않는다.

---

## NFR-002 Repeatability

동일한 입력에 대해 동일한 품질의 결과를 생성해야 한다.

---

## NFR-003 Maintainability

Prompt와 문서는 장기 유지보수가 가능해야 한다.

---

## NFR-004 Extensibility

새로운 Prompt와 Workflow를 쉽게 추가할 수 있어야 한다.

---

## NFR-005 Reusability

VSkript 외 프로젝트에서도 활용할 수 있어야 한다.

---

## NFR-006 Readability

모든 문서는 Markdown 표준을 따른다.

---

## NFR-007 Traceability

모든 결과는 근거를 추적할 수 있어야 한다.

---

# 5. Prompt Requirements

모든 Prompt는 다음 구조를 따른다.

- Purpose
- Scope
- Role
- Required Skills
- Input
- Output
- Workflow
- Rules
- Evidence Policy
- Verification Policy
- Quality Checklist
- Expected Result

---

# 6. Documentation Requirements

모든 문서는 다음 정보를 포함한다.

- Title
- Version
- Status
- Purpose
- Scope
- Last Updated

---

# 7. Verification Requirements

모든 검증은 아래 상태 중 하나로 판정한다.

| Status | Description |
|---------|-------------|
| VERIFIED | 코드와 요구사항이 일치함 |
| PARTIALLY VERIFIED | 일부만 확인됨 |
| NEEDS RUNTIME TEST | 실행 환경에서 확인 필요 |
| ISSUE | 문제 발견 |
| NOT APPLICABLE | 해당 없음 |

모든 판정에는 근거를 포함해야 한다.

---

# 8. Quality Requirements

AI 결과물은 다음 조건을 만족해야 한다.

- 추측 없음
- 코드 기반 분석
- 프로젝트 전체 이해
- 일관된 형식
- 명확한 근거
- 재현 가능한 결과

---

# 9. Acceptance Criteria

다음 조건을 모두 만족해야 완료로 간주한다.

## Analysis

- 프로젝트 구조 분석 완료
- 실행 흐름 분석 완료
- 데이터 흐름 분석 완료

---

## Development

- 변경 이유 설명
- 영향도 분석
- 구조 유지

---

## Verification

- 정적 검증 완료
- 기능 검증 완료
- Runtime Test 분리

---

## Documentation

- Markdown 형식 준수
- 표준 템플릿 사용
- 버전 정보 포함

---

# 10. Out of Scope

다음은 본 프로젝트의 범위에 포함되지 않는다.

- 실제 기능 구현 자체
- 배포 자동화 구현
- 테스트 코드 작성
- CI/CD 구축

단, 이를 위한 프롬프트와 문서는 포함한다.

---

# 11. Priority Matrix

| Priority | Description |
|----------|-------------|
| MUST | 반드시 구현 |
| SHOULD | 가능한 구현 |
| COULD | 선택 구현 |
| WON'T | 현재 제외 |

---

# 12. Definition of Done

다음 조건을 모두 만족하면 작업이 완료된 것으로 판단한다.

- 요구사항 충족
- 코드 근거 제시
- 보고서 작성 완료
- 검증 완료
- Runtime Test 필요 여부 명시
- 품질 기준 충족

---

# 13. Document Information

| Item | Value |
|------|-------|
| Document | REQUIREMENTS.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

REQUIREMENTS.md는 VSkript AI Engineering Kit의 모든 프롬프트, 문서, 템플릿 및 워크플로우가 따라야 하는 기능적·비기능적 요구사항을 정의한다. 모든 개발 및 검증 작업은 본 문서를 기준으로 수행되어야 한다.
