# Effect Contract

Version: 1.0

Status: Draft

---

# Part 1. Effect Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Effect Contract를 정의한다.

Effect Contract는 Runtime에서 수행되는 실행 단위(Executable Action)를 Canonical Model로 표현한다.

Generator는 Effect Contract를 기반으로 다양한 Runtime Artifact를 생성한다.

---

# 2. Scope

본 Contract는 다음 대상에 적용된다.

- Runtime Effect
- Script Effect
- Built-in Effect
- Extension Effect
- Custom Effect

Effect 실행 방식은 별도의 Specification에서 정의한다.

---

# 3. Definition

Effect는 Runtime에서 하나의 동작(Action)을 수행하는 실행 단위이다.

Effect Contract는 실행 가능한 구조를 정의하며 Runtime 구현은 포함하지 않는다.

---

# 4. Design Goals

Effect Contract는 다음 목표를 가진다.

- Stable
- Deterministic
- Reusable
- Language Independent
- Generator Friendly

---

# 5. Canonical Representation

동일한 Effect는 하나의 Effect Contract만 가진다.

```
Runtime Effect

↓

Effect Contract
```

Generator는 Effect Contract를 공식 모델로 사용한다.

---

# 6. Identity

모든 Effect는 고유한 Identity를 가진다.

Identity는 다음 조건을 만족해야 한다.

- Immutable
- Globally Unique
- Stable

Identity는 Name만으로 결정되지 않는다.

---

# 7. Ownership

Effect는 Registry에서 관리된다.

```
Registry

└── Effect Contract
```

다른 Contract는 Effect를 참조할 수 있지만 소유자는 Registry이다.

---

# 8. Canonical Composition

The Canonical Composition defined in this section is the single
authoritative structural definition of the Effect Contract.

Generators, Validators, Serializers, Documentation Generators,
and all downstream tools SHALL derive the Effect structure from
this Canonical Composition.

Effect는 다음 요소로 구성된다.

Effect

├── Identity
├── Name
├── Namespace
├── Parameters
├── Result
├── Context
├── Attributes
├── Documentation
└── Metadata

Generator는 위 구조를 변경해서는 안 된다.

---

# 9. Parameters

Effect는 0개 이상의 Parameter를 가진다.

모든 Parameter는 CONTRACT_PARAMETER를 참조한다.

```
Effect

└── Parameter[]
```

Parameter 선언 순서는 의미를 가진다.

---

# 10. Result

Effect는 결과(Result)를 반환하지 않는 것을 기본 정책(Default Policy)으로 한다.

Result는 Effect Contract의 Canonical Structure에 포함되지만,
기본 정책에서는 Void Result를 사용한다.

필요한 경우 별도의 Specification에서 반환 모델(Return Model)을 정의할 수 있다.

Generator는 기본 정책을 적용해야 한다.

---

# 11. Execution Context

Execution Context는 Effect가 수행되는 환경을 설명한다.

예시

- Runtime
- World
- Session
- Thread
- Tick

Execution Context는 Runtime 구현이 아니라 Effect의 의미를 설명하는 메타 정보이다.

---

# 12. Effect Lifecycle

Effect는 다음 생명주기를 따른다.

```
Create

↓

Execute

↓

Complete
```

세부 실행 정책은 별도의 Specification에서 정의한다.

---

# 13. Validation

Effect Contract는 Generator 이전에 Validation을 수행한다.

대표적인 검증 항목

- Duplicate Identity
- Unknown Parameter
- Invalid Context

---

# 14. Cross Contract Dependencies

Effect Contract는 다음 Contract를 참조한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_EVENT

Effect는 기존 Contract를 재사용하며 자체적으로 재정의하지 않는다.

---

# 15. Part Summary

Part 1에서는 Effect Contract의 목적과 기본 모델을 정의하였다.

Effect Contract는 Runtime에서 수행되는 실행 단위를 표현하며, Parameter와 Execution Context를 중심으로 구성된다.

Part 2에서는 Effect Structure와 공통 필드를 정의한다.

---

# Part 2. Effect Structure

---

# 16. Overview

본 장에서는 Effect Contract의 공통 구조를 정의한다.

모든 Effect는 동일한 구조를 가지며 Generator는 이를 기반으로 Runtime Artifact를 생성한다.

---

# 17. Canonical Structure

The canonical structure of the Effect Contract is defined by the
Canonical Composition specified in Section 8.

Generators, validators, serializers, documentation generators,
and all downstream tools SHALL use the Canonical Composition
as the single authoritative structural definition of the
Effect Contract.

No additional structural fields SHALL exist outside the
Canonical Composition.

---

# 18. Name

Name은 Effect의 공식 이름이다.

요구사항

- Empty String을 허용하지 않는다.
- Namespace 내부에서 유일해야 한다.
- Generator는 Name을 변경하지 않는다.

예시

```
SendMessage

Teleport

GiveItem

Broadcast
```

---

# 19. Namespace

Namespace는 Effect를 논리적으로 구분한다.

예시

```
skript.player

skript.entity

skript.world
```

Namespace는 Identity 생성에도 사용될 수 있다.

---

# 20. Parameters

Effect는 0개 이상의 Parameter를 가진다.

모든 Parameter는 CONTRACT_PARAMETER를 참조한다.

```
Effect

└── Parameter[]
```

Parameter의 선언 순서는 의미를 가진다.

Generator는 Parameter 순서를 변경해서는 안 된다.

---

# 21. Execution Context

Execution Context는 Effect가 수행될 수 있는 환경을 정의한다.

예시

- Runtime
- World
- Session
- Tick
- Thread

Execution Context는 Runtime 구현이 아니라 실행 가능 조건을 설명하는 메타 정보이다.

---

# 22. Attributes

Attributes는 Effect의 추가적인 의미를 표현한다.

예시

- Deprecated
- Experimental
- Internal
- Unsafe
- Async
- Idempotent

Attribute는 Effect의 구조를 변경하지 않는다.

---

# 23. Documentation

Documentation은 Effect에 대한 설명을 제공한다.

대표 항목

- Summary
- Description
- Parameters
- Examples
- Remarks

Generator는 Documentation을 API 문서 생성에 활용할 수 있다.

---

# 24. Metadata

Metadata는 Effect Contract 자체를 설명하는 정보이다.

예시

- Source
- Provider
- Tags
- Created At
- Updated At

Metadata는 Runtime 의미를 변경하지 않는다.

---

# 25. Serialization

Effect Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Effect Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 26. Validation

Effect Contract는 Canonical Composition의 구조를 기준으로 최소한 다음 항목을 검증해야 한다.

- Identity 유효성
- Name 존재 여부
- Namespace 존재 여부
- Parameter 유효성
- Context 유효성
- Documentation 구조 유효성
- Metadata 구조 유효성

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 27. Structural Constraints

Effect Contract는 다음 구조적 제약을 만족해야 한다.

- Identity는 변경할 수 없다.
- Name은 Namespace 내부에서 유일해야 한다.
- Parameter 순서는 유지되어야 한다.
- Metadata는 Effect 의미를 변경해서는 안 된다.

---

# 28. Effect Classification

Effect는 목적에 따라 분류될 수 있다.

예시

- Player Effect
- Entity Effect
- World Effect
- Inventory Effect
- System Effect
- Utility Effect

분류(Classification)는 Documentation 또는 Metadata를 통해 표현할 수 있으며 Effect의 의미를 변경하지 않는다.

---

# 29. Default Behavior

Effect의 기본 정책(Default Policy)은 다음과 같다.

- 하나의 실행 단위를 표현한다.
- 값을 반환하지 않는다.
- 실행 성공 여부는 Runtime의 책임이다.
- 실행 순서는 Runtime 정책에 따른다.

필요한 경우 별도의 Specification에서 확장할 수 있다.

---

# 30. Part Summary

Part 2에서는 Effect Contract의 공통 구조를 정의하였다.

Effect는 Parameter와 Execution Context를 중심으로 구성되며, Runtime과 독립적인 실행 모델을 표현한다.

Part 3에서는 Execution Model, Resolution 및 Validation Rules를 정의한다.

---

# Part 3. Execution Model, Resolution & Validation

---

# 31. Overview

본 장에서는 Effect Contract의 Execution Model, Resolution 및 Validation 규칙을 정의한다.

Effect Contract는 실행 가능한 동작(Action)을 표현하며, 실제 실행은 Runtime 또는 Generator가 생성한 Artifact가 담당한다.

---

# 32. Execution Model

Effect는 실행(Execute) 가능한 객체이다.

Execution은 Effect Contract 자체의 책임이 아니라 Runtime 또는 Generator가 생성한 Artifact의 책임이다.

```
Create

↓

Execute

↓

Complete
```

Execution 구현은 별도의 Specification에서 정의한다.

---

# 33. Effect Resolution

Effect Resolution은 Effect Identity를 기준으로 수행한다.

Resolution 순서는 다음과 같다.

```
Lookup

↓

Identity Resolution

↓

Parameter Resolution

↓

Execution Context Resolution

↓

Resolved Effect
```

Generator는 항상 동일한 Effect Contract를 반환해야 한다.

---

# 34. Execution Semantics

Effect는 Runtime 상태를 변경할 수 있다.

예시

- 메시지 전송
- 엔티티 생성
- 블록 변경
- 변수 수정
- 사운드 재생

