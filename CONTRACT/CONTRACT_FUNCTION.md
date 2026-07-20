# Function Contract

Version: 1.0

Status: Draft

---

# Part 1. Function Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Function Contract를 정의한다.

Function Contract는 호출 가능한(Function-like) API를 표현하는 Canonical Model이다.

Generator는 Function Contract를 기반으로 다양한 Artifact를 생성한다.

---

# 2. Scope

본 Contract는 다음 대상에 적용된다.

- Global Function
- Built-in Function
- Library Function
- Extension Function
- Class Method

Method는 별도의 Contract를 가지지 않는다.

Class Contract는 Method를 표현하기 위해
Function Contract를 재사용한다.

---

# 3. Definition

Function은 이름(Name), 입력(Parameter), 반환(Return Type)을 가지는 호출 가능한 Interface Contract이다.

Function은 Runtime 구현이 아니라 Interface를 표현한다.

---

# 4. Design Goals

Function Contract는 다음 목표를 가진다.

- Stable
- Deterministic
- Reusable
- Language Independent
- Generator Friendly

---

# 5. Canonical Representation
Generator는 Function Contract를 공식 모델로 사용한다.

Function Contract는 Function을 표현하는
유일한 Canonical Representation이다.

```
Function

↓

Function Contract
```

Generator는 Function Contract를 공식 모델로 사용한다.

---

# 6. Identity

모든 Function은 고유한 Identity를 가진다.

Identity는 다음 조건을 만족해야 한다.

- Immutable
- Globally Unique
- Stable

Identity는 Name만으로 결정되지 않는다.

---

# 7. Ownership

Function은 Registry에서 관리된다.

```
Registry

└── Function Contract
```

다른 Contract는 Function을 참조할 수 있지만 소유자는 Registry이다.

---

# 8. Function Signature

Function Signature는 Function을 구별하는 핵심 정보이다.

Signature는 다음 요소로 구성된다.

- Name
- Parameter List
- Generic Parameters (선택)
- Return Type

Signature는 Generator와 Validator에서 사용된다.

---

# 9. Parameter Model

Function은 하나 이상의 Parameter를 가진다.

Parameter는 CONTRACT_PARAMETER를 사용한다.

```
Function

└── Parameter[]
```

Function은 Parameter 구조를 직접 정의하지 않는다.

---

# 10. Return Type

Function은 하나의 Return Type을 가진다.

Return Type은 CONTRACT_TYPE를 참조한다.

```
Function

↓

Type Contract
```

Void 역시 하나의 Type으로 표현한다.

---

# 11. Generic Support

Function은 Generic Parameter를 가질 수 있다.

예시

```
<T>

<T,K>

<T extends Entity>
```

Generic Parameter는 Type Contract를 참조한다.

---

# 12. Language Independence

Function Contract는 특정 언어에 종속되지 않는다.

동일한 Contract에서 다음을 생성할 수 있다.

- TypeScript
- Kotlin
- Java
- JSON Schema
- Documentation

---

# 13. Validation

Function Contract는 Generator 이전에 Validation을 수행한다.

대표적인 검증 항목

- Duplicate Signature
- Unknown Type
- Invalid Parameter
- Invalid Generic Parameter

---

# 14. Compatibility

Function Contract는 가능한 한 하위 호환성을 유지한다.

새로운 Field는 Optional로 추가하는 것을 권장한다.

---

# 15. Part Summary

Part 1에서는 Function Contract의 목적과 기본 모델을 정의하였다.

Function Contract는 Name, Parameter 및 Return Type으로 구성되는 호출 가능한 Canonical Model이며, Parameter와 Type Contract를 재사용한다.

Part 2에서는 Function Structure와 공통 필드를 정의한다.

---

# Part 2. Function Structure

---

# 16. Overview

본 장에서는 Function Contract의 공통 구조를 정의한다.

모든 Function은 동일한 구조를 공유하며, Generator는 이를 기반으로 Artifact를 생성한다.

