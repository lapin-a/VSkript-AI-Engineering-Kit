# Event Contract

Version: 1.0

Status: Draft

---

# Part 1. Event Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Event Contract를 정의한다.

Event Contract는 Runtime에서 발생하는 사건(Event)을 Canonical Model로 표현한다.

Generator는 Event Contract를 기반으로 Event Artifact를 생성한다.

---

# 2. Scope

본 Contract는 다음 대상에 적용된다.

- Runtime Event
- Script Event
- System Event
- User Event
- Extension Event

Event Trigger 및 Runtime Dispatch는 별도의 Specification에서 정의한다.

---

# 3. Definition

Event는 Runtime에서 발생하는 하나의 의미 있는 사건(Event Occurrence)을 표현한다.

Event Contract는 Event의 구조를 정의하며 Runtime 구현을 포함하지 않는다.

---

# 4. Design Goals

Event Contract는 다음 목표를 가진다.

- Stable
- Deterministic
- Reusable
- Language Independent
- Generator Friendly

---

# 5. Canonical Representation

동일한 Event는 하나의 Event Contract만 가진다.

```
Runtime Event

↓

Event Contract
```

Generator는 Event Contract를 공식 모델로 사용한다.

---

# 6. Identity

모든 Event는 고유한 Identity를 가진다.

Identity는 다음 조건을 만족해야 한다.

- Immutable
- Globally Unique
- Stable

Identity는 Name만으로 결정되지 않는다.

---

# 7. Ownership

Event는 Registry에서 관리된다.

```
Registry

└── Event Contract
```

다른 Contract는 Event를 참조할 수 있지만 소유자는 Registry이다.

---

# 8. Event Composition

Event는 다음 요소로 구성된다.

```
Event

├── Identity
├── Name
├── Namespace
├── Parameters
├── Source
├── Context
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경해서는 안 된다.

---

# 9. Event Parameters

Event는 0개 이상의 Parameter를 가진다.

모든 Parameter는 CONTRACT_PARAMETER를 참조한다.

```
Event

└── Parameter[]
```

Parameter의 선언 순서는 의미를 가진다.

---

# 10. Event Source

Source는 Event를 발생시키는 주체를 의미한다.

예시

```
Player

Entity

Server

World

Plugin
```

Source는 Type Contract를 참조한다.

---

# 11. Event Context

Context는 Event가 발생한 실행 환경을 의미한다.

예시

- Runtime
- Thread
- Tick
- World
- Session

Context는 Runtime 구현이 아니라 Event의 의미를 설명하는 메타 정보이다.

---

# 12. Event Lifecycle

Event는 다음 생명주기를 따른다.

```
Create

↓

Dispatch

↓

Consume

↓

Complete
```

세부 Dispatch 정책은 별도의 Specification에서 정의한다.

---

# 13. Validation

Event Contract는 Generator 이전에 Validation을 수행한다.

대표적인 검증 항목

- Duplicate Identity
- Unknown Parameter
- Unknown Source Type
- Invalid Context

---

# 14. Cross Contract Dependencies

Event Contract는 다음 Contract를 참조한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE

Event는 Parameter와 Type을 재사용하며 자체적으로 정의하지 않는다.

---

# 15. Part Summary

Part 1에서는 Event Contract의 목적과 기본 모델을 정의하였다.

Event Contract는 Runtime에서 발생하는 사건을 표현하며, Parameter와 Source를 중심으로 구성된다.

Part 2에서는 Event Structure와 공통 필드를 정의한다.

---

# Part 2. Event Structure

---

# 16. Overview

본 장에서는 Event Contract의 공통 구조를 정의한다.

모든 Event는 동일한 구조를 가지며 Generator는 이를 기반으로 Artifact를 생성한다.

---

# 17. Canonical Structure

모든 Event Contract는 다음 구조를 따른다.

```
Event

├── Identity
├── Name
├── Namespace
├── Source
├── Parameters
├── Context
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경해서는 안 된다.

---

# 18. Name

Name은 Event의 공식 이름이다.

요구사항

- Empty String을 허용하지 않는다.
- Namespace 내부에서 유일해야 한다.
- Generator는 Name을 변경하지 않는다.

예시

```
PlayerJoin

PlayerQuit

EntityDeath

InventoryClick
```

---

# 19. Namespace

Namespace는 Event를 논리적으로 구분한다.

예시

```
skript.player

skript.entity

skript.inventory
```

