# Parameter Contract

Version: 1.0

Status: Draft

---

# Part 1. Parameter Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Parameter Contract를 정의한다.

Parameter Contract는 Function, Event, Effect, Expression, Constructor 및 Method에서 공통적으로 사용하는 Parameter 표현 모델이다.

---

# 2. Scope

본 Contract는 다음 Contract에서 재사용된다.

- Function Contract
- Event Contract
- Effect Contract
- Expression Contract
- Method Contract
- Constructor Contract

---

# 3. Definition

Parameter는 호출자(Caller)가 대상(Target)에 전달하는 입력 값을 표현하는 Contract이다.

Parameter는 실행 로직을 포함하지 않는다.

---

# 4. Design Goals

Parameter Contract는 다음 목표를 가진다.

- Stable
- Reusable
- Immutable
- Language Independent
- Generator Friendly

---

# 5. Canonical Model

동일한 Parameter는 하나의 Contract만 가진다.

```
Parameter

↓

Parameter Contract
```

Generator는 Parameter를 직접 해석하지 않고 Contract를 사용한다.

---

# 6. Parameter Identity

Parameter는 자신의 Identity를 가질 수 있다.

Identity는 다음을 만족해야 한다.

- Immutable
- Stable
- Unique within Owner

Owner(Function/Event 등)가 달라도 Identity 규칙은 동일하다.

---

# 7. Ownership

Parameter는 항상 하나의 Owner에 속한다.

예시

```
Function

└── Parameters
```

```
Event

└── Parameters
```

Parameter는 독립적으로 존재하지 않는다.

---

# 8. Ordering

Parameter의 순서는 의미를 가진다.

```
Parameter 1

↓

Parameter 2

↓

Parameter 3
```

Generator는 순서를 변경해서는 안 된다.

---

# 9. Immutability

Parameter Contract는 생성 이후 변경되지 않는 것을 원칙으로 한다.

변경이 필요한 경우 새로운 Contract를 생성한다.

---

# 10. Language Independence

Parameter는 특정 언어의 Parameter 표현 방식에 의존하지 않는다.

예를 들어

- TypeScript
- Java
- Kotlin
- C#

모두 동일한 Contract에서 생성된다.

---

# 11. Reusability

동일한 Parameter Model은 여러 Generator에서 재사용할 수 있다.

예시

Contract

↓

TypeScript

↓

JSON Schema

↓

OpenAPI

↓

LSP Types

---

# 12. Validation

Parameter는 Generator 이전에 Validation을 수행한다.

대표적인 Validation

- Required Field
- Duplicate Name
- Invalid Type
- Invalid Default Value

---

# 13. Compatibility

Parameter Contract는 가능한 한 하위 호환성을 유지한다.

새로운 필드는 Optional로 추가하는 것을 권장한다.

---

# 14. Cross References

본 Contract는 다음 문서를 기반으로 한다.

- CONTRACT_DESIGN_PRINCIPLES
- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY

---

# 15. Part Summary

Part 1에서는 Parameter Contract의 목적과 기본 구조를 정의하였다.

Parameter는 모든 호출 가능한 Contract에서 공통적으로 사용하는 Canonical Input Model이다.

Part 2에서는 Parameter Structure와 공통 필드를 정의한다.

---

# Part 2. Parameter Structure

---

# 16. Overview

본 장에서는 Parameter Contract의 공통 구조를 정의한다.

모든 Parameter는 동일한 필드 구조를 공유하며, Generator는 본 구조를 기준으로 Artifact를 생성한다.

---

# 17. Canonical Structure

모든 Parameter Contract는 다음 구조를 따른다.

```
Parameter

├── Identity
├── Name
├── Type
├── Position
├── Required
├── Default Value
├── Variadic
├── Attributes
├── Documentation
└── Metadata
```

각 필드는 명확한 의미를 가져야 한다.

---

# 18. Name

Name은 Parameter의 식별 가능한 이름이다.

요구사항

- Owner 내부에서 유일해야 한다.
- Empty String을 허용하지 않는다.
- Case Sensitive 여부는 Specification에서 정의한다.

예시

```
player
world
amount
location
```

---

# 19. Type

Type은 Parameter가 허용하는 데이터 형식을 정의한다.

Type은 반드시 Type Contract를 참조한다.

예시

```
Player
Number
String
Boolean
Location
```

Generator는 Type을 직접 해석하지 않고 Type Contract를 참조한다.

---

# 20. Position

Position은 Parameter의 선언 순서를 나타낸다.

예시

```
0

1

2
```

Position은 호출 순서와 동일해야 한다.

---

# 21. Required

Required는 Parameter의 필수 여부를 나타낸다.

