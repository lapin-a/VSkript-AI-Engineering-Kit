# Type Contract

Version: 1.0

Status: Draft

---

# Part 1. Type Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Type Contract를 정의한다.

Type Contract는 프로젝트에서 사용하는 모든 데이터 형식의 Canonical Representation이다.

Generator와 Runtime은 Type Contract를 기준으로 타입 정보를 해석한다.

---

# 2. Scope

Type Contract는 다음 Contract에서 공통적으로 사용된다.

- Parameter Contract
- Property Contract
- Function Contract
- Event Contract
- Effect Contract
- Expression Contract
- Class Contract
- Module Contract

---

# 3. Definition

Type Contract는 하나의 Type을 표현하는 공식 데이터 계약이다.

Type은 Primitive Type과 Complex Type을 모두 포함한다.

---

# 4. Design Goals

Type Contract는 다음 목표를 가진다.

- Stable
- Reusable
- Deterministic
- Language Independent
- Generator Friendly

---

# 5. Canonical Representation

동일한 Type은 하나의 Type Contract만 가진다.

```
Player

↓

Type Contract
```

Generator는 Type Contract를 공식적인 타입 정보로 사용한다.

---

# 6. Identity

모든 Type은 고유한 Identity를 가진다.

Identity는 다음 조건을 만족한다.

- Immutable
- Globally Unique
- Stable

---

# 7. Ownership

Type은 Registry에서 관리된다.

```
Registry

└── Type Contract
```

Type은 다른 Contract에 포함될 수 있지만, 소유자는 Registry이다.

---

# 8. Type Categories

Type은 다음 범주 중 하나에 속한다.

- Primitive
- Complex
- Collection
- Generic
- Alias

새로운 Category는 Specification을 통해 추가한다.

---

# 9. Language Independence

Type Contract는 특정 언어에 종속되지 않는다.

동일한 Type Contract에서 다음 Artifact를 생성할 수 있다.

- TypeScript Type
- JSON Schema
- OpenAPI Schema
- LSP Type

---

# 10. Stability

Type의 의미는 Version 변경 없이 변경해서는 안 된다.

의미 변경이 필요한 경우 새로운 Version 또는 새로운 Type을 정의한다.

---

# 11. Validation

Type Contract는 Generator 이전에 Validation을 수행한다.

대표적인 검증 항목

- Duplicate Identity
- Invalid Namespace
- Invalid Category
- Circular Alias

---

# 12. Compatibility

Type Contract는 장기적인 하위 호환성을 유지한다.

새로운 필드는 Optional로 추가하는 것을 권장한다.

---

# 13. Cross References

본 문서는 다음 문서를 기반으로 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES

---

# 14. Terminology

본 문서에서 "Type"은 Contract를 의미하며, 특정 언어의 타입 시스템을 의미하지 않는다.

언어별 타입은 Generator가 생성하는 Artifact이다.

---

# 15. Part Summary

Part 1에서는 Type Contract의 목적과 기본 철학을 정의하였다.

Type Contract는 프로젝트의 모든 데이터 형식을 표현하는 Canonical Model이며, Parameter를 비롯한 모든 Contract의 공통 기반이 된다.

Part 2에서는 Type Structure와 공통 필드를 정의한다.

---

# Part 2. Type Structure

---

# 16. Overview

본 장에서는 Type Contract의 공통 구조를 정의한다.

모든 Type은 동일한 구조를 공유하며, Generator는 이를 기반으로 Artifact를 생성한다.

---

# 17. Canonical Structure

모든 Type Contract는 다음 구조를 따른다.

```
Type

├── Identity
├── Name
├── Namespace
├── Category
├── Base Type
├── Generic Arguments
├── Attributes
├── Documentation
└── Metadata
```

---

# 18. Name

Name은 Type의 공식 이름이다.

요구사항

- Empty String을 허용하지 않는다.
- Namespace 내부에서 유일해야 한다.
- Generator는 Name을 변경해서는 안 된다.

예시

```
Player
World
Location
Inventory
Number
```

---

# 19. Namespace

Namespace는 Type를 논리적으로 구분하기 위한 영역이다.

예시

```
skript.entity

skript.world

skript.inventory
```

Namespace는 Identity 생성에도 사용될 수 있다.

---

# 20. Category

Category는 Type의 종류를 나타낸다.

허용되는 Category

- Primitive
- Complex
- Collection
- Generic
- Alias

Category는 Type의 의미를 분류하기 위한 메타 정보이다.

---

# 21. Base Type