---

# 17. Canonical Structure

Function Contract의 Canonical Structure는
Section 8의 Canonical Composition으로 정의된다.

Generator, Validator, Serializer,
Documentation Generator 및
기타 모든 도구는
Section 8의 Canonical Composition을
Function Contract의 유일한 구조 정의로 사용해야 한다.

Generator는 Canonical Composition을 변경해서는 안 된다.

---

# 18. Name

Name은 Function의 공식 이름이다.

요구사항

- Empty String을 허용하지 않는다.
- Namespace 내부에서 유일해야 한다.
- Generator는 Name을 변경하지 않는다.

예시

```
broadcast

teleport

randomInteger

size
```

---

# 19. Namespace

Namespace는 Function을 논리적으로 구분한다.

예시

```
skript.player

skript.world

skript.math
```

Namespace는 Identity 생성에도 사용될 수 있다.

---

# 20. Parameters

Parameters는 Function의 입력이다.

모든 Parameter는 CONTRACT_PARAMETER를 사용한다.

```
Function

└── Parameter[]
```

Parameter 순서는 호출 순서를 의미한다.

Generator는 순서를 변경해서는 안 된다.

---

# 21. Return Type

Return Type은 Function의 결과 타입이다.

Return Type은 CONTRACT_TYPE를 참조한다.

```
Function

↓

TypeRef

↓

Type Contract
```

반환값이 없는 경우에도 Void Type을 사용한다.

---

# 22. Generic Parameters

Function은 Generic Parameter를 가질 수 있다.

예시

```
<T>

<T extends Entity>

<K,V>
```

Generic Parameter 역시 Type Contract를 참조한다.

---

# 23. Attributes

Attributes는 Function의 추가적인 의미를 표현한다.

예시

- Deprecated
- Experimental
- Async
- Pure
- Internal

Attribute는 Function Signature를 변경하지 않는다.

---

# 24. Documentation

Documentation은 Function 설명을 제공한다.

대표 항목

- Summary
- Description
- Parameters
- Return Value
- Example
- Remarks

Generator는 Documentation을 API 문서 생성에 활용할 수 있다.

---

# 25. Metadata

Metadata는 Function Contract 자체를 설명하는 정보이다.

예시

- Source
- Provider
- Tags
- Created At
- Updated At

Metadata는 Runtime 의미를 변경하지 않는다.

---

# 26. Serialization

Function Contract SHALL support deterministic serialization.

Supported formats

- JSON
- YAML

The same Function Contract SHALL always produce the same serialized representation.

Serialization SHALL preserve the Canonical Composition defined by this Contract.

---

# 27. Validation

Function Contract는 최소한 다음 항목을 검증해야 한다.

- Name 존재 여부
- Namespace 존재 여부
- Parameter 유효성
- Return Type 존재 여부
- Generic Parameter 유효성

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 28. Structural Constraints

Function Contract는 다음 구조적 제약을 만족해야 한다.

- Identity는 변경할 수 없다.
- Name은 Namespace 내부에서 유일해야 한다.
- Parameter 순서는 유지되어야 한다.
- Return Type은 반드시 하나 존재한다.
- Generic Parameter는 중복될 수 없다.

---

# 29. Signature Resolution

Function Signature는 다음 요소를 기준으로 구성된다.

```
Namespace

+

Name

+

Parameter Types

+

Generic Parameters
```

Return Type은 Signature 식별 기준에 포함하지 않는다.

---

# 30. Contract Example

개념적인 예시는 다음과 같다.

```
Function

Name:
    teleport

Parameters:
    Player
    Location

Return Type:
    Void
```

JSON 표현은 Serialization Specification에서 정의한다.

---

# 31. Part Summary

Part 2에서는 Function Contract의 공통 구조를 정의하였다.

Function은 Name, Namespace, Parameter, Return Type을 중심으로 구성되며, Generic Parameter와 Documentation을 통해 다양한 호출 모델을 표현한다.

