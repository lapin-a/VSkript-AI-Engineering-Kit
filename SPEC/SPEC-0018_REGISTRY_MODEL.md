# SPEC-0018
# Registry Model

Version: 1.0

Status: Draft

---

# Part 1. Registry Fundamentals

---

# 1. Abstract

Registry는 VSkript Database에서 모든 식별 가능한 객체를 저장, 관리 및 조회하는 중앙 데이터 모델이다.

Registry는 객체의 Source of Truth 역할을 수행하며, Identity를 기준으로 객체를 관리한다.

본 Specification은 Registry의 구조, 책임 및 동작 원칙을 정의한다.

---

# 2. Motivation

프로젝트에는 다양한 객체가 존재한다.

예시

- Types
- Events
- Effects
- Expressions
- Providers
- Symbols
- Pipelines
- Artifacts

이 객체들을 일관된 방식으로 관리하기 위해 Registry가 필요하다.

---

# 3. Goals

Registry는 다음 목표를 가진다.

- Single Source of Truth
- Stable Lookup
- Deterministic Enumeration
- Referential Integrity
- Efficient Query
- Extensible Storage

---

# 4. Non Goals

Registry는 다음 역할을 수행하지 않는다.

- Cache
- Pipeline Execution
- Business Logic
- Rendering
- UI State

Registry는 객체를 실행하지 않으며 저장과 조회만 담당한다.

---

# 5. Registry Definition

Registry는 Identity를 기준으로 객체를 저장하는 중앙 저장소이다.

```
Object

↓

Identity

↓

Registry

↓

Lookup
```

Registry는 프로젝트의 공식 데이터 저장소이다.

---

# 6. Registry Hierarchy

프로젝트에서 사용하는 Registry의 예시는 다음과 같다.

```
Registry

├── Type Registry
├── Event Registry
├── Effect Registry
├── Expression Registry
├── Provider Registry
├── Symbol Registry
└── Artifact Registry
```

모든 Registry는 본 Specification을 따른다.

---

# 7. Core Components

모든 Registry는 다음 요소를 가진다.

```
Registry

├── Registry ID
├── Entries
├── Identity Index
├── Metadata
└── Version
```

Registry는 Entry의 집합으로 구성된다.

---

# 8. Registry Identity

모든 Registry는 고유한 Registry ID를 가진다.

예시

```
type-registry

event-registry

provider-registry
```

Registry ID는 생성 이후 변경되지 않는다.

---

# 9. Registry Scope

Registry는 관리 범위를 가진다.

대표적인 Scope

- Global
- Workspace
- Project
- Session

Scope는 Registry의 저장 대상과 Lifetime을 결정한다.

---

# 10. Registry Entry

Registry Entry는 Registry의 최소 저장 단위이다.

```
Registry Entry

├── Identity
├── Object
├── Metadata
└── State
```

Entry는 Identity를 기준으로 관리된다.

---

# 11. Identity Index

Registry는 Identity Index를 유지해야 한다.

Identity Index는 다음 기능을 제공한다.

- Fast Lookup
- Duplicate Detection
- Referential Integrity

Identity Index는 Registry의 핵심 인덱스이다.

---

# 12. Metadata

Registry는 다음 Metadata를 포함할 수 있다.

- Created Time
- Updated Time
- Source
- Provider
- Tags

Metadata는 객체 자체를 변경하지 않는다.

---

# 13. Registry Lifetime

Registry는 Scope에 따라 Lifetime을 가진다.

예시

- Session Registry
- Workspace Registry
- Global Registry

Lifetime 종료 시 Registry는 Dispose될 수 있다.

---

# 14. Registry State

Registry는 다음 상태를 가질 수 있다.

- Initializing
- Ready
- Updating
- Disposed

상태는 Registry의 사용 가능 여부를 나타낸다.

---

# 15. Part Summary

Part 1에서는 Registry의 기본 개념과 공통 구조를 정의하였다.

Registry는 Identity를 기준으로 객체를 저장하고 관리하는 중앙 데이터 모델이며, 프로젝트 전체에서 Source of Truth 역할을 수행한다.

Part 2에서는 Registry Entry, Lookup, Registration 및 Enumeration 규칙을 정의한다.

---

# Part 2. Registry Structure & Lookup Model

---

# 16. Overview

Registry는 Registry Entry의 집합으로 구성된다.