Base Type은 현재 Type이 상속하거나 확장하는 상위 Type이다.

예시

```
Entity

↓

Player
```

Base Type은 Type Contract를 참조한다.

---

# 22. Generic Arguments

Generic Type은 하나 이상의 Generic Argument를 가질 수 있다.

예시

```
List<Player>

Map<String, Player>
```

Generic Argument는 모두 Type Contract를 참조한다.

---

# 23. Attributes

Attributes는 Type의 추가적인 특성을 표현한다.

예시

- Abstract
- Final
- Deprecated
- Experimental
- Internal

Attribute는 Type 자체의 의미를 변경하지 않는다.

---

# 24. Documentation

Documentation은 Type에 대한 설명을 제공한다.

대표 항목

- Summary
- Description
- Example
- Remarks

Generator는 Documentation을 API 문서 생성에 활용할 수 있다.

---

# 25. Metadata

Metadata는 Type Contract 자체를 설명하는 정보이다.

예시

- Source
- Provider
- Tags
- Created At
- Updated At

Metadata는 Runtime 의미를 변경하지 않는다.

---

# 26. Serialization

Type Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Type Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 27. Validation

Type Contract는 최소한 다음 항목을 검증해야 한다.

- Name 존재 여부
- Namespace 존재 여부
- Identity 중복
- Category 유효성
- Base Type 존재 여부
- Generic Argument 유효성

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 28. Structural Constraints

Type Contract는 다음 구조적 제약을 만족해야 한다.

- Identity는 변경할 수 없다.
- Namespace는 비어 있을 수 없다.
- Base Type은 자기 자신을 참조할 수 없다.
- Generic Argument는 유효한 Type이어야 한다.
- Circular Inheritance를 허용하지 않는다.

---

# 29. Inheritance Rules

Type은 하나의 Base Type을 가질 수 있다.

다중 상속은 지원하지 않는다.

향후 Interface 또는 Trait 모델이 추가될 경우 별도의 Contract로 정의한다.

---

# 30. Contract Example

개념적인 예시는 다음과 같다.

```
Type

Name:
    Player

Namespace:
    skript.entity

Category:
    Complex

Base Type:
    Entity
```

JSON 표현 형식은 Serialization Specification에서 정의한다.

---

# 31. Part Summary

Part 2에서는 Type Contract의 공통 구조를 정의하였다.

Type은 Name, Namespace 및 Category를 중심으로 구성되며, Base Type과 Generic Arguments를 통해 계층 구조와 타입 관계를 표현한다.

Part 3에서는 Type System, Alias, Generic Model 및 Validation Rules를 정의한다.

---

# Part 3. Type System, Generic Model & Validation

---

# 32. Overview

본 장에서는 Type Contract의 Type System과 Generic Model을 정의한다.

Type System은 프로젝트 전체에서 사용하는 모든 타입 관계를 표현하는 공식 모델이다.

---

# 33. Category

Category는 Type의 데이터 분류를 나타낸다.

허용되는 Category

- Primitive
- Complex
- Collection

Category는 데이터의 본질적인 특성을 표현한다.

예시

```
String

Category:
Primitive
```

```
Player

Category:
Complex
```

```
List<Player>

Category:
Collection
```

---

# 34. Kind

Kind는 Type의 구조적 형태를 나타낸다.

허용되는 Kind

- Class
- Interface
- Enum
- Alias
- Generic
- Union
- Intersection
- Record

Kind는 Type의 구현 형태를 표현하며 Category와 독립적으로 관리한다.

예시

```
Player

Category:
Complex

Kind:
Class
```

---

# 35. Base Type

Type는 하나의 Base Type을 가질 수 있다.

예시

```
Entity

↓

LivingEntity

↓

Player
```

Base Type은 Type Contract를 참조한다.

다중 상속은 지원하지 않는다.

---

# 36. Generic Types

Generic Type은 하나 이상의 Generic Parameter를 가진다.

예시

```
List<T>

Map<K,V>

Optional<T>
```

Generic Parameter 역시 Type Contract를 참조한다.

---

# 37. Type References

다른 Contract는 Type 자체를 포함하지 않는다.

항상 Type Reference를 사용한다.

```
Parameter

↓

TypeRef

↓

Type Contract
```

Reference는 Identity를 기반으로 한다.

---

# 38. Alias Types

Alias는 기존 Type의 다른 이름이다.

예시

```
Number

↓

Integer
```

Alias는 새로운 Type 의미를 생성하지 않는다.

---

# 39. Union Types

Union은 여러 Type 중 하나를 허용한다.

예시

```
Player

|

Console
```