Part 3에서는 Function Signature, Overload Model 및 Validation Rules를 정의한다.

---

# Part 3. Signature Model, Overload & Validation

---

# 32. Overview

본 장에서는 Function Contract의 Signature Model, Overload 규칙 및 Validation 절차를 정의한다.

Function Signature는 Registry에서 Function을 식별하고 Generator가 호출 모델을 생성하는 기준이 된다.

---

# 33. Signature Model

Function Signature는 Function을 식별하는 Canonical Identifier이다.

Signature는 다음 요소로 구성된다.

```
Namespace

+

Name

+

Parameter Types

+

Generic Parameters
```

Return Type은 Signature 식별에 포함되지 않는다.

---

# 34. Signature Equality

두 Function은 다음 조건이 모두 동일하면 같은 Signature를 가진다.

- Namespace
- Name
- Parameter 개수
- Parameter Type 순서
- Generic Parameter 구성

Signature가 동일하면 동일한 Function으로 간주한다.

---

# 35. Overload Model

Function은 Overload를 지원할 수 있다.

예시

```
print(String)

print(Number)

print(Player)
```

각 Overload는 서로 다른 Signature를 가져야 한다.

---

# 36. Overload Resolution

Generator와 Validator는 다음 순서로 Overload를 선택한다.

```
Function Name

↓

Parameter Count

↓

Parameter Types

↓

Generic Resolution
```

동일한 후보가 둘 이상 남는 경우 Ambiguous Function으로 처리한다.

---

# 37. Generic Resolution

Generic Function은 호출 시 Generic Parameter를 결정한다.

예시

```
identity<T>(value:T)

↓

identity<Player>()
```

모든 Generic Constraint는 호출 전에 검증되어야 한다.

---

# 38. Return Type Rules

Function은 반드시 하나의 Return Type을 가진다.

Return Type은 다음 중 하나이다.

- Type Contract
- Void Type

Return Type만 다른 Function을 별도의 Overload로 정의해서는 안 된다.

---

# 39. Parameter Compatibility

호출 시 Parameter는 선언된 Type과 호환되어야 한다.

검사 항목

- Parameter Count
- Required Parameter
- Optional Parameter
- Variadic Parameter
- Type Compatibility

모든 Parameter 검증이 완료된 후 Function 호출이 가능하다.

---

# 40. Generic Constraints

Generic Parameter는 Constraint를 가질 수 있다.

예시

```
<T extends Entity>
```

Constraint를 만족하지 않으면 해당 Function은 호출 대상에서 제외된다.

---

# 41. Resolution Failures

다음 상황은 Resolution 실패로 간주한다.

- Function 없음
- Signature 불일치
- Generic Constraint 실패
- Parameter Type 불일치
- Ambiguous Overload

Generator는 적절한 Diagnostic을 생성해야 한다.

---

# 42. Validation Rules

Validation은 다음 순서로 수행한다.

```
Identity

↓

Signature

↓

Parameter Validation

↓

Return Type Validation

↓

Generic Validation

↓

Overload Validation
```

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 43. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Duplicate Signature
- Duplicate Generic Parameter
- Unknown Return Type
- Unknown Parameter Type
- Ambiguous Overload
- Invalid Generic Constraint

가능한 한 여러 오류를 한 번에 보고하는 것을 권장한다.

---

# 44. Compatibility Guarantees

Function Contract는 다음 사항을 보장한다.

- Stable Signature
- Stable Parameter Ordering
- Stable Generic Model
- Stable Return Type
- Stable Serialization

---

# 45. Resolution Architecture

Function Resolution은 다음 절차를 따른다.

```
Lookup

↓

Signature Match

↓

Generic Resolution

↓

Constraint Validation

↓

Resolved Function
```

---

# 46. Model Guarantees

Function Contract는 다음을 보장한다.

