# Core Glossary

Version: 1.0

Status: Draft

---

# Part 1. Core Terminology

---

# 1. Purpose

본 문서는 VSkript Database 프로젝트에서 사용하는 공식 용어를 정의한다.

모든 Specification, RFC, ADR, Contract 및 Implementation 문서는 본 Glossary를 기준으로 동일한 의미의 용어를 사용해야 한다.

---

# 2. Scope

본 문서는 다음 영역에서 사용하는 핵심 용어를 정의한다.

- Architecture
- Domain Model
- Registry
- Identity
- Pipeline
- Graph
- Generator
- Runtime

---

# 3. Glossary Rules

모든 용어는 다음 원칙을 따른다.

- 하나의 용어는 하나의 의미만 가진다.
- 동일한 개념에 여러 용어를 사용하지 않는다.
- 약어는 최초 등장 시 전체 이름을 함께 표기한다.
- Specification에서는 본 Glossary의 정의를 따른다.

---

# 4. Artifact

Artifact는 Generator가 생성하는 결과물이다.

예시

- TypeScript
- JSON Schema
- OpenAPI
- LSP Types

Artifact는 Runtime에서 소비될 수 있다.

---

# 5. Boundary

Boundary는 시스템 간의 경계를 의미한다.

대표적인 Boundary

- Contract Boundary
- API Boundary
- Runtime Boundary
- Serialization Boundary

Boundary에서는 Validation이 수행된다.

---

# 6. Cache

Cache는 Registry Lookup 결과를 임시 저장하여 성능을 향상시키는 구성 요소이다.

Cache는 Source of Truth가 아니다.

---

# 7. Component

Component는 독립적인 책임을 수행하는 소프트웨어 단위이다.

예시

- Registry
- Generator
- Resolver
- Pipeline

---

# 8. Consumer

Consumer는 Registry 또는 Generator의 결과를 사용하는 주체이다.

예시

- Language Server
- Runtime
- IDE

Consumer는 Source of Truth를 변경하지 않는다.

---

# 9. Context

Context는 실행 중 필요한 상태 정보를 저장하는 모델이다.

Context는 객체의 저장소가 아니다.

---

# 10. Contract

Contract는 Generator와 Runtime 사이의 공식 데이터 계약이다.

Contract는 구현이 아니라 구조를 정의한다.

---

# 11. Diagnostic

Diagnostic은 오류, 경고 또는 정보를 표현하는 구조화된 메시지이다.

대표적인 Severity

- Error
- Warning
- Information
- Hint

---

# 12. Domain Model

Domain Model은 비즈니스 개념을 표현하는 데이터 모델이다.

예시

- Symbol
- Type
- Event
- Registry Entry

---

# 13. Generator

Generator는 Contract를 기반으로 Artifact를 생성하는 컴포넌트이다.

Generator는 Runtime 로직을 포함하지 않는다.

---

# 14. Identity

Identity는 객체를 고유하게 식별하는 불변(Immutable) 식별자이다.

객체 간 참조는 Identity를 기준으로 수행한다.

---

# 15. Part Summary

Part 1에서는 프로젝트 전반에서 사용하는 핵심 용어의 기본 정의를 제공하였다.

Part 2에서는 Registry, Resolver, Pipeline, Provider 및 Graph 관련 용어를 정의한다.

---

# Part 2. Core Component Terminology

---

# 16. Graph

Graph는 객체 간의 관계(Relationship)를 표현하는 데이터 모델이다.

Graph는 Identity를 통해 Node를 연결하며, 객체를 직접 참조하지 않는다.

---

# 17. Identity Reference

Identity Reference는 객체 자체가 아니라 Identity를 이용하여 객체를 참조하는 방식이다.

프로젝트의 모든 참조는 Identity Reference를 기본으로 사용한다.

---

# 18. Implementation

Implementation은 Specification과 Contract를 실제 코드로 구현한 결과이다.

Implementation은 상위 문서의 계약을 준수해야 한다.

---

# 19. Incremental Update

Incremental Update는 변경된 데이터만 갱신하는 업데이트 방식이다.

변경되지 않은 객체는 그대로 유지한다.

---

# 20. Language Server

Language Server는 Registry와 Generator가 제공하는 정보를 기반으로 IDE 기능을 제공하는 구성 요소이다.

예시 기능

- Completion
- Hover
- Definition
- References
- Diagnostics

---

# 21. Metadata

Metadata는 Domain Object를 설명하는 부가 정보이다.

대표적인 예

- Created Time
- Updated Time
- Source
- Provider
- Tags

Metadata는 객체의 의미를 변경하지 않는다.

---

# 22. Namespace

