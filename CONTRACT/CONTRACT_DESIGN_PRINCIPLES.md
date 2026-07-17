# Contract Design Principles

Version: 1.0

Status: Draft

---

# Part 1. Contract Philosophy

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 모든 Contract의 공통 설계 원칙을 정의한다.

Contract는 Generator와 Runtime이 공유하는 공식 데이터 계약이며, 구현과 독립적으로 유지되어야 한다.

---

# 2. Scope

본 문서는 다음 Contract에 공통적으로 적용된다.

- Type Contract
- Event Contract
- Effect Contract
- Expression Contract
- Function Contract
- Property Contract
- Module Contract
- Class Contract

새로운 Contract도 본 문서를 준수해야 한다.

---

# 3. Definition

Contract는 Generator가 Artifact를 생성하기 위한 Canonical Data Model이다.

Contract는 Runtime 구현이나 IDE 구현에 종속되지 않는다.

---

# 4. Goals

Contract는 다음 목표를 가진다.

- Stable Structure
- Deterministic Serialization
- Language Independence
- Generator Compatibility
- Version Compatibility

---

# 5. Non Goals

Contract는 다음 역할을 수행하지 않는다.

- Runtime Logic
- Execution State
- UI State
- Cache
- Registry

Contract는 데이터 구조만 정의한다.

---

# 6. Canonical Representation

동일한 Domain Object는 하나의 Contract만 가진다.

예시

```
Type

↓

Type Contract
```

동일 개념의 Contract를 중복 정의해서는 안 된다.

---

# 7. Contract First

모든 Generator는 Contract를 입력으로 사용한다.

```
Specification

↓

Contract

↓

Generator

↓

Artifact
```

Generator는 Domain Object를 직접 처리하지 않는다.

---

# 8. Stable Schema

Contract의 구조는 가능한 한 안정적으로 유지한다.

Breaking Change는 RFC와 ADR 절차를 통해 승인되어야 한다.

---

# 9. Language Independence

Contract는 특정 프로그래밍 언어에 종속되지 않는다.

TypeScript, JSON Schema, OpenAPI, LSP Types는 모두 동일한 Contract에서 생성된다.

---

# 10. Deterministic Output

동일한 Contract는 항상 동일한 Artifact를 생성해야 한다.

Generator 구현 차이로 결과가 달라져서는 안 된다.

---

# 11. Separation of Concerns

Contract는 데이터 구조만 정의한다.

다음 내용은 Contract에 포함하지 않는다.

- 실행 로직
- 캐시 정책
- Registry 상태
- UI 정보

---

# 12. Extensibility

새로운 필드는 기존 Contract를 깨뜨리지 않는 방식으로 추가한다.

Optional Field 또는 새로운 Contract를 사용하는 것을 권장한다.

---

# 13. Compatibility

Contract는 가능한 한 하위 호환성을 유지한다.

Deprecated 정책과 Migration Strategy를 함께 제공해야 한다.

---

# 14. Documentation First

새로운 Contract는 구현 이전에 문서로 정의한다.

권장 순서

RFC

↓

Specification

↓

Contract

↓

Generator

↓

Implementation

---

# 15. Part Summary

Part 1에서는 Contract의 목적과 철학을 정의하였다.

Contract는 Generator와 Runtime 사이의 공식 데이터 계약이며, 언어 독립성과 안정성을 유지하는 Canonical Model이다.

Part 2에서는 Contract Architecture와 공통 구조를 정의한다.

---

# Part 2. Contract Architecture

---

# 16. Overview

본 장에서는 모든 Contract가 공통적으로 따라야 하는 구조와 설계 규칙을 정의한다.

모든 Contract는 동일한 Header 구조와 Metadata 규칙을 공유해야 한다.

---

# 17. Canonical Structure

모든 Contract는 다음 논리 구조를 따른다.

```
Contract

├── Header
├── Identity
├── Version
├── Metadata
├── Content
├── Extensions
└── Validation
```

세부 구현은 Contract 종류에 따라 달라질 수 있다.

---

# 18. Header

Header는 Contract 자체를 식별하는 정보이다.

대표적인 항목

- Contract Name
- Contract Type
- Contract Version
- Specification Version

Header는 Contract의 본문(Content)과 분리한다.

---

# 19. Identity

모든 Contract는 Identity를 포함해야 한다.

Identity는 다음을 만족해야 한다.

- Immutable
- Globally Unique
- Stable

Identity는 Contract 생명주기 동안 변경되어서는 안 된다.

---

# 20. Version

모든 Contract는 Version 정보를 포함해야 한다.

Version은 Contract 구조의 변경 이력을 나타낸다.

Version 변경이 Identity 변경을 의미하지는 않는다.

