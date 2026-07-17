# SPEC-0013
# Context Model

Version: 1.0

Status: Draft

---

# 1. Abstract

Context는 컴포넌트가 작업을 수행하는 동안 필요한 실행 정보를 캡슐화하는 불변 객체(Immutable Object)이다.

Context는 실행 상태(State)가 아니라 실행 환경(Execution Environment)을 표현한다.

모든 주요 컴포넌트는 Context를 기반으로 동작한다.

---

# 2. Motivation

프로젝트는 여러 실행 단계를 가진다.

예시

- Build
- Validation
- Generation
- Language Server
- Registry

각 단계는 서로 다른 정보를 필요로 하지만,

공통적으로

- 실행 환경
- Registry
- Configuration
- Snapshot

등을 공유한다.

이를 표준화하기 위해 Context Model을 정의한다.

---

# 3. Goals

Context는 다음 목표를 가진다.

- Immutable
- Serializable (Optional)
- Thread-safe
- Snapshot-based
- Explicit Ownership
- Deterministic

---

# 4. Non Goals

Context는 다음 역할을 수행하지 않는다.

- Business Logic
- Data Storage
- Mutable State
- Global Singleton
- Cache

---

# 5. Context Definition

Context는 특정 작업(Task)을 수행하기 위해 필요한 실행 환경의 집합이다.

Context는 실행 중 변경되지 않는다.

---

# 6. Context Hierarchy

```
Context

├── BuildContext
├── ValidationContext
├── GenerationContext
├── ExecutionContext
├── RegistryContext
├── WorkspaceContext
└── PluginContext
```

모든 Context는 동일한 설계 원칙을 따른다.

---

# 7. Core Characteristics

모든 Context는 다음 특성을 가진다.

- Immutable
- Thread-safe
- Read-only
- Explicit
- Deterministic

---

# 8. Context Ownership

각 Context는 하나의 Owner를 가진다.

예시

```
BuildContext

↓

Builder
```

```
ExecutionContext

↓

Language Server
```

Owner만 Context를 생성할 수 있다.

---

# 9. Context Lifecycle

모든 Context는 다음 생명주기를 따른다.

```
Create

↓

Initialize

↓

Freeze

↓

Use

↓

Dispose
```

Freeze 이후에는 변경할 수 없다.

---

# 10. Context Scope

Context는 실행 단위(Task Scope)에만 유효하다.

Context를 장기 저장소로 사용해서는 안 된다.

---

# 11. Immutable Rule

모든 필드는 읽기 전용이어야 한다.

Context 내부 객체 역시 Immutable이거나 Snapshot이어야 한다.

Mutable Collection은 허용하지 않는다.

---

# 12. Snapshot Rule

Context는 Snapshot만 참조한다.

예시

```
ExecutionContext

↓

Registry Snapshot
```

실행 중 Snapshot은 변경되지 않는다.

---

# 13. Thread Safety

Context는 여러 스레드에서 동시에 사용할 수 있어야 한다.

Lock을 요구해서는 안 된다.

---

# 14. Identity

각 Context는 고유한 Context ID를 가질 수 있다.

예시

```
build-001

validation-001

execution-001
```

Identity는 추적과 로깅에 사용된다.

---

# 15. Part Summary

Part 1에서는 Context의 기본 개념과 설계 목표를 정의하였다.

Context는 실행 환경을 표현하는 Immutable 객체이며, 모든 컴포넌트는 Context를 통해 필요한 정보를 전달받는다.

Part 2에서는 Context의 내부 구성 요소와 공통 필드를 정의한다.

---

# Part 2. Context Structure

---

# 16. Overview

모든 Context는 공통 구조(Common Structure)를 가진다.

컴포넌트마다 세부 필드는 달라질 수 있지만,
핵심 구조는 동일해야 한다.

Context는 실행 환경을 표현하는 객체이며,
실행 중 변경되지 않는다.

---

# 17. Common Structure

모든 Context는 다음과 같은 구조를 따른다.

```
Context

├── Context ID
├── Context Type
├── Version
├── Owner
├── Configuration
├── Registry Snapshot
├── Metadata
├── Services
├── Logger
├── Metrics
└── Cancellation Token (Optional)
```

각 구성 요소는 명확한 책임을 가진다.

---

# 18. Context ID

Context는 고유한 식별자를 가질 수 있다.

예시

```
build-001

validation-001

generation-001

execution-001
```

Context ID는 추적과 진단 목적으로 사용한다.

---

