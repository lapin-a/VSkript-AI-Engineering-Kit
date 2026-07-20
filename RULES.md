# RULES.md

# VSkript Data Platform Rules

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform에서 적용되는 모든 개발 규칙을 정의한다.

모든 문서, Specification, 구현, AI Workflow는 본 문서를 기준으로 수행한다.

본 문서는 프로젝트의 최상위 규칙(Constitution)이다.

---

# 2. Scope

본 규칙은 다음 대상에 적용된다.

- Documentation
- Specification
- Architecture
- Source Code
- AI-generated Content
- Review
- Verification
- Runtime
- DataPack
- Registry

---

# 3. Rule Priority

문서 간 내용이 충돌하는 경우 다음 우선순위를 따른다.

```
RULES

↓

PROJECT

↓

REQUIREMENTS

↓

ARCHITECTURE

↓

WORKFLOW

↓

AI_GUIDELINES

↓

STYLE_GUIDE

↓

QUALITY_GATE

↓

PROMPT_STANDARD

↓

REPORT_STANDARD

↓

Implementation
```

상위 문서는 하위 문서보다 우선한다.

---

# 4. Fundamental Principles

프로젝트는 다음 원칙을 따른다.

- Specification First
- Evidence First
- Project First
- Documentation First
- Single Source of Truth
- Maintainability First
- Consistency First
- Runtime Independence
- Reproducibility

---

# 5. Evidence Rules

모든 결론은 근거를 가져야 한다.

허용되는 근거는 다음과 같다.

- Source Code
- Specification
- Documentation
- Runtime Result
- Test Result

추측은 근거가 아니다.

---

# 6. Specification Rules

모든 구현은 Specification을 기반으로 수행한다.

Specification은 프로젝트의 공식 Source of Truth이다.

Implementation은 Specification을 변경해서는 안 된다.

---

# 7. Documentation Rules

모든 기능은 문서가 먼저 존재해야 한다.

권장 순서는 다음과 같다.

```
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

# 8. Development Rules

모든 개발은 다음 원칙을 따른다.

- 작은 단위로 구현한다.
- 변경 범위를 최소화한다.
- 기존 기능을 불필요하게 변경하지 않는다.
- 테스트 가능한 구조를 유지한다.

---

# 9. AI Behavior Rules

AI는 다음 규칙을 반드시 따른다.

- 추측하지 않는다.
- 필요한 파일을 직접 확인한다.
- 프로젝트 전체를 먼저 이해한다.
- 근거 없는 결론을 내리지 않는다.
- 동일한 용어를 일관되게 사용한다.

---

# 10. Workflow Rules

모든 작업은 WORKFLOW.md를 따른다.

Workflow를 임의로 변경해서는 안 된다.

필요한 단계만 선택적으로 수행할 수 있다.

---

# 11. Review Rules

Review는 다음 기준으로 수행한다.

- Structural Validation
- Semantic Validation
- Cross-document Validation
- Runtime Validation
- Evidence Review

---

# 12. Verification Rules

모든 변경은 가능한 범위에서 검증한다.

검증 종류는 다음과 같다.

- Static Verification
- Functional Verification
- Runtime Test
- Regression Test

---

# 13. Versioning Rules

모든 공식 문서는 Version을 가진다.

Version은 Semantic Versioning을 따른다.

Major 변경은 Architecture 변경을 의미한다.

---

# 14. Repository Rules

Repository는 다음 구조를 유지한다.

```
Documentation

↓

Specification

↓

Implementation

↓

Distribution
```

문서는 항상 최신 상태를 유지해야 한다.

---

# 15. Prohibited Actions

다음 행위는 허용하지 않는다.

- 근거 없는 추측
- 확인되지 않은 정보 작성
- Specification 없이 구현
- 문서와 구현 불일치
- 순환 의존성 생성
- Runtime 의존적인 설계

---

# 16. Decision Priority

기술적 의사결정은 다음 기준으로 수행한다.

1. Specification
2. Architecture
3. Maintainability
4. Consistency
5. Performance
6. Convenience

---

# 17. Exception Policy

예외가 필요한 경우 반드시 문서화한다.

예외는 다음 내용을 포함해야 한다.

- 이유
- 영향 범위
- 대안
- 승인 여부

---

# 18. Summary

VSkript Data Platform의 모든 개발은 Specification을 기준으로 수행하며, Evidence를 기반으로 검증하고, Documentation과 Implementation의 일관성을 유지해야 한다.

본 문서는 프로젝트 전체의 최상위 규칙으로 사용된다.

---

# Document Information

| Item | Value |
|------|-------|
| Document | RULES.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |