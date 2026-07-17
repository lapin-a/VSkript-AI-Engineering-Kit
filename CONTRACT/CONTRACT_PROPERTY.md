# Property Contract

Version: 1.0

Status: Draft

---

# Part 1. Property Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Property Contract를 정의한다.

Property Contract는 객체 또는 데이터 모델이 가지는 상태(State)를 표현하는 Canonical Model이다.

Generator는 Property Contract를 기반으로 다양한 Artifact를 생성한다.

---

# 2. Scope

본 Contract는 다음 대상에 적용된다.

- Object Property
- Class Field
- Configuration Property
- Registry Property
- Metadata Property

Variable은 별도의 Contract에서 정의한다.

---

# 3. Definition

Property는 이름(Name)과 타입(Type)을 가지는 데이터 항목이다.

Property는 실행 가능한 동작을 표현하지 않으며, 데이터의 구조와 상태를 표현한다.

---

# 4. Design Goals

Property Contract는 다음 목표를 가진다.

- Stable
- Deterministic
- Reusable
- Language Independent
- Generator Friendly

---

# 5. Canonical Representation

동일한 Property는 하나의 Property Contract만 가진다.

```
Property

↓

Property Contract
```

Generator는 Property Contract를 공식 모델로 사용한다.

---

# 6. Identity

모든 Property는 고유한 Identity를 가진다.

Identity는 다음 조건을 만족해야 한다.

- Immutable
- Globally Unique
- Stable

Identity는 Name만으로 결정되지 않는다.

---

# 7. Ownership

Property는 Registry에서 관리되거나 다른 Contract에 소속될 수 있다.

대표적인 소유자

- Class Contract
- Module Contract
- Registry

Property 자체는 소유자의 구현 방식에 의존하지 않는다.

---

# 8. Property Model

Property는 다음 요소로 구성된다.

- Name
- Type
- Attributes
- Documentation

Type은 CONTRACT_TYPE를 참조한다.

---

# 9. Type Reference

Property는 Type 정보를 직접 정의하지 않는다.

항상 Type Contract를 참조한다.

```
Property

↓

TypeRef

↓

Type Contract
```

---

# 10. Value Semantics

Property Contract는 값(Value)을 저장하지 않는다.

Property Contract는 값의 구조와 의미만 정의한다.

실제 값은 Runtime 또는 데이터 저장소가 관리한다.

---

# 11. Language Independence

Property Contract는 특정 언어에 종속되지 않는다.

동일한 Contract에서 다음을 생성할 수 있다.

- TypeScript Property
- JSON Schema Property
- OpenAPI Property
- Documentation

---

# 12. Validation

Property Contract는 Generator 이전에 Validation을 수행한다.

대표적인 검증 항목

- Duplicate Identity
- Unknown Type
- Invalid Attribute

---

# 13. Compatibility

Property Contract는 가능한 한 하위 호환성을 유지한다.

새로운 Field는 Optional로 추가하는 것을 권장한다.

---

# 14. Cross References

본 문서는 다음 문서를 기반으로 한다.

- CORE_DESIGN_PRINCIPLES
- CONTRACT_TYPE
- CONTRACT_DESIGN_PRINCIPLES

---

# 15. Part Summary

Part 1에서는 Property Contract의 목적과 기본 모델을 정의하였다.

Property Contract는 Name과 Type을 중심으로 구성되는 Canonical Property Model이며, Type Contract를 재사용하여 데이터 구조를 표현한다.

Part 2에서는 Property Structure와 공통 필드를 정의한다.

===

# Part 2. Property Structure

---

# 16. Overview

본 장에서는 Property Contract의 공통 구조를 정의한다.

모든 Property는 동일한 구조를 공유하며, Generator는 이를 기반으로 Artifact를 생성한다.

---

# 17. Canonical Structure

모든 Property Contract는 다음 구조를 따른다.

```
Property

├── Identity
├── Name
├── Namespace
├── Type
├── Default Value
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경해서는 안 된다.

---

# 18. Name

Name은 Property의 공식 이름이다.

요구사항

- Empty String을 허용하지 않는다.
- Namespace 내부에서 유일해야 한다.
- Generator는 Name을 변경하지 않는다.

예시

```
health

location

displayName

inventory
```

---

# 19. Namespace

Namespace는 Property를 논리적으로 구분한다.

예시

```
skript.player

skript.entity

skript.inventory
```

Namespace는 Identity 생성에도 사용될 수 있다.

---

# 20. Type

Property는 반드시 하나의 Type을 가진다.

Type은 CONTRACT_TYPE를 참조한다.

```
Property