---

# 21. Metadata

Metadata는 Contract 자체를 설명하는 정보이다.

예시

- Author
- Provider
- Source
- Tags
- Created At
- Updated At

Metadata는 Domain Data와 분리하여 관리한다.

---

# 22. Content

Content는 Contract의 실제 데이터이다.

예시

Type Contract

- Name
- Namespace
- Description
- Members

Event Contract

- Parameters
- Return Type

Content는 Contract의 핵심 정보이다.

---

# 23. Extensions

모든 Contract는 확장을 지원할 수 있다.

Extension은 다음 조건을 만족해야 한다.

- Optional
- Backward Compatible
- Ignorable

Extension이 존재하지 않아도 Contract는 유효해야 한다.

---

# 24. Validation Rules

모든 Contract는 Validation 규칙을 가진다.

대표적인 검증 항목

- Required Fields
- Identity Format
- Duplicate Detection
- Type Validation
- Version Compatibility

Validation은 Generator 이전에 수행한다.

---

# 25. Serialization

Contract는 안정적으로 직렬화되어야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 26. Deserialization

직렬화된 Contract는 동일한 Contract Model로 복원되어야 한다.

복원 이후 Validation을 수행해야 한다.

---

# 27. Contract Lifecycle

Contract는 다음 생명주기를 따른다.

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

각 단계는 이전 단계의 결과를 기반으로 수행한다.

---

# 28. Schema Compatibility

Contract Schema는 Generator와 Runtime이 공유하는 공식 계약이다.

새로운 필드는 기존 Schema와의 호환성을 유지하는 방식으로 추가해야 한다.

---

# 29. Contract Composition

복잡한 Contract는 여러 하위 Contract를 포함할 수 있다.

예시

```
Module Contract

├── Type Contract
├── Event Contract
├── Effect Contract
└── Expression Contract
```

Composition은 순환 참조(Circular Reference)를 만들지 않아야 한다.

---

# 30. Contract Independence

Contract는 특정 Generator나 Runtime 구현에 의존해서는 안 된다.

Contract는 프로젝트 전체에서 재사용 가능한 Canonical Data Model이어야 한다.

---

# 31. Part Summary

Part 2에서는 모든 Contract가 공유하는 공통 구조와 생명주기를 정의하였다.

Contract는 Header, Identity, Metadata 및 Content를 중심으로 구성되며, Validation과 Serialization을 통해 Generator와 Runtime이 동일한 데이터를 공유할 수 있도록 설계된다.

Part 3에서는 Contract Modeling Rules와 데이터 설계 원칙을 정의한다.

---

# Part 3. Contract Modeling Rules

---

# 32. Overview

본 장에서는 Contract를 구성하는 데이터 모델의 설계 원칙을 정의한다.

Contract는 Domain Model을 안정적이고 결정적인 형태로 표현해야 한다.

---

# 33. Canonical Contract

하나의 Domain Object는 하나의 Canonical Contract를 가진다.

예시

```
Type

↓

Type Contract
```

동일한 Domain Object를 여러 Contract로 표현해서는 안 된다.

---

# 34. Explicit Structure

Contract의 구조는 항상 명시적으로 정의되어야 한다.

암묵적인 필드나 구현 의존적인 속성은 포함하지 않는다.

모든 필드는 Specification에서 정의되어야 한다.

---

# 35. Required and Optional Fields

Contract 필드는 다음 두 종류로 구분한다.

- Required
- Optional

Required Field의 의미를 Optional로 변경하는 것은 Breaking Change이다.

---

# 36. Stable Naming

Contract의 필드 이름은 장기적으로 안정성을 유지해야 한다.

필드 이름 변경은 가능한 피하며, 필요한 경우 Deprecated 절차를 따른다.

---

# 37. Deterministic Ordering

동일한 Contract는 항상 동일한 필드 순서를 유지해야 한다.

예시

```
Identity

↓

Metadata

↓

Content

↓

Extensions
```

필드 순서는 Generator의 출력 일관성을 보장한다.

---

# 38. Nested Contracts

복잡한 데이터는 중첩 Contract로 표현할 수 있다.

예시

```
Module Contract

├── Type Contract
├── Event Contract
└── Effect Contract
```

중첩 구조는 명확한 소유 관계를 가져야 한다.

---

# 39. References

Contract 간 참조는 Identity를 사용한다.

```
Type Contract

↓

Identity

↓

Event Contract
```

객체 자체를 포함하는 직접 참조는 권장하지 않는다.

---

# 40. Collections

Collection은 결정적인 순서를 유지해야 한다.

허용되는 Collection

- List
- Set
- Map

Collection의 선택 기준은 의미에 따라 결정한다.

---

# 41. Enumerations