Namespace는 Identity 생성에도 사용될 수 있다.

---

# 20. Source

Source는 Event를 발생시키는 주체를 의미한다.

Source는 반드시 하나의 Type Contract를 참조한다.

예시

```
Player

Server

Entity

Plugin
```

Source는 Event의 Origin을 나타내며 Consumer를 의미하지 않는다.

---

# 21. Parameters

Event는 0개 이상의 Parameter를 가진다.

모든 Parameter는 CONTRACT_PARAMETER를 참조한다.

```
Event

└── Parameter[]
```

Parameter의 순서는 선언 순서를 의미한다.

Generator는 Parameter 순서를 변경해서는 안 된다.

---

# 22. Context

Context는 Event 발생 시점의 환경 정보를 표현한다.

대표적인 Context

- World
- Tick
- Thread
- Runtime
- Session

Context는 Event의 의미를 보조하는 정보이며, Event 자체를 구성하는 데이터는 아니다.

---

# 23. Attributes

Attributes는 Event의 추가적인 의미를 표현한다.

예시

- Deprecated
- Experimental
- Internal
- Cancellable
- Synchronous
- Asynchronous

Attribute는 Event의 구조를 변경하지 않는다.

---

# 24. Documentation

Documentation은 Event에 대한 설명을 제공한다.

대표 항목

- Summary
- Description
- Parameters
- Examples
- Remarks

Generator는 Documentation을 API 문서 생성에 활용할 수 있다.

---

# 25. Metadata

Metadata는 Event Contract 자체를 설명하는 정보이다.

예시

- Source
- Provider
- Tags
- Created At
- Updated At

Metadata는 Runtime 의미를 변경하지 않는다.

---

# 26. Serialization

Event Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Event Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 27. Validation

Event Contract는 최소한 다음 항목을 검증해야 한다.

- Name 존재 여부
- Namespace 존재 여부
- Source Type 존재 여부
- Parameter 유효성
- Context 유효성

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 28. Structural Constraints

Event Contract는 다음 구조적 제약을 만족해야 한다.

- Identity는 변경할 수 없다.
- Name은 Namespace 내부에서 유일해야 한다.
- Source는 반드시 하나 존재해야 한다.
- Parameter 순서는 유지되어야 한다.
- Metadata는 Event 의미를 변경해서는 안 된다.

---

# 29. Event Classification

Event는 목적에 따라 분류될 수 있다.

예시

- Lifecycle Event
- Entity Event
- Player Event
- World Event
- Inventory Event
- System Event

분류(Classification)는 Documentation 또는 Metadata를 통해 표현할 수 있으며 Event의 의미를 변경하지 않는다.

---

# 30. Part Summary

Part 2에서는 Event Contract의 공통 구조를 정의하였다.

Event는 Source와 Parameter를 중심으로 구성되며, Context와 Attributes를 통해 발생 환경과 특성을 표현한다.

Part 3에서는 Event Dispatch Model, Event Resolution 및 Validation Rules를 정의한다.

---

# Part 3. Event Dispatch, Resolution & Validation

---

# 31. Overview

본 장에서는 Event Contract의 Dispatch Model, Resolution 및 Validation 규칙을 정의한다.

Event Contract는 Runtime Dispatch를 직접 구현하지 않으며, Dispatch 가능한 Event Model만 정의한다.

---

# 32. Dispatch Model

Event는 Dispatch 가능한 객체이다.

Dispatch는 Event Contract 자체의 책임이 아니라 Runtime 또는 Generator가 생성한 Artifact의 책임이다.

```
Create

↓

Dispatch

↓

Consume

↓

Complete
```

Dispatch 구현은 별도의 Specification에서 정의한다.

---

# 33. Event Resolution

Event Resolution은 Event Identity를 기준으로 수행한다.

Resolution 순서는 다음과 같다.

```
Lookup

↓

Identity Resolution

↓

Source Resolution

↓

Parameter Resolution

↓

Resolved Event
```

Generator는 항상 동일한 Event Contract를 반환해야 한다.

---

# 34. Event Consumers

Event는 하나 이상의 Consumer에 의해 처리될 수 있다.

Consumer는 Event Contract의 일부가 아니다.

예시

- Trigger
- Listener
- Effect
- Runtime Handler

Consumer 정보는 Runtime에서 관리한다.

---

# 35. Event Propagation

