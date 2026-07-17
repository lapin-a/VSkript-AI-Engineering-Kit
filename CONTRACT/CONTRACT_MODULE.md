# Module Contract

Version: 1.0

Status: Draft

---

# Part 1. Module Model

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Module Contract를 정의한다.

Module Contract는 프로젝트의 Language Elements를 조직(Organize)하는 최상위 Aggregate Model을 정의한다.

Generator는 Module Contract를 기반으로 Runtime Artifact와 Package 구조를 생성한다.

---

# 2. Scope

본 Contract는 다음 대상에 적용된다.

- Built-in Modules
- Runtime Modules
- Extension Modules
- Plugin Modules
- Library Modules

Module Loading 정책은 별도의 Specification에서 정의한다.

---

# 3. Definition

Module은 하나 이상의 Language Contract를 논리적으로 그룹화하는 Aggregate Contract이다.

Module은 실행 대상이 아니며, Language Elements를 조직하기 위한 구조적 단위이다.

---

# 4. Design Goals

Module Contract는 다음 목표를 가진다.

- Stable
- Modular
- Reusable
- Extensible
- Language Independent
- Generator Friendly

---

# 5. Canonical Representation

동일한 Module은 하나의 Module Contract만 가진다.

```
Runtime Module

↓

Module Contract
```

Generator는 Module Contract를 공식 Package Model로 사용한다.

---

# 6. Identity

모든 Module은 고유한 Identity를 가진다.

Identity는 다음 조건을 만족해야 한다.

- Immutable
- Globally Unique
- Stable

Identity는 Name만으로 결정되지 않는다.

---

# 7. Ownership

Module은 Registry에서 관리된다.

```
Registry

└── Module Contract
```

Module은 포함된 Contract들의 소유권을 변경하지 않는다.

---

# 8. Module Composition

Module은 다음 요소로 구성된다.

```
Module

├── Identity
├── Name
├── Namespace
├── Imports
├── Exports
├── Members
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경해서는 안 된다.

---

# 9. Members

Module은 하나 이상의 Language Element를 포함할 수 있다.

예시

- Class
- Function
- Expression
- Effect
- Event
- Type

Module은 포함 관계를 정의할 뿐 각 Contract의 의미를 변경하지 않는다.

---

# 10. Imports

Module은 다른 Module을 참조할 수 있다.

Import는 의존성을 표현하지만 구현을 복사하지 않는다.

---

# 11. Exports

Module은 외부에 공개할 Contract를 정의할 수 있다.

Export 정책은 Generator와 Runtime에서 활용될 수 있다.

---

# 12. Validation

Module Contract는 Generator 이전에 Validation을 수행한다.

대표적인 검증 항목

- Duplicate Identity
- Invalid Import
- Invalid Export
- Circular Dependency

---

# 13. Cross Contract Dependencies

Module Contract는 다음 Contract를 참조할 수 있다.

- CONTRACT_CLASS
- CONTRACT_FUNCTION
- CONTRACT_EXPRESSION
- CONTRACT_EFFECT
- CONTRACT_EVENT
- CONTRACT_TYPE

---

# 14. Aggregate Responsibility

Module은 포함된 Contract의 실행이나 평가를 담당하지 않는다.

Module의 책임은 다음과 같다.

- Organization
- Dependency
- Visibility
- Packaging

---

# 15. Part Summary

Part 1에서는 Module Contract의 목적과 Aggregate Model을 정의하였다.

Module은 프로젝트의 모든 Language Element를 조직하는 최상위 Aggregate Contract이다.

Part 2에서는 Module Structure를 정의한다.

---

# Part 2. Module Structure

---

# 16. Overview

본 장에서는 Module Contract의 공통 구조를 정의한다.

모든 Module은 동일한 구조를 가지며 Generator는 이를 기반으로 Package와 Runtime Artifact를 생성한다.

---

# 17. Canonical Structure

모든 Module Contract는 다음 구조를 따른다.

```
Module

├── Identity
├── Name
├── Namespace
├── Version
├── Imports
├── Exports
├── Members
├── Attributes
├── Documentation
└── Metadata
```

Generator는 위 구조를 변경해서는 안 된다.

---

# 18. Name

Name은 Module의 공식 이름이다.

요구사항

- Empty String을 허용하지 않는다.
- Namespace 내부에서 유일해야 한다.
- Generator는 Name을 변경해서는 안 된다.

예시

```
player

entity

world

inventory
```

---

# 19. Namespace

Namespace는 Module을 논리적으로 구분한다.

예시

```
skript.player

skript.entity

skript.world
```

Namespace는 Identity 생성에도 사용될 수 있다.

---

# 20. Version

Module은 Version 정보를 포함할 수 있다.

Version은 Module의 배포 및 호환성 관리에 사용된다.

예시

```
1.0.0

