# CONTRACT_AUTHORING_GUIDE

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database의 모든 Contract Specification 작성 규칙을 정의한다.

모든 Contract 문서는 본 문서를 반드시 준수해야 한다.

Generator는 Contract를 구현하지 않는다.

Contract는 Canonical Model을 정의한다.

---

# 2. Scope

본 문서는 다음 Contract에 적용된다.

- CONTRACT_TYPE
- CONTRACT_PARAMETER
- CONTRACT_PROPERTY
- CONTRACT_FUNCTION
- CONTRACT_EXPRESSION
- CONTRACT_EFFECT
- CONTRACT_EVENT
- CONTRACT_CLASS
- CONTRACT_MODULE

향후 추가되는 모든 Contract에도 동일하게 적용된다.

---

# 3. Core Principles

모든 Contract는 다음 원칙을 따른다.

- Contract First
- Canonical Model
- Single Source of Truth
- Explicit Dependency
- Runtime Independence
- Layered Architecture
- Deterministic Structure

---

# 4. Required Document Structure

모든 Contract는 다음 순서를 따른다.

```
Part 1
    Purpose
    Scope
    Definition
    Design Goals
    Canonical Composition

Part 2
    Canonical Structure

Part 3
    Relationship Model

Part 4
    Validation
    Serialization
    Runtime Independence

Part 5
    Architecture Summary
```

Section 순서는 변경해서는 안 된다.

---

# 5. Canonical Composition Rule

Canonical Composition은 Contract의 구성 요소를 정의한다.

Canonical Composition은 Contract의 최초 정의이다.

모든 필드는 Canonical Composition에서 처음 등장해야 한다.

새로운 필드를 이후 Section에서 추가해서는 안 된다.

---

# 6. Canonical Structure Rule

Canonical Structure는 Canonical Composition과 동일해야 한다.

다음 사항은 허용되지 않는다.

- 필드 추가
- 필드 삭제
- 필드 이름 변경
- 필드 순서 변경

Canonical Structure는 Generator의 공식 입력 모델이다.

---

# 7. Architecture Summary Rule

Architecture Summary는 Canonical Structure를 다시 설명하는 문서이다.

Architecture Summary는

- 새로운 필드를 추가해서는 안 된다.
- 기존 필드를 제거해서는 안 된다.
- 구조를 변경해서는 안 된다.

Architecture Summary는 설명만 수행한다.

---

# 8. Field Introduction Rule

모든 필드는 최초 한 번만 정의한다.

예를 들어

Kind

Constraint

Optionality

Result

Version

등은 반드시 Canonical Composition에서 처음 정의되어야 한다.

Validation이나 Summary에서 처음 등장해서는 안 된다.

---

# 9. Cross Contract Dependency Rule

Cross Contract Dependency에는 실제 구조에서 참조하는 Contract만 포함한다.

문서 참고(Cross References)는 Dependency가 아니다.

Cross References와 Cross Contract Dependency를 혼용해서는 안 된다.

---

# 10. Cross References Rule

Cross References는 참고 문서만 기술한다.

Cross References는 구조적 의존성을 의미하지 않는다.

---

# 11. Relationship Model Rule

Relationship Model은 Contract 간 관계를 설명한다.

Relationship Model은 Canonical Structure를 변경해서는 안 된다.

---

# 12. Version Rule

Version은 Specification Version을 의미한다.

Contract 자체는 Version 필드를 기본적으로 가지지 않는다.

Version 필드가 필요한 경우 Specification에서 명시적으로 정의한다.

---

# 13. Runtime Independence Rule

모든 Contract는 Runtime 구현과 독립적이어야 한다.

Runtime 설명은 구조를 변경해서는 안 된다.

---

# 14. Validation Rule

Validation은 Canonical Structure를 검증한다.

Validation에서 새로운 구조를 정의해서는 안 된다.

---

# 15. Serialization Rule

Serialization은 Canonical Structure를 그대로 직렬화한다.

직렬화 과정에서

- 필드 추가
- 필드 제거
- 필드 순서 변경

을 수행해서는 안 된다.

---

# 16. Layer Rule

모든 Contract는 다음 Layer를 따른다.

Aggregation Layer

↓

Object Layer

↓

Callable Layer

↓

Atomic Layer

Layer를 건너뛰는 구조를 정의해서는 안 된다.

---

# 17. Contract Completeness Rule

다른 Contract를 참조하는 경우

해당 Contract는 반드시 존재해야 한다.

존재하지 않는 Contract를 참조해서는 안 된다.

---

# 18. Terminology Rule

CORE_GLOSSARY에 등록되지 않은 용어는 사용해서는 안 된다.

새로운 용어를 사용하는 경우

먼저 CORE_GLOSSARY에 정의해야 한다.

---

# 19. Review Checklist

Contract를 작성한 후 반드시 다음 항목을 검토한다.

- Canonical Composition과 Canonical Structure가 동일한가
- Architecture Summary가 동일한 구조를 사용하는가
- 새로운 필드가 중간에 등장하지 않는가
- Cross Contract Dependency가 실제 구조와 일치하는가
- Cross References가 Dependency와 혼동되지 않는가
- CORE_GLOSSARY에 없는 용어를 사용하지 않았는가
- Runtime 설명이 구조를 변경하지 않는가
- Validation이 새로운 모델을 정의하지 않는가
- 존재하지 않는 Contract를 참조하지 않는가

---

# 20. Compliance

모든 Contract는 본 Guide를 준수해야 한다.

본 Guide를 위반하는 Contract는 Canonical Contract로 인정하지 않는다.