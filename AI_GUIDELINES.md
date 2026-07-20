# AI_GUIDELINES.md

# VSkript Data Platform AI Guidelines

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform에서 AI가 따라야 하는 행동 기준(Behavior Policy)을 정의한다.

AI는 프로젝트의 분석, 설계, 구현, 검증 및 문서화를 지원하지만, 모든 결과는 프로젝트의 Specification과 RULES를 기준으로 수행해야 한다.

---

# 2. Scope

본 문서는 다음 모든 AI 작업에 적용된다.

- Project Analysis
- Architecture Review
- Implementation
- Refactoring
- Verification
- Documentation
- Code Review
- DataPack Generation
- Registry Management
- Report Generation

---

# 3. Relationship with RULES

본 문서는 RULES.md를 보완하는 행동 지침이다.

문서 간 충돌이 발생하는 경우 항상 RULES.md를 우선한다.

---

# 4. Core Behavior

AI는 항상 다음 원칙을 따른다.

- Specification First
- Evidence First
- Project First
- Documentation First
- Consistency First
- Maintainability First

---

# 5. Project Understanding

AI는 구현 전에 프로젝트 전체를 이해해야 한다.

필요한 경우 다음을 반복 수행한다.

- 문서 열람
- Specification 확인
- Architecture 확인
- Source Code 확인
- Dependency 확인

파일을 충분히 이해하기 전에는 결론을 내려서는 안 된다.

---

# 6. Evidence Policy

AI는 모든 주장에 대해 근거를 제시해야 한다.

허용되는 근거는 다음과 같다.

- Source Code
- Specification
- Documentation
- Runtime Result
- Test Result

근거 없는 판단은 허용되지 않는다.

---

# 7. No Guessing Policy

AI는 다음 행위를 하지 않는다.

- 추측
- 가정
- 경험 기반 판단
- 확인되지 않은 설명

정보가 부족한 경우 필요한 문서를 요청하거나 직접 확인한다.

---

# 8. Specification First

모든 구현은 Specification을 기준으로 수행한다.

AI는 Specification을 임의로 변경하거나 재해석해서는 안 된다.

Specification과 구현이 다를 경우 Specification을 우선한다.

---

# 9. Documentation First

새로운 기능을 구현하기 전에 관련 문서가 존재해야 한다.

필요한 경우 다음 순서를 따른다.

```text
Requirement

↓

Architecture

↓

Specification

↓

Implementation

↓

Verification

↓

Documentation Update
```

---

# 10. Project First

AI는 개별 파일이 아니라 프로젝트 전체를 기준으로 판단한다.

다음 요소를 함께 고려한다.

- Architecture
- Workflow
- Specification
- Registry
- Runtime
- DataPack

---

# 11. File Inspection

필요한 파일은 언제든지 다시 열람할 수 있다.

AI는 한 번 읽은 파일이라도 필요하면 다시 확인한다.

읽지 않은 파일에 대해 단정해서는 안 된다.

---

# 12. Implementation Policy

구현 시 다음 원칙을 따른다.

- 작은 변경
- 최소 영향
- 기존 기능 보존
- 명확한 책임 분리
- 테스트 가능성 유지

---

# 13. Documentation Policy

문서를 작성할 때는 다음을 준수한다.

- Markdown 사용
- 표준 용어 사용
- 일관된 제목 구조
- 명확한 목적
- 근거 기반 설명

---

# 14. Verification Policy

AI는 가능한 범위에서 검증을 수행한다.

검증 유형은 다음과 같다.

- Static Verification
- Functional Verification
- Runtime Test
- Regression Test

검증하지 않은 내용을 검증 완료로 표시해서는 안 된다.

---

# 15. Reporting Policy

보고서는 REPORT_STANDARD.md를 따른다.

모든 보고서는 다음을 포함한다.

- Executive Summary
- Findings
- Evidence
- Impact
- Recommendation

---

# 16. Runtime Independence

AI는 특정 Runtime에 종속된 설계를 제안하지 않는다.

동일한 DataPack은 다양한 Runtime에서 사용할 수 있어야 한다.

예시

- VSCode Extension
- CLI
- Language Server
- Documentation Generator
- Static Analyzer

---

# 17. DataPack Awareness

AI는 프로젝트의 핵심 산출물이 DataPack임을 이해해야 한다.

모든 구현은 DataPack 생성 및 활용을 고려해야 한다.

---

# 18. Registry Awareness

Registry는 프로젝트의 공식 데이터 관리 시스템이다.

AI는 Registry를 Source of Truth로 간주한다.

Registry를 우회하는 설계를 제안해서는 안 된다.

---

# 19. Error Handling

정보가 부족한 경우 AI는 다음 순서를 따른다.

```text
Search Documentation

↓

Read Related Files

↓

Collect Evidence

↓

Ask User (if required)

↓

Continue
```

추측으로 빈 내용을 채우지 않는다.

---

# 20. Communication

AI는 다음과 같이 응답한다.

- 명확하게 설명한다.
- 근거를 제시한다.
- 변경 이유를 설명한다.
- 영향 범위를 설명한다.

---

# 21. Prohibited Behavior

AI는 다음 행위를 하지 않는다.

- 근거 없는 결론
- 존재하지 않는 기능 설명
- 읽지 않은 파일에 대한 단정
- Specification과 다른 구현 제안
- 문서와 구현의 불일치 유도

---

# 22. Success Criteria

AI 작업은 다음 조건을 만족해야 완료된다.

- 프로젝트 이해 완료
- Specification 확인 완료
- 근거 확보
- 구현 또는 분석 완료
- 검증 완료
- 문서 최신화

---

# 23. Summary

AI는 VSkript Data Platform의 표준 Workflow와 Specification을 기반으로 프로젝트를 분석, 구현 및 검증한다.

모든 결과는 Evidence를 기반으로 하며, 추측 없이 프로젝트 전체의 일관성과 유지보수성을 최우선으로 고려한다.

---

# Document Information

| Item | Value |
|------|-------|
| Document | AI_GUIDELINES.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |