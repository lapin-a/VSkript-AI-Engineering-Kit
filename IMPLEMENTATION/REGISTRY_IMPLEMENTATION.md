# Part 1. Registry Architecture

---

# 1. Introduction

Registry는 VSkript Database의 핵심 데이터 저장소이다.

Repository에서 수집된 모든 정보는 Registry에 저장되며, 이후 Builder, Validator, Generator, Language Server는 Registry를 통해 동일한 데이터를 참조한다.

Registry는 프로젝트의 Single Source of Truth이다.

---

# 2. Responsibilities

Registry의 주요 책임은 다음과 같다.

- Resource 저장
- Resource 조회
- Index 관리
- Version 관리
- Lookup 제공
- Serialization 지원
- Immutable Snapshot 제공

Registry는 Validation이나 Generation 로직을 포함하지 않는다.

---

# 3. Design Goals

Registry는 다음 목표를 가진다.

- Immutable
- Thread-safe
- Deterministic
- Serializable
- High-performance Lookup
- Low Memory Overhead
- Extensible

---

# 4. Non Goals

Registry는 다음 기능을 제공하지 않는다.

- Repository 탐색
- Validation
- Artifact 생성
- File I/O
- Business Logic

---

# 5. Registry Principles

Registry는 다음 원칙을 따른다.

- Single Source of Truth
- Read-Mostly
- Immutable Snapshot
- Deterministic Ordering
- Stable Identity

---

# 6. Registry Architecture

```
Repository

↓

Builder

↓

Registry

↓

Validator

↓

Generator

↓

Language Server
```

Registry는 모든 주요 컴포넌트의 중심 계층이다.

---

# 7. Internal Structure

Registry는 여러 하위 Registry로 구성된다.

```
Registry

├── Resource Registry
├── Symbol Registry
├── Type Registry
├── Plugin Registry
├── Namespace Registry
├── Index Registry
└── Metadata Registry
```

각 하위 Registry는 독립적으로 관리된다.

---

# 8. Registry Context

Registry는 다음 정보를 포함한다.

```
RegistryContext

├── Version
├── Build ID
├── Schema Version
├── Metadata
└── Configuration
```

Context는 Registry 생성 후 변경되지 않는다.

---

# 9. Registry Lifecycle

Registry는 다음 생명주기를 가진다.

```
Created

↓

Populated

↓

Validated

↓

Frozen

↓

Serialized

↓

Disposed
```

Frozen 이후에는 수정할 수 없다.

---

# 10. Registry Ownership

Builder만 Registry를 생성할 수 있다.

Validator, Generator, Language Server는 읽기 전용으로 접근한다.

Registry의 소유권은 Builder에 있다.

---

# 11. Registry Identity

모든 Registry는 고유한 ID를 가진다.

예시

```
core

symbols

plugins

types

metadata
```

ID는 실행마다 변경되지 않아야 한다.

---

# 12. Registry Snapshot

Registry는 Snapshot을 제공한다.

Snapshot은 Immutable이다.

예시

```
Registry

↓

Snapshot

↓

Read Only Access
```

Snapshot은 여러 컴포넌트에서 안전하게 공유할 수 있다.

---

# 13. Registry State

Registry는 다음 상태를 가진다.

- Mutable (Builder 내부)
- Frozen (공개)
- Serialized
- Disposed

Frozen 상태에서는 어떠한 변경도 허용하지 않는다.

---

# 14. Registry Metrics

Registry는 다음 정보를 수집할 수 있다.

- Resource Count
- Index Count
- Memory Usage
- Lookup Count
- Snapshot Count

Metrics는 진단 및 성능 분석에 활용된다.

---

# 15. Part Summary

Part 1에서는 Registry의 기본 아키텍처와 역할을 정의하였다.

Registry는 모든 컴포넌트가 공유하는 핵심 데이터 계층이며, Immutable Snapshot과 Single Source of Truth 원칙을 기반으로 설계된다.

Part 2에서는 다음 내용을 다룬다.