↓

TypeRef

↓

Type Contract
```

Property는 Type 정보를 직접 정의하지 않는다.

---

# 21. Default Value

Property는 기본값(Default Value)을 가질 수 있다.

기본값은 선택 사항이다.

예시

```
health = 20

enabled = true

displayName = ""
```

Default Value는 선언 시점의 초기값을 의미하며 Runtime 상태를 저장하지 않는다.

---

# 22. Attributes

Attributes는 Property의 추가적인 의미를 표현한다.

예시

- ReadOnly
- WriteOnly
- Required
- Optional
- Deprecated
- Experimental
- Internal

Attribute는 Property의 타입 의미를 변경하지 않는다.

---

# 23. Documentation

Documentation은 Property에 대한 설명을 제공한다.

대표 항목

- Summary
- Description
- Default Value
- Example
- Remarks

Generator는 Documentation을 API 문서 생성에 활용할 수 있다.

---

# 24. Metadata

Metadata는 Property Contract 자체를 설명하는 정보이다.

예시

- Source
- Provider
- Tags
- Created At
- Updated At

Metadata는 Runtime 의미를 변경하지 않는다.

---

# 25. Serialization

Property Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Property Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 26. Validation

Property Contract는 최소한 다음 항목을 검증해야 한다.

- Name 존재 여부
- Namespace 존재 여부
- Type 존재 여부
- Default Value 타입 일치 여부
- Attribute 유효성

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 27. Structural Constraints

Property Contract는 다음 구조적 제약을 만족해야 한다.

- Identity는 변경할 수 없다.
- Name은 Namespace 내부에서 유일해야 한다.
- Type은 반드시 존재해야 한다.
- Default Value는 선언된 Type과 호환되어야 한다.
- Metadata는 Property 의미를 변경해서는 안 된다.

---

# 28. Default Value Rules

Default Value는 다음 원칙을 따른다.

- 생략 가능하다.
- Type과 호환되어야 한다.
- Runtime 변경값을 저장하지 않는다.
- Generator는 기본값을 변경해서는 안 된다.

---

# 29. Read/Write Semantics

Property는 접근 정책을 가질 수 있다.

대표적인 접근 정책

- Read Only
- Write Only
- Read / Write

접근 정책은 Attribute를 통해 표현한다.

---

# 30. Contract Example

개념적인 예시는 다음과 같다.

```
Property

Name:
    health

Type:
    Integer

Default Value:
    20

Attributes:
    Required
```

JSON 표현 형식은 Serialization Specification에서 정의한다.

---

# 31. Part Summary

Part 2에서는 Property Contract의 공통 구조를 정의하였다.

Property는 Name, Namespace 및 Type을 중심으로 구성되며, Default Value와 Attributes를 통해 데이터 모델의 의미를 보강한다.

Part 3에서는 Property Validation, Access Model 및 Type Compatibility를 정의한다.

---

# Part 3. Access Model, Type Compatibility & Validation

---

# 32. Overview

본 장에서는 Property Contract의 접근 모델(Access Model), 타입 호환성(Type Compatibility) 및 Validation 규칙을 정의한다.

Property는 상태(State)를 표현하는 Contract이며, 접근 방식과 타입 일관성을 유지해야 한다.

---

# 33. Access Model

Property는 접근 정책(Access Policy)을 가진다.

Access Policy는 Property의 읽기(Read)와 쓰기(Write) 가능 여부를 정의한다.

지원되는 정책은 다음과 같다.

- Read Only
- Write Only
- Read / Write

Access Policy는 Attribute를 통해 표현한다.

---

# 34. Read Only Property

Read Only Property는 값을 조회할 수 있지만 수정할 수 없다.

대표적인 예시

```
entity.id

server.version

player.uuid
```

Generator는 대상 언어에서 가능한 경우 읽기 전용 속성으로 생성해야 한다.

---

# 35. Write Only Property

Write Only Property는 값을 설정할 수 있지만 조회할 수 없다.

대표적인 예시

```
password

secretKey
```

Write Only Property는 특별한 경우에만 사용하는 것을 권장한다.

---

# 36. Read / Write Property

Read / Write Property는 값을 조회하고 수정할 수 있다.

대표적인 예시

```
health

displayName