2.1.3
```

Version 형식은 별도의 Specification에서 정의한다.

---

# 21. Imports

Imports는 Module이 의존하는 다른 Module을 정의한다.

예시

```
player

↓

entity

↓

type
```

Import는 의존성만 표현하며 Ownership은 변경하지 않는다.

---

# 22. Exports

Exports는 외부에 공개되는 Contract를 정의한다.

대표 대상

- Class
- Function
- Expression
- Effect
- Event
- Type

Export되지 않은 Contract는 Module 내부 구현으로 간주할 수 있다.

---

# 23. Members

Members는 Module이 포함하는 Language Element이다.

```
Module

├── Classes
├── Functions
├── Expressions
├── Effects
├── Events
└── Types
```

Module은 Member의 의미를 변경하지 않는다.

---

# 24. Attributes

Attributes는 Module의 추가적인 의미를 표현한다.

예시

- Deprecated
- Experimental
- Internal
- Extension
- BuiltIn

Attribute는 Module의 구조를 변경하지 않는다.

---

# 25. Documentation

Documentation은 Module에 대한 설명을 제공한다.

대표 항목

- Summary
- Description
- Examples
- Remarks

Generator는 Documentation을 API 문서 생성에 활용할 수 있다.

---

# 26. Metadata

Metadata는 Module Contract 자체를 설명하는 정보이다.

예시

- Author
- Provider
- Source
- Tags
- Created At
- Updated At

Metadata는 Module의 의미를 변경하지 않는다.

---

# 27. Visibility

Module은 공개 범위를 정의할 수 있다.

예시

- Public
- Internal
- Private

Visibility 정책은 Generator와 Runtime에서 활용될 수 있다.

---

# 28. Structural Constraints

Module Contract는 다음 구조적 제약을 만족해야 한다.

- Identity는 변경할 수 없다.
- Import Cycle은 허용하지 않는다.
- Member Identity는 중복될 수 없다.
- Export는 Member만 참조할 수 있다.

---

# 29. Module Classification

Module은 목적에 따라 분류될 수 있다.

예시

- Core Module
- Runtime Module
- Extension Module
- Plugin Module
- Library Module

분류는 Documentation 또는 Metadata를 통해 표현할 수 있으며 Module의 의미를 변경하지 않는다.

---

# 30. Default Behavior

Module의 기본 정책(Default Policy)은 다음과 같다.

- Language Element를 조직한다.
- Import를 통해 의존성을 표현한다.
- Export를 통해 공개 범위를 정의한다.
- Runtime 동작은 수행하지 않는다.

Module은 Aggregate Contract이다.

---

# 31. Part Summary

Part 2에서는 Module Contract의 공통 구조를 정의하였다.

Module은 Language Element를 조직하는 최상위 Aggregate이며, Import와 Export를 통해 프로젝트 구조를 표현한다.

Part 3에서는 Registry, Dependency Resolution 및 Validation Rules를 정의한다.

---

# Part 3. Registry, Dependency Resolution & Validation

---

# 32. Overview

본 장에서는 Module Contract의 Registry, Dependency Resolution 및 Validation 규칙을 정의한다.

Module은 프로젝트 전체의 Language Boundary이며, Registry는 모든 Module의 공식 관리 지점을 제공한다.

---

# 33. Module Registry

모든 Module은 Module Registry를 통해 관리된다.

```
Registry

├── Module A
├── Module B
├── Module C
└── ...
```

Registry는 Module Identity를 기준으로 관리한다.

---

# 34. Dependency Graph

Module 간 의존성은 Dependency Graph로 표현된다.

```
Module A

↓

Module B

↓

Module C
```

Dependency Graph는 방향성을 가진다.

Generator는 Dependency Graph를 변경해서는 안 된다.

---

# 35. Dependency Resolution

Dependency Resolution은 다음 절차를 따른다.

```
Lookup

↓

Resolve Imports

↓

Resolve Exports

↓

Validate

↓

Resolved Module
```

Generator는 동일한 입력에 대해 항상 동일한 Resolution 결과를 생성해야 한다.

---

# 36. Visibility Graph

Visibility는 Module 간 접근 가능 범위를 정의한다.

```
Module A

↓

Export

↓

Module B
```

Export되지 않은 Member는 외부에서 직접 접근해서는 안 된다.

---

# 37. Import Rules

Import는 다른 Module의 공개 API를 참조한다.

Import는 다음 원칙을 따른다.

- 순환 Import를 허용하지 않는다.
- Import는 Module Identity를 기준으로 수행한다.
- Import는 Ownership을 변경하지 않는다.

---

# 38. Export Rules

Export는 Module 외부에 공개되는 Contract를 정의한다.

Export 대상은 반드시 Module의 Member여야 한다.

Generator는 Export되지 않은 Member를 Public API에 포함해서는 안 된다.

---

# 39. Circular Dependency

Module은 순환 의존성을 가져서는 안 된다.

다음 구조는 허용되지 않는다.

```
Module A

