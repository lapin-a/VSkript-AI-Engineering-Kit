# SPEC-0017
# Identity Model

Version: 1.0

Status: Draft

---

# Part 1. Identity Fundamentals

---

# 1. Abstract

Identity는 VSkript Database에서 객체를 일관되고 안정적으로 식별하기 위한 공통 모델이다.

모든 식별 가능한 객체는 Identity를 가지며, Identity는 객체의 생명주기 동안 변경되지 않는다.

본 Specification은 프로젝트 전체에서 사용하는 Identity의 구조와 규칙을 정의한다.

---

# 2. Motivation

프로젝트에는 다양한 종류의 객체가 존재한다.

예시

- Registry Entry
- Symbol
- Type
- Event
- Effect
- Expression
- Provider
- Pipeline
- Cache Entry
- Artifact

이러한 객체를 안정적으로 참조하기 위해 공통 Identity 모델이 필요하다.

---

# 3. Goals

Identity는 다음 목표를 가진다.

- Stable Identification
- Global Uniqueness (필요한 범위 내에서)
- Deterministic Generation
- Immutable Representation
- Efficient Lookup
- Referential Integrity

---

# 4. Non Goals

Identity는 다음 역할을 수행하지 않는다.

- Object Storage
- Business Logic
- Version Management
- Permission Control
- Ownership Management

Identity는 객체를 식별할 뿐 객체 자체를 표현하지 않는다.

---

# 5. Identity Definition

Identity는 객체를 고유하게 식별하는 값이다.

```
Object

↓

Identity

↓

Reference
```

Identity는 객체의 내용이 아니라 객체 자체를 식별한다.

---

# 6. Identity Hierarchy

프로젝트에서 사용하는 대표적인 Identity는 다음과 같다.

```
Identity

├── Registry Identity
├── Symbol Identity
├── Provider Identity
├── Pipeline Identity
├── Cache Identity
├── Artifact Identity
└── Workspace Identity
```

모든 Identity는 본 Specification을 따른다.

---

# 7. Core Components

모든 Identity는 다음 요소를 가진다.

```
Identity

├── Identifier
├── Identity Type
├── Namespace
├── Metadata
└── Version
```

Identity는 객체의 내용과 독립적으로 관리된다.

---

# 8. Identifier

Identifier는 Identity의 핵심 값이다.

Identifier는

- Stable
- Immutable
- Comparable

해야 한다.

동일 객체는 항상 동일한 Identifier를 가져야 한다.

---

# 9. Identity Type

Identity는 종류를 가진다.

대표적인 Type

- Registry
- Symbol
- Provider
- Pipeline
- Cache
- Artifact
- Workspace

Type은 생성 이후 변경되지 않는다.

---

# 10. Namespace

Namespace는 Identifier의 충돌을 방지하기 위한 논리적 영역이다.

예시

```
skript:event

skript:effect

skript:type

plugin:custom
```

Namespace는 Identity의 의미를 변경하지 않는다.

---

# 11. Metadata

Identity는 다음과 같은 Metadata를 포함할 수 있다.

- Display Name
- Description
- Tags
- Source
- Created At

Metadata는 Identity의 비교 기준이 아니다.

---

# 12. Identity Lifetime

Identity는 객체의 생명주기와 동일한 Lifetime을 가진다.

객체가 삭제되기 전까지 Identifier는 변경되지 않는다.

새로운 객체는 새로운 Identity를 생성해야 한다.

---

# 13. Identity Equality

두 Identity가 동일하다고 판단하기 위한 기준은 Identifier와 Identity Type이다.

Metadata나 Display Name은 비교 기준으로 사용하지 않는다.

---

# 14. Identity Scope

Identity는 적용 범위를 가진다.

대표적인 Scope

- Global
- Workspace
- Project
- Session

Scope는 Namespace와 함께 충돌을 방지한다.

---

# 15. Part Summary

Part 1에서는 Identity의 기본 개념과 공통 구조를 정의하였다.

Identity는 객체를 안정적으로 식별하기 위한 공통 모델이며, Identifier, Type, Namespace를 중심으로 구성된다.

Part 2에서는 Identity 구조, 생성 규칙 및 참조 모델을 정의한다.

---

# Part 2. Identity Structure & Reference Model

---

# 16. Overview

