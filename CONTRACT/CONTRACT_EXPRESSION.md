# Expression Contract

Version: 1.0

Status: Draft

---

# Part 1. Query Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Expression Contract를 정의한다.

Expression Contract는 Runtime에서 값을 조회(Query)하거나 계산(Evaluate)하는 Canonical Query Model을 정의한다.

Generator는 Expression Contract를 기반으로 다양한 Runtime Artifact를 생성한다.

---

# 2. Scope

본 Contract는 다음 대상에 적용된다.

- Runtime Expression
- Script Expression
- Built-in Expression
- Extension Expression
- Constant Expression

Expression Evaluation은 별도의 Specification에서 정의한다.

---

# 3. Definition

Expression은 Runtime 상태를 변경하지 않고 하나 이상의 값을 계산하거나 조회하는 Query Contract이다.

Expression Contract는 Query의 구조를 정의하며 Runtime 구현은 포함하지 않는다.

---

# 4. Design Goals

Expression Contract는 다음 목표를 가진다.

- Stable
- Deterministic
- Pure
- Reusable
- Language Independent
- Generator Friendly

---

# 5. Canonical Representation

동일한 Expression은 하나의 Expression Contract만 가진다.

```
Runtime Expression

↓

Expression Contract
```

Generator는 Expression Contract를 공식 Query Model로 사용한다.

---

# 6. Query Semantics

Expression는 Runtime 상태를 변경해서는 안 된다.

Expression는 다음 중 하나를 수행한다.

- Value Lookup
- Value Computation
- Constant Evaluation
- Symbol Resolution

Runtime Side Effect는 허용되지 않는다.

---

# 7. Identity

모든 Expression은 고유한 Identity를 가진다.

Identity는 다음 조건을 만족해야 한다.

- Immutable
- Globally Unique
- Stable

Identity는 Name만으로 결정되지 않는다.

---

# 8. Ownership

Expression는 Registry에서 관리된다.

```
Registry

└── Expression Contract
```

다른 Contract는 Expression를 참조할 수 있지만 소유자는 Registry이다.

---

# 9. Expression Composition

Expression는 다음 요소로 구성된다.

```
Expression

├── Identity
├── Name
├── Namespace
├── Parameters
├── Result Type
├── Context
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경해서는 안 된다.

---

# 10. Parameters

Expression는 0개 이상의 Parameter를 가진다.

모든 Parameter는 CONTRACT_PARAMETER를 참조한다.

```
Expression

└── Parameter[]
```

Parameter 선언 순서는 의미를 가진다.

---

# 11. Result Type

모든 Expression은 정확히 하나의 Result Type을 가진다.

Result Type은 CONTRACT_TYPE을 참조한다.

예시

- Boolean
- Number
- String
- Player
- Entity
- ItemStack

---

# 12. Evaluation Context

Evaluation Context는 Expression가 평가되는 환경을 설명한다.

예시

- Runtime
- Player
- World
- Session
- Tick

Context는 Query 환경을 설명하는 메타 정보이다.

---

# 13. Validation

Expression Contract는 Generator 이전에 Validation을 수행한다.

대표적인 검증 항목

- Duplicate Identity
- Unknown Parameter
- Unknown Result Type
- Invalid Context

---

# 14. Cross Contract Dependencies

Expression Contract는 다음 Contract를 참조한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_FUNCTION

Expression는 기존 Contract를 재사용하며 자체적으로 재정의하지 않는다.

---

# 15. Part Summary

Part 1에서는 Expression Contract의 목적과 Query Model을 정의하였다.

Expression는 Runtime 상태를 변경하지 않는 조회(Query) 모델이며, Parameter와 Result Type을 중심으로 구성된다.

Part 2에서는 Expression Structure와 공통 필드를 정의한다.

---

# Part 2. Expression Structure

---

# 16. Overview

본 장에서는 Expression Contract의 공통 구조를 정의한다.

모든 Expression는 동일한 구조를 가지며 Generator는 이를 기반으로 Runtime Artifact를 생성한다.

---

# 17. Canonical Structure

Expression Contract의 Canonical Structure는
Section 8의 Canonical Composition으로 정의된다.

Generator, Validator, Serializer,
Documentation Generator 및
기타 모든 도구는
Section 8의 Canonical Composition을
Expression Contract의 유일한 구조 정의로 사용해야 한다.

Generator는 Canonical Composition을 변경해서는 안 된다.

---

# 18. Name

Name은 Expression의 공식 이름이다.

요구사항

- Empty String을 허용하지 않는다.
- Namespace 내부에서 유일해야 한다.
- Generator는 Name을 변경하지 않는다.

예시

```
PlayerHealth

PlayerName

DistanceBetween