# 19. Context Type

Context Type은 Context의 종류를 나타낸다.

예시

```
Build

Validation

Generation

Execution

Registry
```

Context Type은 변경되지 않는다.

---

# 20. Version

모든 Context는 Version 정보를 포함한다.

예시

- Schema Version
- Context Version
- Build Version

Version은 호환성 검증에 사용된다.

---

# 21. Owner

Owner는 Context를 생성하고 관리하는 컴포넌트이다.

예시

```
Builder

Validator

Generator

Language Server
```

Owner만 Context를 생성할 수 있다.

---

# 22. Configuration

Configuration은 실행 시점의 설정 Snapshot이다.

예시

- Build Options
- Validation Options
- Generator Options
- Language Server Settings

Configuration은 읽기 전용이다.

---

# 23. Registry Snapshot

모든 Context는 Registry Snapshot을 참조할 수 있다.

```
Context

↓

Registry Snapshot
```

Registry는 직접 수정할 수 없다.

---

# 24. Metadata

Metadata는 실행 정보를 저장한다.

예시

- Timestamp
- Project Name
- Workspace
- Repository
- Plugin Set

Metadata는 Context 생성 시 확정된다.

---

# 25. Services

Context는 필요한 서비스를 참조할 수 있다.

예시

```
Services

├── Symbol Resolver
├── Type Resolver
├── Documentation Resolver
├── Formatting Service
└── Diagnostic Service
```

Services는 인터페이스를 통해 제공해야 하며,
구체적인 구현에 직접 의존해서는 안 된다.

---

# 26. Logger

Context는 Logger를 포함할 수 있다.

Logger는 다음 용도로 사용한다.

- Debug
- Trace
- Warning
- Error

Logger는 실행 상태를 변경하지 않는다.

---

# 27. Metrics

Metrics는 실행 통계를 수집한다.

예시

- Execution Time
- Memory Usage
- Lookup Count
- Cache Hit Ratio

Metrics는 선택적으로 제공할 수 있다.

---

# 28. Cancellation Token

장시간 실행되는 작업은 Cancellation Token을 포함할 수 있다.

예시

```
Execution Context

↓

Cancellation Token
```

Cancellation은 협력적(Cooperative) 방식으로 처리한다.

---

# 29. Optional Fields

모든 Context가 동일한 필드를 가질 필요는 없다.

예를 들어

- BuildContext는 Cancellation Token이 필요하지 않을 수 있다.
- ExecutionContext는 Workspace 정보가 필요하다.
- RegistryContext는 Execution Position이 필요하지 않다.

공통 구조를 유지하면서 필요한 필드만 확장한다.

---

# 30. Extension Rule

새로운 Context는 공통 구조를 유지해야 한다.

확장은 허용되지만,
기존 의미를 변경해서는 안 된다.

새로운 공통 필드가 필요한 경우에는
본 SPEC을 먼저 수정한다.

---

# 31. Context Relationships

Context는 서로 직접 포함하지 않는다.

예시

```
ExecutionContext

↓

Registry Snapshot

↓

Metadata
```

BuildContext가 ExecutionContext를 참조하거나,
ValidationContext를 포함해서는 안 된다.

Context 간 관계는 느슨하게 유지한다.

---

# 32. Dependency Rules

Context는 다음 계층만 참조할 수 있다.

```
Configuration

↓

Registry Snapshot

↓

Services
```

상위 Context나 다른 Context 구현체를 참조해서는 안 된다.

---

# 33. Validation Rules

Context 생성 시 다음 항목을 검증한다.

- Required Fields
- Version Compatibility
- Snapshot Availability
- Owner Consistency

검증 실패 시 Context를 생성하지 않는다.

---

# 34. Part Summary

Part 2에서는 모든 Context가 공유하는 공통 구조와 필드를 정의하였다.

모든 Context는 동일한 기반 구조를 유지하며, Registry Snapshot과 Configuration을 중심으로 실행 환경을 구성한다.

Part 3에서는 Context의 생명주기, 생성 규칙, 불변성 및 동시성 모델을 정의한다.

---

# Part 3. Lifecycle, Immutability & Concurrency

---

# 35. Overview

Context는 생성(Create) 이후 변경되지 않는 실행 환경이다.

모든 Context는 동일한 생명주기와 동시성 모델을 따른다.

---

# 36. Lifecycle

Context는 다음 생명주기를 가진다.

```
Create

↓

Initialize

↓

Validate

↓

Freeze

↓

Use

↓

Dispose
```

