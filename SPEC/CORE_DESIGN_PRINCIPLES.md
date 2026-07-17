# Core Design Principles

Version: 1.0

Status: Draft

---

# Part 1. Philosophy

---

# 1. Purpose

본 문서는 VSkript Database 프로젝트 전체에서 공통적으로 적용되는 설계 원칙을 정의한다.

모든 Specification, Contract, Generator, Implementation 및 Code는 본 문서의 원칙을 준수해야 한다.

---

# 2. Design Philosophy

프로젝트는 다음 철학을 기반으로 설계된다.

- Predictability
- Determinism
- Explicitness
- Extensibility
- Maintainability

복잡성을 줄이고, 예측 가능한 동작을 최우선 목표로 한다.

---

# 3. Core Architecture

프로젝트는 명확하게 분리된 계층으로 구성된다.

```
Contracts

↓

Registry

↓

Graph

↓

Pipeline

↓

Generator

↓

Artifacts
```

각 계층은 자신의 책임만 수행한다.

---

# 4. Separation of Concerns

하나의 컴포넌트는 하나의 책임만 가져야 한다.

예시

Registry

→ 객체 저장

Pipeline

→ 실행 순서 관리

Generator

→ 코드 생성

Cache

→ 성능 최적화

역할은 서로 겹쳐서는 안 된다.

---

# 5. Single Source of Truth

프로젝트에는 하나의 공식 데이터만 존재해야 한다.

```
Registry

↓

Source of Truth
```

다른 시스템은 Registry를 참조만 한다.

---

# 6. Identity First

모든 객체는 Identity를 통해 식별된다.

객체 참조는 Object Reference가 아니라 Identity Reference를 사용한다.

Identity는 프로젝트 전체의 공통 식별 체계이다.

---

# 7. Explicit Data Flow

데이터 흐름은 항상 명시적이어야 한다.

```
Input

↓

Transform

↓

Output
```

숨겨진 변경(Hidden Mutation)은 허용하지 않는다.

---

# 8. Immutable Models

가능한 모든 데이터 모델은 Immutable하게 설계한다.

변경이 필요한 경우에는 새로운 모델을 생성한다.

Immutable Model은 Snapshot과 Cache의 일관성을 유지한다.

---

# 9. Deterministic Behaviour

동일한 입력은 항상 동일한 결과를 생성해야 한다.

동일한 Contract는 동일한 Artifact를 생성해야 한다.

동일한 Registry는 동일한 Lookup 결과를 제공해야 한다.

---

# 10. Stable Public Contracts

외부 API와 Contract는 안정성을 유지해야 한다.

호환성을 깨는 변경은 RFC와 ADR 절차를 거쳐야 한다.

---

# 11. Extensibility

새로운 기능은 기존 구조를 변경하지 않고 확장할 수 있어야 한다.

Extension Point는 명확하게 정의되어야 한다.

---

# 12. Testability

모든 컴포넌트는 독립적으로 테스트 가능해야 한다.

- Registry
- Generator
- Pipeline
- Resolver
- Cache

의존성은 최소화한다.

---

# 13. Documentation First

새로운 기능은 구현보다 문서가 먼저 정의되어야 한다.

권장 순서

RFC

↓

SPEC

↓

Contract

↓

Implementation

↓

Code

---

# 14. Compatibility

새로운 설계는 기존 구조와 가능한 한 호환되어야 한다.

불가피한 변경은 Migration Strategy를 함께 정의한다.

---

# 15. Part Summary

Part 1에서는 프로젝트 전체를 관통하는 핵심 설계 철학을 정의하였다.

이 원칙은 모든 Core Specification과 Implementation 문서의 기준이 된다.

Part 2에서는 Architectural Principles를 정의한다.

---

# Part 2. Architectural Principles

---

# 16. Overview

본 장에서는 프로젝트의 아키텍처를 구성하는 핵심 원칙을 정의한다.

모든 컴포넌트는 이러한 원칙을 기반으로 설계되어야 한다.

---

# 17. Layered Architecture