↓

Module B

↓

Module C

↓

Module A
```

Generator는 순환 의존성을 Diagnostic으로 보고해야 한다.

---

# 40. Validation Rules

Validation은 다음 순서로 수행한다.

```
Identity

↓

Imports

↓

Exports

↓

Members

↓

Metadata

↓

Dependency Graph
```

Validation 실패 시 Registry에 등록되어서는 안 된다.

---

# 41. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Duplicate Module Identity
- Invalid Import
- Invalid Export
- Missing Member
- Circular Dependency
- Invalid Namespace

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 42. Compatibility Guarantees

Module Contract는 다음 사항을 보장한다.

- Stable Identity
- Stable Dependency Graph
- Stable Visibility Rules
- Stable Serialization
- Stable Resolution Rules

---

# 43. Language Boundary

Module은 Language Boundary를 정의한다.

Language Boundary는 다음 요소를 포함한다.

- Dependency
- Visibility
- Organization
- Public API

Module은 Runtime 동작을 수행하지 않는다.

---

# 44. Runtime Independence

Module Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Module Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 45. Part Summary

Part 3에서는 Module의 Registry, Dependency Resolution 및 Validation 규칙을 정의하였다.

Module은 Language Boundary를 표현하며 Dependency Graph와 Visibility Graph를 통해 프로젝트 구조를 정의한다.

---

# Part 4. Serialization, Generator Integration & Runtime

---

# 46. Overview

본 장에서는 Module Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의한다.

Module Contract는 Generator가 사용하는 Canonical Package Model이며, Runtime은 Generator가 생성한 Artifact를 통해 Module을 구성한다.

---

# 47. Serialization Principles

Module Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Module Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

Serialization 과정에서 Module의 의미가 변경되어서는 안 된다.

---

# 48. Deserialization

직렬화된 Module Contract는 동일한 Module Model로 복원되어야 한다.

복원 과정은 다음 절차를 따른다.

```
Deserialize

↓

Structural Validation

↓

Resolve Imports

↓

Resolve Members

↓

Ready
```

복원 이후 Module Contract는 Generator가 사용할 수 있는 상태여야 한다.

---

# 49. Generator Input

Generator는 Module Contract를 공식 입력으로 사용한다.

```
Module Contract

↓

Generator
```

Generator는 Module의 구조를 변경해서는 안 된다.

---

# 50. Artifact Mapping

Module Contract는 다양한 Artifact로 변환될 수 있다.

예시

```
Module Contract

├── Runtime Module
├── Package Descriptor
├── Documentation
├── Registry Entry
├── Language Server Index
└── Static Analysis Model
```

모든 Artifact는 동일한 Module Semantics를 유지해야 한다.

---

# 51. Documentation Generation

Generator는 Module Documentation을 기반으로 문서를 생성할 수 있다.

대표 항목

- Name
- Namespace
- Version
- Imports
- Exports
- Members
- Summary
- Description

Documentation은 Module의 의미를 변경하지 않는다.

---

# 52. Runtime Compatibility

Runtime은 Module Contract를 직접 소비하지 않는다.

Runtime은 Generator가 생성한 Artifact를 소비한다.

```
Module Contract

↓

Generator

↓

Artifact

↓

Runtime
```

Runtime은 Module Contract를 수정해서는 안 된다.

---

# 53. Diagnostics

Generator는 다음 Diagnostic을 생성할 수 있다.

- Invalid Import
- Invalid Export
- Circular Dependency
- Duplicate Module
- Invalid Namespace
- Invalid Metadata

가능한 한 여러 오류를 함께 보고하는 것을 권장한다.

---

# 54. Compatibility Rules

Module Contract는 다음 사항을 보장해야 한다.

- Stable Identity
- Stable Dependency Graph
- Stable Visibility Rules
- Stable Serialization
- Stable Resolution Rules

Module의 의미를 변경하는 것은 Breaking Change로 간주한다.

---

# 55. Extension Support

Module Contract는 향후 다음 기능을 지원할 수 있다.

- Dynamic Module
- Lazy Module Loading
- Module Aliasing
- Conditional Module
- Composite Module
- Virtual Module

확장은 기존 Generator와의 호환성을 유지해야 한다.

---

# 56. Runtime Independence

Module Contract는 특정 Runtime 구현에 의존하지 않는다.

동일한 Module Contract는 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 57. Generator Compliance

모든 Generator는 다음 사항을 준수해야 한다.

- Module Identity를 변경하지 않는다.
- Dependency Graph를 변경하지 않는다.
- Visibility를 변경하지 않는다.
- Diagnostics를 제공한다.
- 결정적인 결과를 생성한다.

---

# 58. Module Lifecycle

Module Contract는 다음 생명주기를 따른다.

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

Load

↓

Available
```