Namespace는 Identity의 충돌을 방지하기 위한 논리적 구분 단위이다.

동일한 Identifier라도 Namespace가 다르면 서로 다른 객체로 간주할 수 있다.

---

# 23. Pipeline

Pipeline은 작업(Task)의 실행 순서와 흐름을 정의하는 모델이다.

Pipeline은 객체를 저장하지 않으며 실행 흐름만 관리한다.

---

# 24. Provider

Provider는 Registry에 객체를 등록하거나 제공하는 컴포넌트이다.

대표적인 등록 대상

- Types
- Events
- Effects
- Expressions
- Symbols

---

# 25. Registry

Registry는 프로젝트의 공식 Source of Truth이다.

Registry는 Identity를 기준으로 객체를 등록, 조회 및 관리한다.

---

# 26. Registry Entry

Registry Entry는 Registry가 관리하는 최소 단위이다.

구성 요소

- Identity
- Object
- Metadata
- State

---

# 27. Resolver

Resolver는 Identity를 실제 객체로 변환하는 컴포넌트이다.

Resolver는 Registry를 기준으로 Lookup을 수행한다.

---

# 28. Runtime

Runtime은 Generator가 생성한 Artifact를 실제로 사용하는 실행 환경이다.

Runtime은 Specification을 직접 해석하지 않는다.

---

# 29. Scope

Scope는 객체 또는 Registry의 유효 범위를 의미한다.

대표적인 Scope

- Global
- Workspace
- Project
- Session

---

# 30. Snapshot

Snapshot은 특정 시점의 Registry 상태를 불변(Immutable)으로 저장한 데이터 집합이다.

Snapshot은 Pipeline과 Cache의 일관성을 보장하는 기준이 된다.

---

# 31. Source of Truth

Source of Truth는 프로젝트에서 공식 데이터로 인정되는 유일한 저장소이다.

VSkript Database에서는 Registry가 Source of Truth 역할을 수행한다.

---

# 32. State

State는 객체 또는 Registry의 현재 상태를 의미한다.

대표적인 상태

- Valid
- Invalid
- Deprecated
- Removed

---

# 33. Symbol

Symbol은 프로젝트에서 이름을 가지는 식별 가능한 Domain Object이다.

예시

- Type
- Event
- Effect
- Expression

---

# 34. Part Summary

Part 2에서는 프로젝트를 구성하는 핵심 컴포넌트와 데이터 모델 관련 용어를 정의하였다.

이 용어들은 Core Specification, Contract 및 Implementation 문서에서 동일한 의미로 사용되어야 한다.

Part 3에서는 Architecture, Generator, Serialization, Validation 및 Runtime과 관련된 용어를 정의한다.

---

# Part 3. Architecture & Generation Terminology

---

# 35. Architecture

Architecture는 시스템의 전체 구조와 구성 요소 간의 관계를 정의하는 설계이다.

Architecture는 구현보다 상위 개념이며, 프로젝트 전체의 구조적 일관성을 보장한다.

---

# 36. Contract Model

Contract Model은 Generator와 Runtime이 공유하는 공식 데이터 구조이다.

Contract는 구현 방식과 무관하게 데이터의 구조와 의미를 정의한다.

---

# 37. Dependency

Dependency는 하나의 구성 요소가 다른 구성 요소에 의존하는 관계를 의미한다.

모든 Dependency는 명시적으로 선언되어야 하며 단방향을 유지한다.

---

# 38. Extension Point

Extension Point는 기존 구조를 수정하지 않고 기능을 확장할 수 있도록 제공되는 공식 확장 지점이다.

예시

- Provider
- Generator
- Registry
- Pipeline Stage

---

# 39. Generator Input

Generator Input은 Generator가 Artifact를 생성하기 위해 사용하는 입력 데이터이다.

대표적인 입력

- Contract
- Registry
- Configuration
- Generator Options

---

# 40. Generator Output

Generator Output은 Generator가 생성하는 결과물이다.

예시

- TypeScript Source
- JSON Schema
- OpenAPI Document
- LSP Type Definitions

---

# 41. Mapping

Mapping은 하나의 데이터 구조를 다른 데이터 구조로 변환하는 규칙이다.

예시

```
Contract

↓

TypeScript

↓

JSON Schema

↓

OpenAPI
```

Mapping은 정보 손실을 최소화해야 한다.

---

# 42. Model

Model은 특정 개념을 표현하기 위한 구조화된 데이터 표현이다.

예시

- Domain Model
- Contract Model
- Registry Model
- Graph Model

---

# 43. Runtime Model

Runtime Model은 실행 시점에서 사용하는 데이터 구조이다.