- Resource Registry
- Symbol Registry
- Namespace Registry
- Type Registry
- Plugin Registry
- Metadata Registry
- Registry Relationships

---

# Part 2. Registry Model & Organization

---

# 16. Overview

Registry는 하나의 거대한 Map이 아니다.

Registry는 여러 Domain Registry로 구성되며,
각 Domain Registry는 자신의 데이터와 Index를 관리한다.

모든 Registry는 동일한 생명주기와 설계 원칙을 따른다.

---

# 17. Domain Registry

Registry는 다음 Domain으로 구성된다.

```
Registry

├── Resource Registry
├── Symbol Registry
├── Type Registry
├── Plugin Registry
├── Namespace Registry
├── Metadata Registry
├── Diagnostic Registry
└── Index Registry
```

각 Domain Registry는 독립적으로 확장 가능해야 한다.

---

# 18. Resource Registry

Resource Registry는 Builder가 수집한 모든 Resource를 저장한다.

관리 대상

- Expressions
- Effects
- Conditions
- Events
- Sections
- Types
- Functions

Resource는 Immutable 객체로 저장한다.

---

# 19. Symbol Registry

Symbol Registry는 모든 Symbol을 관리한다.

예시

```
Type

Function

Variable

Property

Event

Expression
```

Language Server는 Symbol Registry를 기본 조회 대상으로 사용한다.

---

# 20. Type Registry

Type Registry는 타입 시스템을 관리한다.

포함 정보

- Type ID
- Display Name
- Namespace
- Parent Type
- Aliases
- Generic Information

Type Registry는 상속 관계를 유지한다.

---

# 21. Plugin Registry

Plugin Registry는 Plugin 정보를 관리한다.

예시

```
Plugin ID

Version

Resources

Dependencies

Metadata
```

Plugin Registry는 Plugin 간 의존성을 추적할 수 있다.

---

# 22. Namespace Registry

Namespace Registry는 Resource를 Namespace별로 분류한다.

예시

```
skript

skript-reflect

skbee

custom-plugin
```

Namespace는 Lookup 속도를 향상시키기 위한 논리적 그룹이다.

---

# 23. Metadata Registry

Metadata Registry는 프로젝트 메타데이터를 저장한다.

포함 정보

- Build Version
- Schema Version
- Generator Version
- Repository Information
- Configuration Snapshot

Metadata는 Registry 전체에 적용된다.

---

# 24. Diagnostic Registry

Diagnostic Registry는 Validation 결과를 저장한다.

포함 정보

- Diagnostic ID
- Severity
- Source
- Message
- Location
- Related Resource

Diagnostic Registry는 Validator가 생성한다.

---

# 25. Index Registry

Index Registry는 모든 조회 인덱스를 관리한다.

예시

```
ID Index

Name Index

Namespace Index

Plugin Index

Type Index
```

Index는 Registry 데이터와 항상 동기화되어야 한다.

---

# 26. Registry Relationships

Registry 간 관계는 Graph로 표현된다.

```
Plugin

↓

Namespace

↓

Resource

↓

Symbol

↓

Type
```

Registry는 관계를 직접 중복 저장하지 않고 참조를 유지한다.

---

# 27. Registry Graph

Registry Graph는 Domain Registry 간 의존성을 표현한다.

```
Plugin Registry

↓

Resource Registry

↓

Symbol Registry

↓

Type Registry
```

Registry Graph는 순환 참조를 허용하지 않는다.

---

# 28. Identity Model

Registry 내부 객체는 Stable ID를 가진다.

예시

```
plugin:skript

type:itemstack

effect:broadcast

event:join
```

Stable ID는 실행 환경에 영향을 받지 않는다.

---

# 29. Registry References

Registry는 객체를 복사하지 않는다.

모든 관계는 Stable ID 기반 참조를 사용한다.

예시

```
Expression

↓

Return Type ID

↓

Type Registry
```

참조 무결성은 Builder가 보장한다.

---