모든 Entry는 Identity를 기준으로 관리되며, Lookup, Registration 및 Enumeration은 Identity를 중심으로 수행된다.

---

# 17. Registry Structure

모든 Registry는 다음 논리 구조를 가진다.

```
Registry

├── Registry ID
├── Entries
├── Identity Index
├── Metadata
├── Statistics
└── Version
```

Registry의 내부 저장 방식은 구현체가 결정한다.

---

# 18. Registry Entry

Registry Entry는 Registry의 최소 관리 단위이다.

```
Registry Entry

├── Identity
├── Object
├── State
├── Created At
├── Updated At
└── Metadata
```

Entry는 독립적으로 관리된다.

---

# 19. Registration

객체는 Registry에 등록(Register)되어야 조회할 수 있다.

```
Object

↓

Identity

↓

Registry Entry

↓

Registry
```

Registration은 Identity를 기준으로 수행한다.

---

# 20. Registration Rules

Registration은 다음 조건을 만족해야 한다.

- Identity가 존재해야 한다.
- 동일 Identity가 이미 존재해서는 안 된다.
- Identity Type이 Registry와 일치해야 한다.
- Entry는 Valid 상태여야 한다.

조건을 만족하지 않으면 Registration은 실패한다.

---

# 21. Lookup Model

Lookup은 Identity를 기준으로 수행한다.

```
Identity

↓

Identity Index

↓

Registry Entry

↓

Object
```

Lookup은 Registry의 핵심 기능이다.

---

# 22. Lookup Result

Lookup은 다음 결과를 반환할 수 있다.

- Success
- Not Found
- Invalid Identity
- Invalid State

Lookup 결과는 객체를 변경하지 않는다.

---

# 23. Enumeration

Registry는 저장된 Entry를 열거(Enumerate)할 수 있다.

Enumeration은 다음 기준으로 수행할 수 있다.

- Identity Type
- Namespace
- Scope
- State

Enumeration 순서는 결정적(Deterministic)이어야 한다.

---

# 24. Query Model

Registry는 다양한 조회(Query)를 지원할 수 있다.

예시

- Lookup by Identity
- Lookup by Namespace
- Lookup by Type
- Lookup by Metadata

Query는 Registry 내용을 변경하지 않는다.

---

# 25. Duplicate Detection

Registry는 Registration 시 중복 Identity를 감지해야 한다.

동일 Scope와 Namespace에서 동일 Identity가 존재하면 Registration을 거부해야 한다.

---

# 26. Update Model

객체의 상태가 변경되는 경우 Registry Entry를 갱신할 수 있다.

Update는 Identity를 유지한 상태에서 Object 또는 Metadata를 최신 상태로 반영한다.

Update는 Entry의 Identity를 변경해서는 안 된다.

---

# 27. Removal

객체가 더 이상 유효하지 않은 경우 Registry에서 제거(Unregister)할 수 있다.

```
Registry Entry

↓

Unregister

↓

Removed
```

제거 이후 Lookup은 Success를 반환해서는 안 된다.

---

# 28. Registry State Management

Registry는 Entry의 상태를 관리한다.

대표적인 상태

- Valid
- Invalid
- Deprecated
- Removed

상태는 Entry의 조회 가능 여부를 결정한다.

---

# 29. Statistics

Registry는 다음 통계를 기록할 수 있다.

- Entry Count
- Lookup Count
- Registration Count
- Removal Count
- Update Count

Statistics는 Registry의 논리적 동작에 영향을 주지 않는다.

---

# 30. Thread Safety

동시 접근을 지원하는 Registry는 다음 연산의 안전성을 보장해야 한다.

- Register
- Lookup
- Update
- Unregister
- Enumerate

Thread Safety는 구현체의 책임이다.

---

# 31. Stable Enumeration

동일한 Registry Snapshot에서는 Enumeration 결과가 항상 동일한 순서를 가져야 한다.

Enumeration 순서는 구현 환경에 따라 달라져서는 안 된다.

---

# 32. Storage Independence

Registry는 특정 저장 방식에 의존하지 않는다.

예시

- In-memory
- Persistent Store
- Database
- Remote Service

Specification은 Registry의 논리적 계약만 정의한다.

---

# 33. Part Summary

Part 2에서는 Registry Entry, Registration, Lookup 및 Enumeration 모델을 정의하였다.

Registry는 Identity를 기준으로 객체를 등록하고 조회하며, 결정적인 Lookup과 Enumeration을 제공한다.