Freeze 이후 Context는 Immutable이 된다.

---

# 37. Create Phase

Owner는 Context를 생성한다.

예시

```
Builder

↓

BuildContext
```

```
Language Server

↓

ExecutionContext
```

생성 직후에는 아직 사용해서는 안 된다.

---

# 38. Initialize Phase

초기화 단계에서는 Context의 필수 필드를 설정한다.

예시

- Configuration
- Registry Snapshot
- Metadata
- Services

초기화 이후 Validate를 수행한다.

---

# 39. Validation Phase

Freeze 이전 Context를 검증한다.

검증 항목

- Required Fields
- Snapshot Availability
- Version Compatibility
- Owner Consistency

검증 실패 시 Context는 폐기한다.

---

# 40. Freeze Phase

Freeze는 Context를 Immutable 상태로 전환한다.

Freeze 이후에는

- 필드 추가
- 필드 변경
- Collection 변경

모두 금지된다.

Freeze는 단 한 번만 수행할 수 있다.

---

# 41. Use Phase

Freeze 이후 Context는 여러 컴포넌트에서 동시에 사용할 수 있다.

예시

```
Registry Snapshot

↓

ExecutionContext

↓

Providers
```

Use 단계에서는 읽기만 허용된다.

---

# 42. Dispose Phase

Context 사용이 끝나면 Dispose한다.

Dispose는 다음 작업을 수행할 수 있다.

- Metrics Flush
- Resource Release
- Logger Finalization

Dispose는 Context의 논리적 내용을 변경하지 않는다.

---

# 43. Immutable Model

모든 Context는 Immutable이다.

허용

- Read
- Share
- Pass

금지

- Update
- Replace
- Remove
- Append

---

# 44. Deep Immutability

Context뿐 아니라 내부 객체도 Immutable이어야 한다.

예시

```
ExecutionContext

↓

Registry Snapshot

↓

Resources
```

Snapshot 내부 객체도 변경해서는 안 된다.

---

# 45. Thread Safety

Freeze된 Context는 여러 스레드에서 동시에 사용할 수 있다.

Lock을 요구하지 않는다.

Thread Safety는 Immutable 구조를 통해 보장한다.

---

# 46. Context Sharing

동일 Context를 여러 컴포넌트가 공유할 수 있다.

예시

```
ExecutionContext

↓

Completion Provider

Hover Provider

Definition Provider
```

공유는 읽기 전용으로 수행한다.

---

# 47. Context Cloning

Context 복제는 권장하지 않는다.

필요한 경우 새로운 Context를 생성한다.

Clone은 Identity를 유지하지 않는다.

---

# 48. Context Replacement

Context는 교체하지 않는다.

새로운 실행이 시작되면

새로운 Context를 생성한다.

```
Execution #1

↓

ExecutionContext #1

Execution #2

↓

ExecutionContext #2
```

---

# 49. Concurrency Model

모든 Context는 Single Creator / Multiple Reader 모델을 따른다.

```
Owner

↓

Create

↓

Freeze

↓

Read Only

↓

Multiple Consumers
```

동시 수정은 허용되지 않는다.

---

# 50. Synchronization Rules

Context 자체는 동기화를 수행하지 않는다.

동기화는 Owner 또는 Scheduler의 책임이다.

Context는 항상 Lock-Free 객체를 목표로 한다.

---

# 51. Lifetime Rules

Context는 Task Scope를 벗어나서는 안 된다.

예시

```
BuildContext

↓

Build Only
```

```
ExecutionContext

↓

Single Request
```

Context를 장기 저장하거나 재사용해서는 안 된다.

---

# 52. Memory Model

Context는 가능한 한 객체를 공유한다.

예시

```
ExecutionContext

↓

Registry Snapshot

↓

Shared Resources
```

불필요한 객체 복사를 금지한다.

---

# 53. Error Handling

Context 생성 중 오류가 발생하면 즉시 생성을 중단한다.

부분적으로 생성된 Context는 사용해서는 안 된다.

---

# 54. Part Summary

Part 3에서는 Context의 생명주기와 Immutable 모델을 정의하였다.

모든 Context는 Create → Initialize → Validate → Freeze → Use → Dispose 순서를 따르며, Freeze 이후에는 여러 컴포넌트가 동시에 안전하게 공유할 수 있다.

Part 4에서는 Context의 확장 모델, 서비스 모델, 직렬화 및 버전 관리 규칙을 정의한다.

---