location
```

명시적인 Attribute가 없는 경우 기본 접근 정책은 Read / Write이다.

---

# 37. Type Compatibility

Property에 할당되는 값은 선언된 Type과 호환되어야 한다.

검사 항목

- Primitive Compatibility
- Complex Type Compatibility
- Collection Compatibility
- Generic Compatibility

호환성 판단은 CONTRACT_TYPE의 규칙을 따른다.

---

# 38. Nullable Support

Property는 Nullable 여부를 명시할 수 있다.

Nullable 여부는 Type 자체가 아니라 Property의 제약 조건이다.

예시

```
displayName : String?

owner : Player?
```

Nullable 여부는 Attribute 또는 Constraint를 통해 표현한다.

---

# 39. Default Value Validation

Default Value는 다음 조건을 만족해야 한다.

- 선언된 Type과 호환된다.
- Generic Constraint를 위반하지 않는다.
- Nullable 정책을 위반하지 않는다.

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 40. Property Constraints

Property는 추가적인 제약 조건을 가질 수 있다.

예시

- Minimum Value
- Maximum Value
- Minimum Length
- Maximum Length
- Pattern
- Range
- Enumeration

Constraint는 Property의 유효 범위를 정의하며 Type 자체를 변경하지 않는다.

---

# 41. Validation Rules

Validation은 다음 순서로 수행한다.

```
Identity

↓

Type Resolution

↓

Access Policy Validation

↓

Default Value Validation

↓

Constraint Validation

↓

Metadata Validation
```

Validation 실패 시 Property는 Registry에 등록되어서는 안 된다.

---

# 42. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Type
- Invalid Default Value
- Invalid Constraint
- Duplicate Property Identity
- Invalid Attribute
- Nullable Violation

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 43. Compatibility Guarantees

Property Contract는 다음 사항을 보장한다.

- Stable Identity
- Stable Type Reference
- Stable Access Policy
- Stable Default Value
- Stable Serialization

---

# 44. Property Resolution

Property Resolution은 다음 절차를 따른다.

```
Lookup

↓

Type Resolution

↓

Constraint Validation

↓

Access Validation

↓

Resolved Property
```

---

# 45. Cross Contract Dependencies

Property Contract는 다음 Contract를 참조한다.

- CONTRACT_TYPE

Property는 Type 정보를 직접 포함하지 않으며 항상 Type Contract를 참조한다.

---

# 46. Model Guarantees

Property Contract는 다음 사항을 보장한다.

- Stable Property Identity
- Stable Type Model
- Deterministic Validation
- Language Independence
- Generator Compatibility

---

# 47. Property Lifecycle

Property Contract는 다음 생명주기를 따른다.

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

# 48. Part Summary

Part 3에서는 Property의 접근 모델, 타입 호환성 및 Validation 규칙을 정의하였다.

Property는 Access Policy와 Constraint를 통해 상태(State)의 의미를 표현하며, Type Contract를 기반으로 일관된 타입 검증을 수행한다.

Part 4에서는 Generator Integration, Serialization 및 Runtime Compatibility를 정의한다.

---

# Part 3. Access Model, Type Compatibility & Validation

---

# 32. Overview

본 장에서는 Property Contract의 접근 모델(Access Model), 타입 호환성(Type Compatibility) 및 Validation 규칙을 정의한다.

Property는 상태(State)를 표현하는 Contract이며, 접근 방식과 타입 일관성을 유지해야 한다.

---

# 33. Access Model

Property는 접근 정책(Access Policy)을 가진다.

Access Policy는 Property의 읽기(Read)와 쓰기(Write) 가능 여부를 정의한다.

지원되는 정책은 다음과 같다.

- Read Only
- Write Only
- Read / Write

Access Policy는 Attribute를 통해 표현한다.

---

# 34. Read Only Property

Read Only Property는 값을 조회할 수 있지만 수정할 수 없다.

대표적인 예시

```
entity.id

server.version

player.uuid
```

Generator는 대상 언어에서 가능한 경우 읽기 전용 속성으로 생성해야 한다.

---

# 35. Write Only Property

Write Only Property는 값을 설정할 수 있지만 조회할 수 없다.

대표적인 예시

```
password

secretKey
```

Write Only Property는 특별한 경우에만 사용하는 것을 권장한다.

---

# 36. Read / Write Property

Read / Write Property는 값을 조회하고 수정할 수 있다.

대표적인 예시

```
health

displayName

location
```

명시적인 Attribute가 없는 경우 기본 접근 정책은 Read / Write이다.

---

# 37. Type Compatibility

Property에 할당되는 값은 선언된 Type과 호환되어야 한다.

검사 항목

- Primitive Compatibility
- Complex Type Compatibility
- Collection Compatibility
- Generic Compatibility

호환성 판단은 CONTRACT_TYPE의 규칙을 따른다.

---

# 38. Nullable Support

Property는 Nullable 여부를 명시할 수 있다.

Nullable 여부는 Type 자체가 아니라 Property의 제약 조건이다.

예시

```
displayName : String?