허용 값

- true
- false

Required=false인 Parameter는 생략될 수 있다.

---

# 22. Default Value

Default Value는 Parameter가 생략되었을 때 사용할 기본값이다.

Default Value는 Optional Parameter에서만 사용할 수 있다.

Generator는 Default Value의 의미를 변경해서는 안 된다.

---

# 23. Variadic

Variadic는 Parameter가 가변 인자를 허용하는지를 나타낸다.

예시

```
print(value...)
```

Variadic Parameter는 마지막 Parameter로 선언하는 것을 권장한다.

---

# 24. Attributes

Attributes는 Parameter의 추가적인 의미를 표현한다.

예시

- Nullable
- Deprecated
- Experimental
- Internal

Attribute는 Parameter의 타입 자체를 변경하지 않는다.

---

# 25. Documentation

Documentation은 Parameter의 설명을 제공한다.

대표 항목

- Summary
- Description
- Example
- Remarks

Documentation은 Generator가 API 문서를 생성할 때 사용할 수 있다.

---

# 26. Metadata

Metadata는 Parameter Contract 자체를 설명하는 정보이다.

예시

- Source
- Provider
- Tags
- Created At
- Updated At

Metadata는 실행 의미를 변경하지 않는다.

---

# 27. Serialization

Parameter Contract는 안정적으로 직렬화되어야 한다.

동일한 Parameter는 항상 동일한 직렬화 결과를 생성해야 한다.

지원 형식

- JSON
- YAML

---

# 28. Validation

Parameter Contract는 최소한 다음 항목을 검증해야 한다.

- Name 존재 여부
- Type 존재 여부
- Position 중복 여부
- Required와 Default Value의 일관성
- Variadic 위치

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 29. Structural Constraints

Parameter는 다음 구조적 제약을 만족해야 한다.

- Position은 음수가 될 수 없다.
- Name은 Owner 내부에서 중복될 수 없다.
- Variadic Parameter는 최대 하나만 존재한다.
- Variadic Parameter 뒤에는 다른 Parameter를 둘 수 없다.
- Required Parameter 뒤에 Optional Parameter가 오는 정책은 Specification에서 정의한다.

---

# 30. Contract Example

개념적인 예시는 다음과 같다.

```
Parameter

Name:
    player

Type:
    Player

Position:
    0

Required:
    true
```

구체적인 JSON 표현은 별도의 Serialization Specification에서 정의한다.

---

# 31. Part Summary

Part 2에서는 Parameter Contract의 공통 구조와 필드를 정의하였다.

모든 Parameter는 Name, Type, Position을 중심으로 구성되며, Optional, Default Value, Variadic 및 Attributes를 통해 다양한 호출 규칙을 표현할 수 있다.

Part 3에서는 Parameter Type System, Constraint Model 및 Validation Rules를 정의한다.

---

# Part 3. Type System & Constraint Model

---

# 32. Overview

본 장에서는 Parameter Contract의 Type System과 Constraint Model을 정의한다.

Parameter는 단순히 값을 전달하는 것이 아니라, 허용 가능한 값의 범위와 의미를 함께 표현해야 한다.

---

# 33. Type Reference

모든 Parameter는 Type Contract를 참조한다.

```
Parameter

↓

Type Contract
```

Parameter는 자체적으로 Type을 정의하지 않는다.

---

# 34. Primitive Types

Primitive Type은 가장 기본적인 데이터 형식을 의미한다.

대표적인 예

- Boolean
- Number
- Integer
- Float
- String

Primitive Type 역시 Type Contract를 통해 표현한다.

---

# 35. Complex Types

Complex Type은 여러 데이터를 조합한 형식이다.

예시

- Object
- List
- Map
- Tuple

Complex Type의 내부 구조는 Type Contract에서 정의한다.

---

# 36. Nullable

Nullable은 Parameter가 Null 값을 허용하는지를 나타낸다.

허용 값

- true
- false

Nullable은 Optional과 동일한 의미가 아니다.

---

# 37. Optional Parameters

Optional Parameter는 호출 시 생략될 수 있다.

Optional Parameter는 다음 조건 중 하나를 만족해야 한다.

- Default Value 존재
- Specification에서 Optional로 정의

Generator는 Optional 여부를 변경해서는 안 된다.

---

# 38. Variadic Parameters

Variadic Parameter는 여러 개의 값을 받을 수 있다.

예시

```
print(values...)
```

규칙

- Variadic Parameter는 최대 하나만 존재한다.
- 마지막 Parameter여야 한다.

---

# 39. Constraint Model

Constraint는 Parameter가 만족해야 하는 조건을 정의한다.

