# SPEC-0014
# Graph Model

Version: 1.0

Status: Draft

---

# 1. Abstract

Graph는 VSkript Database에서 객체(Object)와 객체 간 관계(Relationship)를 표현하는 기본 데이터 모델이다.

모든 Graph는 Node와 Edge로 구성되며, Immutable Snapshot으로 관리된다.

본 Specification은 프로젝트에서 사용하는 모든 Graph의 공통 구조와 규칙을 정의한다.

---

# 2. Motivation

프로젝트는 다양한 종류의 Graph를 사용한다.

예시

- Dependency Graph
- Validation Graph
- Artifact Graph
- Registry Graph
- Symbol Graph
- Reference Graph
- Call Graph

Graph마다 목적은 다르지만 구조와 동작 원칙은 동일하다.

---

# 3. Goals

Graph는 다음 목표를 가진다.

- Immutable
- Deterministic
- Traversable
- Snapshot-based
- Extensible
- Efficient Lookup

---

# 4. Non Goals

Graph는 다음 역할을 수행하지 않는다.

- Business Logic
- Mutable State
- Runtime Cache
- Scheduling
- Request Processing

Graph는 관계를 표현할 뿐, 실행을 제어하지 않는다.

---

# 5. Graph Definition

Graph는 Node와 Edge로 구성되는 관계 모델이다.

```
Node

↓

Edge

↓

Node
```

Graph는 객체와 객체 사이의 의미 있는 관계를 표현한다.

---

# 6. Graph Hierarchy

프로젝트에서 사용하는 Graph의 예시는 다음과 같다.

```
Graph

├── Dependency Graph
├── Validation Graph
├── Artifact Graph
├── Registry Graph
├── Symbol Graph
├── Reference Graph
└── Call Graph
```

모든 Graph는 본 Specification을 따른다.

---

# 7. Core Components

모든 Graph는 다음 요소를 가진다.

```
Graph

├── Graph ID
├── Graph Type
├── Nodes
├── Edges
├── Metadata
├── Version
└── Snapshot
```

각 요소는 명확한 책임을 가진다.

---

# 8. Graph Identity

모든 Graph는 고유한 Graph ID를 가진다.

예시

```
dependency-graph

registry-graph

symbol-graph
```

Graph ID는 식별과 추적에 사용한다.

---

# 9. Graph Type

Graph Type은 Graph의 목적을 나타낸다.

예시

- Dependency
- Registry
- Symbol
- Artifact
- Validation

Graph Type은 생성 이후 변경되지 않는다.

---

# 10. Node

Node는 Graph의 기본 구성 요소이다.

Node는 하나의 객체를 표현한다.

예시

- Symbol
- Resource
- File
- Function
- Event
- Type

Node는 Graph 내에서 고유해야 한다.

---

# 11. Edge

Edge는 두 Node 사이의 관계를 표현한다.

예시

```
Function

↓

calls

↓

Function
```

또는

```
Resource

↓

depends-on

↓

Resource
```

Edge는 방향(Direction)을 가질 수 있다.

---

# 12. Directed Graph

기본적으로 Graph는 Directed Graph를 사용한다.

```
A

↓

depends-on

↓

B
```

역방향 관계는 명시적으로 표현한다.

---

# 13. Metadata

Graph는 다음과 같은 Metadata를 포함할 수 있다.

- Project ID
- Build ID
- Workspace
- Timestamp
- Generator Version

Metadata는 Graph 구조를 변경하지 않는다.

---

# 14. Snapshot

모든 Graph는 Snapshot으로 관리한다.

```
Graph

↓

Snapshot

↓

Read Only
```

Snapshot은 생성 이후 변경되지 않는다.

---

# 15. Part Summary

Part 1에서는 Graph의 기본 개념과 공통 구조를 정의하였다.

Graph는 Node와 Edge로 구성되는 Immutable 관계 모델이며, 모든 구현은 Graph ID, Graph Type, Metadata, Snapshot을 포함하는 공통 구조를 따른다.

Part 2에서는 Node와 Edge의 상세 모델, 관계 유형, 식별 규칙 및 무결성 제약을 정의한다.

---

# Part 2. Nodes, Edges & Integrity

---

# 16. Overview

Graph는 Node와 Edge로 구성된다.

Node는 객체를 표현하고,

Edge는 객체 간 관계를 표현한다.

모든 Graph는 동일한 Node 및 Edge 규칙을 따른다.

---

# 17. Node Model

Node는 하나의 개별 객체(Entity)를 나타낸다.

예시

- Resource
- Function
- Event
- Type
- Variable
- File
- Namespace
- Plugin

Node는 Graph 내부에서 고유해야 한다.

---

# 18. Node Structure

모든 Node는 다음 구조를 가진다.

```
Node

├── Node ID
├── Node Type
├── Metadata
├── Attributes
└── Version
```

필드의 의미는 모든 Graph에서 일관되어야 한다.

---

# 19. Node Identity

Node ID는 Graph 내부에서 유일해야 한다.

Node ID는 생성 이후 변경해서는 안 된다.

Graph 간 동일 객체를 표현하는 경우에는
동일한 논리적 Identity를 사용할 수 있다.

---

# 20. Node Types

대표적인 Node Type

```
Resource
Function
Event
Type
Variable
Namespace
Plugin
File
Module
```

새로운 Node Type 추가는 허용되지만
기존 의미를 변경해서는 안 된다.

---

# 21. Node Attributes

Attributes는 Node의 속성을 표현한다.

예시

- Name
- Visibility
- Location
- Documentation
- Flags

Attributes는 Node의 Identity를 변경하지 않는다.

---

# 22. Edge Model

Edge는 두 Node 사이의 관계를 표현한다.

```
Source Node

↓

Relationship

↓

Target Node
```

Edge는 방향성을 가질 수 있다.

---

# 23. Edge Structure

모든 Edge는 다음 구조를 가진다.

```
Edge

├── Edge ID
├── Source Node
├── Target Node
├── Edge Type
├── Metadata
└── Version
```

Edge 역시 불변(Immutable)이어야 한다.

---

# 24. Edge Types

대표적인 Edge Type

- depends-on
- references
- contains
- calls
- inherits
- implements
- resolves-to
- generated-from

새로운 관계는 기존 의미와 충돌하지 않아야 한다.

---

# 25. Directed vs Undirected

기본적으로 모든 Graph는 Directed Graph를 사용한다.

```
A

↓

depends-on

↓

B
```

양방향 관계는 두 개의 Directed Edge로 표현하는 것을 권장한다.

---

# 26. Self Edges

Node가 자기 자신을 참조하는 Edge는 특별한 의미가 있는 경우에만 허용한다.

예시

- Recursive Call
- Self Reference

불필요한 Self Edge는 생성하지 않는다.

---

# 27. Parallel Edges

동일한 두 Node 사이에 여러 Edge를 둘 수 있다.

예시

```
Function A

↓

calls

↓

Function B

Function A

↓

references

↓

Function B
```

관계의 의미가 다르면 각각 독립적인 Edge로 유지한다.

---

# 28. Cycles

Graph는 순환(Cycle)을 포함할 수 있다.

예시

- Recursive Functions
- Circular Dependencies
- Mutual References

Cycle 허용 여부는 Graph 구현체가 결정한다.

---

# 29. Graph Integrity

Graph는 다음 무결성 규칙을 만족해야 한다.

- 모든 Edge는 유효한 Source Node를 가져야 한다.
- 모든 Edge는 유효한 Target Node를 가져야 한다.
- Node ID는 중복될 수 없다.
- Edge ID는 중복될 수 없다.

무결성이 깨진 Graph는 사용할 수 없다.

---

# 30. Referential Integrity

Node를 제거할 경우 관련 Edge 처리 정책을 명확히 정의해야 한다.

권장 방식

- Edge도 함께 제거
- 새로운 Snapshot 생성

Dangling Edge는 허용하지 않는다.

---

# 31. Graph Validation

Graph 생성 시 다음을 검증한다.

- Duplicate Nodes
- Duplicate Edges
- Missing Nodes
- Invalid References
- Invalid Edge Types

Validation 실패 시 Graph를 생성하지 않는다.

---

# 32. Stable Ordering

Node와 Edge의 순서는 결정적이어야 한다.

동일한 입력은 항상 동일한 Graph 구조와 순서를 생성해야 한다.

Stable Ordering은 테스트와 직렬화에 중요하다.

---

# 33. Part Summary

Part 2에서는 Node와 Edge의 공통 구조, 관계 유형, 무결성 규칙을 정의하였다.

