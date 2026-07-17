# Class Contract

Version: 1.0

Status: Draft

---

# Part 1. Class Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Class Contract를 정의한다.

Class Contract는 데이터와 동작을 하나의 논리적인 단위로 구성하는 Canonical Object Model이다.

Generator는 Class Contract를 기반으로 다양한 Artifact를 생성한다.

---

# 2. Scope

본 Contract는 다음 대상에 적용된다.

- Domain Class
- Registry Class
- Runtime Class
- API Class
- Data Class

Interface와 Enum은 Type Contract에서 정의하며, Class Contract는 실제 객체 모델을 정의한다.

---

# 3. Definition

Class는 Property와 Function(Method)을 소유하는 객체 모델이다.

Class Contract는 객체의 구조를 정의하며 Runtime 구현을 포함하지 않는다.

---

# 4. Design Goals

Class Contract는 다음 목표를 가진다.

- Stable
- Deterministic
- Reusable
- Language Independent
- Generator Friendly

---

# 5. Canonical Representation

동일한 Class는 하나의 Class Contract만 가진다.

```
Class

↓

Class Contract
```

Generator는 Class Contract를 공식 모델로 사용한다.

---

# 6. Identity

모든 Class는 고유한 Identity를 가진다.

Identity는 다음 조건을 만족해야 한다.

- Immutable
- Globally Unique
- Stable

Identity는 Name만으로 결정되지 않는다.

---

# 7. Ownership

Class는 Registry에서 관리된다.

```
Registry

└── Class Contract
```

다른 Contract는 Class를 참조할 수 있지만 소유자는 Registry이다.

---

# 8. Class Composition

Class는 다음 요소로 구성된다.

```
Class

├── Properties
├── Methods
├── Constructors
├── Base Class
├── Interfaces
└── Metadata
```

각 요소는 독립적인 Contract를 참조한다.

---

# 9. Property Model

Class는 하나 이상의 Property를 가질 수 있다.

모든 Property는 CONTRACT_PROPERTY를 참조한다.

```
Class

└── Property[]
```

Class는 Property를 직접 정의하지 않는다.

---

# 10. Method Model

Class는 하나 이상의 Method를 가질 수 있다.

Method는 CONTRACT_FUNCTION을 기반으로 정의한다.

```
Class

└── Function[]
```

Method는 Function Contract를 재사용한다.

---

# 11. Constructor Model

Class는 Constructor를 가질 수 있다.

Constructor는 초기화 절차를 정의하며 일반 Function과 구분된다.

Constructor의 상세 모델은 별도 Contract에서 정의한다.

---

# 12. Base Class

Class는 하나의 Base Class를 가질 수 있다.

Base Class는 다른 Class Contract를 참조한다.

다중 상속은 지원하지 않는다.

---

# 13. Interface Implementation

Class는 하나 이상의 Interface(Type Contract)를 구현할 수 있다.

Interface 정보는 Type Contract를 통해 참조한다.

---

# 14. Validation

Class Contract는 Generator 이전에 Validation을 수행한다.

대표적인 검증 항목

- Duplicate Identity
- Unknown Property
- Unknown Method
- Invalid Base Class
- Circular Inheritance

---

# 15. Part Summary

Part 1에서는 Class Contract의 목적과 기본 모델을 정의하였다.

Class Contract는 Property와 Function을 조합하여 객체 모델을 표현하며, Base Class와 Interface를 통해 계층 구조를 구성한다.

Part 2에서는 Class Structure와 공통 필드를 정의한다.

---

# Part 2. Class Structure

---

# 16. Overview

본 장에서는 Class Contract의 공통 구조를 정의한다.

모든 Class는 동일한 구조를 가지며 Generator는 이를 기반으로 Artifact를 생성한다.

---

# 17. Canonical Structure

모든 Class Contract는 다음 구조를 따른다.

```
Class

├── Identity
├── Name
├── Namespace
├── Properties
├── Methods
├── Constructors
├── Base Class
├── Implemented Interfaces
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경해서는 안 된다.

---

# 18. Name

Name은 Class의 공식 이름이다.

요구사항

- Empty String을 허용하지 않는다.
- Namespace 내부에서 유일해야 한다.
- Generator는 Name을 변경하지 않는다.

예시

```
Player

World

Location

Inventory
```

---

# 19. Namespace

Namespace는 Class를 논리적으로 구분한다.

예시

```
skript.entity

skript.player

skript.world
```

Namespace는 Identity 생성에도 사용될 수 있다.

---

# 20. Properties

Class는 0개 이상의 Property를 가진다.

모든 Property는 CONTRACT_PROPERTY를 참조한다.

```
Class