Part 3에서는 Registry Lifecycle, Synchronization, Snapshot 및 변경 관리 모델을 정의한다.

---

# Part 3. Registry Lifecycle & Change Management

---

# 34. Overview

Registry는 명확한 Lifecycle을 가지며, 객체의 등록, 변경 및 제거 과정을 일관되게 관리한다.

Registry는 변경 사항을 추적하고 Snapshot 기반 실행 모델과 연계되어야 한다.

---

# 35. Registry Lifecycle

모든 Registry는 다음 Lifecycle을 따른다.

```
Create

↓

Initialize

↓

Ready

↓

Update

↓

Snapshot

↓

Dispose
```

Registry는 Ready 상태가 되기 전까지 Lookup을 제공해서는 안 된다.

---

# 36. Initialization

Registry 초기화 시 수행되는 작업은 다음과 같다.

- Registry ID 생성
- Identity Index 초기화
- Metadata 초기화
- Statistics 초기화
- Entry Collection 생성

초기화 완료 후 Registry는 Ready 상태가 된다.

---

# 37. Change Model

Registry 변경(Change)은 Entry 단위로 관리한다.

대표적인 변경 유형

- Register
- Update
- Unregister
- Deprecate
- Restore

모든 변경은 Identity를 기준으로 수행한다.

---

# 38. Change Detection

Registry는 변경 사항을 감지할 수 있어야 한다.

대표적인 변경 원인

- Source File 변경
- Provider 변경
- Configuration 변경
- Plugin 변경
- Manual Registration

변경 정보는 Snapshot 생성의 기준이 된다.

---

# 39. Snapshot Integration

Registry는 Snapshot 생성과 연계된다.

```
Registry

↓

Snapshot

↓

Pipeline

↓

Cache
```

Snapshot은 특정 시점의 Registry 상태를 나타낸다.

---

# 40. Snapshot Consistency

Snapshot은 생성 시점의 Registry 상태를 변경 없이 유지해야 한다.

Snapshot 생성 이후 Registry가 변경되더라도 기존 Snapshot은 영향을 받지 않는다.

---

# 41. Synchronization

여러 Registry가 존재하는 경우 동기화가 필요할 수 있다.

예시

- Global Registry
- Workspace Registry
- Session Registry

동기화 정책은 구현체가 정의한다.

---

# 42. Incremental Update

Registry는 변경된 Entry만 갱신할 수 있다.

```
Registry

↓

Changed Entries

↓

Update

↓

New Registry State
```

변경되지 않은 Entry는 그대로 유지한다.

---

# 43. Event Model

Registry는 다음 이벤트를 발생시킬 수 있다.

- Entry Registered
- Entry Updated
- Entry Removed
- Registry Initialized
- Snapshot Created

이벤트는 Monitoring 및 Diagnostics를 위한 목적으로 사용한다.

---

# 44. Consistency Rules

Registry는 항상 다음 사항을 보장해야 한다.

- Identity Index와 Entry Collection은 일치해야 한다.
- Invalid Entry는 Lookup 대상이 아니다.
- Snapshot은 불변(Immutable)이다.
- 동일 Snapshot에서는 동일 Lookup 결과를 반환한다.

---

# 45. Recovery

Registry는 오류 발생 시 복구를 지원할 수 있다.

예시

- Snapshot Rollback
- Entry Rebuild
- Full Reinitialization

복구 정책은 Registry 구현체가 정의한다.

---

# 46. Resource Management

Registry 구현은 메모리 및 리소스를 관리해야 한다.

권장 사항

- 불필요한 Entry 제거
- Deprecated Entry 정리
- Snapshot 정리
- Metadata 최적화

리소스 관리는 Registry의 논리적 상태를 변경해서는 안 된다.

---

# 47. Disposal

Registry가 더 이상 필요하지 않은 경우 다음 순서로 종료한다.

```
Stop Updates

↓

Release Resources

↓

Dispose
```

Dispose 이후 Registry는 사용할 수 없다.

---

# 48. Lifecycle Guarantees

Registry는 다음 사항을 보장해야 한다.

- Ready 이전에는 Lookup을 허용하지 않는다.
- Snapshot은 Immutable이다.
- Incremental Update는 Identity를 유지한다.
- Dispose 이후 Entry를 반환하지 않는다.

---

# 49. Relationship with Other Models

Registry Lifecycle은 다른 Core Model과 연계된다.