- Deterministic Resolution
- Stable Overload Model
- Language Independence
- Generator Compatibility
- Runtime Independence

---

# 47. Cross Contract Dependencies

Function Contract는 다음 Contract를 참조한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE

Function은 Parameter와 Type을 직접 정의하지 않는다.

---

# 48. Part Summary

Part 3에서는 Function Signature, Overload Model 및 Validation Rules를 정의하였다.

Function은 Signature를 기준으로 식별되며, Overload와 Generic Resolution을 통해 다양한 호출 모델을 지원한다.

Part 4에서는 Generator Integration, Serialization 및 Runtime Compatibility를 정의한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 49. Overview

본 장에서는 Function Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의한다.

Function Contract는 Generator가 사용하는 Canonical Function Model이며, Runtime은 Generator가 생성한 Artifact를 통해 Function 정보를 소비한다.

---

# 50. Serialization Principles

Function Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Function Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 Function의 의미가 변경되어서는 안 된다.

---

# 51. Deserialization

직렬화된 Function Contract는 동일한 Function Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Reference Resolution

↓

Signature Validation

↓

Ready
```

복원 이후 Function Contract는 Generator가 사용할 수 있는 상태여야 한다.

---

# 52. Generator Input

Generator는 Function Contract를 공식 입력으로 사용한다.

```
Function Contract

↓

Generator
```

Generator는 Function의 구조를 변경하지 않는다.

---

# 53. Artifact Mapping

Function Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Function Contract

├── TypeScript Function
├── Language Server Symbol
├── Completion Item
├── Signature Help
├── Documentation
└── Runtime Metadata
```

모든 Artifact는 동일한 의미를 유지해야 한다.

---

# 54. Documentation Generation

Generator는 Function Documentation을 기반으로 문서를 생성할 수 있다.

대표 항목

- Name
- Namespace
- Parameters
- Return Type
- Summary
- Description
- Examples
- Remarks

Documentation은 Function의 동작 의미를 변경하지 않는다.

---

# 55. Runtime Compatibility

Runtime은 Function Contract를 직접 소비하지 않는다.

Runtime은 Generator가 생성한 Artifact만 사용한다.

```
Function Contract

↓

Generator

↓

Artifact

↓

Runtime
```

Runtime은 Contract를 수정해서는 안 된다.

---

# 56. Diagnostics

Generator는 Function 처리 과정에서 다음 Diagnostic을 생성할 수 있다.

예시

- Duplicate Signature
- Unknown Return Type
- Unknown Parameter Type
- Invalid Generic Constraint
- Invalid Overload
- Missing Documentation

가능한 한 여러 Diagnostic을 함께 제공하는 것을 권장한다.

---

# 57. Compatibility Rules

Function Contract는 다음 사항을 보장해야 한다.

- Stable Identity
- Stable Signature
- Stable Parameter Ordering
- Stable Generic Model
- Stable Return Type

기존 Function의 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 58. Extension Support

Function Contract는 향후 다음 기능을 지원할 수 있다.

- Named Parameters
- Keyword Parameters
- Default Arguments
- Async Functions
- Coroutine Functions
- Callable Objects

확장은 기존 Generator와의 호환성을 유지해야 한다.

---

# 59. Runtime Independence

Function Contract SHALL NOT depend on any specific Runtime implementation.

The same Function Contract SHALL preserve identical semantics across
different Runtime implementations.

Runtime implementations SHALL NOT modify the canonical definition
of the Function Contract.

Examples

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 60. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Signature를 변경하지 않는다.
- Parameter를 변경하지 않는다.
- Return Type을 변경하지 않는다.
- Generic 정보를 유지한다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 61. Function Lifecycle

Function Contract는 다음 생명주기를 따른다.

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

Part 4에서는 Function Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Function Contract는 프로젝트의 공식 Function Model이며, Generator는 이를 기반으로 다양한 Artifact를 생성하고 Runtime은 생성된 Artifact를 소비한다.