Identity는 객체를 생성하는 순간 함께 생성되며, 객체의 생명주기 동안 변경되지 않는다.

Identity는 객체 간 참조(Reference)의 기준이 되며, 모든 참조는 Identity를 통해 수행한다.

---

# 17. Identity Structure

모든 Identity는 다음 논리 구조를 가진다.

```
Identity

├── Identifier
├── Type
├── Namespace
├── Scope
└── Metadata
```

Identifier는 필수이며 나머지 요소는 구현 목적에 따라 확장할 수 있다.

---

# 18. Identity Generation

Identity는 객체 생성 시 한 번만 생성한다.

Identity 생성 이후에는 Identifier를 변경해서는 안 된다.

객체를 복제(Clone)하는 경우에도 새로운 Identity를 생성해야 한다.

---

# 19. Generation Principles

Identity 생성은 다음 원칙을 따른다.

- Stable
- Deterministic (가능한 경우)
- Collision-resistant
- Immutable

동일한 객체를 다시 로드하는 경우에는 동일한 Identity를 재사용할 수 있다.

---

# 20. Identity Reference

객체는 서로를 직접 참조하지 않는다.

모든 참조는 Identity를 통해 수행한다.

```
Object A

↓

Identity

↓

Object B
```

이를 통해 객체 간 결합도를 낮추고 참조 무결성을 유지한다.

---

# 21. Reference Resolution

Identity를 이용한 참조는 Resolver를 통해 실제 객체로 변환한다.

```
Identity

↓

Resolver

↓

Object
```

Resolver는 Registry를 기준으로 객체를 조회한다.

---

# 22. Equality Rules

Identity 비교는 다음 항목만 사용한다.

- Identifier
- Identity Type

다음 항목은 비교 대상이 아니다.

- Display Name
- Description
- Metadata
- Tags

---

# 23. Identity Serialization

Identity는 직렬화가 가능해야 한다.

직렬화 시 포함되는 정보

- Identifier
- Type
- Namespace
- Scope

Metadata는 선택적으로 포함할 수 있다.

---

# 24. Identity Deserialization

직렬화된 Identity는 동일한 의미를 유지한 채 복원되어야 한다.

복원 과정에서 Identifier를 변경해서는 안 된다.

---

# 25. Identity Validation

Identity는 사용 전에 검증할 수 있다.

검증 항목 예시

- Identifier 존재 여부
- Type 일치 여부
- Namespace 유효성
- Scope 유효성

검증 실패 시 객체 참조를 수행해서는 안 된다.

---

# 26. Identity Collision

동일 Scope와 Namespace에서 동일 Identifier를 가진 두 개의 서로 다른 객체는 존재해서는 안 된다.

충돌이 발생하면 Registry는 이를 오류로 처리해야 한다.

---

# 27. Identity Resolution Failure

Resolver가 Identity를 객체로 변환하지 못하는 경우 다음 중 하나를 선택할 수 있다.

- Null 반환
- Error 반환
- Diagnostic 생성

선택은 구현체의 책임이다.

---

# 28. Referential Integrity

Identity를 통한 참조는 항상 참조 무결성을 유지해야 한다.

삭제된 객체를 참조하는 Identity는 유효하지 않은 참조(Broken Reference)로 간주한다.

Broken Reference는 Resolver가 감지해야 한다.

---

# 29. Identity Stability

동일한 객체는 다음 상황에서도 Identity를 유지해야 한다.

- Snapshot 생성
- Cache 생성
- Pipeline 실행
- Serialization / Deserialization

Identity는 객체의 논리적 존재를 나타내므로 실행 환경에 영향을 받지 않는다.

---

# 30. Relationship with Other Models

Identity는 다른 Core Model의 기반이 된다.

```
Identity

↓

Registry

↓

Graph

↓

Context

↓

Pipeline

↓

Cache
```

모든 모델은 객체를 직접 참조하지 않고 Identity를 통해 참조한다.

---

# 31. Part Summary

Part 2에서는 Identity 생성 규칙, 참조 모델 및 비교 규칙을 정의하였다.

Identity는 객체의 생명주기 동안 변경되지 않으며, 모든 객체 참조와 비교는 Identity를 기준으로 수행된다.

Part 3에서는 Identity Lifecycle, Namespace 관리 및 Versioning 전략을 정의한다.