```
Identity

↓

Registry

↓

Snapshot

↓

Pipeline

↓

Cache
```

Registry 변경은 Snapshot과 Pipeline 실행의 기준이 된다.

---

# 50. Part Summary

Part 3에서는 Registry Lifecycle과 변경 관리 모델을 정의하였다.

Registry는 객체의 등록부터 제거까지의 전 과정을 관리하며, Snapshot과 Incremental Update를 통해 안정적이고 효율적인 데이터 관리 모델을 제공한다.

Part 4에서는 Registry Resolution, Integration Model 및 다른 Core Model과의 상호작용을 정의한다.

---

# Part 4. Registry Resolution & Integration Model

---

# 51. Overview

Registry는 프로젝트의 모든 식별 가능한 객체를 조회하기 위한 공통 Resolution 모델을 제공한다.

모든 Resolution은 Registry를 기준으로 수행되며, 다른 Core Model은 Registry를 통해 객체를 획득한다.

---

# 52. Resolution Model

Registry Resolution은 다음 절차를 따른다.

```
Identity

↓

Identity Index

↓

Registry Entry

↓

Object
```

Registry는 Identity를 실제 객체로 변환하는 공식 Lookup 경로를 제공한다.

---

# 53. Resolution Rules

Resolution은 다음 조건을 만족해야 한다.

- Identity가 존재해야 한다.
- Registry Entry가 존재해야 한다.
- Entry가 Valid 상태여야 한다.
- Identity Type이 Registry와 일치해야 한다.

조건을 만족하지 않으면 Resolution은 실패한다.

---

# 54. Resolution Results

Resolution은 다음 결과 중 하나를 반환한다.

- Success
- Not Found
- Invalid Identity
- Invalid Entry
- Registry Error

결과는 Registry 상태를 변경하지 않는다.

---

# 55. Identity Integration

Registry는 Identity를 기본 인덱스로 사용한다.

```
Identity

↓

Registry

↓

Object
```

Identity 없는 객체는 Registry에 등록해서는 안 된다.

---

# 56. Graph Integration

Graph는 Registry를 통해 Node를 획득한다.

```
Graph

↓

Identity

↓

Registry

↓

Node
```

Graph는 Registry를 우회하여 객체를 생성하거나 저장해서는 안 된다.

---

# 57. Context Integration

Execution Context는 Registry를 통해 필요한 객체를 조회한다.

Context는 객체의 저장소가 아니며 Registry의 조회 결과를 사용한다.

---

# 58. Pipeline Integration

Pipeline는 Execution Plan에 Identity를 저장할 수 있다.

실행 시 Registry Resolution을 통해 실제 객체를 획득한다.

Pipeline는 Registry의 저장 내용을 직접 변경해서는 안 된다.

---

# 59. Cache Integration

Cache는 Registry Lookup을 최적화하기 위한 계층이다.

```
Identity

↓

Cache

↓

Hit

↓

Object

or

↓

Registry

↓

Object
```

Cache는 Registry를 대체하지 않는다.

---

# 60. Provider Integration

Provider는 Registry에 객체를 등록할 수 있다.

대표적인 등록 대상

- Type
- Event
- Effect
- Expression
- Symbol

Provider는 Registry Entry를 직접 수정해서는 안 된다.

---

# 61. Generator Integration

Generator는 Registry를 참조하여 Artifact를 생성한다.

Generator는 Registry를 Source of Truth로 사용해야 한다.

---

# 62. Language Server Integration

Language Server는 Registry를 기반으로 다음 기능을 제공한다.

- Completion
- Hover
- Definition
- References
- Symbols

Language Server는 Registry를 조회하며 Registry의 구조를 변경하지 않는다.

---

# 63. Diagnostics Integration

Registry 관련 오류는 Diagnostic으로 보고할 수 있다.

예시

- Duplicate Identity
- Missing Entry
- Invalid Registration
- Broken Reference

Diagnostics는 Registry 상태를 변경하지 않는다.

---

# 64. Extension Model

새로운 Registry 구현은 본 Specification을 확장하여 정의한다.

예시

```
Registry

├── Built-in Registry
├── Workspace Registry
├── Plugin Registry
└── Custom Registry
```

확장 Registry도 동일한 Resolution 모델을 따라야 한다.

---

# 65. Part Summary

Part 4에서는 Registry Resolution과 다른 Core Model과의 통합 방식을 정의하였다.