프로젝트는 계층형 구조를 따른다.

```
RFC

↓

Specification

↓

Contract

↓

Generator

↓

Implementation

↓

Runtime
```

상위 계층은 하위 계층에 의존하지 않는다.

---

# 18. Dependency Direction

의존성은 항상 한 방향으로만 흐른다.

```
Contract

↓

Generator

↓

Implementation
```

역방향 의존성은 허용하지 않는다.

---

# 19. Dependency Inversion

구현은 인터페이스에 의존해야 한다.

```
Interface

↓

Implementation
```

구체 구현 간 직접 의존은 최소화한다.

---

# 20. Interface-first Design

모든 외부 기능은 Interface 또는 Contract를 먼저 정의한다.

권장 순서

```
Interface

↓

Specification

↓

Implementation
```

---

# 21. Composition over Inheritance

기능 확장은 상속보다 조합을 우선한다.

예시

```
Registry

+ Cache

+ Resolver

+ Provider
```

상속은 공통 행위가 명확한 경우에만 사용한다.

---

# 22. Loose Coupling

컴포넌트 간 결합도를 최소화한다.

권장 방법

- Identity Reference
- Interface
- Resolver
- Event

객체 직접 참조는 가능한 피한다.

---

# 23. High Cohesion

하나의 컴포넌트는 관련된 기능만 포함해야 한다.

예시

Registry

→ 저장 및 조회

Generator

→ 코드 생성

Pipeline

→ 실행 순서

책임이 혼합되어서는 안 된다.

---

# 24. Explicit Dependencies

모든 의존성은 명시적으로 선언한다.

숨겨진 전역 상태(Global State)나 암묵적인 초기화는 허용하지 않는다.

---

# 25. Event-driven Collaboration

상태 변경은 이벤트를 통해 전달할 수 있다.

예시

- Registry Updated
- Cache Invalidated
- Pipeline Completed

이벤트는 상태를 변경하지 않으며 변경 사실만 전달한다.

---

# 26. Service Boundaries

서비스 간 경계를 명확하게 유지한다.

예시

```
Registry

↓

Resolver

↓

Language Server
```

Language Server는 Registry 내부 구현을 알 필요가 없다.

---

# 27. Configuration over Hardcoding

설정 가능한 요소는 코드에 하드코딩하지 않는다.

예시

- Generator Options
- Cache Policy
- Output Path
- Naming Rules

환경에 따라 쉽게 변경 가능해야 한다.

---

# 28. Replaceable Components

컴포넌트는 다른 구현으로 교체 가능해야 한다.

예시

```
Cache

↓

Memory Cache

↓

Persistent Cache
```

외부 계약은 동일하게 유지한다.

---

# 29. Explicit Lifecycle

모든 주요 컴포넌트는 생명주기를 명확히 정의한다.

예시

```
Create

↓

Initialize

↓

Ready

↓

Dispose
```

Lifecycle은 문서화되어야 한다.

---

# 30. Architectural Stability

아키텍처 변경은 다음 순서를 따른다.

```
RFC

↓

ADR

↓

Specification

↓

Implementation
```

구현이 설계를 선행해서는 안 된다.

---

# 31. Part Summary

Part 2에서는 프로젝트의 핵심 아키텍처 원칙을 정의하였다.

모든 컴포넌트는 계층형 구조와 단방향 의존성을 유지하며, Interface 중심 설계를 통해 교체 가능성과 유지보수성을 확보해야 한다.

Part 3에서는 Data Modeling Principles와 Domain Modeling 규칙을 정의한다.

---

# Part 3. Data Modeling Principles

---

# 32. Overview

본 장에서는 프로젝트 전체에서 사용하는 데이터 모델의 설계 원칙을 정의한다.

모든 Domain Model은 일관된 구조와 규칙을 따라야 한다.

---

# 33. Domain-driven Modeling

데이터 모델은 구현이 아니라 Domain을 표현해야 한다.

잘못된 예

- Database 구조를 그대로 Domain Model로 사용하는 경우
- UI 구조를 Domain Model로 사용하는 경우