모든 Graph는 Node와 Edge를 중심으로 구성되며, Referential Integrity와 Stable Ordering을 유지해야 한다.

Part 3에서는 Graph의 생성, Snapshot, 변경 모델, 동시성 및 생명주기를 정의한다.

---

# Part 3. Lifecycle, Snapshots & Concurrency

---

# 34. Overview

Graph는 생성 이후 변경되지 않는 Immutable Snapshot이다.

Graph의 변경은 기존 Graph를 수정하는 것이 아니라 새로운 Snapshot을 생성하는 방식으로 수행한다.

---

# 35. Graph Lifecycle

모든 Graph는 동일한 생명주기를 가진다.

```
Create

↓

Populate Nodes

↓

Populate Edges

↓

Validate

↓

Freeze

↓

Snapshot

↓

Read

↓

Dispose
```

Freeze 이후 Graph는 변경할 수 없다.

---

# 36. Graph Creation

Graph 생성은 Graph Builder 또는 Factory를 통해 수행하는 것을 권장한다.

생성 과정

1. Graph 생성
2. Node 추가
3. Edge 추가
4. Validation
5. Freeze
6. Snapshot 생성

부분적으로 생성된 Graph는 사용할 수 없다.

---

# 37. Graph Validation

Freeze 이전 다음 항목을 검증한다.

- Duplicate Nodes
- Duplicate Edges
- Missing References
- Invalid Relationships
- Version Consistency

검증 실패 시 Graph를 폐기한다.

---

# 38. Freeze

Freeze는 Graph를 Immutable 상태로 전환한다.

Freeze 이후에는

- Node 추가
- Node 제거
- Edge 추가
- Edge 제거
- Metadata 변경

모두 금지된다.

Freeze는 한 번만 수행한다.

---

# 39. Snapshot Model

Graph는 Snapshot 단위로 관리한다.

```
Graph v1

↓

Snapshot #1
```

변경이 발생하면

```
Graph v1

↓

Graph v2

↓

Snapshot #2
```

기존 Snapshot은 변경되지 않는다.

---

# 40. Snapshot Sharing

동일 Snapshot은 여러 Consumer가 동시에 공유할 수 있다.

예시

```
Registry

↓

Graph Snapshot

↓

Builder

Generator

Language Server
```

Snapshot 공유는 읽기 전용이다.

---

# 41. Graph Evolution

Graph 변경은 새로운 Graph를 생성하는 방식으로 수행한다.

허용

```
Old Graph

↓

New Graph
```

비허용

```
Old Graph

↓

Modify
```

---

# 42. Incremental Graph Construction

대규모 Graph는 변경된 영역만 다시 생성할 수 있다.

예시

```
Workspace Change

↓

Affected Nodes

↓

Affected Edges

↓

New Snapshot
```

전체 Graph를 항상 다시 생성할 필요는 없다.

---

# 43. Graph Versioning

모든 Graph는 Version을 가진다.

예시

- Graph Version
- Schema Version
- Snapshot Version

Version은 Snapshot과 함께 관리한다.

---

# 44. Concurrency Model

Graph는 Single Writer / Multiple Reader 모델을 따른다.

```
Builder

↓

Freeze

↓

Snapshot

↓

Multiple Readers
```

Reader는 Lock 없이 Graph를 사용할 수 있다.

---

# 45. Memory Model

가능한 한 객체를 공유한다.

예시

```
Snapshot #1

↓

Shared Nodes

↓

Shared Metadata
```

변경되지 않은 객체는 재사용할 수 있다.

---

# 46. Traversal Model

Graph 순회는 다음 원칙을 따른다.

- Stable Ordering
- Deterministic Traversal
- Immutable View

Traversal 중 Graph를 수정해서는 안 된다.

---

# 47. Lookup Model

Graph는 효율적인 조회를 지원해야 한다.

예시

- Node ID Lookup
- Edge Lookup
- Type Lookup
- Neighbor Lookup

Lookup은 Graph를 변경하지 않는다.

---

# 48. Disposal

Graph Snapshot은 더 이상 사용되지 않으면 폐기할 수 있다.

Snapshot 폐기는 다른 Snapshot에 영향을 주지 않는다.

---

# 49. Part Summary

Part 3에서는 Graph의 생명주기와 Snapshot 모델을 정의하였다.