└── Property[]
```

Property의 순서는 선언 순서를 의미한다.

Generator는 순서를 변경해서는 안 된다.

---

# 21. Methods

Class는 0개 이상의 Method를 가진다.

Method는 CONTRACT_FUNCTION을 재사용한다.

```
Class

└── Function[]
```

Method의 Signature는 Class 내부에서 유일해야 한다.

---

# 22. Constructors

Class는 Constructor를 하나 이상 가질 수 있다.

Constructor는 객체 생성 규칙을 정의한다.

예시

```
Player()

Player(String)

Player(UUID)
```

Constructor는 Method와 구분된다.

---

# 23. Base Class

Class는 하나의 Base Class를 가질 수 있다.

```
Entity

↓

Player
```

Base Class는 다른 Class Contract를 참조한다.

Base Class가 없는 경우 Root Class로 간주한다.

---

# 24. Implemented Interfaces

Class는 하나 이상의 Interface를 구현할 수 있다.

Interface는 Type Contract를 참조한다.

예시

```
Player

implements

Serializable

Comparable
```

---

# 25. Attributes

Attributes는 Class의 추가적인 의미를 표현한다.

예시

- Abstract
- Final
- Sealed
- Internal
- Deprecated
- Experimental

Attribute는 Class의 의미를 변경하지 않는다.

---

# 26. Documentation

Documentation은 Class 설명을 제공한다.

대표 항목

- Summary
- Description
- Examples
- Remarks

Generator는 Documentation을 API 문서 생성에 활용할 수 있다.

---

# 27. Metadata

Metadata는 Class Contract 자체를 설명하는 정보이다.

예시

- Source
- Provider
- Tags
- Created At
- Updated At

Metadata는 Runtime 의미를 변경하지 않는다.

---

# 28. Serialization

Class Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Class Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 29. Validation

Class Contract는 최소한 다음 항목을 검증해야 한다.

- Name 존재 여부
- Namespace 존재 여부
- Property 유효성
- Method 유효성
- Base Class 유효성
- Interface 유효성

Validation 실패 시 Generator를 실행해서는 안 된다.

---

# 30. Structural Constraints

Class Contract는 다음 구조적 제약을 만족해야 한다.

- Identity는 변경할 수 없다.
- Name은 Namespace 내부에서 유일해야 한다.
- Base Class는 최대 하나이다.
- 동일 Property를 중복 정의할 수 없다.
- 동일 Method Signature를 중복 정의할 수 없다.
- Interface는 중복 구현할 수 없다.

---

# 31. Part Summary

Part 2에서는 Class Contract의 공통 구조를 정의하였다.

Class는 Property와 Method를 중심으로 구성되며, Base Class와 Interface를 통해 객체 계층 구조를 표현한다.

Part 3에서는 Inheritance Model, Member Resolution 및 Validation Rules를 정의한다.

---

# Part 3. Inheritance, Member Resolution & Validation

---

# 32. Overview

본 장에서는 Class Contract의 상속(Inheritance), Member Resolution 및 Validation 규칙을 정의한다.

Class는 Property와 Method를 계층적으로 상속하며, Generator는 일관된 Resolution 규칙을 적용해야 한다.

---

# 33. Inheritance Model

Class는 단일 상속(Single Inheritance)을 지원한다.

```
Entity

↓

LivingEntity

↓

Player
```

하나의 Class는 최대 하나의 Base Class만 가질 수 있다.

---

# 34. Inherited Members

Class는 Base Class의 Member를 상속받는다.

상속 대상

- Property
- Method

상속되지 않는 대상

- Constructor
- Metadata
- Documentation

Generator는 상속된 Member를 올바르게 해석해야 한다.

---

# 35. Member Resolution

Member 탐색은 다음 순서로 수행한다.

```
Current Class

↓

Base Class

↓

Root Class
```

동일한 이름의 Member가 존재하면 가장 가까운 Class의 Member를 우선한다.

---

# 36. Property Redeclaration

기본 정책(Default Policy)에서는 Property의 재선언(Redeclaration)을 허용하지 않는다.

동일한 상속 계층에서 같은 이름의 Property를 선언하는 경우 Validation 오류로 처리한다.

Property Override 또는 Shadowing은 Class Contract의 기본 기능이 아니다.

필요한 경우 별도의 Specification에서 다음 정책을 정의할 수 있다.

- Property Override
- Property Shadowing
- Property Alias
- Property Merge

Generator는 기본 정책(Default Policy)을 적용해야 한다.

---

# 37. Method Override

Method는 Base Class의 Method를 Override할 수 있다.

Override 조건

- 동일한 Name
- 동일한 Signature
- 반환 타입(Return Type)의 의미 유지
- 접근 수준(Access Level) 축소 금지

Generator는 Override 여부를 명확히 식별해야 한다.

---

# 38. Interface Resolution

Class는 여러 Interface를 구현할 수 있다.

Member Resolution 순서는 다음과 같다.

```
Class