Registry는 프로젝트의 공식 Lookup 모델을 제공하며, Graph, Context, Pipeline, Cache, Generator 및 Language Server는 모두 Registry를 기반으로 객체를 조회한다.

Part 5에서는 Design Principles, Best Practices, Compliance 및 Reference Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Design Principles

---

# 66. Overview

본 장에서는 모든 Registry 구현이 따라야 하는 설계 원칙과 구현 지침을 정의한다.

Registry는 프로젝트의 공식 Source of Truth이며, 모든 구현은 동일한 원칙을 준수해야 한다.

---

# 67. Design Principles

모든 Registry는 다음 원칙을 따른다.

- Single Source of Truth
- Identity-based Storage
- Deterministic Lookup
- Immutable Identity
- Explicit Registration
- Stable Enumeration
- Snapshot Compatibility
- Observable State

Registry는 객체를 저장하고 조회하는 역할만 수행하며, 비즈니스 로직을 포함하지 않는다.

---

# 68. Best Practices

권장 사항

- 모든 객체는 Identity를 기준으로 등록한다.
- Registry를 유일한 Lookup 지점으로 사용한다.
- Registration과 Update를 명확히 구분한다.
- Snapshot 기반으로 변경 사항을 관리한다.
- Incremental Update를 우선 적용한다.
- Enumeration 순서를 안정적으로 유지한다.
- Registry Statistics를 지속적으로 기록한다.
- Registry 구현은 확장 가능하도록 설계한다.

---

# 69. Anti-Patterns

다음 구현은 권장하지 않는다.

- Registry를 Cache처럼 사용하는 구현
- Identity 없이 객체를 저장하는 구현
- Registry를 우회한 객체 접근
- Lookup 중 Registry 수정
- Entry를 직접 변경하는 구현
- Enumeration 순서가 실행마다 달라지는 구현
- Registry를 여러 Source of Truth로 분리하는 구현
- Registration 시 중복 검사를 생략하는 구현

이러한 구현은 프로젝트의 일관성과 참조 무결성을 저하시킨다.

---

# 70. Registry Guarantees

모든 Registry는 다음 사항을 보장해야 한다.

- 동일 Identity는 동일 객체를 반환한다.
- Identity Index와 Entry Collection은 항상 일치한다.
- 동일 Snapshot에서는 동일한 Lookup 결과를 반환한다.
- Enumeration은 결정적(Deterministic)이다.
- Registration과 Lookup은 참조 무결성을 유지한다.

---

# 71. Relationship with Other Models

Registry는 Core Specification의 중심 데이터 모델이다.

```
Identity
     │
     ▼
Registry
     │
     ├── Graph
     ├── Context
     ├── Pipeline
     ├── Cache
     ├── Generator
     └── Language Server
```

모든 Core Model은 Registry를 통해 객체를 조회한다.

---

# 72. Compliance

모든 Registry 구현은 다음 Specification을 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- SPEC-0013 Context Model
- SPEC-0014 Graph Model
- SPEC-0015 Pipeline Model
- SPEC-0016 Cache Model
- SPEC-0017 Identity Model

Registry는 프로젝트 전반에서 공식 Source of Truth 역할을 수행해야 한다.

---

# 73. Reference Architecture

Registry의 참조 구조는 다음과 같다.

```
Provider

↓

Identity

↓

Registry

↓

Resolver

↓

Object

↓

Consumer
```

Provider는 객체를 등록하고, Consumer는 Registry를 통해 객체를 조회한다.

---

# 74. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- SPEC-0013 Context Model
- SPEC-0014 Graph Model
- SPEC-0015 Pipeline Model
- SPEC-0016 Cache Model
- SPEC-0017 Identity Model

RFC 및 Implementation 문서는 Registry를 정의할 때 본 Specification을 참조한다.

---

# 75. Document Summary

Registry는 VSkript Database에서 모든 객체를 저장하고 관리하는 공통 데이터 모델이다.

Registry는 Identity를 기준으로 객체를 등록하고 조회하며, 프로젝트 전체의 Source of Truth 역할을 수행한다.

Graph, Context, Pipeline, Cache, Generator 및 Language Server를 포함한 모든 구성 요소는 Registry를 통해 객체를 조회하며, Registry의 일관성과 참조 무결성을 유지해야 한다.

모든 Registry 구현은 본 Specification을 준수해야 한다.

---

# 76. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Registry Model Specification |