RandomInteger
```

---

# 19. Namespace

Namespace는 Expression를 논리적으로 구분한다.

예시

```
skript.player

skript.entity

skript.math

skript.world
```

Namespace는 Identity 생성에도 사용될 수 있다.

---

# 20. Parameters

Expression는 0개 이상의 Parameter를 가진다.

모든 Parameter는 CONTRACT_PARAMETER를 참조한다.

```
Expression

└── Parameter[]
```

Parameter 선언 순서는 의미를 가진다.

Generator는 Parameter 순서를 변경해서는 안 된다.

---

# 21. Result Type

Expression는 정확히 하나의 Result Type을 가진다.

Result Type은 CONTRACT_TYPE을 참조한다.

Generator는 Result Type을 변경해서는 안 된다.

---

# 23. Attributes

Attributes는 Expression의 추가적인 의미를 표현한다.

예시

- Deprecated
- Experimental
- Internal
- Constant
- Pure
- Cacheable

Attribute는 Expression의 구조를 변경하지 않는다.

---

# 24. Documentation

Documentation은 Expression에 대한 설명을 제공한다.

대표 항목

- Summary
- Description
- Parameters
- Return Value
- Examples
- Remarks

Generator는 Documentation을 API 문서 생성에 활용할 수 있다.

---

# 25. Metadata

Metadata는 Expression Contract 자체를 설명하는 정보이다.

예시

- Source
- Provider
- Tags
- Created At
- Updated At

Metadata는 Query 의미를 변경하지 않는다.

---

# 26. Serialization

Expression Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Expression Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 27. Validation

Expression Contract는 최소한 다음 항목을 검증해야 한다.

- Name 존재 여부
- Namespace 존재 여부
- Parameter 유효성
- Result Type 존재 여부
- Evaluation Context 유효성

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 28. Structural Constraints

Expression Contract는 다음 구조적 제약을 만족해야 한다.

- Identity는 변경할 수 없다.
- Name은 Namespace 내부에서 유일해야 한다.
- Result Type은 반드시 하나 존재해야 한다.
- Parameter 순서는 유지되어야 한다.
- Metadata는 Query 의미를 변경해서는 안 된다.

---

# 29. Expression Classification

Expression는 목적에 따라 분류될 수 있다.

예시

- Constant Expression
- Property Expression
- Mathematical Expression
- Collection Expression
- Entity Expression
- Runtime Expression

분류(Classification)는 Documentation 또는 Metadata를 통해 표현할 수 있으며 Expression의 의미를 변경하지 않는다.

---

# 30. Default Behavior

Expression의 기본 정책(Default Policy)은 다음과 같다.

- 하나의 Query를 표현한다.
- 하나의 Result Type을 반환한다.
- Runtime 상태를 변경하지 않는다.
- 동일한 입력에 대해 동일한 의미를 유지한다.

평가 전략(Evaluation Strategy)은 별도의 Specification에서 정의한다.

---

# 31. Part Summary

Part 2에서는 Expression Contract의 공통 구조를 정의하였다.

Expression는 Parameter와 Result Type을 중심으로 구성되며, Runtime과 독립적인 Query Model을 표현한다.

Part 3에서는 Evaluation Model, Resolution 및 Validation Rules를 정의한다.

---

# Part 3. Evaluation Model, Resolution & Validation

---

# 32. Overview

본 장에서는 Expression Contract의 Evaluation Model, Resolution 및 Validation 규칙을 정의한다.

Expression Contract는 Runtime 상태를 변경하지 않는 Query Model이며, Evaluation은 Runtime 또는 Generator가 생성한 Artifact에 의해 수행된다.

---

# 33. Evaluation Model

Expression는 평가(Evaluate) 가능한 객체이다.

Evaluation은 Expression Contract 자체의 책임이 아니라 Runtime 또는 Generator가 생성한 Artifact의 책임이다.

Expression는 다음 절차를 따른다.

```
Resolve

↓

Evaluate

↓

Return Value
```

Evaluation 구현은 별도의 Specification에서 정의한다.

---

# 34. Expression Resolution

Expression Resolution은 Expression Identity를 기준으로 수행한다.

Resolution 순서는 다음과 같다.

```
Lookup

↓

Identity Resolution

↓

Parameter Resolution

↓

Result Type Resolution

↓