# 30. Part Summary

Part 2에서는 Registry를 구성하는 Domain Registry와 데이터 모델을 정의하였다.

Registry는 Resource, Symbol, Type, Plugin 등의 데이터를 독립적인 Registry로 분리하여 관리하며, Registry Graph를 통해 이들 간의 관계를 유지한다.

Part 3에서는 다음 내용을 다룬다.

- Lookup Engine
- Indexing
- Search
- Query API
- Caching
- Performance

---

# Part 3. Lookup Engine & Query System

---

# 31. Overview

Registry의 가장 중요한 역할은 저장(Storage)이 아니라 조회(Lookup)이다.

모든 컴포넌트는 Registry를 직접 탐색하지 않고 Lookup Engine을 통해 접근한다.

```
Consumer

↓

Query API

↓

Lookup Engine

↓

Indexes

↓

Registry
```

---

# 32. Lookup Engine

Lookup Engine은 Registry의 공식 조회 계층이다.

책임

- Resource 조회
- Symbol 조회
- Type 조회
- Plugin 조회
- Namespace 조회
- Index 활용
- Query 최적화

Lookup Engine은 Registry를 변경하지 않는다.

---

# 33. Query Model

모든 조회는 Query 기반으로 수행한다.

예시

```
Find by ID

Find by Name

Find by Namespace

Find by Plugin

Find by Type
```

Query는 결정적(Deterministic)이어야 한다.

---

# 34. Query API

권장 인터페이스

```typescript
interface RegistryQuery {

    findById(id: ResourceId);

    findByName(name: string);

    findByNamespace(namespace: NamespaceId);

    findByPlugin(pluginId: PluginId);

    findAll();
}
```

구현체는 내부 Index를 활용하여 최적화한다.

---

# 35. Index Strategy

Registry는 모든 데이터를 선형 탐색하지 않는다.

권장 Index

- ID Index
- Name Index
- Namespace Index
- Plugin Index
- Type Index
- Alias Index

필요한 경우 복합 Index를 추가할 수 있다.

---

# 36. Primary Index

모든 Resource는 Primary ID Index를 가진다.

예시

```
effect:broadcast

↓

Resource
```

Primary Index는 O(1) 조회를 목표로 한다.

---

# 37. Secondary Index

Secondary Index는 다양한 조회를 지원한다.

예시

```
Name

↓

broadcast

↓

Resource List
```

하나의 이름이 여러 Resource에 매핑될 수 있다.

---

# 38. Composite Index

복합 조건 조회를 위해 Composite Index를 사용할 수 있다.

예시

```
Namespace

+

Type

↓

Resource
```

Composite Index는 자주 사용되는 Query에 한하여 생성한다.

---

# 39. Reverse Index

참조 관계를 빠르게 조회하기 위해 Reverse Index를 유지할 수 있다.

예시

```
Type

↓

Referenced Resources
```

Reverse Index는 Generator와 Language Server에서 활용된다.

---

# 40. Lookup Result

Lookup 결과는 Immutable Collection으로 반환한다.

예시

```
Lookup Result

↓

Read-only List<Resource>
```

호출자는 결과를 수정할 수 없다.

---

# 41. Query Optimization

Lookup Engine은 다음 최적화를 수행할 수 있다.

- Index Selection
- Query Reordering
- Cached Lookup
- Lazy Materialization

동일 Query는 동일 결과를 반환해야 한다.

---

# 42. Lazy Lookup

필요한 경우 실제 객체 생성은 지연할 수 있다.

```
Query

↓

ID

↓

Object Materialization
```

Lazy Lookup은 메모리 사용량 감소를 목표로 한다.

---

# 43. Cached Lookup

Lookup 결과는 캐시할 수 있다.

Cache Key

- Query Type
- Parameters
- Registry Version

Registry Version이 변경되면 Cache를 폐기한다.

---

# 44. Lookup Consistency

모든 조회는 동일한 Registry Snapshot을 기준으로 수행한다.