owner : Player?
```

Nullable 여부는 Attribute 또는 Constraint를 통해 표현한다.

---

# 39. Default Value Validation

Default Value는 다음 조건을 만족해야 한다.

- 선언된 Type과 호환된다.
- Generic Constraint를 위반하지 않는다.
- Nullable 정책을 위반하지 않는다.

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 40. Property Constraints

Property는 추가적인 제약 조건을 가질 수 있다.

예시

- Minimum Value
- Maximum Value
- Minimum Length
- Maximum Length
- Pattern
- Range
- Enumeration

Constraint는 Property의 유효 범위를 정의하며 Type 자체를 변경하지 않는다.

---

# 41. Validation Rules

Validation은 다음 순서로 수행한다.

```
Identity

↓

Type Resolution

↓

Access Policy Validation

↓

Default Value Validation

↓

Constraint Validation

↓

Metadata Validation
```

Validation 실패 시 Property는 Registry에 등록되어서는 안 된다.

---

# 42. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Type
- Invalid Default Value
- Invalid Constraint
- Duplicate Property Identity
- Invalid Attribute
- Nullable Violation

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 43. Compatibility Guarantees

Property Contract는 다음 사항을 보장한다.

- Stable Identity
- Stable Type Reference
- Stable Access Policy
- Stable Default Value
- Stable Serialization

---

# 44. Property Resolution

Property Resolution은 다음 절차를 따른다.

```
Lookup

↓

Type Resolution

↓

Constraint Validation

↓

Access Validation

↓

Resolved Property
```

---

# 45. Cross Contract Dependencies

Property Contract는 다음 Contract를 참조한다.

- CONTRACT_TYPE

Property는 Type 정보를 직접 포함하지 않으며 항상 Type Contract를 참조한다.

---

# 46. Model Guarantees

Property Contract는 다음 사항을 보장한다.

- Stable Property Identity
- Stable Type Model
- Deterministic Validation
- Language Independence
- Generator Compatibility

---

# 47. Property Lifecycle

Property Contract는 다음 생명주기를 따른다.

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

# 48. Part Summary

Part 3에서는 Property의 접근 모델, 타입 호환성 및 Validation 규칙을 정의하였다.

Property는 Access Policy와 Constraint를 통해 상태(State)의 의미를 표현하며, Type Contract를 기반으로 일관된 타입 검증을 수행한다.

Part 4에서는 Generator Integration, Serialization 및 Runtime Compatibility를 정의한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 49. Overview

본 장에서는 Property Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의한다.

Property Contract는 Generator가 사용하는 Canonical Property Model이며, Runtime은 Generator가 생성한 Artifact를 통해 Property 정보를 소비한다.

---

# 50. Serialization Principles

Property Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Property Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 Property의 의미가 변경되어서는 안 된다.

---

# 51. Deserialization

직렬화된 Property Contract는 동일한 Property Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Type Resolution

↓

Constraint Validation

↓

Ready
```

복원 이후 Property Contract는 Generator가 사용할 수 있는 상태여야 한다.

---

# 52. Generator Input

Generator는 Property Contract를 공식 입력으로 사용한다.

```
Property Contract

↓

Generator
```

Generator는 Property 구조를 변경하지 않는다.

---

# 53. Artifact Mapping

Property Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Property Contract

├── TypeScript Property
├── JSON Schema Property
├── OpenAPI Property
├── Runtime Metadata
├── Documentation
└── Registry Metadata
```

모든 Artifact는 동일한 의미를 유지해야 한다.

---

# 54. Documentation Generation

Generator는 Property Documentation을 기반으로 문서를 생성할 수 있다.

대표 항목

- Name
- Namespace
- Type
- Optionality
- Default Value
- Summary
- Description
- Examples

Documentation은 Property 의미를 변경하지 않는다.

---

# 55. Runtime Compatibility

Runtime은 Property Contract를 직접 해석하지 않는다.

Runtime은 Generator가 생성한 Artifact를 소비한다.

```
Property Contract

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

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Type
- Invalid Default Value
- Invalid Optionality
- Invalid Constraint
- Duplicate Identity
- Invalid Metadata

Diagnostic은 가능한 한 여러 오류를 함께 제공하는 것을 권장한다.

---

# 57. Compatibility Rules