Resolved Expression
```

Generator는 항상 동일한 Expression Contract를 반환해야 한다.

---

# 35. Query Semantics

Expression는 Query를 수행한다.

Query는 다음과 같은 동작을 포함할 수 있다.

- Property Lookup
- Symbol Resolution
- Constant Evaluation
- Mathematical Computation
- Runtime Value Lookup

Expression는 Runtime 상태를 변경해서는 안 된다.

---

# 36. Evaluation Semantics

Evaluation은 입력 Parameter를 기반으로 Result Type의 값을 생성한다.

동일한 입력에 대해 동일한 의미를 유지해야 한다.

Generator는 Evaluation 의미를 변경해서는 안 된다.

---

# 37. Purity

Expression는 기본적으로 Pure Contract이다.

Pure Expression은 다음 조건을 만족한다.

- Runtime 상태를 변경하지 않는다.
- 외부 Observable Side Effect가 없다.
- 동일한 입력은 동일한 의미를 가진다.

필요한 경우 Attribute를 통해 Pure 여부를 명시할 수 있다.

---

# 38. Constant Evaluation

Expression는 Constant Evaluation을 지원할 수 있다.

예시

```
1 + 2

↓

3
```

```
true and false

↓

false
```

Constant Evaluation 정책은 별도의 Specification에서 정의한다.

---

# 39. Caching

Runtime은 Expression Evaluation 결과를 Cache할 수 있다.

Caching 여부는 Runtime 정책에 의해 결정된다.

Expression Contract는 Cache 구현을 포함하지 않는다.

---

# 40. Validation Rules

Validation은 다음 순서로 수행한다.

```
Identity

↓

Parameter Validation

↓

Result Type Validation

↓

Evaluation Context Validation

↓

Metadata Validation
```

Validation 실패 시 Registry에 등록되어서는 안 된다.

---

# 41. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Result Type
- Unknown Parameter
- Duplicate Identity
- Invalid Namespace
- Invalid Metadata
- Invalid Evaluation Context

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 42. Compatibility Guarantees

Expression Contract는 다음 사항을 보장한다.

- Stable Identity
- Stable Parameter Ordering
- Stable Result Type
- Stable Evaluation Model
- Stable Serialization

---

# 43. Resolution Lifecycle

Expression Resolution은 다음 절차를 따른다.

```
Lookup

↓

Resolve Parameters

↓

Resolve Result Type

↓

Validate

↓

Evaluate
```

---

# 44. Runtime Independence

Expression Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Expression Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 45. Cross Contract Dependencies

Expression Contract는 다음 Contract를 참조한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_FUNCTION

Expression는 기존 Contract를 재사용하며 자체 구현을 포함하지 않는다.

---

# 46. Part Summary

Part 3에서는 Expression의 Evaluation Model, Resolution 및 Validation 규칙을 정의하였다.

Expression Contract는 Runtime 상태를 변경하지 않는 Query Contract이며, Resolve와 Evaluate 과정을 통해 하나의 Result Type 값을 생성한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 47. Overview

본 장에서는 Expression Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의한다.

Expression Contract는 Generator가 사용하는 Canonical Query Model이며, Runtime은 Generator가 생성한 Artifact를 통해 Expression을 평가(Evaluate)한다.

---

# 48. Serialization Principles

Expression Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Expression Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 Expression의 의미가 변경되어서는 안 된다.

---

# 49. Deserialization

직렬화된 Expression Contract는 동일한 Query Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Parameter Resolution

↓

Result Type Resolution

↓

Ready
```

복원 이후 Expression Contract는 Generator가 사용할 수 있는 상태여야 한다.

---

# 50. Generator Input

Generator는 Expression Contract를 공식 입력으로 사용한다.

```
Expression Contract

↓

Generator
```

Generator는 Expression의 구조를 변경해서는 안 된다.

---

# 51. Artifact Mapping

Expression Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Expression Contract

├── Runtime Expression
├── Query Evaluator
├── Constant Evaluator
├── Documentation
├── Registry Metadata
└── Language Server Symbol
```

모든 Artifact는 동일한 Query Semantics를 유지해야 한다.

---

# 52. Documentation Generation

Generator는 Expression Documentation을 기반으로 문서를 생성할 수 있다.

대표 항목

- Name
- Namespace
- Parameters
- Result Type
- Summary
- Description
- Examples

Documentation은 Expression의 의미를 변경하지 않는다.

---

# 53. Runtime Compatibility

Runtime은 Expression Contract를 직접 소비하지 않는다.

Runtime은 Generator가 생성한 Artifact를 소비한다.

```
Expression Contract

↓

Generator

↓

Artifact

↓