---

# Part 3. Identity Lifecycle & Namespace Management

---

# 32. Overview

Identity는 객체의 생명주기와 함께 생성되고 종료된다.

Identity는 객체의 논리적 존재를 나타내며, 객체의 상태 변화와 독립적으로 유지된다.

---

# 33. Identity Lifecycle

모든 Identity는 다음 생명주기를 따른다.

```
Create

↓

Register

↓

Resolve

↓

Use

↓

Retire
```

Identity는 생성 이후 변경되지 않는다.

---

# 34. Identity Creation

Identity는 객체가 최초 생성될 때 함께 생성한다.

생성 시 결정되는 요소

- Identifier
- Identity Type
- Namespace
- Scope

생성 이후 이러한 값은 변경해서는 안 된다.

---

# 35. Identity Registration

생성된 Identity는 Registry에 등록되어야 한다.

```
Identity

↓

Registry

↓

Available for Resolution
```

등록되지 않은 Identity는 Resolver가 조회할 수 없다.

---

# 36. Identity Usage

Identity는 객체를 직접 변경하지 않는다.

Identity는 다음 목적으로 사용된다.

- Object Lookup
- Reference
- Equality Comparison
- Serialization
- Diagnostics

Identity는 객체의 상태를 표현하지 않는다.

---

# 37. Identity Retirement

객체가 더 이상 존재하지 않는 경우 Identity는 Retired 상태가 된다.

Retired Identity는 새로운 객체에 재사용해서는 안 된다.

---

# 38. Namespace Model

Namespace는 Identity를 논리적으로 구분한다.

예시

```
skript

minecraft

plugin

workspace

user
```

Namespace는 충돌 방지와 관리 목적을 가진다.

---

# 39. Namespace Rules

Namespace는 다음 조건을 만족해야 한다.

- Stable
- Unique within Scope
- Human-readable (권장)
- Consistent Naming

Namespace는 실행 중 변경해서는 안 된다.

---

# 40. Scope Management

Scope는 Identity가 유효한 범위를 정의한다.

대표적인 Scope

- Global
- Workspace
- Project
- Session

동일 Identifier라도 Scope가 다르면 서로 다른 Identity가 될 수 있다.

---

# 41. Identity Versioning

Identity 자체는 Version을 가지지 않는다.

Version이 필요한 경우 객체(Object)가 Version을 관리한다.

Identity는 동일 객체의 여러 Version을 연결하는 기준이 될 수 있다.

---

# 42. Identity Migration

객체를 다른 저장소나 환경으로 이동하는 경우에도 가능한 한 Identity를 유지해야 한다.

Identity를 변경해야 하는 경우에는 새로운 객체로 간주한다.

---

# 43. Identity Reuse

Retired Identity는 재사용하지 않는 것을 원칙으로 한다.

동일 Identifier를 새로운 객체에 부여하는 것은 참조 무결성을 훼손할 수 있다.

---

# 44. Broken Identity

다음과 같은 경우 Broken Identity로 간주한다.

- Registry에 존재하지 않음
- Namespace 불일치
- Scope 불일치
- Resolver 실패

Broken Identity는 객체를 참조할 수 없다.

---

# 45. Diagnostics

Identity 관련 문제는 Diagnostic으로 보고할 수 있다.

예시

- Duplicate Identifier
- Invalid Namespace
- Broken Reference
- Missing Identity

Diagnostics는 Identity를 수정하지 않는다.

---

# 46. Lifecycle Guarantees

모든 Identity는 다음을 보장해야 한다.

- 생성 이후 Identifier 불변
- 객체와 동일한 생명주기
- Registry 등록 전 사용 금지
- Retired Identity 재사용 금지

---

# 47. Relationship with Registry

Registry는 Identity의 저장소가 아니라 Identity를 이용해 객체를 관리하는 시스템이다.

Registry는 Identity를 생성하지 않으며, 이미 생성된 Identity를 등록하고 조회한다.

---

# 48. Identity Ownership

Identity의 소유자는 객체(Object)이다.

Registry, Cache, Pipeline 또는 Graph는 Identity를 생성하거나 변경해서는 안 된다.

이들은 Identity를 참조만 할 수 있다.

---

# 49. Lifecycle Events

Identity는 다음 이벤트를 발생시킬 수 있다.