# Part 4. Extension, Services & Compatibility

---

# 55. Overview

Context는 프로젝트 전반에서 공통으로 사용하는 실행 환경이다.

새로운 Context는 기존 Context를 변경하지 않고 확장(Extension)해야 한다.

공통 구조는 유지하면서 필요한 기능만 추가하는 것을 원칙으로 한다.

---

# 56. Extension Model

새로운 Context는 기존 Context Model을 기반으로 확장한다.

```
Context

├── Common Fields
└── Component-specific Fields
```

공통 필드는 모든 Context에서 동일한 의미를 유지해야 한다.

---

# 57. Context Services

Context는 실행에 필요한 공통 서비스를 참조할 수 있다.

예시

```
Services

├── Symbol Resolver
├── Type Resolver
├── Documentation Resolver
├── Formatting Service
├── Diagnostic Service
└── Configuration Provider
```

Services는 구현 객체가 아니라 인터페이스를 통해 제공한다.

---

# 58. Service Usage Rules

Services 사용 시 다음 규칙을 따른다.

- 필요한 서비스만 노출한다.
- 구현체에 직접 의존하지 않는다.
- 서비스는 실행 중 교체하지 않는다.
- 서비스는 읽기 전용으로 사용한다.

Context를 Service Locator처럼 사용하는 것은 권장하지 않는다.

---

# 59. Context Composition

Context는 다른 Context를 포함하지 않는다.

허용

```
ExecutionContext

↓

Registry Snapshot

↓

Configuration
```

비허용

```
ExecutionContext

↓

BuildContext
```

Context는 서로 독립적으로 유지한다.

---

# 60. Serialization

Context는 필요에 따라 직렬화할 수 있다.

지원 가능 형식

- JSON
- Binary
- MessagePack (Optional)

직렬화는 디버깅, 테스트, 재현(Reproducibility) 목적으로 사용한다.

---

# 61. Serialization Rules

직렬화 시 다음 규칙을 따른다.

- Stable Ordering
- Stable IDs
- Explicit Version
- Immutable Snapshot

런타임 객체나 열린 리소스는 직렬화하지 않는다.

---

# 62. Versioning

모든 Context는 명시적인 Version 정보를 가진다.

예시

- Context Version
- Schema Version
- Registry Version

Version은 생성 이후 변경되지 않는다.

---

# 63. Compatibility

Context를 재사용하거나 복원할 때는 다음 항목을 검증한다.

- Context Version
- Schema Version
- Registry Version
- Required Fields

호환되지 않는 Context는 사용해서는 안 된다.

---

# 64. Context Evolution

Context는 하위 호환성을 고려하여 발전해야 한다.

허용

- Optional Field 추가
- 새로운 Metadata 추가

비권장

- 기존 필드 의미 변경
- Required Field 제거
- Version 없이 구조 변경

---

# 65. Context Metadata

Metadata는 실행과 관련된 부가 정보를 포함한다.

예시

- Project ID
- Workspace ID
- Build ID
- Timestamp
- Execution Mode

Metadata는 실행 결과를 변경해서는 안 된다.

---

# 66. Context Validation

모든 Context는 Freeze 이전에 검증한다.

검증 대상

- Required Fields
- Version Compatibility
- Snapshot Availability
- Service Availability

검증 실패 시 Context를 사용할 수 없다.

---

# 67. Context Factories

Context는 Factory를 통해 생성하는 것을 권장한다.

```
Context Factory

↓

Create

↓

Validate

↓

Freeze

↓

Context
```

Factory는 생성 절차를 표준화한다.

---

# 68. Context Replacement

Context는 수정하지 않고 교체한다.

```
Old Context

↓

Dispose

↓

Create New Context
```

부분 수정(Partial Mutation)은 허용하지 않는다.

---

# 69. Extension Guidelines

새로운 Context를 정의할 때는 다음 절차를 따른다.

1. 공통 구조 유지
2. 필요한 필드만 추가
3. Freeze 규칙 준수
4. Version 정의
5. Validation 규칙 정의

---

# 70. Part Summary

Part 4에서는 Context의 확장 모델과 서비스 모델을 정의하였다.

모든 Context는 공통 구조를 유지하면서 확장되며, Services와 Registry Snapshot을 통해 실행 환경을 제공한다.

Context는 Factory를 통해 생성하고, Version과 Compatibility를 통해 일관성을 유지한다.

Part 5에서는 Best Practices, Anti-Patterns, Design Principles 및 문서 요약을 다룬다.