조회 중 Registry가 변경되어서는 안 된다.

Snapshot Isolation을 유지한다.

---

# 45. Search API

Lookup 외에 Search 기능을 제공할 수 있다.

예시

```
Prefix Search

Substring Search

Fuzzy Search

Regex Search
```

Search는 Lookup보다 비용이 높다.

---

# 46. Pagination

대량 조회는 Pagination을 지원할 수 있다.

예시

```
Offset

Limit
```

Pagination은 결과 순서를 변경해서는 안 된다.

---

# 47. Query Metrics

Lookup Engine은 다음 정보를 수집할 수 있다.

- Query Count
- Average Lookup Time
- Cache Hit Ratio
- Index Usage
- Search Count

Metrics는 성능 분석에 활용된다.

---

# 48. Thread Safety

Lookup Engine은 다중 스레드에서 동시에 호출될 수 있다.

모든 조회는 Lock 없이(Read-Only) 수행되는 것을 목표로 한다.

---

# 49. Error Handling

Lookup 실패는 예외보다 명시적인 결과를 반환하는 것을 권장한다.

예시

```
Optional<Resource>

Result<T>

Empty Collection
```

Lookup 실패는 Registry를 변경하지 않는다.

---

# 50. Part Summary

Part 3에서는 Registry의 Lookup Engine과 Query System을 정의하였다.

Registry는 Index 기반 조회를 제공하며, 모든 접근은 Query API를 통해 수행된다.

Snapshot Isolation과 Immutable Collection을 기반으로 Builder, Validator, Generator, Language Server가 동일한 Registry를 안전하게 공유할 수 있다.

Part 4에서는 다음 내용을 다룬다.

- Serialization
- Persistence
- Versioning
- Compatibility
- Import / Export
- Snapshot Management

---

# Part 4. Serialization, Snapshot & Versioning

---

# 51. Overview

Registry는 메모리에만 존재하는 객체가 아니다.

Registry는 직렬화(Serialization)할 수 있으며,
Snapshot으로 저장하고,
다시 복원(Deserialization)할 수 있어야 한다.

또한 Registry는 Version과 Compatibility를 관리한다.

---

# 52. Serialization

Registry는 결정적(Deterministic) 직렬화를 지원한다.

지원 형식

- JSON
- Binary
- MessagePack (Optional)

직렬화 결과는 실행 환경에 영향을 받아서는 안 된다.

---

# 53. Serialization Rules

직렬화 시 다음 규칙을 따른다.

- Stable Ordering
- Stable IDs
- Immutable Snapshot
- No Runtime State
- No Transient Fields

동일 Registry는 항상 동일한 출력 결과를 생성해야 한다.

---

# 54. Registry Snapshot

Snapshot은 Registry의 읽기 전용 상태를 나타낸다.

```
Registry

↓

Freeze

↓

Snapshot
```

Snapshot은 Build Pipeline 전체에서 공유될 수 있다.

---

# 55. Snapshot Lifecycle

Snapshot은 다음 생명주기를 가진다.

```
Create

↓

Freeze

↓

Serialize

↓

Store

↓

Load

↓

Dispose
```

Snapshot은 생성 이후 변경되지 않는다.

---

# 56. Snapshot Isolation

모든 Consumer는 동일한 Snapshot을 사용한다.

예시

```
Builder

↓

Snapshot

↓

Validator

↓

Generator

↓

Language Server
```

Snapshot은 실행 중 변경되지 않는다.

---

# 57. Registry Version

Registry는 자체 Version을 가진다.

예시

```
Registry Version

Schema Version

Build Version

Generator Version
```

Version 정보는 Metadata Registry에 저장한다.

---

# 58. Compatibility

Registry는 Compatibility를 검증해야 한다.

검증 대상

- Schema Version
- Builder Version
- Generator Version
- Validator Version

호환되지 않는 Registry는 로드해서는 안 된다.

---

# 59. Import

Registry는 외부에서 Import할 수 있다.

예시