올바른 예

- Type
- Symbol
- Event
- Effect
- Registry Entry

---

# 34. Canonical Model

하나의 개념은 하나의 Canonical Model을 가진다.

예시

```
Type

↓

Type Model
```

동일한 개념을 여러 Model로 중복 정의해서는 안 된다.

---

# 35. Identity-based Modeling

모든 주요 객체는 Identity를 가진다.

객체 간 연결은 Identity를 통해 수행한다.

```
Object

↓

Identity

↓

Reference
```

Object Reference를 직접 저장하는 것은 권장하지 않는다.

---

# 36. Immutable Domain Objects

가능한 모든 Domain Object는 Immutable하게 설계한다.

변경이 필요한 경우

```
Old Object

↓

Transformation

↓

New Object
```

기존 객체를 직접 수정하지 않는다.

---

# 37. Explicit State

객체의 상태는 명시적으로 표현한다.

예시

- Valid
- Invalid
- Deprecated
- Removed

Boolean Flag의 조합으로 상태를 표현하는 것은 권장하지 않는다.

---

# 38. Deterministic Transformations

동일한 입력은 항상 동일한 Domain Model을 생성해야 한다.

Transformation은 외부 상태에 의존해서는 안 된다.

---

# 39. Validation at Boundaries

데이터 검증은 시스템 경계(Boundary)에서 수행한다.

예시

- Contract Import
- Registry Registration
- External API Input

내부 Domain Model은 이미 검증된 데이터를 전제로 한다.

---

# 40. Separation of Domain and Transport

Domain Model과 Transport Model은 분리한다.

예시

```
Domain Model

↓

Serializer

↓

JSON

↓

Deserializer

↓

Domain Model
```

JSON Schema나 OpenAPI는 Domain Model 자체가 아니다.

---

# 41. Version-aware Models

Version이 필요한 경우 객체 자체가 Version을 관리한다.

Identity는 Version과 독립적으로 유지한다.

Version 변경으로 Identity가 변경되어서는 안 된다.

---

# 42. Metadata Separation

Metadata는 Domain Data와 분리한다.

예시

```
Registry Entry

├── Domain Object
└── Metadata
```

Metadata는 객체의 의미를 변경하지 않는다.

---

# 43. Stable Serialization

모든 Domain Model은 안정적으로 직렬화 가능해야 한다.

동일한 Domain Object는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 44. Snapshot Compatibility

Domain Model은 Snapshot 기반 실행을 지원해야 한다.

Snapshot 생성 이후 객체는 변경되지 않는 것을 원칙으로 한다.

---

# 45. Evolution without Breaking

새로운 필드 추가는 기존 구조를 깨뜨리지 않는 방식으로 수행한다.

권장 방식

- Optional Field 추가
- 새로운 Model 추가
- Deprecated 처리

기존 필드의 의미를 변경하는 것은 지양한다.

---

# 46. Domain Consistency

모든 Domain Model은 다음 사항을 만족해야 한다.

- Identity 일관성
- 참조 무결성
- 결정적 비교
- 안정적인 직렬화
- Snapshot 호환성

---

# 47. Part Summary

Part 3에서는 프로젝트 전반의 데이터 모델링 원칙을 정의하였다.

모든 Domain Model은 Identity 중심으로 설계되며, Immutable 구조와 안정적인 직렬화를 통해 장기적인 유지보수성과 확장성을 확보해야 한다.

Part 4에서는 Error Handling, Diagnostics 및 Reliability Principles를 정의한다.

---

# Part 4. Reliability, Error Handling & Diagnostics

---

# 48. Overview

본 장에서는 프로젝트 전반의 오류 처리, Diagnostics 및 신뢰성(Reliability)에 대한 공통 원칙을 정의한다.

오류 처리는 예측 가능하고 결정적이어야 하며, 가능한 한 시스템 전체의 동작을 지속할 수 있도록 설계한다.

---

# 49. Fail Fast

명백한 오류는 가능한 한 빠르게 감지한다.