Graph는 Create → Validate → Freeze → Snapshot → Read → Dispose 순서를 따르며, 모든 변경은 새로운 Snapshot을 생성하는 방식으로 수행된다.

Part 4에서는 Graph의 탐색 모델, 알고리즘, 성능 특성 및 확장 모델을 정의한다.

---

# Part 4. Traversal, Queries & Extension

---

# 50. Overview

Graph는 관계를 저장하는 것뿐만 아니라 효율적인 탐색과 조회를 제공해야 한다.

모든 Graph 구현은 동일한 탐색 원칙과 확장 모델을 따른다.

---

# 51. Traversal Principles

Graph Traversal은 다음 원칙을 따른다.

- Deterministic
- Stable Ordering
- Read-only
- Snapshot-based

Traversal은 Graph를 변경해서는 안 된다.

---

# 52. Traversal Types

권장하는 탐색 방식은 다음과 같다.

- Node Traversal
- Edge Traversal
- Neighbor Traversal
- Path Traversal

각 구현체는 목적에 맞는 탐색 방식을 선택할 수 있다.

---

# 53. Lookup Operations

모든 Graph는 최소한 다음 조회 연산을 제공해야 한다.

- Find Node by ID
- Find Nodes by Type
- Find Edge by ID
- Find Edges by Type
- Find Neighbors
- Contains Node
- Contains Edge

조회 연산은 Graph를 변경하지 않는다.

---

# 54. Query Model

복잡한 조회는 Query 객체를 사용하는 것을 권장한다.

예시

```
Graph Query

↓

Filter

↓

Traversal

↓

Result
```

Query는 Graph와 독립적으로 구성할 수 있다.

---

# 55. Path Resolution

Graph는 경로(Path)를 계산할 수 있다.

예시

```
Function A

↓

calls

↓

Function B

↓

calls

↓

Function C
```

Path Resolution은 Graph의 구조를 변경하지 않는다.

---

# 56. Filtering

Traversal 결과는 다양한 조건으로 필터링할 수 있다.

예시

- Node Type
- Edge Type
- Metadata
- Attributes
- Version

Filtering은 읽기 전용 연산이다.

---

# 57. Indexing

대규모 Graph는 Index를 사용할 수 있다.

예시

```
Node ID Index

Edge Type Index

Node Type Index
```

Index는 조회 성능 향상을 위한 보조 구조이며, Graph 자체의 의미를 변경하지 않는다.

---

# 58. Extension Model

새로운 Graph 유형은 기존 모델을 확장하여 정의한다.

예시

```
Graph

├── Dependency Graph
├── Registry Graph
├── Symbol Graph
└── Custom Graph
```

공통 구조와 규칙은 유지해야 한다.

---

# 59. Graph Builders

Graph 생성은 Builder 또는 Factory를 통해 수행하는 것을 권장한다.

예시

```
Graph Builder

↓

Nodes

↓

Edges

↓

Validate

↓

Freeze
```

Builder는 Graph의 무결성을 보장해야 한다.

---

# 60. Composition Rules

Graph는 다른 Graph를 직접 포함하지 않는다.

허용

```
Registry

↓

Registry Graph
```

비허용

```
Dependency Graph

↓

Registry Graph
```

Graph 간 연결은 참조(Reference)를 통해 표현한다.

---

# 61. Serialization

Graph는 직렬화할 수 있다.

지원 가능 형식

- JSON
- Binary
- MessagePack (Optional)

직렬화 결과는 Stable Ordering을 유지해야 한다.

---

# 62. Compatibility

Graph를 복원하거나 공유할 때 다음 항목을 검증한다.

- Graph Version
- Schema Version
- Snapshot Version

호환되지 않는 Graph는 사용해서는 안 된다.

---

# 63. Performance Guidelines

Graph 구현은 다음을 목표로 한다.

- O(1) ID Lookup
- Efficient Neighbor Lookup
- Stable Traversal
- Minimal Memory Overhead

구체적인 자료구조는 구현체가 선택한다.

---

# 64. Graph APIs

권장되는 최소 API는 다음과 같다.

- getNode(id)
- getEdge(id)
- neighbors(node)
- incoming(node)
- outgoing(node)
- contains(id)
- query(criteria)

API 이름은 구현 언어에 맞게 변경할 수 있다.

---

# 65. Part Summary

Part 4에서는 Graph의 탐색, 조회, 확장 및 직렬화 모델을 정의하였다.