Runtime Model은 Contract Model과 구분된다.

---

# 44. Schema

Schema는 데이터의 형식과 제약 조건을 정의하는 구조이다.

대표적인 예

- JSON Schema
- OpenAPI Schema

Schema는 데이터의 의미보다 구조를 정의한다.

---

# 45. Serialization

Serialization은 객체를 저장 또는 전송 가능한 형식으로 변환하는 과정이다.

대표적인 형식

- JSON
- YAML

Serialization은 객체의 의미를 유지해야 한다.

---

# 46. Deserialization

Deserialization은 직렬화된 데이터를 Domain Model로 복원하는 과정이다.

복원된 객체는 Validation을 통과해야 한다.

---

# 47. Transformation

Transformation은 하나의 Model을 다른 Model로 변환하는 과정이다.

Transformation은 결정적(Deterministic)이어야 하며 동일한 입력에 대해 동일한 결과를 생성해야 한다.

---

# 48. Validation

Validation은 데이터가 Specification과 Contract를 만족하는지 확인하는 과정이다.

Validation은 시스템 경계에서 수행하는 것을 원칙으로 한다.

---

# 49. Version

Version은 Specification, Contract 또는 Model의 변경 이력을 나타내는 식별 정보이다.

Version은 Identity와 독립적으로 관리된다.

---

# 50. Part Summary

Part 3에서는 Architecture, Generator, Serialization, Transformation 및 Validation과 관련된 핵심 용어를 정의하였다.

이 용어들은 Generator 체계와 Contract 기반 개발 프로세스 전반에서 동일한 의미로 사용되어야 한다.

Part 4에서는 RFC, ADR, Specification, Governance 및 프로젝트 운영과 관련된 용어를 정의한다.

---

# Part 4. Documentation & Governance Terminology

---

# 51. ADR (Architecture Decision Record)

ADR은 프로젝트의 중요한 아키텍처 의사결정을 기록하는 문서이다.

ADR에는 다음 내용을 포함할 수 있다.

- Context
- Decision
- Alternatives
- Consequences

ADR은 구현 이전에 작성하는 것을 원칙으로 한다.

---

# 52. Backward Compatibility

Backward Compatibility는 새로운 변경이 기존 구현과 호환되는 성질을 의미한다.

호환성을 깨는 변경은 Migration Strategy를 함께 정의해야 한다.

---

# 53. Breaking Change

Breaking Change는 기존 Contract, API 또는 Specification과 호환되지 않는 변경이다.

Breaking Change는 RFC와 ADR 절차를 거쳐 승인되어야 한다.

---

# 54. Canonical Representation

Canonical Representation은 특정 개념을 표현하는 유일한 공식 형태를 의미한다.

예시

- Registry Model
- Contract Model
- Domain Model

동일한 개념은 하나의 Canonical Representation만 가져야 한다.

---

# 55. Compliance

Compliance는 구현이 Specification과 Design Principles를 준수하는 정도를 의미한다.

Compliance는 Code Review 및 Verification의 기준으로 사용된다.

---

# 56. Governance

Governance는 프로젝트의 변경 절차와 승인 정책을 정의하는 규칙이다.

대표적인 절차

- RFC
- ADR
- Specification Review
- Verification

---

# 57. Migration Strategy

Migration Strategy는 기존 구조를 새로운 구조로 안전하게 전환하기 위한 절차이다.

Migration은 가능한 자동화하는 것을 권장한다.

---

# 58. RFC (Request for Comments)

RFC는 새로운 기능이나 변경 사항을 제안하기 위한 문서이다.

RFC는 다음 내용을 포함할 수 있다.

- Motivation
- Proposal
- Alternatives
- Compatibility
- Open Questions

RFC는 구현보다 먼저 작성한다.

---

# 59. Review

Review는 문서 또는 구현이 프로젝트 기준을 만족하는지 검토하는 과정이다.

대표적인 Review

- Design Review
- Specification Review
- Code Review

---

# 60. Source of Truth

Source of Truth는 특정 데이터의 유일한 공식 출처를 의미한다.

프로젝트에서는 Registry가 객체 데이터의 Source of Truth이다.

---

# 61. Specification

Specification은 구현과 독립적으로 시스템의 구조와 동작을 정의하는 문서이다.

Specification은 구현의 기준이며, 구현을 설명하는 문서가 아니다.

---

# 62. Verification

Verification은 구현이 Specification과 일치하는지 확인하는 과정이다.

Verification은 다음을 포함할 수 있다.

- Static Analysis
- Runtime Test
- Manual Review

---

# 63. Versioning

Versioning은 문서, Contract 또는 Model의 변경 이력을 관리하는 체계이다.