예시

- Invalid Identity
- Duplicate Registration
- Invalid Contract
- Unsupported Generator

오류를 늦게 발견하는 것보다 초기에 감지하는 것을 우선한다.

---

# 50. Graceful Degradation

치명적이지 않은 오류는 전체 시스템을 중단시키지 않는다.

예시

```
Registry

↓

Entry Failure

↓

Diagnostic

↓

Continue
```

가능한 경우 정상적인 기능은 계속 수행한다.

---

# 51. Explicit Error Reporting

모든 오류는 명확하게 보고되어야 한다.

오류에는 다음 정보가 포함될 수 있다.

- Error Code
- Message
- Source
- Related Identity
- Severity

오류를 무시하거나 숨겨서는 안 된다.

---

# 52. Diagnostics Model

Diagnostics는 오류와 경고를 표현하는 공통 모델이다.

대표적인 Severity

- Error
- Warning
- Information
- Hint

Diagnostics는 가능한 한 구조화된 형태로 제공한다.

---

# 53. Error Boundaries

오류는 시스템 경계에서 처리한다.

예시

- Contract Import
- Registry Registration
- Generator Execution
- External API
- Language Server Request

내부 Domain Model은 정상 상태를 가정한다.

---

# 54. Recoverable Errors

복구 가능한 오류는 시스템이 계속 실행될 수 있어야 한다.

예시

- Broken Reference
- Missing Optional Provider
- Cache Miss
- Deprecated Entry

복구 가능한 오류는 Diagnostic과 함께 처리한다.

---

# 55. Unrecoverable Errors

다음과 같은 경우 실행을 중단할 수 있다.

- Registry Corruption
- Invalid Snapshot
- Identity Collision
- Internal Consistency Failure

치명적인 오류는 명확하게 보고되어야 한다.

---

# 56. Logging Principles

로그는 구현체가 제공할 수 있다.

권장 수준

- Trace
- Debug
- Information
- Warning
- Error

로그는 Domain Model을 변경해서는 안 된다.

---

# 57. Observability

시스템은 관찰 가능(Observable)해야 한다.

권장 수집 항목

- Registry Statistics
- Cache Statistics
- Pipeline Execution
- Generator Duration
- Diagnostics Count

Observability는 운영과 디버깅을 지원한다.

---

# 58. Reliability

모든 컴포넌트는 다음 사항을 만족해야 한다.

- Predictable Behaviour
- Deterministic Result
- Stable State
- Consistent Recovery

부분적인 오류가 전체 시스템으로 전파되지 않도록 설계한다.

---

# 59. Defensive Programming

외부 입력은 신뢰하지 않는다.

다음 입력은 항상 검증한다.

- External Files
- Contracts
- Configuration
- User Input
- Plugin Data

검증은 시스템 경계에서 수행한다.

---

# 60. Retry Policy

재시도가 가능한 작업은 명확한 정책을 가져야 한다.

예시

- Remote Provider
- File System
- Network Access

Domain Logic 자체는 재시도 정책에 의존하지 않는다.

---

# 61. Monitoring

구현체는 다음 정보를 모니터링할 수 있다.

- Error Rate
- Warning Count
- Cache Hit Ratio
- Registry Size
- Generator Failures

Monitoring은 시스템 동작을 변경하지 않는다.

---

# 62. Reliability Guarantees

모든 구현은 다음 사항을 보장해야 한다.

- 오류는 명확하게 보고된다.
- Recoverable Error는 가능한 한 복구한다.
- Fatal Error는 시스템 상태를 손상시키지 않는다.
- Diagnostics는 결정적이다.

---

# 63. Part Summary

Part 4에서는 프로젝트 전체의 오류 처리 및 신뢰성 원칙을 정의하였다.

모든 컴포넌트는 명확한 Diagnostics와 결정적인 오류 처리를 제공하며, 가능한 경우 Graceful Degradation을 통해 시스템의 안정성을 유지해야 한다.

Part 5에서는 프로젝트 전반의 Best Practices, Anti-Patterns, Compliance 및 최종 설계 원칙을 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Governance