모든 Graph는 안정적인 Traversal과 Query를 제공하며, Builder를 통해 생성되고 Snapshot 기반으로 공유된다.

Part 5에서는 Best Practices, Anti-Patterns, Design Principles 및 문서 요약을 다룬다.

---

# Part 5. Best Practices, Anti-Patterns & Design Principles

---

# 66. Overview

본 장에서는 Graph 구현이 따라야 하는 권장 사항과 금지 사항을 정의한다.

본 규칙은 Dependency Graph, Registry Graph, Symbol Graph, Reference Graph, Call Graph를 포함한 모든 Graph 구현에 동일하게 적용된다.

---

# 67. Design Principles

모든 Graph는 다음 원칙을 따른다.

- Immutable by Default
- Snapshot-based
- Deterministic
- Stable Ordering
- Explicit Identity
- Referential Integrity
- Read-optimized
- Extensible

Graph는 관계를 표현하는 모델이며, 실행 로직을 포함하지 않는다.

---

# 68. Best Practices

권장 사항

- Graph Builder 또는 Factory를 통해 생성한다.
- Freeze 이후에는 변경하지 않는다.
- Snapshot 단위로 공유한다.
- Stable Ordering을 유지한다.
- Query와 Lookup을 명확히 구분한다.
- 참조 무결성을 항상 검증한다.
- Graph 변경은 새로운 Snapshot 생성으로 처리한다.
- 대규모 Graph는 Index를 활용한다.

---

# 69. Anti-Patterns

다음 구현은 권장하지 않는다.

- Mutable Graph
- Graph 내부 Business Logic
- Freeze 이후 Node/Edge 수정
- Dangling Edge 허용
- Node ID 중복
- 순회 중 Graph 수정
- Graph를 Cache처럼 사용하는 설계
- Graph 간 직접 포함(Composition)

이러한 구현은 Graph의 일관성과 예측 가능성을 저하시킨다.

---

# 70. Integrity Rules

모든 Graph는 다음 조건을 만족해야 한다.

- 모든 Node ID는 유일하다.
- 모든 Edge ID는 유일하다.
- 모든 Edge는 유효한 Node를 참조한다.
- Snapshot은 완전한 상태를 유지한다.
- Validation을 통과한 Graph만 Freeze할 수 있다.

---

# 71. Evolution Policy

Graph를 변경할 때는 다음 정책을 따른다.

허용

- 새로운 Node Type 추가
- 새로운 Edge Type 추가
- Optional Metadata 추가

비허용

- 기존 관계 의미 변경
- Node ID 재사용
- Snapshot 직접 수정

모든 구조 변경은 Version 정책을 따라야 한다.

---

# 72. Compliance

모든 Graph 구현은 다음 요구사항을 만족해야 한다.

- Immutable
- Snapshot-based
- Deterministic
- Stable Ordering
- Referential Integrity
- Validation before Freeze

구현체는 본 Specification을 준수해야 한다.

---

# 73. Reference Architecture

Graph는 프로젝트 전체에서 다음 위치를 차지한다.

```
Identity
    │
    ▼
Graph
    │
    ▼
Registry
    │
    ▼
Context
    │
    ▼
Planner
    │
    ▼
Pipeline
    │
    ▼
Components
```

Graph는 모든 관계 정보를 표현하는 공통 데이터 모델이다.

---

# 74. Cross References

본 문서는 다음 문서와 함께 사용한다.

- SPEC-0013 Context Model
- SPEC-0015 Pipeline Model
- SPEC-0016 Cache Model
- SPEC-0017 Identity Model
- SPEC-0018 Registry Model

RFC 및 Implementation 문서는 Graph를 정의할 때 본 Specification을 참조한다.

---

# 75. Document Summary

Graph는 VSkript Database에서 객체와 객체 간 관계를 표현하는 공통 데이터 모델이다.

모든 Graph는 Node와 Edge를 중심으로 구성되며, Immutable Snapshot으로 관리된다.

Graph는 Stable Ordering, Referential Integrity, Deterministic Traversal을 보장하며, Builder를 통해 생성되고 여러 컴포넌트에서 안전하게 공유된다.

Builder, Validator, Generator, Registry, Language Server를 포함한 모든 구현은 본 Specification을 준수해야 한다.

---

# 76. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Graph Model Specification |