Event는 Runtime에서 전파(Propagation)될 수 있다.

Propagation 방식은 Event Contract에서 정의하지 않는다.

예시

- Broadcast
- Bubble
- Direct Dispatch
- Priority Dispatch

전파 정책은 별도의 Specification에서 정의한다.

---

# 36. Event Ordering

Event Contract는 발생 순서를 보장하지 않는다.

Ordering은 Runtime 정책에 의해 결정된다.

Generator는 Ordering을 가정해서는 안 된다.

---

# 37. Event Cancellation

Event는 취소(Cancellation)를 지원할 수 있다.

Cancellation 가능 여부는 Attribute를 통해 표현한다.

예시

- Cancellable
- Non-Cancellable

Cancellation 동작은 Runtime에서 정의한다.

---

# 38. Event Filtering

Runtime은 Event를 다양한 조건으로 필터링할 수 있다.

예시

- Event Type
- Source Type
- Parameter Values
- Metadata
- Attributes

Filtering은 Event Contract의 책임이 아니다.

---

# 39. Validation Rules

Validation은 다음 순서로 수행한다.

```
Identity

↓

Source Validation

↓

Parameter Validation

↓

Context Validation

↓

Metadata Validation
```

Validation 실패 시 Registry에 등록되어서는 안 된다.

---

# 40. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Source
- Unknown Parameter Type
- Duplicate Identity
- Invalid Namespace
- Invalid Metadata
- Invalid Context

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 41. Compatibility Guarantees

Event Contract는 다음 사항을 보장한다.

- Stable Identity
- Stable Parameter Ordering
- Stable Source Model
- Stable Serialization
- Stable Resolution Rules

---

# 42. Event Resolution Lifecycle

Event Resolution은 다음 절차를 따른다.

```
Lookup

↓

Resolve Source

↓

Resolve Parameters

↓

Validate

↓

Resolved Event
```

---

# 43. Runtime Independence

Event Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Event Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 44. Cross Contract Dependencies

Event Contract는 다음 Contract를 참조한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE

Event는 Parameter와 Type을 재사용하며 자체 구현을 포함하지 않는다.

---

# 45. Part Summary

Part 3에서는 Event Dispatch Model, Resolution 및 Validation 규칙을 정의하였다.

Event Contract는 Dispatch 가능한 사건(Event)을 표현하며, Runtime은 이를 기반으로 Event를 전달하고 Consumer는 생성된 Artifact를 통해 Event를 처리한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 46. Overview

본 장에서는 Event Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의한다.

Event Contract는 Generator가 사용하는 Canonical Event Model이며, Runtime은 Generator가 생성한 Artifact를 통해 Event 정보를 소비한다.

---

# 47. Serialization Principles

Event Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Event Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 Event의 의미가 변경되어서는 안 된다.

---

# 48. Deserialization

직렬화된 Event Contract는 동일한 Event Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Source Resolution

↓

Parameter Resolution

↓

Ready
```

복원 이후 Event Contract는 Generator가 사용할 수 있는 상태여야 한다.

---

# 49. Generator Input

Generator는 Event Contract를 공식 입력으로 사용한다.

```
Event Contract

↓

Generator
```

Generator는 Event 구조를 변경해서는 안 된다.

---

# 50. Artifact Mapping

Event Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Event Contract

├── Runtime Event
├── Event Metadata
├── Trigger Definition
├── Documentation
├── Registry Metadata
└── Language Server Symbol
```

모든 Artifact는 동일한 Event Semantics를 유지해야 한다.

---

# 51. Documentation Generation

Generator는 Event Documentation을 기반으로 문서를 생성할 수 있다.

대표 항목

- Name
- Namespace
- Source
- Parameters
- Summary
- Description
- Examples

Documentation은 Event의 의미를 변경하지 않는다.

---

# 52. Runtime Compatibility

Runtime은 Event Contract를 직접 소비하지 않는다.

Runtime은 Generator가 생성한 Artifact를 소비한다.

```
Event Contract

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

- Unknown Source
- Unknown Parameter
- Duplicate Identity
- Invalid Context
- Invalid Metadata

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 54. Compatibility Rules

Event Contract는 다음 사항을 보장해야 한다.

- Stable Identity
- Stable Source Model
- Stable Parameter Ordering
- Stable Serialization
- Stable Resolution Rules

Event 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 55. Extension Support

Event Contract는 향후 다음 기능을 지원할 수 있다.

- Event Categories
- Event Priorities
- Event Channels
- Event Propagation Models
- Event Filters
- Event Payload Extensions

확장은 기존 Generator와의 호환성을 유지해야 한다.

---

# 56. Runtime Independence

Event Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Event Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 57. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Event Identity를 변경하지 않는다.
- Source를 변경하지 않는다.
- Parameter 순서를 유지한다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 58. Event Lifecycle

Event Contract는 다음 생명주기를 따른다.

```
Create