제한된 값 집합은 Enumeration으로 정의한다.

예시

- Visibility
- Severity
- Symbol Kind

문자열 상수의 중복 사용은 지양한다.

---

# 42. Nullability

Null 사용은 최소화한다.

권장 순서

- Required Field
- Optional Field
- Nullable Field

Optional과 Nullable은 동일한 의미로 사용하지 않는다.

---

# 43. Default Values

기본값이 존재하는 경우 Specification에서 명시한다.

Generator는 기본값을 임의로 결정해서는 안 된다.

---

# 44. Validation Constraints

Contract는 구조적 제약 조건을 포함할 수 있다.

예시

- Minimum Length
- Maximum Length
- Pattern
- Range
- Unique Constraint

Validation 규칙은 Generator와 Runtime에서 동일하게 해석되어야 한다.

---

# 45. Forward Compatibility

새로운 Contract는 향후 확장을 고려하여 설계한다.

권장 방식

- Optional Field 추가
- Extension 사용
- 새로운 Contract 정의

기존 구조를 변경하는 방식은 최소화한다.

---

# 46. Backward Compatibility

기존 Contract는 가능한 한 유지한다.

필드 제거보다 Deprecated를 우선 사용한다.

---

# 47. Model Consistency

모든 Contract는 다음 사항을 만족해야 한다.

- Stable Identity
- Stable Serialization
- Deterministic Structure
- Explicit Validation
- Canonical Representation

---

# 48. Part Summary

Part 3에서는 Contract 내부 데이터 모델의 설계 원칙을 정의하였다.

모든 Contract는 명확한 구조와 결정적인 표현 방식을 유지하며, Identity 중심의 참조 모델을 통해 장기적인 호환성과 확장성을 보장해야 한다.

Part 4에서는 Generator Integration, Serialization Strategy 및 Runtime Integration을 정의한다.

---

# Part 4. Generator Integration & Runtime Integration

---

# 49. Overview

본 장에서는 Contract가 Generator 및 Runtime과 상호작용하는 방식을 정의한다.

Contract는 Generator의 입력이며, Runtime이 소비하는 Artifact의 생성 기준이다.

---

# 50. Generation Pipeline

모든 Artifact는 동일한 생성 파이프라인을 따른다.

```
Specification

↓

Contract

↓

Validation

↓

Generator

↓

Artifact

↓

Runtime
```

Generator는 Contract만을 입력으로 사용한다.

---

# 51. Generator Independence

Generator는 Domain Object 또는 Registry 내부 구조에 직접 의존해서는 안 된다.

Generator가 접근 가능한 공식 데이터는 Contract이다.

Generator는 Contract를 Canonical Input으로 사용한다.

---

# 52. Multi-target Generation

동일한 Contract는 여러 종류의 Artifact를 생성할 수 있다.

예시

```
Contract

├── TypeScript
├── JSON Schema
├── OpenAPI
├── LSP Types
└── Documentation
```

각 Generator는 동일한 의미를 유지해야 한다.

---

# 53. Generator Determinism

동일한 Contract와 동일한 Generator 설정은 항상 동일한 Artifact를 생성해야 한다.

Generator 구현 차이에 의해 결과가 달라져서는 안 된다.

---

# 54. Validation Before Generation

모든 Contract는 Generator 실행 전에 Validation을 통과해야 한다.

Validation에 실패한 Contract는 Artifact를 생성해서는 안 된다.

Validation 실패는 구조화된 Diagnostic으로 보고한다.

---

# 55. Runtime Consumption

Runtime은 Generator가 생성한 Artifact를 소비한다.

```
Artifact

↓

Runtime
```

Runtime은 Contract를 직접 해석하지 않는다.

---

# 56. Serialization Strategy

Contract는 안정적인 직렬화 형식을 제공해야 한다.

지원 가능한 형식

- JSON
- YAML

동일한 Contract는 항상 동일한 직렬화 결과를 생성해야 한다.

---

# 57. Transformation Rules

Generator는 Contract를 Artifact로 변환한다.

Transformation은 다음 사항을 만족해야 한다.

- Deterministic
- Loss-aware
- Repeatable
- Stable

Transformation 과정에서 의미가 변경되어서는 안 된다.

---

# 58. Diagnostics Integration

Generator는 Validation 또는 Generation 과정에서 발생한 문제를 Diagnostic으로 보고한다.

대표적인 오류

- Invalid Contract
- Unsupported Feature
- Missing Required Field
- Version Mismatch

Generator는 가능한 한 여러 문제를 한 번에 보고하는 것을 권장한다.

---

# 59. Extension Support

Generator는 Contract Extension을 처리할 수 있다.