Version은 의미 있는 변경 단위로 관리한다.

---

# 64. Workspace

Workspace는 프로젝트를 구성하는 작업 단위 또는 실행 범위를 의미한다.

Workspace는 Registry, Context 및 Scope의 기준이 될 수 있다.

---

# 65. Part Summary

Part 4에서는 프로젝트 운영, 문서 체계 및 거버넌스와 관련된 핵심 용어를 정의하였다.

RFC, ADR, Specification, Verification 및 Governance는 프로젝트 변경 관리의 공통 기준으로 사용되어야 한다.

Part 5에서는 약어(Abbreviations), 용어 사용 규칙, Cross Reference 및 Glossary 관리 원칙을 정의한다.

---

# Part 5. Abbreviations, Usage Rules & Governance

---

# 66. Overview

본 장에서는 프로젝트에서 사용하는 약어, 용어 사용 규칙 및 Glossary 관리 원칙을 정의한다.

Glossary는 프로젝트 전체에서 사용하는 공식 용어의 기준 문서이다.

---

# 67. Common Abbreviations

다음 약어를 프로젝트 전반에서 공식적으로 사용한다.

| Abbreviation | Meaning |
|--------------|---------|
| ADR | Architecture Decision Record |
| API | Application Programming Interface |
| AST | Abstract Syntax Tree |
| IDE | Integrated Development Environment |
| JSON | JavaScript Object Notation |
| LSP | Language Server Protocol |
| RFC | Request for Comments |
| URI | Uniform Resource Identifier |
| UUID | Universally Unique Identifier |

새로운 약어를 추가하는 경우 본 문서를 함께 갱신해야 한다.

---

# 68. Naming Rules

용어 사용 시 다음 규칙을 따른다.

- 동일 개념에는 동일한 명칭을 사용한다.
- 하나의 용어에 여러 의미를 부여하지 않는다.
- 약어는 최초 등장 시 전체 명칭을 함께 표기한다.
- 문서 간 용어 표기는 일관성을 유지한다.

예를 들어 `Registry`를 `Store`, `Repository`, `Container` 등으로 혼용하지 않는다.

---

# 69. Preferred Terminology

다음 용어를 우선적으로 사용한다.

| Preferred | Avoid |
|-----------|-------|
| Registry | Store |
| Identity | Object ID |
| Resolver | Lookup Helper |
| Artifact | Output File |
| Specification | Spec Document |
| Contract | DTO (일반적인 의미로 사용할 경우) |
| Snapshot | Cached State |
| Provider | Loader |

문서에서는 Preferred 용어를 사용한다.

---

# 70. Cross References

Glossary는 다음 문서의 공통 기준이 된다.

- CORE_DESIGN_PRINCIPLES
- RFC
- ADR
- Core Specifications
- Contract Specifications
- Generator Specifications
- Implementation Documents

새로운 문서는 본 Glossary를 참조해야 한다.

---

# 71. Glossary Maintenance

Glossary는 프로젝트와 함께 지속적으로 관리한다.

다음 경우 갱신한다.

- 새로운 Core 개념 추가
- 새로운 공식 용어 채택
- 용어 의미 변경
- 용어 폐기(Deprecated)

---

# 72. Governance

Glossary 변경은 일반 문서 수정이 아니라 설계 변경으로 간주한다.

권장 절차

```
RFC

↓

Review

↓

Glossary Update

↓

Specification Update
```

Glossary 변경 이후 관련 문서를 함께 갱신하는 것을 권장한다.

---

# 73. Compliance

모든 프로젝트 문서는 다음 사항을 준수해야 한다.

- Glossary에 정의된 용어를 사용한다.
- 동일 개념을 다른 이름으로 표현하지 않는다.
- 새로운 핵심 용어는 먼저 Glossary에 정의한다.

---

# 74. Document Summary

CORE_GLOSSARY는 VSkript Database 프로젝트에서 사용하는 공식 용어를 정의한다.

모든 Specification, Contract, RFC, ADR 및 Implementation 문서는 본 Glossary를 기준으로 동일한 의미와 명칭을 사용해야 한다.

Glossary는 프로젝트의 공통 언어(Ubiquitous Language)를 제공하며, 장기적인 유지보수성과 문서 일관성을 지원한다.

---

# 75. Future Extensions

향후 필요에 따라 다음 영역의 용어를 추가할 수 있다.

- Generator Framework
- Plugin System
- Runtime Environment
- Storage Layer
- Testing Framework
- CLI
- Build System

Glossary는 프로젝트의 성장에 맞추어 확장될 수 있다.

---

# 76. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Core Glossary |