대표적인 Constraint

- Minimum
- Maximum
- Min Length
- Max Length
- Pattern
- Enumeration

Constraint는 Validation 과정에서 사용된다.

---

# 40. Range Constraints

숫자 타입은 범위를 지정할 수 있다.

예시

```
Minimum

0

Maximum

100
```

Generator는 Constraint를 보존해야 한다.

---

# 41. Enumeration Constraints

Parameter는 허용 가능한 값 집합을 정의할 수 있다.

예시

```
Difficulty

Easy

Normal

Hard
```

Enumeration은 Type 의미를 변경하지 않는다.

---

# 42. Pattern Constraints

문자열 Parameter는 Pattern을 정의할 수 있다.

대표적인 예

- Identifier
- Namespace
- UUID
- URI

Pattern은 Validation 규칙으로 사용한다.

---

# 43. Semantic Constraints

일부 Constraint는 구조가 아니라 의미를 표현한다.

예시

- Existing Registry Entry
- Valid Identity
- Registered Type
- Existing Namespace

이러한 Constraint는 Runtime이 아니라 Validation 단계에서 확인한다.

---

# 44. Constraint Composition

하나의 Parameter는 여러 Constraint를 동시에 가질 수 있다.

예시

```
String

+

Minimum Length

+

Maximum Length

+

Pattern
```

Constraint는 서로 충돌하지 않아야 한다.

---

# 45. Validation Order

Validation은 다음 순서로 수행하는 것을 권장한다.

```
Structure

↓

Type

↓

Constraint

↓

Semantic Validation
```

앞 단계가 실패하면 다음 단계는 수행하지 않을 수 있다.

---

# 46. Generator Responsibilities

Generator는 Constraint를 변경하지 않는다.

Generator는 다음 역할만 수행한다.

- Constraint 전달
- Artifact 변환
- Diagnostic 생성

Constraint 해석은 대상 Runtime 또는 Validator가 수행한다.

---

# 47. Model Guarantees

Parameter Type System은 다음을 보장한다.

- Stable Type Reference
- Explicit Constraints
- Deterministic Validation
- Language Independence
- Generator Compatibility

---

# 48. Part Summary

Part 3에서는 Parameter Contract의 Type System과 Constraint Model을 정의하였다.

Parameter는 Type Contract를 참조하며, 다양한 Constraint를 통해 허용 가능한 값의 구조와 의미를 명확하게 표현한다.

Part 4에서는 Generator Integration, Serialization Rules 및 Runtime Compatibility를 정의한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 49. Overview

본 장에서는 Parameter Contract의 직렬화, Generator 통합 및 Runtime 호환 규칙을 정의한다.

Parameter Contract는 Generator의 입력이며, Runtime에서 사용하는 Artifact의 생성 기준이 된다.

---

# 50. Serialization Principles

Parameter Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Parameter Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 의미(Semantics)가 변경되어서는 안 된다.

---

# 51. Deserialization

직렬화된 Parameter Contract는 동일한 Parameter Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Type Validation

↓

Constraint Validation

↓

Ready
```

복원 이후 Parameter는 Generator가 사용할 수 있는 상태여야 한다.

---

# 52. Generator Input

Generator는 Parameter Contract를 직접 입력으로 사용할 수 있다.

예시

```
Function Contract

└── Parameter[]
```

Generator는 Parameter 구조를 변경하지 않는다.

---

# 53. Artifact Mapping

Parameter Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Parameter Contract

├── TypeScript Parameter
├── JSON Schema Property
├── OpenAPI Parameter
├── LSP Signature Parameter
└── Documentation
```

모든 Artifact는 동일한 의미를 유지해야 한다.

---

# 54. Documentation Generation

Generator는 Parameter Documentation을 활용하여 API 문서를 생성할 수 있다.

대표 항목

- Name
- Type
- Description
- Default Value
- Required 여부
- Example

Documentation은 Contract의 의미를 변경하지 않는다.

---

# 55. Runtime Compatibility

Runtime은 Parameter Contract를 직접 소비하지 않는다.

Runtime은 Generator가 생성한 Artifact만 사용한다.

```
Parameter Contract

↓

Generator

↓

Artifact

↓

Runtime
```

---

# 56. Diagnostics

Generator는 Parameter 처리 과정에서 다음 Diagnostic을 생성할 수 있다.

예시

- Duplicate Parameter Name
- Unknown Type
- Invalid Constraint
- Invalid Default Value
- Invalid Variadic Position

Diagnostic은 가능한 한 여러 문제를 함께 보고하는 것을 권장한다.

---

# 57. Compatibility Rules

Parameter Contract는 다음 사항을 보장해야 한다.