---

# Part 5. Best Practices, Anti-Patterns & Design Principles

---

# 71. Overview

본 장에서는 모든 Context 구현이 따라야 하는 권장 사항과 금지 사항을 정의한다.

본 규칙은 BuildContext, ValidationContext, GenerationContext, ExecutionContext, RegistryContext 및 향후 추가되는 모든 Context에 동일하게 적용된다.

---

# 72. Design Principles

모든 Context는 다음 원칙을 따른다.

- Immutable by Default
- Explicit Ownership
- Snapshot-based
- Thread-safe
- Deterministic
- Single Responsibility
- Composition over Inheritance
- Version-aware

Context는 실행 환경만 표현하며, 실행 로직을 포함하지 않는다.

---

# 73. Best Practices

권장 사항

- Factory를 통해 생성한다.
- Freeze 이후에는 변경하지 않는다.
- Snapshot만 참조한다.
- 필요한 정보만 포함한다.
- 인터페이스 기반 Services를 사용한다.
- Context를 여러 Consumer와 안전하게 공유한다.
- Version을 명시적으로 관리한다.
- Logger와 Metrics는 부수 효과 없이 사용한다.

---

# 74. Anti-Patterns

다음 구현은 권장하지 않는다.

- Mutable Context
- Global Singleton Context
- Context 내부 Business Logic
- Context 간 직접 참조
- Context를 장기 저장소로 사용
- 런타임 중 Context 수정
- 구현체(Concrete Class)에 직접 의존하는 Services
- Context를 Service Locator처럼 사용하는 설계

이러한 구현은 일관성, 테스트 용이성, 확장성을 저하시킨다.

---

# 75. Ownership Rules

각 Context는 하나의 Owner만 가진다.

예시

```
BuildContext
    ↓
Builder

ExecutionContext
    ↓
Language Server

GenerationContext
    ↓
Generator
```

Owner 외부에서 Context를 수정하거나 재초기화해서는 안 된다.

---

# 76. Context Boundaries

Context는 자신의 실행 범위를 벗어나지 않는다.

예시

```
BuildContext
    ↓
Build Pipeline

ExecutionContext
    ↓
Single LSP Request
```

Context를 다른 실행 단계에서 재사용해서는 안 된다.

---

# 77. Context Evolution Policy

Context를 변경할 때는 다음 규칙을 따른다.

허용

- Optional Field 추가
- Metadata 추가
- 새로운 Service Interface 추가

비허용

- Required Field 제거
- 기존 필드 의미 변경
- Version 증가 없이 구조 변경

모든 구조 변경은 버전 정책을 따라야 한다.

---

# 78. Compliance

모든 Context 구현은 다음 항목을 만족해야 한다.

- Immutable
- Thread-safe
- Snapshot-based
- Freeze Lifecycle
- Explicit Ownership
- Context Factory 사용
- Validation 수행

구현체는 본 SPEC을 준수해야 한다.

---

# 79. Reference Lifecycle

모든 Context는 다음 참조 생명주기를 따른다.

```
Factory
   │
   ▼
Create
   ▼
Initialize
   ▼
Validate
   ▼
Freeze
   ▼
Read
   ▼
Dispose
```

이 순서는 변경하지 않는다.

---

# 80. Reference Architecture

프로젝트에서 Context는 다음 위치에 존재한다.

```
Identity
    │
    ▼
Context
    │
    ▼
Registry
    │
    ▼
Graph
    │
    ▼
Planner
    │
    ▼
Pipeline
    │
    ▼
Component
```

Context는 모든 실행 컴포넌트의 공통 기반이다.

---

# 81. Cross References

본 문서는 다음 Specification과 함께 사용한다.

- SPEC-0014 Graph Model
- SPEC-0015 Pipeline Model
- SPEC-0016 Cache Model
- SPEC-0017 Identity Model
- SPEC-0018 Registry Model

Implementation 문서는 본 문서를 참조하여 Context를 정의한다.

---

# 82. Document Summary

Context는 VSkript Database에서 실행 환경을 표현하는 핵심 모델이다.

모든 Context는 Factory를 통해 생성되고, Validate와 Freeze를 거쳐 Immutable 상태로 전환된다.

Context는 Snapshot 기반으로 동작하며, Thread-safe한 공유를 지원한다.

Builder, Validator, Generator, Registry, Language Server를 포함한 모든 실행 컴포넌트는 본 Specification을 준수해야 한다.

---

# 83. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Context Model Specification |