Union은 Type 집합을 표현한다.

---

# 40. Intersection Types

Intersection은 여러 Type을 동시에 만족해야 한다.

예시

```
Entity

&

Serializable
```

Intersection은 모든 Type 조건을 만족해야 한다.

---

# 41. Record Types

Record는 Key-Value 구조를 표현한다.

예시

```
Record<String, Player>
```

Record는 Collection Category에 속한다.

---

# 42. Generic Constraints

Generic Parameter는 Constraint를 가질 수 있다.

예시

```
T extends Entity
```

Constraint는 Generator가 그대로 유지해야 한다.

---

# 43. Type Compatibility

Type Compatibility는 다음 기준으로 판단한다.

- Identity
- Base Type
- Generic Compatibility
- Alias Resolution

Compatibility 판단은 Runtime이 아니라 Type System의 책임이다.

---

# 44. Circular Dependency

Type은 순환 상속을 가져서는 안 된다.

다음 구조는 허용하지 않는다.

```
A

↓

B

↓

C

↓

A
```

Validation은 이러한 구조를 감지해야 한다.

---

# 45. Validation Rules

Type Validation은 다음 순서로 수행한다.

```
Identity

↓

Category

↓

Kind

↓

Base Type

↓

Generic

↓

Constraint
```

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 46. Generator Responsibilities

Generator는 Type System을 변경하지 않는다.

Generator의 역할은

- Type Reference 유지
- Generic 유지
- Alias 유지
- Diagnostic 생성

이다.

---

# 47. Model Guarantees

Type Contract는 다음 사항을 보장한다.

- Stable Identity
- Stable Category
- Stable Kind
- Stable Generic Model
- Stable Serialization

---

# 48. Part Summary

Part 3에서는 Type System과 Generic Model을 정의하였다.

Type은 Category와 Kind를 통해 데이터의 성격과 구조를 명확히 표현하며, Base Type과 Generic Model을 통해 계층 구조와 재사용성을 제공한다.

Part 4에서는 Generator Integration, Serialization 및 Runtime Compatibility를 정의한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 49. Overview

본 장에서는 Type Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의한다.

Type Contract는 Generator가 사용하는 Canonical Type Model이며, Runtime은 Generator가 생성한 Artifact를 통해 타입 정보를 소비한다.

---

# 50. Serialization Principles

Type Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Type Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 Type의 의미가 변경되어서는 안 된다.

---

# 51. Deserialization

직렬화된 Type Contract는 동일한 Type Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Reference Resolution

↓

Type Validation

↓

Ready
```

복원 이후 Type Contract는 Generator가 사용할 수 있는 상태여야 한다.

---

# 52. Generator Input

Generator는 Type Contract를 공식 입력으로 사용한다.

예시

```
Parameter

↓

Type Contract

↓

Generator
```

Generator는 Type 구조를 변경하지 않는다.

---

# 53. Artifact Mapping

Type Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Type Contract

├── TypeScript Type
├── JSON Schema Definition
├── OpenAPI Schema
├── LSP Type Information
├── Runtime Metadata
└── Documentation
```

모든 Artifact는 동일한 의미를 유지해야 한다.

---

# 54. Documentation Generation

Generator는 Type Documentation을 기반으로 문서를 생성할 수 있다.

대표 항목

- Name
- Namespace
- Summary
- Description
- Category
- Kind
- Base Type

Documentation은 Type의 의미를 변경하지 않는다.

---

# 55. Runtime Compatibility

Runtime은 Type Contract를 직접 해석하지 않는다.

Runtime은 Generator가 생성한 Type Artifact를 사용한다.

```
Type Contract

↓

Generator

↓

Artifact

↓

Runtime
```

---

# 56. Diagnostics

Generator는 Type 처리 과정에서 다음 Diagnostic을 생성할 수 있다.

예시

- Duplicate Type Identity
- Unknown Base Type
- Invalid Generic Argument
- Circular Inheritance
- Invalid Category
- Invalid Kind

Diagnostic은 가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 57. Compatibility Rules

Type Contract는 다음 사항을 보장해야 한다.

- Stable Identity
- Stable Namespace
- Stable Category
- Stable Kind
- Stable Generic Model

기존 Type의 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 58. Extension Support

Type Contract는 향후 새로운 Type Model을 지원할 수 있다.

예시

- Tuple
- Function Type
- Callable Type
- Literal Type
- Conditional Type
- Mapped Type

확장은 기존 Generator를 손상시키지 않아야 한다.

---

# 59. Runtime Independence