Property Contract는 다음 사항을 보장해야 한다.

- Stable Identity
- Stable Type Reference
- Stable Optionality
- Stable Constraint Model
- Stable Serialization

Property 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 58. Extension Support

Property Contract는 향후 다음 기능을 지원할 수 있다.

- Computed Property
- Lazy Property
- Observable Property
- Derived Property
- Cached Property

확장은 기존 Generator와의 호환성을 유지해야 한다.

---

# 59. Runtime Independence

Property Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Property Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 60. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Type Reference를 변경하지 않는다.
- Optionality를 유지한다.
- Constraint를 유지한다.
- Default Value를 유지한다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 61. Property Lifecycle

Property Contract는 다음 생명주기를 따른다.

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

Part 4에서는 Property Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Property Contract는 프로젝트의 공식 Property Model이며, Generator는 이를 기반으로 다양한 Artifact를 생성하고 Runtime은 생성된 Artifact를 소비한다.

Part 5에서는 Best Practices, Anti-Patterns 및 Property Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Governance

---

# 63. Overview

본 장에서는 Property Contract 설계 시 따라야 하는 Best Practices와 Anti-Patterns를 정의한다.

Property Contract는 프로젝트 전체의 상태(State)를 표현하는 Canonical Contract이며, 장기적인 안정성과 일관성을 유지해야 한다.

---

# 64. Best Practices

다음 사항을 권장한다.

- Property는 Registry 또는 Owner Contract를 통해 관리한다.
- Identity를 변경하지 않는다.
- Type Contract를 재사용한다.
- Optionality를 명시적으로 정의한다.
- Default Value는 선언 정보만 표현한다.
- Documentation을 함께 제공한다.
- Constraint를 독립적으로 정의한다.

---

# 65. Anti-Patterns

다음 구현은 권장하지 않는다.

- Type을 문자열로 표현하는 경우
- Runtime 값을 Contract에 저장하는 경우
- Optionality를 Type에 포함시키는 경우
- Constraint 없이 범위를 암묵적으로 정의하는 경우
- Generator 전용 필드를 추가하는 경우
- 동일 Property를 중복 정의하는 경우

이러한 구현은 Contract의 일관성과 Generator의 예측 가능성을 저하시킨다.

---

# 66. Evolution Strategy

Property Contract는 장기적인 확장을 고려하여 설계한다.

권장 방식

- Optional Field 추가
- Optionality 확장
- Constraint 확장
- Documentation 확장
- Extension 사용

기존 Property의 의미를 변경하는 것은 최소화한다.

---

# 67. Compatibility

Property Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Identity 변경
- Type 변경
- Optionality 변경
- Constraint 의미 변경
- Default Value 의미 변경

---

# 68. Compliance

모든 Property Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- CONTRACT_TYPE
- 관련 Specification
- ADR
- RFC

Property Contract는 프로젝트 전체의 Canonical Property Model이다.

---

# 69. Property Architecture Summary

Property Contract의 공식 구조는 다음과 같다.

```
Property
│
├── Identity
├── Name
├── Namespace
│
├── Type
│     └── Type Contract
│
├── Optionality
│     ├── Required
│     ├── Optional
│     └── Nullable
│
├── Default Value
├── Constraints
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경하지 않고 Artifact를 생성해야 한다.

---

# 70. Relationship Model

Property Contract는 다음 Contract에서 재사용된다.

```
Property
│
├── Class
├── Event
├── Module
├── Configuration
├── Registry Entry
└── Metadata Model
```

Property는 데이터 구조를 표현하며 동작(Behavior)을 포함하지 않는다.

---

# 71. Future Extensions

향후 다음 기능을 지원할 수 있다.

- Computed Property
- Lazy Property
- Observable Property
- Immutable Property
- Indexed Property
- Derived Property

새로운 기능은 기존 Generator와의 호환성을 유지해야 한다.

---

# 72. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CONTRACT_TYPE
- CONTRACT_CLASS
- CONTRACT_MODULE
- CONTRACT_EVENT
- CONTRACT_EXPRESSION

---

# 73. Document Summary

CONTRACT_PROPERTY는 프로젝트에서 사용하는 모든 Property의 공식 표현 모델을 정의한다.

Property는 Type Contract를 기반으로 상태(State)를 표현하며, Optionality와 Constraint를 통해 데이터의 의미를 명확하게 정의한다.

Generator는 Property Contract를 기반으로 일관된 Artifact를 생성하며, Runtime은 생성된 Artifact를 소비한다.

---

# 74. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Property Contract |