Effect Contract는 이러한 의미를 표현하지만, 실제 동작은 구현하지 않는다.

---

# 35. Side Effects

Effect의 기본 목적은 Side Effect를 발생시키는 것이다.

예시

```
Send Message

↓

Player Chat 변경
```

```
Teleport

↓

Entity Position 변경
```

Effect Contract는 Side Effect의 의미만 정의한다.

---

# 36. Idempotency

Effect는 Idempotent일 수도 있고 아닐 수도 있다.

예시

Idempotent

- Set Variable
- Set Health

Non-Idempotent

- Spawn Entity
- Give Item
- Play Sound

필요한 경우 Attribute를 통해 표현할 수 있다.

---

# 37. Execution Ordering

Effect Contract는 실행 순서를 보장하지 않는다.

Execution Ordering은 Runtime 정책에 의해 결정된다.

Generator는 실행 순서를 가정해서는 안 된다.

---

# 38. Error Handling

Effect 실행 중 오류가 발생할 수 있다.

오류 처리 방식은 Runtime의 책임이다.

예시

- Ignore
- Abort
- Retry
- Report Diagnostic

Effect Contract는 오류 처리 정책을 포함하지 않는다.

---

# 39. Validation Rules

Validation은 다음 순서로 수행한다.

```
Identity

↓

Parameter Validation

↓

Execution Context Validation

↓

Metadata Validation
```

Validation 실패 시 Registry에 등록되어서는 안 된다.

---

# 40. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Parameter Type
- Duplicate Identity
- Invalid Namespace
- Invalid Metadata
- Invalid Execution Context

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 41. Compatibility Guarantees

Effect Contract는 다음 사항을 보장한다.

- Stable Identity
- Stable Parameter Ordering
- Stable Execution Model
- Stable Serialization
- Stable Resolution Rules

---

# 42. Resolution Lifecycle

Effect Resolution은 다음 절차를 따른다.

```
Lookup

↓

Resolve Parameters

↓

Resolve Context

↓

Validate

↓

Resolved Effect
```

---

# 43. Runtime Independence

Effect Contract SHALL NOT depend on any specific Runtime implementation.

The same Effect Contract SHALL preserve identical semantics across
different Runtime implementations.

Runtime implementations SHALL NOT modify the canonical definition
of the Effect Contract.

Examples

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

---

# 44. Cross Contract Dependencies

Effect Contract는 다음 Contract를 참조한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_EVENT

Effect는 기존 Contract를 재사용하며 자체 구현을 포함하지 않는다.

---

# 45. Part Summary

Part 3에서는 Effect의 Execution Model, Resolution 및 Validation 규칙을 정의하였다.

Effect Contract는 실행 가능한 동작을 표현하며, Runtime은 이를 기반으로 실제 Side Effect를 수행한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 46. Overview

본 장에서는 Effect Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의한다.

Effect Contract는 Generator가 사용하는 Canonical Execution Model이며, Runtime은 Generator가 생성한 Artifact를 통해 Effect를 수행한다.

---

# 47. Serialization Principles

Effect Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Effect Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 Effect의 의미가 변경되어서는 안 된다.

---

# 48. Deserialization

직렬화된 Effect Contract는 동일한 Effect Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Parameter Resolution

↓

Execution Context Resolution

↓

Ready
```

복원 이후 Effect Contract는 Generator가 사용할 수 있는 상태여야 한다.

---

# 49. Generator Input

Generator는 Effect Contract를 공식 입력으로 사용한다.

```
Effect Contract

↓

Generator
```

Generator는 Effect의 구조를 변경해서는 안 된다.

---

# 50. Artifact Mapping

Effect Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Effect Contract

├── Runtime Effect
├── Command Handler
├── Script Action
├── Documentation
├── Registry Metadata
└── Language Server Symbol
```

모든 Artifact는 동일한 실행 의미(Execution Semantics)를 유지해야 한다.

---

# 51. Documentation Generation

Generator는 Effect Documentation을 기반으로 문서를 생성할 수 있다.

대표 항목

- Name
- Namespace
- Parameters
- Execution Context
- Summary
- Description
- Examples

Documentation은 Effect의 의미를 변경하지 않는다.

---

# 52. Runtime Compatibility

Runtime은 Effect Contract를 직접 소비하지 않는다.

Runtime은 Generator가 생성한 Artifact를 소비한다.

```
Effect Contract

↓

Generator

↓

Artifact

↓

Runtime
```

Runtime은 Contract를 수정해서는 안 된다.

---

# 53. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Parameter
- Duplicate Identity
- Invalid Namespace
- Invalid Execution Context
- Invalid Metadata

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 54. Compatibility Rules

Effect Contract는 다음 사항을 보장해야 한다.