Type Contract는 특정 Runtime 구현에 종속되지 않는다.

동일한 Type Contract는 여러 Runtime에서 재사용될 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 60. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Type Reference를 유지한다.
- Category를 변경하지 않는다.
- Kind를 변경하지 않는다.
- Generic 정보를 유지한다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 61. Type Lifecycle

Type Contract는 다음 생명주기를 따른다.

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

Part 4에서는 Type Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Type Contract는 프로젝트 전체의 공식 Type Model이며, 모든 Generator는 이를 기반으로 다양한 Artifact를 생성하고, Runtime은 생성된 Artifact를 소비한다.

Part 5에서는 Best Practices, Anti-Patterns, Compliance 및 Type Contract Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Governance

---

# 63. Overview

본 장에서는 Type Contract 설계 시 따라야 하는 Best Practices와 Anti-Patterns를 정의한다.

Type Contract는 프로젝트 전체의 모든 데이터 모델을 표현하는 Canonical Type Model이며, 장기적인 안정성과 일관성을 유지해야 한다.

---

# 64. Best Practices

다음 사항을 권장한다.

- 모든 Type은 Registry에서 관리한다.
- Identity는 절대 변경하지 않는다.
- Namespace를 명확하게 유지한다.
- Type Reference를 사용한다.
- Category와 Kind를 명확히 구분한다.
- Generic Model을 일관되게 사용한다.
- Documentation을 함께 제공한다.
- Type 의미를 명확하게 유지한다.

---

# 65. Anti-Patterns

다음 구현은 권장하지 않는다.

- 문자열 기반 Type 참조
- Runtime 전용 정보를 Type에 포함
- Generator 전용 필드를 추가
- 순환 상속 허용
- Identity 변경
- Namespace 없는 Type 정의
- Base Type을 문자열로 표현
- Type 중복 정의

이러한 구현은 프로젝트 전체의 타입 일관성을 저하시킨다.

---

# 66. Evolution Strategy

Type Contract는 장기적인 확장을 고려하여 설계한다.

권장 방식

- Optional Field 추가
- 새로운 Kind 추가
- 새로운 Category 추가
- Extension 사용
- Deprecated 처리

기존 Type의 의미를 변경하는 것은 최소화한다.

---

# 67. Compatibility

Type Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Identity 변경
- Namespace 변경
- Category 변경
- Kind 변경
- Base Type 의미 변경
- Generic 의미 변경

---

# 68. Compliance

모든 Type Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- 관련 Specification
- RFC
- ADR

Type Contract는 프로젝트 전체의 Canonical Type Model이다.

---

# 69. Type Architecture Summary

Type Contract의 공식 구조는 다음과 같다.

```
Type
│
├── Identity
├── Name
├── Namespace
│
├── Category
│     ├── Primitive
│     ├── Complex
│     └── Collection
│
├── Kind
│     ├── Class
│     ├── Interface
│     ├── Enum
│     ├── Alias
│     ├── Generic
│     ├── Union
│     ├── Intersection
│     └── Record
│
├── BaseType
├── GenericArguments
├── Constraints
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경하지 않고 Artifact로 변환해야 한다.

---

# 70. Relationship Model

Type Contract는 프로젝트 전체에서 다음과 같이 재사용된다.

```
Type
│
├── Parameter
├── Property
├── Variable
├── Return Type
├── Field
├── Generic Argument
└── Registry
```

모든 Contract는 Type 정보를 직접 정의하지 않고 Type Contract를 참조해야 한다.

---

# 71. Future Extensions

향후 다음 Kind를 추가할 수 있다.

- Service
- Trait
- Value Object
- Data Class
- Anonymous Type
- Dynamic Type

새로운 Kind는 기존 Generator와의 호환성을 유지해야 한다.

---

# 72. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CONTRACT_PARAMETER
- CONTRACT_FUNCTION
- CONTRACT_PROPERTY
- CONTRACT_EVENT
- CONTRACT_EFFECT
- CONTRACT_EXPRESSION
- CONTRACT_CLASS
- CONTRACT_MODULE

---

# 73. Document Summary

CONTRACT_TYPE는 프로젝트에서 사용하는 모든 타입의 공식 표현 모델을 정의한다.

Type Contract는 Category와 Kind를 중심으로 타입의 의미와 구조를 표현하며, Base Type과 Generic Model을 통해 타입 계층과 관계를 정의한다.

모든 Contract는 Type Contract를 참조하여 타입 정보를 표현해야 하며, Generator는 이를 기반으로 일관된 Artifact를 생성한다.

---

# 74. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Type Contract |