Runtime
```

Runtime은 Contract를 수정해서는 안 된다.

---

# 54. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Result Type
- Unknown Parameter
- Duplicate Identity
- Invalid Namespace
- Invalid Evaluation Context
- Invalid Metadata

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 55. Compatibility Rules

Expression Contract는 다음 사항을 보장해야 한다.

- Stable Identity
- Stable Parameter Ordering
- Stable Result Type
- Stable Evaluation Model
- Stable Serialization

Expression의 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 56. Extension Support

Expression Contract는 향후 다음 기능을 지원할 수 있다.

- Lazy Evaluation
- Cached Expression
- Incremental Evaluation
- Reactive Expression
- Deferred Evaluation
- Symbolic Evaluation

확장은 기존 Generator와의 호환성을 유지해야 한다.

---

# 57. Runtime Independence

Expression Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Expression Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 58. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Expression Identity를 변경하지 않는다.
- Parameter 순서를 유지한다.
- Result Type을 변경하지 않는다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 59. Expression Lifecycle

Expression Contract는 다음 생명주기를 따른다.

```
Create

↓

Validate

↓

Serialize

↓

Generate

↓

Resolve

↓

Evaluate

↓

Return Value
```

---

# 60. Part Summary

Part 4에서는 Expression Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Expression Contract는 프로젝트의 공식 Query Model이며, Generator는 이를 기반으로 다양한 Artifact를 생성하고 Runtime은 생성된 Artifact를 통해 값을 계산하거나 조회한다.

Part 5에서는 Best Practices, Anti-Patterns 및 Query Architecture를 정의한다.

---

# Part 5. Query Architecture, Best Practices & Governance

---

# 61. Overview

본 장에서는 Expression Contract의 Query Architecture와 Best Practices를 정의한다.

Expression Contract는 프로젝트 전체의 Canonical Query Model이며, 장기적인 안정성과 일관성을 유지해야 한다.

---

# 62. Query Architecture

Expression는 프로젝트의 공식 Query Layer를 구성한다.

```
Query

↓

Expression

↓

Resolve

↓

Evaluate

↓

Result Descriptor

↓

Value
```

Expression는 Runtime 상태를 변경하지 않는다.

---

# 63. Result Descriptor

Expression의 결과는 Result Descriptor로 표현한다.

Result Descriptor는 다음 요소로 구성된다.

```
Result Descriptor

├── Type
└── Cardinality
```

Generator는 Result Descriptor를 변경해서는 안 된다.

---

# 64. Cardinality

Cardinality는 Query 결과의 개수를 표현한다.

지원되는 Cardinality

```
Single
```

정확히 하나의 결과

예시

```
player
```

```
Optional
```

결과가 없을 수도 있음

예시

```
attacker
```

```
Collection
```

여러 개의 결과

예시

```
all players
```

향후 다음 Cardinality를 지원할 수 있다.

- Stream
- Async

---

# 65. Best Practices

다음 사항을 권장한다.

- Expression는 가능한 한 Pure하게 유지한다.
- Runtime 상태를 변경하지 않는다.
- Result Descriptor를 명확하게 정의한다.
- Parameter를 CONTRACT_PARAMETER에서 재사용한다.
- Result Type을 CONTRACT_TYPE에서 재사용한다.
- Documentation을 함께 제공한다.

---

# 66. Anti-Patterns

다음 구현은 권장하지 않는다.

- Runtime 상태를 변경하는 Expression
- Effect를 Expression 내부에서 실행하는 경우
- Result Type을 변경하는 경우
- Parameter 순서를 변경하는 경우
- Generator 전용 필드를 추가하는 경우

---

# 67. Evolution Strategy

Expression Contract는 장기적인 확장을 고려하여 설계한다.

권장 방식

- Optional Metadata 추가
- Result Descriptor 확장
- Documentation 확장
- Extension Specification 추가

기존 Query 의미를 변경하는 것은 최소화한다.

---

# 68. Compatibility

Expression Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Identity 변경
- Result Type 변경
- Cardinality 변경
- Parameter 의미 변경
- Query Semantics 변경

---

# 69. Compliance

모든 Expression Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_FUNCTION
- 관련 Specification
- ADR
- RFC

Expression Contract는 프로젝트 전체의 Canonical Query Model이다.

---

# 70. Relationship Model

Expression는 다음 Contract와 함께 Query Layer를 구성한다.

```
Function

↓

Signature

Expression

↓

Query

↓

Result Descriptor

↓

Type
+

Cardinality
```

Effect는 Command Layer를 구성한다.

Event는 Notification Layer를 구성한다.

---

# 71. Query / Command / Notification Architecture

프로젝트는 다음 구조를 따른다.

```
Query Layer
    └── Expression

Command Layer
    └── Effect

Notification Layer
    └── Event

Object Layer
    └── Class

Aggregation Layer
    └── Module
```

이는 프로젝트 전체의 Language Architecture를 구성한다.

---

# 72. Future Extensions

향후 다음 기능을 지원할 수 있다.

- Query Optimizer
- Lazy Evaluation
- Reactive Query
- Cached Query
- Incremental Evaluation
- Symbolic Evaluation

필요한 경우 별도의 Specification으로 정의한다.

---

# 73. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Expression Contract |