---

# 59. Part Summary

Part 4에서는 Module Contract의 Serialization, Generator Integration 및 Runtime Compatibility를 정의하였다.

Module Contract는 프로젝트의 공식 Package Model이며, Generator는 이를 기반으로 다양한 Artifact를 생성하고 Runtime은 생성된 Artifact를 통해 Module을 구성한다.

Part 5에서는 Best Practices, Anti-Patterns 및 Governance를 정의한다.

---

# Part 5. Governance, Best Practices & Architecture

---

# 60. Overview

본 장에서는 Module Contract의 Governance, Best Practices 및 Language Architecture에서의 역할을 정의한다.

Module Contract는 프로젝트의 최상위 Aggregate Contract이며, 모든 Language Element를 조직하는 공식 구조를 제공한다.

---

# 61. Language Architecture

Module은 프로젝트 전체의 Language Architecture를 구성하는 최상위 Aggregate이다.

```
Module

├── Classes
├── Functions
├── Expressions
├── Effects
├── Events
├── Types
└── Metadata
```

Module은 포함된 Contract의 의미를 변경하지 않는다.

---

# 62. Aggregate Model

Module은 Aggregate Contract이다.

Module은 다음 책임을 가진다.

- Organization
- Dependency Management
- Visibility Management
- Public API Definition
- Package Organization

Module은 Runtime 동작을 수행하지 않는다.

---

# 63. Best Practices

다음 사항을 권장한다.

- 하나의 Module은 하나의 책임(Responsibility)을 가진다.
- Module 간 의존성은 최소화한다.
- Public API는 명확하게 정의한다.
- Internal Contract는 Export하지 않는다.
- Documentation을 함께 제공한다.

---

# 64. Anti-Patterns

다음 구현은 권장하지 않는다.

- 순환 의존성을 가지는 Module
- 지나치게 많은 책임을 가진 Module
- Export되지 않은 Contract에 외부 의존성을 만드는 경우
- Runtime 동작을 Module에 구현하는 경우
- Generator 전용 구조를 Module에 포함하는 경우

---

# 65. Evolution Strategy

Module Contract는 장기적인 확장을 고려하여 설계한다.

권장 방식

- Optional Metadata 추가
- Documentation 확장
- Extension Module 추가
- Import/Export 정책 확장

기존 Aggregate 의미를 변경하는 것은 최소화한다.

---

# 66. Compatibility

Module Contract는 가능한 한 하위 호환성을 유지한다.

다음 변경은 Breaking Change로 간주한다.

- Module Identity 변경
- Import 의미 변경
- Export 의미 변경
- Dependency Resolution 변경
- Public API 변경

---

# 67. Compliance

모든 Module Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- CONTRACT_DESIGN_PRINCIPLES
- 모든 관련 CONTRACT 문서
- 관련 Specification
- ADR
- RFC

Module Contract는 프로젝트의 Canonical Aggregate Model이다.

---

# 68. Relationship Model

Module은 프로젝트의 모든 Contract를 조직한다.

```
Module

├── Type
├── Parameter
├── Property
├── Function
├── Expression
├── Effect
├── Event
└── Class
```

Module은 Contract 간의 관계를 표현하지만 각 Contract의 동작은 변경하지 않는다.

---

# 69. Architecture Layers

프로젝트는 다음 계층 구조를 따른다.

```
Atomic Layer

├── Type
├── Parameter
└── Property

↓

Callable Layer

├── Function
├── Expression
├── Effect
└── Event

↓

Object Layer

└── Class

↓

Aggregation Layer

└── Module
```

각 계층은 자신의 책임만 수행하며 상위 계층은 하위 계층을 조직한다.

---

# 70. Future Extensions

향후 다음 기능을 지원할 수 있다.

- Dynamic Module Loading
- Virtual Module
- Composite Module
- Conditional Module
- Distributed Module
- Module Federation

필요한 경우 별도의 Specification으로 정의한다.

---

# 71. Core Architecture Summary

Core Contract는 다음 Language Model을 구성한다.

```
Language Architecture

├── Object Model
│     └── Class
│
├── Query Model
│     └── Expression
│
├── Command Model
│     └── Effect
│
├── Notification Model
│     └── Event
│
├── Signature Model
│     └── Function
│
└── Aggregation Model
      └── Module
```

이 구조는 프로젝트 전체의 Canonical Language Architecture이다.

---

# 72. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Module Contract |