Part 5에서는 Best Practices, Anti-Patterns, Compliance 및 Function Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Governance

---

# 63. Overview

본 장에서는 Function Contract 설계 시 따라야 하는 Best Practices와 Anti-Patterns를 정의한다.

Function Contract는 프로젝트 전체의 호출 모델을 정의하는 Canonical Contract이며, 장기적인 안정성과 일관성을 유지해야 한다.

---

# 64. Best Practices

다음 사항을 권장한다.

- Function은 Registry에서 관리한다.
- Identity를 변경하지 않는다.
- Signature를 안정적으로 유지한다.
- Parameter Contract를 재사용한다.
- Return Type은 Type Contract를 참조한다.
- Generic Parameter를 명확하게 정의한다.
- Documentation을 함께 제공한다.
- Overload는 최소한으로 사용한다.

---

# 65. Anti-Patterns

다음 구현은 권장하지 않는다.

- Parameter를 Function 내부에서 직접 정의하는 경우
- Return Type을 문자열로 표현하는 경우
- Signature 없이 Function을 식별하는 경우
- Return Type만 다른 Overload를 정의하는 경우
- Parameter 순서를 Generator가 변경하는 경우
- Runtime 전용 정보를 Contract에 포함하는 경우
- Generator 전용 필드를 Contract에 추가하는 경우

이러한 구현은 Contract의 안정성과 Generator의 예측 가능성을 저하시킨다.

---

# 66. Evolution Strategy

Function Contract는 장기적인 확장을 고려하여 설계한다.

권장 방식

- Optional Field 추가
- Attribute 추가
- Documentation 확장
- Generic 기능 확장
- Extension 사용

기존 Signature의 의미를 변경하는 것은 최소화한다.

---

# 67. Compatibility

Function Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Identity 변경
- Signature 변경
- Parameter Type 변경
- Parameter 순서 변경
- Return Type 의미 변경
- Generic Constraint 변경

---

# 68. Compliance

모든 Function Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- CONTRACT_PARAMETER
- CONTRACT_TYPE
- 관련 Specification
- ADR
- RFC

Function Contract는 프로젝트 전체의 Canonical Callable Model이다.

---

# 69. Function Architecture Summary

Function Contract의 공식 구조는 다음과 같다.

```
Function
│
├── Identity
├── Name
├── Namespace
├── Signature
│
├── Parameters
│     └── Parameter Contract[]
│
├── Return Type
│     └── Type Contract
│
├── Generic Parameters
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경하지 않고 Artifact를 생성해야 한다.

---

# 70. Relationship Model

Function Contract는 다음 Contract를 재사용한다.

```
Function
│
├── Parameter Contract
├── Type Contract
├── Documentation
└── Metadata
```

다른 Contract는 Function을 참조할 수 있지만 Function 내부 구조를 재정의해서는 안 된다.

---

# 71. Future Extensions

향후 다음 기능을 지원할 수 있다.

- Async Function
- Coroutine Function
- Named Parameter
- Keyword Parameter
- Extension Function
- Callable Object
- Lambda Signature
- Partial Application

새로운 기능은 기존 Generator와의 호환성을 유지해야 한다.

---

# 72. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CONTRACT_PARAMETER
- CONTRACT_TYPE
- CONTRACT_PROPERTY
- CONTRACT_EVENT
- CONTRACT_EFFECT
- CONTRACT_EXPRESSION
- CONTRACT_CLASS
- CONTRACT_MODULE

---

# 73. Document Summary

CONTRACT_FUNCTION는 프로젝트에서 사용하는 모든 Function의 공식 표현 모델을 정의한다.

Function은 Signature를 중심으로 식별되며, Parameter Contract와 Type Contract를 재사용하여 입력과 반환 타입을 표현한다.

모든 Generator는 Function Contract를 기반으로 일관된 Artifact를 생성해야 하며, Runtime은 생성된 Artifact를 통해 Function 정보를 소비한다.

---

# 74. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Function Contract |