↓

Base Class

↓

Implemented Interface
```

Interface는 구현을 제공하지 않으며 계약만 정의한다.

---

# 39. Circular Inheritance

순환 상속은 허용하지 않는다.

예시

```
A

↓

B

↓

C

↓

A
```

Validation은 이러한 구조를 반드시 탐지해야 한다.

---

# 40. Member Compatibility

상속된 Member는 다음 조건을 만족해야 한다.

- Type Compatibility
- Signature Compatibility
- Generic Compatibility
- Attribute Compatibility

호환되지 않는 Member는 Validation 오류로 처리한다.

---

# 41. Validation Rules

Validation은 다음 순서로 수행한다.

```
Identity

↓

Base Class Resolution

↓

Interface Resolution

↓

Property Validation

↓

Method Validation

↓

Inheritance Validation
```

Validation 실패 시 Class는 Registry에 등록되어서는 안 된다.

---

# 42. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Unknown Base Class
- Unknown Interface
- Duplicate Property
- Duplicate Method Signature
- Invalid Override
- Circular Inheritance
- Invalid Generic Resolution

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 43. Compatibility Guarantees

Class Contract는 다음 사항을 보장한다.

- Stable Identity
- Stable Member Ordering
- Stable Inheritance Model
- Stable Resolution Rules
- Stable Serialization

---

# 44. Class Resolution

Class Resolution은 다음 절차를 따른다.

```
Lookup

↓

Base Class Resolution

↓

Interface Resolution

↓

Member Resolution

↓

Resolved Class
```

---

# 45. Model Guarantees

Class Contract는 다음 사항을 보장한다.

- Stable Object Model
- Deterministic Resolution
- Language Independence
- Generator Compatibility
- Runtime Independence

---

# 46. Cross Contract Dependencies

Class Contract는 다음 Contract를 참조한다.

- CONTRACT_PROPERTY
- CONTRACT_FUNCTION
- CONTRACT_TYPE

Class는 Property와 Function을 직접 정의하지 않으며, 각각의 Contract를 참조하여 구성한다.

---

# 47. Class Lifecycle

Class Contract는 다음 생명주기를 따른다.

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

Part 3에서는 Class의 상속 모델, Member Resolution 및 Validation 규칙을 정의하였다.

Class는 단일 상속을 기반으로 Property와 Method를 계층적으로 구성하며, Generator는 정의된 Resolution 규칙을 통해 일관된 객체 모델을 생성해야 한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 49. Overview

본 장에서는 Class Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의한다.

Class Contract는 Generator가 사용하는 Canonical Object Model이며, Runtime은 Generator가 생성한 Artifact를 통해 Class 정보를 소비한다.

---

# 50. Serialization Principles

Class Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Class Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 Class의 의미가 변경되어서는 안 된다.

---

# 51. Deserialization

직렬화된 Class Contract는 동일한 Class Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Base Class Resolution

↓

Member Resolution

↓

Ready
```

복원 이후 Class Contract는 Generator가 사용할 수 있는 상태여야 한다.

---

# 52. Generator Input

Generator는 Class Contract를 공식 입력으로 사용한다.

```
Class Contract

↓

Generator
```

Generator는 Class의 구조를 변경해서는 안 된다.

---

# 53. Artifact Mapping

Class Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Class Contract

├── TypeScript Class
├── Java Class
├── Kotlin Class
├── Runtime Metadata
├── Documentation
├── Registry Metadata
└── Language Server Symbol
```

모든 Artifact는 동일한 객체 의미(Object Semantics)를 유지해야 한다.

---

# 54. Documentation Generation

Generator는 Class Documentation을 기반으로 문서를 생성할 수 있다.

대표 항목

- Name
- Namespace
- Base Class
- Implemented Interfaces
- Properties
- Methods
- Summary
- Description
- Examples

Documentation은 Class의 의미를 변경하지 않는다.

---

# 55. Runtime Compatibility

Runtime은 Class Contract를 직접 사용하지 않는다.

Runtime은 Generator가 생성한 Artifact를 소비한다.

```
Class Contract

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

- Unknown Base Class
- Unknown Interface
- Duplicate Property
- Duplicate Method
- Circular Inheritance
- Invalid Override
- Invalid Metadata

가능한 한 여러 오류를 동시에 보고하는 것을 권장한다.

---