- Created
- Registered
- Resolved
- Retired

이벤트는 Monitoring과 Diagnostics를 위한 목적으로만 사용한다.

---

# 50. Part Summary

Part 3에서는 Identity의 Lifecycle과 Namespace 관리 규칙을 정의하였다.

Identity는 객체와 함께 생성되고 함께 종료되며, Namespace와 Scope를 통해 충돌 없이 관리된다.

Part 4에서는 Identity Resolution, Lookup Strategy 및 Integration Model을 정의한다.

---

# Part 4. Identity Resolution & Integration Model

---

# 51. Overview

Identity는 객체를 직접 포함하지 않는다.

객체 접근은 항상 Resolver를 통해 수행하며, Resolver는 Registry를 기준으로 Identity를 실제 객체로 변환한다.

---

# 52. Resolution Model

Identity Resolution은 다음 절차를 따른다.

```
Identity

↓

Resolver

↓

Registry Lookup

↓

Object
```

Resolver는 Identity와 Registry를 연결하는 역할을 수행한다.

---

# 53. Resolver Responsibilities

Resolver는 다음 책임을 가진다.

- Identity Validation
- Registry Lookup
- Broken Reference Detection
- Resolution Result 생성

Resolver는 객체를 생성하거나 수정해서는 안 된다.

---

# 54. Resolution Rules

Identity Resolution은 다음 규칙을 따른다.

- Identifier가 일치해야 한다.
- Identity Type이 일치해야 한다.
- Namespace가 일치해야 한다.
- Scope가 유효해야 한다.

모든 조건을 만족할 경우에만 Resolution에 성공한다.

---

# 55. Resolution Results

Resolution은 다음 결과 중 하나를 반환한다.

- Success
- Not Found
- Invalid Identity
- Broken Reference
- Resolution Error

결과는 구현체에 따라 Diagnostic으로 변환될 수 있다.

---

# 56. Lookup Strategy

Lookup은 항상 Registry를 기준으로 수행한다.

```
Identity

↓

Resolver

↓

Registry

↓

Object
```

Cache는 Lookup을 최적화할 수 있지만 Registry를 우회해서는 안 된다.

---

# 57. Cache Integration

Resolver는 Cache를 사용할 수 있다.

```
Identity

↓

Cache Lookup

↓

Hit

↓

Object

or

↓

Miss

↓

Registry Lookup
```

Cache는 Lookup 성능을 향상시키기 위한 보조 계층이다.

---

# 58. Graph Integration

Graph는 Node를 직접 참조하지 않는다.

모든 Node는 Identity를 통해 연결된다.

```
Node A

↓

Identity

↓

Node B
```

Graph는 Identity 기반 참조를 유지해야 한다.

---

# 59. Pipeline Integration

Pipeline는 실행 대상 객체를 Identity로 관리한다.

Execution Plan에는 객체 대신 Identity를 저장할 수 있다.

실행 시 Resolver를 통해 객체를 획득한다.

---

# 60. Context Integration

Execution Context는 Identity를 저장할 수 있다.

Context는 Identity를 통해 필요한 객체를 조회하며, 객체 자체의 생명주기를 관리하지 않는다.

---

# 61. Registry Integration

Registry는 Identity를 기준으로 객체를 저장하고 조회한다.

Registry는 다음 기능을 제공한다.

- Register
- Unregister
- Lookup
- Enumerate

Identity는 Registry의 기본 인덱스 역할을 수행한다.

---

# 62. Serialization Integration

Identity는 Serialization 시 객체 대신 저장될 수 있다.

```
Object

↓

Identity

↓

Serialized Form
```

복원 시 Resolver를 통해 객체를 다시 연결한다.

---

# 63. Diagnostics Integration

Identity Resolution 과정에서 발생하는 오류는 Diagnostic으로 보고할 수 있다.

예시

- Unknown Identifier
- Namespace Conflict
- Broken Reference
- Invalid Scope

Diagnostic은 Resolution 결과를 변경하지 않는다.

---

# 64. Performance Considerations

Identity Resolution 구현은 다음 사항을 고려해야 한다.

- Fast Lookup
- Cache Support
- Stable Resolution
- Low Allocation
- Incremental Update Compatibility

성능 최적화는 Resolution 결과를 변경해서는 안 된다.