- Stable Serialization
- Stable Identity
- Stable Ordering
- Stable Type Reference

기존 Parameter의 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 58. Extension Support

Parameter Contract는 향후 확장을 지원한다.

예시

- Generic Parameter
- Named Parameter
- Keyword Parameter
- Context Parameter
- Injected Parameter

확장은 기존 Generator를 손상시키지 않아야 한다.

---

# 59. Runtime Independence

Parameter Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Contract는 여러 Runtime에서 재사용될 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Testing Framework

---

# 60. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Parameter 구조를 변경하지 않는다.
- Type Reference를 유지한다.
- Constraint를 유지한다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 61. Parameter Lifecycle

Parameter Contract는 다음 생명주기를 따른다.

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

# 62. Part Summary

Part 4에서는 Parameter Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Parameter Contract는 Generator의 안정적인 입력이며, 모든 Runtime은 Generator가 생성한 Artifact를 통해 Parameter 정보를 소비한다.

Part 5에서는 Best Practices, Anti-Patterns, Compliance 및 Parameter Contract Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Governance

---

# 63. Overview

본 장에서는 Parameter Contract 설계 시 따라야 하는 Best Practices와 Anti-Patterns를 정의한다.

Parameter Contract는 프로젝트 전체에서 가장 많이 재사용되는 Contract 중 하나이며, 장기적인 안정성과 일관성을 유지해야 한다.

---

# 64. Best Practices

다음 사항을 권장한다.

- Parameter는 명확한 이름을 사용한다.
- Type Contract를 항상 참조한다.
- Optional과 Nullable을 구분한다.
- Constraint를 명시적으로 정의한다.
- Documentation을 함께 제공한다.
- Default Value는 의미가 명확한 경우에만 사용한다.
- Parameter 순서를 변경하지 않는다.
- Identity를 변경하지 않는다.

---

# 65. Anti-Patterns

다음 구현은 권장하지 않는다.

- Parameter Type을 문자열로 직접 표현하는 경우
- Runtime 상태를 Parameter에 저장하는 경우
- Generator 전용 필드를 Parameter에 포함하는 경우
- Parameter 순서를 Generator가 변경하는 경우
- 동일한 Name을 여러 Parameter에서 사용하는 경우
- Constraint 없이 의미를 추론하도록 하는 경우

이러한 구현은 Contract의 안정성과 Generator의 예측 가능성을 저하시킨다.

---

# 66. Evolution Strategy

Parameter Contract는 확장을 고려하여 설계한다.

권장 방식

- Optional Field 추가
- 새로운 Constraint 추가
- Attribute 추가
- Extension 사용

기존 필드의 의미를 변경하는 것은 최소화한다.

---

# 67. Compatibility

Parameter Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Required Field 제거
- Required → Optional 의미 변경
- Type 의미 변경
- Position 의미 변경
- Identity 변경

---

# 68. Compliance

모든 Parameter Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- 관련 Specification

Parameter Contract는 모든 호출 가능한 Contract의 공통 기반이다.

---

# 69. Parameter Architecture Summary

Parameter Contract의 공식 구조는 다음과 같다.

```
Parameter
    │
    ├── Identity
    ├── Name
    ├── Type Reference
    ├── Position
    ├── Required
    ├── Default Value
    ├── Variadic
    ├── Constraints
    ├── Attributes
    ├── Documentation
    └── Metadata
```

Generator는 위 구조를 변경하지 않고 Artifact로 변환해야 한다.

---

# 70. Reuse Model

Parameter Contract는 다음 Contract에서 재사용된다.

```
Function
    │
    ├── Parameter[]
    │
Event
    │
    ├── Parameter[]
    │
Effect
    │
    ├── Parameter[]
    │
Expression
    │
    ├── Parameter[]
    │
Constructor
    │
    └── Parameter[]
```

새로운 호출 가능한 Contract는 Parameter Contract를 재사용하는 것을 원칙으로 한다.

---

# 71. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CONTRACT_DESIGN_PRINCIPLES
- CONTRACT_TYPE
- CONTRACT_FUNCTION
- CONTRACT_EVENT
- CONTRACT_EFFECT
- CONTRACT_EXPRESSION
- CORE_GLOSSARY

---

# 72. Document Summary

CONTRACT_PARAMETER는 프로젝트에서 사용하는 Parameter의 공식 표현 모델을 정의한다.

Parameter는 Type Contract를 참조하며, Constraint와 Documentation을 포함한 안정적인 Canonical Model로 관리된다.

모든 호출 가능한 Contract는 본 Parameter Contract를 공통 기반으로 사용해야 한다.

---

# 73. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Parameter Contract |