# 57. Compatibility Rules

Class Contract는 다음 사항을 보장해야 한다.

- Stable Identity
- Stable Member Ordering
- Stable Inheritance
- Stable Interface Model
- Stable Serialization

기존 Class 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 58. Extension Support

Class Contract는 향후 다음 기능을 지원할 수 있다.

- Nested Class
- Partial Class
- Mixins
- Traits
- Companion Object
- Static Members
- Extension Members

확장은 기존 Generator와의 호환성을 유지해야 한다.

---

# 59. Runtime Independence

Class Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Class Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 60. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Class Identity를 변경하지 않는다.
- Property 순서를 유지한다.
- Method Signature를 유지한다.
- Inheritance 구조를 유지한다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 61. Class Lifecycle

Class Contract는 다음 생명주기를 따른다.

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

Part 4에서는 Class Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Class Contract는 프로젝트의 공식 객체 모델(Object Model)이며, Generator는 이를 기반으로 다양한 Artifact를 생성하고 Runtime은 생성된 Artifact를 소비한다.

Part 5에서는 Best Practices, Anti-Patterns 및 Class Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Governance

---

# 63. Overview

본 장에서는 Class Contract 설계 시 따라야 하는 Best Practices와 Anti-Patterns를 정의한다.

Class Contract는 프로젝트 전체의 객체 모델(Object Model)을 표현하는 Canonical Contract이며, 장기적인 안정성과 일관성을 유지해야 한다.

---

# 64. Best Practices

다음 사항을 권장한다.

- Class는 Registry를 통해 관리한다.
- Identity를 변경하지 않는다.
- Property와 Method는 각각의 Contract를 재사용한다.
- Base Class는 명확하게 정의한다.
- Interface는 계약(Contract)으로만 사용한다.
- Documentation을 함께 제공한다.
- Metadata는 Runtime 의미를 변경하지 않도록 유지한다.

---

# 65. Anti-Patterns

다음 구현은 권장하지 않는다.

- 순환 상속(Circular Inheritance)
- 중복 Property 선언
- 중복 Method Signature
- Runtime 상태를 Contract에 저장하는 경우
- Generator 전용 필드를 추가하는 경우
- Identity를 Name만으로 결정하는 경우

이러한 구현은 객체 모델의 일관성과 Generator의 예측 가능성을 저하시킨다.

---

# 66. Evolution Strategy

Class Contract는 장기적인 확장을 고려하여 설계한다.

권장 방식

- Optional Field 추가
- Optional Metadata 추가
- Extension Specification 추가
- Documentation 확장

기존 Class의 의미를 변경하는 것은 최소화한다.

---

# 67. Compatibility

Class Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Identity 변경
- Base Class 변경
- Property 의미 변경
- Method Signature 변경
- Interface 의미 변경

---

# 68. Compliance

모든 Class Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- CONTRACT_PROPERTY
- CONTRACT_FUNCTION
- CONTRACT_TYPE
- 관련 Specification
- ADR
- RFC

Class Contract는 프로젝트 전체의 Canonical Object Model이다.

---

# 69. Class Architecture Summary

Class Contract의 공식 구조는 다음과 같다.

```
Class
│
├── Identity
├── Name
├── Namespace
├── Properties
├── Methods
├── Constructors
├── Base Class
├── Implemented Interfaces
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경하지 않고 Artifact를 생성해야 한다.

---

# 70. Relationship Model

Class Contract는 다음 Contract를 조합하여 구성된다.

```
Class
│
├── Property[]
├── Function[]
├── Type
└── Metadata
```

Class는 Aggregate Contract이며, 다른 Contract를 조합하여 객체 모델을 표현한다.

---

# 71. Future Extensions

향후 다음 기능을 지원할 수 있다.

- Nested Class
- Partial Class
- Mixins
- Traits
- Companion Object
- Static Members
- Extension Members
- Unified Member Model

새로운 기능은 기존 Generator와의 호환성을 유지해야 하며, 필요한 경우 별도의 Specification으로 정의한다.

---

# 72. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CONTRACT_TYPE
- CONTRACT_PROPERTY
- CONTRACT_FUNCTION
- CONTRACT_EVENT
- CONTRACT_MODULE

---

# 73. Document Summary

CONTRACT_CLASS는 프로젝트에서 사용하는 모든 Class의 공식 표현 모델을 정의한다.

Class는 Property와 Function을 조합하여 객체 모델을 구성하며, 상속과 인터페이스를 통해 계층 구조를 표현한다.

Generator는 Class Contract를 기반으로 일관된 Artifact를 생성하며, Runtime은 생성된 Artifact를 소비한다.

---

# 74. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Class Contract |