---

# 65. Part Summary

Part 4에서는 Identity Resolution과 다른 Core Model과의 통합 방식을 정의하였다.

모든 객체 접근은 Resolver를 통해 수행되며, Registry를 기준으로 Lookup을 수행한다.

Cache, Graph, Pipeline 및 Context는 Identity를 공통 참조 모델로 사용한다.

Part 5에서는 Design Principles, Best Practices, Compliance 및 Reference Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Design Principles

---

# 66. Overview

본 장에서는 모든 Identity 구현이 따라야 하는 설계 원칙과 구현 지침을 정의한다.

본 규칙은 Registry, Graph, Context, Pipeline, Cache 및 모든 확장 컴포넌트에 동일하게 적용된다.

---

# 67. Design Principles

모든 Identity는 다음 원칙을 따른다.

- Stable Identity
- Immutable Identifier
- Explicit Ownership
- Deterministic Resolution
- Referential Integrity
- Namespace Isolation
- Resolver-based Lookup
- Registry-backed Resolution

Identity는 객체의 존재를 식별하며 객체의 상태를 표현하지 않는다.

---

# 68. Best Practices

권장 사항

- 객체 생성 시 Identity를 함께 생성한다.
- Identifier는 절대 변경하지 않는다.
- 모든 참조는 Identity를 통해 수행한다.
- Resolver를 통해 객체를 조회한다.
- Namespace를 명확하게 구분한다.
- Scope를 명시적으로 정의한다.
- Registry를 단일 조회 지점으로 사용한다.
- Broken Reference를 Diagnostic으로 보고한다.

---

# 69. Anti-Patterns

다음 구현은 권장하지 않는다.

- Mutable Identifier
- 객체 직접 참조(Object Reference) 남용
- Identity 재사용
- Registry를 우회한 Lookup
- Namespace 없는 Identifier
- Resolver 없이 객체 조회
- Identity를 Cache Key와 동일시하는 구현
- Identity를 객체 상태 저장소로 사용하는 구현

이러한 구현은 참조 무결성과 시스템 일관성을 저하시킨다.

---

# 70. Identity Guarantees

모든 Identity는 다음 사항을 보장해야 한다.

- Identifier는 생성 이후 변경되지 않는다.
- 동일 Identity는 동일 객체를 의미한다.
- 서로 다른 객체는 서로 다른 Identity를 가진다.
- Identity 비교는 결정적(Deterministic)이어야 한다.
- Identity는 객체 생명주기 동안 유지된다.

---

# 71. Relationship with Other Models

Identity는 모든 Core Model의 기반이 된다.

```
Identity

↓

Registry

↓

Graph

↓

Context

↓

Pipeline

↓

Cache
```

다른 모델은 객체를 직접 참조하지 않고 Identity를 기준으로 객체를 식별한다.

---

# 72. Compliance

모든 Identity 구현은 다음 Specification을 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- SPEC-0013 Context Model
- SPEC-0014 Graph Model
- SPEC-0015 Pipeline Model
- SPEC-0016 Cache Model
- SPEC-0018 Registry Model

Identity는 프로젝트 전반에서 공통 식별 체계를 제공해야 한다.

---

# 73. Reference Architecture

Identity의 참조 구조는 다음과 같다.

```
Object

↓

Identity

↓

Resolver

↓

Registry

↓

Resolved Object
```

Identity는 객체를 직접 포함하지 않으며, Resolver와 Registry를 통해 객체를 연결한다.

---

# 74. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- SPEC-0013 Context Model
- SPEC-0014 Graph Model
- SPEC-0015 Pipeline Model
- SPEC-0016 Cache Model
- SPEC-0018 Registry Model

RFC 및 Implementation 문서는 Identity를 정의할 때 본 Specification을 참조한다.

---

# 75. Document Summary

Identity는 VSkript Database에서 객체를 안정적으로 식별하기 위한 공통 모델이다.

모든 객체는 생성 시 Identity를 가지며, 객체 간 참조는 Resolver와 Registry를 통해 수행된다.

Identity는 객체의 생명주기 동안 변경되지 않으며, 프로젝트 전체에서 참조 무결성과 결정적인 객체 식별을 보장한다.

모든 구현은 본 Specification을 준수해야 한다.

---

# 76. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Identity Model Specification |