- Stable Identity
- Stable Parameter Ordering
- Stable Execution Model
- Stable Serialization
- Stable Resolution Rules

Effect의 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 55. Extension Support

Effect Contract는 향후 다음 기능을 지원할 수 있다.

- Async Effects
- Deferred Effects
- Transactional Effects
- Batch Effects
- Conditional Effects
- Effect Pipelines

확장은 기존 Generator와의 호환성을 유지해야 한다.

---

# 56. Runtime Independence

Effect Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Effect Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 57. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Effect Identity를 변경하지 않는다.
- Parameter 순서를 유지한다.
- Execution Context를 변경하지 않는다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 58. Effect Lifecycle

Effect Contract는 다음 생명주기를 따른다.

```
Create

↓

Validate

↓

Serialize

↓

Generate

↓

Execute

↓

Archive
```

---

# 59. Part Summary

Part 4에서는 Effect Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Effect Contract는 프로젝트의 공식 실행 모델(Execution Model)이며, Generator는 이를 기반으로 다양한 Artifact를 생성하고 Runtime은 생성된 Artifact를 통해 실제 동작을 수행한다.

Part 5에서는 Best Practices, Anti-Patterns 및 Effect Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Governance

---

# 60. Overview

본 장에서는 Effect Contract 설계 시 따라야 하는 Best Practices와 Anti-Patterns를 정의한다.

Effect Contract는 프로젝트 전체에서 실행 가능한 동작(Action)을 표현하는 Canonical Contract이며, 장기적인 안정성과 일관성을 유지해야 한다.

---

# 61. Best Practices

다음 사항을 권장한다.

- Effect는 Registry를 통해 관리한다.
- Identity를 변경하지 않는다.
- Parameter는 CONTRACT_PARAMETER를 재사용한다.
- Execution Context를 명확하게 정의한다.
- Runtime 구현과 Contract를 분리한다.
- Documentation을 함께 제공한다.
- 실행 정책은 별도의 Specification으로 정의한다.

---

# 62. Anti-Patterns

다음 구현은 권장하지 않는다.

- Runtime 상태를 Contract에 저장하는 경우
- Effect 실행 코드를 Contract에 포함하는 경우
- Parameter 순서를 변경하는 경우
- Generator 전용 필드를 추가하는 경우
- Identity를 Name만으로 결정하는 경우
- Runtime 의존성을 Contract에 포함하는 경우

이러한 구현은 Execution Model의 일관성과 Generator의 예측 가능성을 저하시킨다.

---

# 63. Evolution Strategy

Effect Contract는 장기적인 확장을 고려하여 설계한다.

권장 방식

- Optional Field 추가
- Optional Metadata 추가
- Documentation 확장
- Extension Specification 추가

기존 Effect의 의미를 변경하는 것은 최소화한다.

---

# 64. Compatibility

Effect Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Identity 변경
- Parameter 의미 변경
- Parameter 순서 변경
- Execution Context 변경
- Effect Semantics 변경

---

# 65. Compliance

모든 Effect Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_EVENT
- 관련 Specification
- ADR
- RFC

Effect Contract는 프로젝트 전체의 Canonical Execution Model이다.

---

# 66. Effect Architecture Summary

Effect Contract의 공식 구조는 다음과 같다.

```
Effect
│
├── Identity
├── Name
├── Namespace
├── Parameters
├── Execution Context
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경하지 않고 Artifact를 생성해야 한다.

---

# 67. Relationship Model

Effect Contract는 다음 Contract를 참조한다.

```
Effect
│
├── Parameters
│     └── Parameter[]
│
├── Context
│
└── Metadata
```

Effect는 Callable Contract이며, 실행 가능한 동작(Action)을 표현한다.

---

# 68. Future Extensions

향후 다음 기능을 지원할 수 있다.

- Async Effect
- Deferred Effect
- Transaction Effect
- Effect Pipeline
- Batch Effect
- Conditional Effect
- Rollback Effect

새로운 기능은 기존 Generator와의 호환성을 유지해야 하며, 필요한 경우 별도의 Specification으로 정의한다.

---

# 69. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_EVENT
- CONTRACT_EXPRESSION
- CONTRACT_MODULE

---

# 70. Document Summary

CONTRACT_EFFECT는 프로젝트에서 사용하는 모든 실행 가능한 동작(Action)의 공식 표현 모델을 정의한다.

Effect는 Parameter와 Execution Context를 중심으로 구성되며, Runtime 구현과 분리된 Canonical Execution Contract이다.

Generator는 Effect Contract를 기반으로 다양한 Artifact를 생성하며, Runtime은 생성된 Artifact를 통해 실제 Side Effect를 수행한다.

---

# 71. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Effect Contract |