```
Registry File

↓

Loader

↓

Registry
```

Import 시 Integrity를 반드시 검증한다.

---

# 60. Export

Registry는 Export를 지원한다.

예시

```
Registry

↓

Serializer

↓

Registry File
```

Export는 Snapshot 상태에서만 수행한다.

---

# 61. Registry Manifest

Registry는 자체 Manifest를 생성할 수 있다.

예시

```
registry-manifest.json
```

포함 정보

- Registry Version
- Schema Version
- Build ID
- Resource Count
- Checksum
- Generated Time

Manifest는 Registry 검증의 기준 문서이다.

---

# 62. Integrity Validation

Registry를 로드하기 전에 무결성을 검증한다.

검증 항목

- Version
- Manifest
- Checksum
- Required Sections
- Index Consistency

무결성 검증 실패 시 Registry를 폐기한다.

---

# 63. Registry Migration

Schema 변경 시 Registry Migration을 수행할 수 있다.

```
Old Registry

↓

Migration

↓

New Registry
```

Migration은 항상 새로운 Registry를 생성한다.

기존 Registry를 직접 수정해서는 안 된다.

---

# 64. Registry Diff

두 Registry 간 차이를 계산할 수 있다.

비교 대상

- Resources
- Symbols
- Types
- Plugins
- Metadata

Diff는 Incremental Build와 Debugging에 활용된다.

---

# 65. Incremental Snapshot

Snapshot은 변경된 부분만 저장할 수 있다.

```
Snapshot V1

↓

Diff

↓

Snapshot V2
```

Incremental Snapshot은 저장 공간을 줄이는 것을 목표로 한다.

---

# 66. Compression

Snapshot은 압축 저장할 수 있다.

권장 방식

- Zstandard
- GZIP

압축은 Snapshot의 논리적 내용을 변경하지 않는다.

---

# 67. Persistence Strategy

Registry 저장 전략

- Local File
- Memory
- Embedded Database (Optional)
- Remote Storage (Future)

Core는 저장 매체에 의존하지 않는다.

---

# 68. Recovery

손상된 Registry는 가능한 경우 복구를 시도한다.

복구 대상

- Missing Index
- Invalid Cache
- Stale Metadata

Core Data가 손상된 경우 즉시 실패한다.

---

# 69. Future Extensions

향후 확장

- Distributed Registry
- Remote Registry
- Shared Registry Cache
- Read Replica
- Cloud Registry

현재 구조는 이러한 확장을 제한하지 않는다.

---

# 70. Part Summary

Part 4에서는 Registry의 Serialization, Snapshot, Versioning, Compatibility를 정의하였다.

Registry는 Immutable Snapshot을 기반으로 직렬화되며, Version과 Manifest를 통해 무결성과 호환성을 보장한다.

Part 5에서는 다음 내용을 다룬다.

- Thread Safety
- Memory Model
- Performance
- Registry Cache
- Best Practices
- Design Principles
- Document Summary

---

# Part 5. Performance, Thread Safety & Best Practices

---

# 71. Overview

Registry는 프로젝트 전체에서 가장 많이 참조되는 컴포넌트이다.

따라서 Registry는 다음 특성을 만족해야 한다.

- High Performance
- Thread-safe
- Immutable
- Deterministic
- Low Memory Usage

Registry는 읽기(Read)가 쓰기(Write)보다 압도적으로 많은 구조(Read-Mostly)를 전제로 설계한다.

---

# 72. Thread Safety

Frozen Registry는 여러 스레드에서 동시에 접근할 수 있어야 한다.

```
Builder

↓

Freeze

↓

Read Only Registry

↓

Validator

Generator

Language Server
```

읽기 작업은 Lock-Free를 목표로 한다.

---

# 73. Concurrency Model

Registry는 단일 Writer와 다수의 Reader를 지원한다.

```
Builder
   │
   ▼
Registry (Mutable)
   │
 Freeze
   ▼
Registry (Immutable)
   │
 ├─────────────┐
 ▼             ▼
Validator   Generator
      │
      ▼
Language Server
```