↓

Validate

↓

Serialize

↓

Generate

↓

Consume

↓

Archive
```

---

# 59. Part Summary

Part 4에서는 Event Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Event Contract는 프로젝트의 공식 Event Model이며, Generator는 이를 기반으로 다양한 Artifact를 생성하고 Runtime은 생성된 Artifact를 소비한다.

Part 5에서는 Best Practices, Anti-Patterns 및 Event Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Governance

---

# 60. Overview

본 장에서는 Event Contract 설계 시 따라야 하는 Best Practices와 Anti-Patterns를 정의한다.

Event Contract는 프로젝트 전체의 사건(Event)을 표현하는 Canonical Contract이며, 장기적인 안정성과 일관성을 유지해야 한다.

---

# 61. Best Practices

다음 사항을 권장한다.

- Event는 Registry를 통해 관리한다.
- Identity를 변경하지 않는다.
- Source는 명확하게 정의한다.
- Parameter는 CONTRACT_PARAMETER를 재사용한다.
- Event의 의미는 Documentation으로 설명한다.
- Runtime 구현과 Contract를 분리한다.
- Dispatch 정책은 별도의 Specification으로 정의한다.

---

# 62. Anti-Patterns

다음 구현은 권장하지 않는다.

- Runtime 상태를 Contract에 저장하는 경우
- Event Dispatch 구현을 Contract에 포함하는 경우
- Source 없이 Event를 정의하는 경우
- Parameter 순서를 변경하는 경우
- Generator 전용 필드를 추가하는 경우
- Identity를 Name만으로 결정하는 경우

이러한 구현은 Event Model의 일관성과 Generator의 예측 가능성을 저하시킨다.

---

# 63. Evolution Strategy

Event Contract는 장기적인 확장을 고려하여 설계한다.

권장 방식

- Optional Field 추가
- Optional Metadata 추가
- Documentation 확장
- Extension Specification 추가

기존 Event의 의미를 변경하는 것은 최소화한다.

---

# 64. Compatibility

Event Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Identity 변경
- Source 변경
- Parameter 의미 변경
- Parameter 순서 변경
- Context 의미 변경

---

# 65. Compliance

모든 Event Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- CONTRACT_PARAMETER
- CONTRACT_TYPE
- 관련 Specification
- ADR
- RFC

Event Contract는 프로젝트 전체의 Canonical Event Model이다.

---

# 66. Event Architecture Summary

Event Contract의 공식 구조는 다음과 같다.

```
Event
│
├── Identity
├── Name
├── Namespace
├── Source
├── Parameters
├── Context
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경하지 않고 Artifact를 생성해야 한다.

---

# 67. Relationship Model

Event Contract는 다음 Contract를 참조한다.

```
Event
│
├── Source
│     └── Type
│
├── Parameters
│     └── Parameter[]
│
└── Metadata
```

Event는 Aggregate Contract가 아니며, 다른 Contract를 참조하여 사건(Event)을 표현하는 Callable Contract이다.

---

# 68. Future Extensions

향후 다음 기능을 지원할 수 있다.

- Event Priority
- Event Category
- Event Channels
- Event Filters
- Event Payload Model
- Event Propagation Policy
- Event Scheduling

새로운 기능은 기존 Generator와의 호환성을 유지해야 하며, 필요한 경우 별도의 Specification으로 정의한다.

---

# 69. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_EFFECT
- CONTRACT_EXPRESSION
- CONTRACT_MODULE

---

# 70. Document Summary

CONTRACT_EVENT는 프로젝트에서 사용하는 모든 Event의 공식 표현 모델을 정의한다.

Event는 Source와 Parameter를 중심으로 사건(Event)을 표현하며, Runtime 구현과 분리된 Canonical Contract이다.

Generator는 Event Contract를 기반으로 다양한 Artifact를 생성하며, Runtime은 생성된 Artifact를 통해 Event를 전달하고 처리한다.

---

# 71. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Event Contract |