지원하지 않는 Extension은 무시하거나 Warning Diagnostic으로 보고할 수 있다.

Extension의 존재로 인해 Generator가 비정상 종료되어서는 안 된다.

---

# 60. Runtime Compatibility

Generator가 생성한 Artifact는 대상 Runtime과 호환되어야 한다.

호환성은 Contract Version과 Generator Version을 기준으로 판단한다.

---

# 61. Generator Compliance

모든 Generator 구현은 다음 사항을 준수해야 한다.

- Contract를 Canonical Input으로 사용한다.
- Validation을 선행한다.
- 결정적인 출력을 생성한다.
- Contract의 의미를 변경하지 않는다.
- 구조화된 Diagnostic을 제공한다.

---

# 62. Part Summary

Part 4에서는 Contract와 Generator, Runtime의 관계를 정의하였다.

Contract는 Generator의 유일한 입력이며, Runtime은 Generator가 생성한 Artifact를 소비한다.

Generator는 Contract의 의미를 유지하면서 안정적이고 결정적인 Artifact를 생성해야 한다.

Part 5에서는 Best Practices, Anti-Patterns, Compliance 및 Contract Architecture Summary를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Contract Governance

---

# 63. Overview

본 장에서는 Contract 설계 시 따라야 하는 Best Practices와 Anti-Patterns를 정의한다.

Contract는 프로젝트 전체에서 Generator와 Runtime을 연결하는 공식 계약으로서 장기적인 안정성과 호환성을 유지해야 한다.

---

# 64. Best Practices

다음 사항을 권장한다.

- Domain Model을 그대로 Contract에 반영한다.
- Identity를 기준으로 참조한다.
- Required Field를 최소화한다.
- Optional Field를 통해 확장을 지원한다.
- 구조보다 의미(Semantics)를 우선한다.
- Validation Rule을 Specification과 함께 정의한다.
- Contract를 Version 단위로 관리한다.
- Generator는 Contract 외의 데이터에 의존하지 않는다.

---

# 65. Anti-Patterns

다음 구현은 권장하지 않는다.

- Runtime 상태를 Contract에 저장하는 경우
- Generator 전용 필드를 Contract에 추가하는 경우
- UI 정보를 Contract에 포함하는 경우
- Registry 내부 구조를 Contract에 노출하는 경우
- 객체 직접 참조(Object Reference)
- Mutable Identity
- Generator마다 다른 Contract 구조를 사용하는 경우

이러한 구현은 Contract의 독립성과 재사용성을 저하시킨다.

---

# 66. Contract Evolution

Contract는 시간이 지나면서 확장될 수 있다.

권장 전략

- Optional Field 추가
- Extension 사용
- Deprecated 처리
- 새로운 Contract 정의

기존 Contract의 의미를 변경하는 것은 최소화한다.

---

# 67. Version Compatibility

Contract Version은 Generator Version과 독립적으로 관리한다.

호환성 판단 기준

- Contract Version
- Specification Version
- Generator Capability

Version 불일치는 Diagnostic으로 보고한다.

---

# 68. Compliance

모든 Contract는 다음 문서를 준수해야 한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- Core Specifications
- RFC
- ADR

Contract는 프로젝트 전체에서 Canonical Data Model 역할을 수행한다.

---

# 69. Contract Architecture Summary

Contract 계층의 공식 구조는 다음과 같다.

```
Domain Model
      │
      ▼
Contract Builder
      │
      ▼
Contract
      │
      ▼
Validation
      │
      ▼
Generator
      │
      ▼
Artifact
      │
      ▼
Runtime
```

Contract는 Generator와 Runtime 사이의 유일한 공식 데이터 계약이다.

---

# 70. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CORE_DESIGN_PRINCIPLES
- CORE_GLOSSARY
- SPEC-0013 ~ SPEC-0018
- RFC
- ADR
- Generator Specifications

---

# 71. Future Extensions

향후 다음 Contract를 추가할 수 있다.

- CONTRACT_ENUM
- CONTRACT_PARAMETER
- CONTRACT_ATTRIBUTE
- CONTRACT_PACKAGE
- CONTRACT_PLUGIN
- CONTRACT_WORKSPACE

모든 신규 Contract는 본 문서를 준수해야 한다.

---

# 72. Document Summary

CONTRACT_DESIGN_PRINCIPLES는 VSkript Database 프로젝트의 모든 Contract가 따라야 하는 공통 설계 원칙을 정의한다.

Contract는 Domain Model을 표현하는 Canonical Data Model이며, Generator의 유일한 입력으로 사용된다.

모든 Contract는 안정적인 구조, 결정적인 직렬화, 명확한 Validation 및 장기적인 호환성을 유지해야 한다.

---

# 73. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Contract Design Principles |