Builder만 Registry를 수정할 수 있다.

---

# 74. Memory Model

Registry는 객체 중복 생성을 최소화해야 한다.

권장 사항

- Shared References
- Immutable Objects
- Lazy Materialization
- String Interning (Optional)

동일한 Resource는 하나의 인스턴스만 유지한다.

---

# 75. Cache Strategy

Registry는 내부 Cache를 사용할 수 있다.

권장 Cache

- Query Cache
- Lookup Cache
- Symbol Cache
- Namespace Cache

Cache는 Registry Version에 종속된다.

---

# 76. Cache Invalidation

다음 경우 Cache를 폐기한다.

- Registry Version 변경
- Snapshot 변경
- Index 재생성
- Schema 변경

Frozen Registry에서는 Cache가 변경되어서는 안 된다.

---

# 77. Performance Targets

권장 성능 목표

| Metric | Target |
|---------|--------:|
| ID Lookup | O(1) |
| Name Lookup | O(log n) 이하 |
| Symbol Lookup | O(log n) 이하 |
| Snapshot Creation | Linear |
| Serialization | Linear |

Lookup 성능은 Repository 크기에 크게 영향을 받지 않아야 한다.

---

# 78. Monitoring

Registry는 다음 정보를 제공할 수 있다.

- Resource Count
- Symbol Count
- Query Count
- Cache Hit Ratio
- Memory Usage
- Serialization Time

Metrics는 Telemetry 시스템과 연동할 수 있다.

---

# 79. Failure Handling

Registry는 다음 오류를 처리해야 한다.

- Missing Resource
- Invalid Reference
- Broken Index
- Corrupted Snapshot
- Version Mismatch

오류는 가능한 한 Registry 외부로 전파하지 않고 명확한 Result 또는 Diagnostic으로 표현하는 것을 권장한다.

---

# 80. Security Considerations

Registry는 외부 입력을 신뢰하지 않는다.

Import 시 반드시 검증한다.

- Checksum
- Version
- Manifest
- Schema
- Structure

검증 실패 시 Registry를 생성하지 않는다.

---

# 81. Extension Points

Registry는 다음 확장을 허용한다.

- Custom Registry
- Custom Index
- Custom Query Planner
- Custom Serializer
- Custom Cache Provider

확장은 Core API를 변경하지 않아야 한다.

---

# 82. Design Principles

Registry 구현은 다음 원칙을 따른다.

- Single Source of Truth
- Immutable by Default
- Deterministic Ordering
- Stable Identity
- Read-Mostly
- Snapshot Isolation
- Explicit Versioning

이 원칙은 모든 Registry 구현에 적용된다.

---

# 83. Best Practices

권장 사항

- Registry를 직접 수정하지 않는다.
- Builder 외부에서 Mutable Registry를 생성하지 않는다.
- Stable ID를 사용한다.
- Query API를 통해 접근한다.
- Index를 우회하지 않는다.
- Snapshot을 공유한다.

---

# 84. Anti-Patterns

다음 구현은 권장하지 않는다.

- Registry 직접 탐색
- Mutable Snapshot
- Runtime ID 생성
- 순환 참조 저장
- Index 없는 선형 탐색
- Consumer에서 Registry 수정

이러한 구현은 성능과 일관성을 저하시킨다.

---

# 85. Document Summary

Registry는 VSkript Database의 핵심 데이터 계층이며, Builder가 생성하고 Validator, Generator, Language Server가 공유하는 Single Source of Truth이다.

Registry는 Domain Registry, Query Engine, Index Registry, Snapshot, Versioning을 기반으로 동작하며, 결정적이고 불변(Immutable)인 데이터 모델을 제공한다.

모든 데이터 접근은 Query API를 통해 수행되며, Registry는 프로젝트 전체의 데이터 일관성과 성능을 보장하는 기반 컴포넌트이다.

---

# 86. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Registry Implementation Guide |