---

# 64. Overview

본 장에서는 프로젝트 전체에서 따라야 하는 Best Practices, Anti-Patterns 및 설계 거버넌스를 정의한다.

본 문서는 모든 하위 Specification보다 우선되는 설계 원칙을 제공한다.

---

# 65. Best Practices

다음 사항을 권장한다.

- Specification을 먼저 작성한다.
- Interface를 먼저 정의한다.
- Identity를 기준으로 객체를 식별한다.
- Registry를 Source of Truth로 사용한다.
- Immutable Model을 우선 적용한다.
- Snapshot 기반 처리를 지원한다.
- 결정적(Deterministic) 동작을 유지한다.
- Diagnostics를 구조적으로 제공한다.

---

# 66. Anti-Patterns

다음 구현은 권장하지 않는다.

- 문서 없이 구현을 시작하는 경우
- 전역 상태(Global State)에 의존하는 경우
- Registry를 우회하여 객체를 조회하는 경우
- Mutable Identity
- 순환 의존성(Circular Dependency)
- 숨겨진 상태 변경(Hidden Mutation)
- 동일 개념의 중복 모델 정의
- 구현 세부사항을 Contract에 노출하는 경우

이러한 구현은 장기적인 유지보수성과 확장성을 저하시킨다.

---

# 67. Governance

프로젝트 변경은 다음 절차를 따른다.

```
RFC

↓

ADR

↓

Specification

↓

Contract

↓

Implementation

↓

Verification
```

구현은 승인된 Specification을 기준으로 수행한다.

---

# 68. Documentation Hierarchy

문서 간 우선순위는 다음과 같다.

```
CORE_DESIGN_PRINCIPLES

↓

RFC / ADR

↓

Specification

↓

Contract

↓

Implementation

↓

Source Code
```

상위 문서와 하위 문서가 충돌하는 경우 상위 문서를 우선한다.

---

# 69. Backward Compatibility

공개 Contract와 Specification은 가능한 한 하위 호환성을 유지한다.

호환성을 깨는 변경은 다음 사항을 함께 제공해야 한다.

- 변경 사유
- 영향 범위
- Migration Strategy
- Deprecation Plan

---

# 70. Evolution Strategy

새로운 기능은 기존 구조를 변경하기보다 확장하는 방식을 우선한다.

권장 방법

- Extension Point 추가
- Optional Field 추가
- 새로운 Specification 추가
- Deprecated 처리

---

# 71. Compliance

모든 구현은 최소한 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- RFC
- ADR
- Core Specifications
- Contract Specifications

Compliance는 구현 검토(Code Review)의 기준이 된다.

---

# 72. Architecture Summary

프로젝트의 핵심 구조는 다음과 같다.

```
RFC / ADR
      │
      ▼
Specification
      │
      ▼
Contract
      │
      ▼
Generator
      │
      ▼
Implementation
      │
      ▼
Runtime
```

모든 데이터는 Registry를 중심으로 관리되며, 모든 객체는 Identity를 통해 식별된다.

---

# 73. Core Principles Summary

프로젝트의 핵심 원칙은 다음과 같다.

- Contract First
- Documentation First
- Identity First
- Registry First
- Interface First
- Deterministic Behaviour
- Immutable Models
- Explicit Dependencies
- Single Source of Truth
- Separation of Concerns

이 원칙은 프로젝트 전체에서 일관되게 적용된다.

---

# 74. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CORE_GLOSSARY
- RFC
- ADR
- SPEC-0013 ~ SPEC-0018
- Contract Specifications
- Implementation Documents

---

# 75. Document Summary

CORE_DESIGN_PRINCIPLES는 VSkript Database 프로젝트의 최상위 설계 원칙을 정의한다.

모든 Specification, Contract, Generator 및 Implementation은 본 문서의 원칙을 기반으로 작성되어야 하며, 프로젝트 전반에서 일관된 구조와 예측 가능한 동작을 보장해야 한다.

---

# 76. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Core Design Principles |