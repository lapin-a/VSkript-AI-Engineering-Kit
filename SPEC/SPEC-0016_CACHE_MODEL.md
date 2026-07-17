# SPEC-0016
# Cache Model

Version: 1.0

Status: Draft

---

# Part 1. Cache Fundamentals

---

# 1. Abstract

Cache는 VSkript Database에서 반복 계산을 줄이고 실행 성능을 향상시키기 위한 공통 데이터 모델이다.

모든 Cache는 명확한 Key와 Value를 가지며, Cache Policy와 Lifetime에 따라 관리된다.

본 Specification은 프로젝트에서 사용하는 모든 Cache의 공통 구조와 동작 원칙을 정의한다.

---

# 2. Motivation

프로젝트의 다양한 컴포넌트는 동일한 데이터를 반복적으로 계산한다.

예시

- Symbol Resolution
- Definition Lookup
- Completion Items
- Generated Artifacts
- Registry Lookup
- Graph Traversal

Cache는 이러한 반복 작업을 줄여 응답성과 처리 성능을 향상시킨다.

---

# 3. Goals

Cache는 다음 목표를 가진다.

- Fast Lookup
- Deterministic Results
- Snapshot Compatibility
- Incremental Updates
- Predictable Invalidation
- Low Overhead

---

# 4. Non Goals

Cache는 다음 역할을 수행하지 않는다.

- Primary Data Storage
- Business Logic
- Long-term Persistence
- Source of Truth
- Object Ownership

Cache는 원본 데이터를 대체하지 않는다.

---

# 5. Cache Definition

Cache는 계산 결과를 재사용하기 위한 임시 저장소이다.

```
Input

↓

Compute

↓

Cache

↓

Lookup
```

Cache는 동일한 입력에 대해 동일한 결과를 빠르게 제공한다.

---

# 6. Cache Hierarchy

프로젝트에서 사용하는 Cache의 예시는 다음과 같다.

```
Cache

├── Registry Cache
├── Symbol Cache
├── Definition Cache
├── Completion Cache
├── Artifact Cache
├── Graph Cache
└── Pipeline Cache
```

모든 Cache는 본 Specification을 따른다.

---

# 7. Core Components

모든 Cache는 다음 요소를 가진다.

```
Cache

├── Cache ID
├── Cache Key
├── Cache Value
├── Policy
├── Lifetime
├── Metadata
└── Version
```

---

# 8. Cache Identity

모든 Cache는 고유한 Cache ID를 가진다.

예시

```
registry-cache

symbol-cache

artifact-cache
```

Cache ID는 생성 이후 변경되지 않는다.

---

# 9. Cache Scope

Cache는 적용 범위를 가진다.

대표적인 Scope

- Global
- Workspace
- Project
- Session
- Request
- Stage

Scope는 Cache Lifetime과 일관되어야 한다.

---

# 10. Cache Entry

Cache는 하나 이상의 Cache Entry를 포함한다.

```
Cache

↓

Entry A

Entry B

Entry C
```

Entry는 Key와 Value의 쌍으로 구성된다.

---

# 11. Cache Key

Cache Key는 Entry를 식별하는 유일한 키이다.

Key는 Stable하며 동일 입력에 대해 동일하게 생성되어야 한다.

---

# 12. Cache Value

Cache Value는 계산 결과를 저장한다.

예시

- Symbol
- Definition
- Diagnostics
- Generated Artifact
- Execution Result

Value는 원본 객체의 소유권을 가져서는 안 된다.

---

# 13. Metadata

Cache는 실행 관련 Metadata를 포함할 수 있다.

예시

- Created Time
- Last Access
- Hit Count
- Miss Count
- Producer

Metadata는 Cache Value를 변경하지 않는다.

---

# 14. Lifetime

모든 Cache는 Lifetime을 가진다.

예시

- Request
- Session
- Workspace
- Process

Lifetime은 Scope보다 길어서는 안 된다.

---

# 15. Part Summary

Part 1에서는 Cache의 기본 개념과 공통 구조를 정의하였다.

Cache는 Key와 Value를 중심으로 구성되며, Scope와 Lifetime을 통해 관리되는 성능 최적화 모델이다.

Part 2에서는 Cache 구조, Entry 모델, Lookup 및 Storage 규칙을 정의한다.

---

# Part 2. Cache Structure & Lookup Model

---

# 16. Overview

Cache는 Cache Entry의 집합으로 구성된다.

각 Entry는 Cache Key를 통해 식별되며, Cache Value를 저장한다.

Lookup은 Cache Key를 기준으로 수행된다.

---

# 17. Cache Structure

모든 Cache는 다음 구조를 가진다.

```
Cache

├── Cache ID
├── Entries
├── Policy
├── Lifetime
├── Metadata
└── Statistics
```

Cache 구현은 내부 저장 방식과 관계없이 동일한 논리 구조를 제공해야 한다.

---

# 18. Cache Entry

Cache Entry는 Cache의 최소 저장 단위이다.

```
Cache Entry

├── Key
├── Value
├── State
├── Created At
├── Updated At
└── Metadata
```

Entry는 독립적으로 관리된다.

---

# 19. Cache Key Rules

Cache Key는 다음 조건을 만족해야 한다.

- Unique
- Stable
- Deterministic
- Comparable

동일한 입력은 항상 동일한 Cache Key를 생성해야 한다.

---

# 20. Lookup Model

Lookup은 Cache Key를 기준으로 수행한다.

```
Lookup Request

↓

Cache Key

↓

Cache Store

↓

Cache Entry
```

Lookup은 Cache의 내부 구현에 의존하지 않는다.

---

# 21. Lookup Result

Lookup은 다음 결과 중 하나를 반환한다.

- Hit
- Miss

Hit는 Cache Entry를 반환하며,

Miss는 새로운 계산을 요구한다.

---

# 22. Cache Store

Cache Store는 Entry를 관리하는 저장소이다.

대표적인 구현 예시

- Hash Map
- Concurrent Map
- LRU Store
- Snapshot Store

구현 방식은 외부 API에 영향을 주어서는 안 된다.

---

# 23. Read Policy

Cache 조회는 읽기 전용으로 수행한다.

```
Lookup

↓

Read Entry

↓

Return Value
```

Lookup은 Cache를 변경하지 않는다.

---

# 24. Write Policy

Cache 저장은 계산 완료 후 수행한다.

```
Compute

↓

Create Entry

↓

Store Entry
```

동일 Key에 대한 저장 정책은 Cache 구현체가 정의한다.

---

# 25. Update Policy

Entry 갱신은 다음 경우에 수행된다.

- 새로운 계산 결과
- Invalidation 이후 재생성
- 명시적 Refresh

Update는 기존 Key를 유지하는 것을 원칙으로 한다.

---

# 26. Entry State

모든 Entry는 상태를 가진다.

- Valid
- Invalid
- Expired
- Refreshing

상태는 Cache Policy에 따라 변경된다.

---

# 27. Statistics

Cache는 다음 통계를 기록할 수 있다.

- Hit Count
- Miss Count
- Entry Count
- Eviction Count
- Refresh Count

Statistics는 Cache의 동작을 변경하지 않는다.

---

# 28. Consistency

Cache는 동일 Snapshot에 대해 일관된 결과를 반환해야 한다.

동일한 Snapshot에서는 동일한 Key가 항상 동일한 Value를 반환해야 한다.

---

# 29. Thread Safety

동시 접근을 지원하는 Cache는 다음을 보장해야 한다.

- Safe Lookup
- Safe Insert
- Safe Update
- Safe Remove

Thread Safety는 구현체의 책임이다.

---

# 30. Storage Independence

Cache Model은 특정 저장 방식에 의존하지 않는다.

예를 들어 다음 구현은 모두 허용된다.

- In-memory
- Shared Memory
- Persistent Cache
- Distributed Cache

Specification은 논리적 동작만 정의한다.

---

# 31. Cache Metrics

Cache는 다음 메트릭을 제공할 수 있다.

- Hit Ratio
- Average Lookup Time
- Average Insert Time
- Memory Usage
- Entry Lifetime

Metrics는 운영 및 성능 분석 목적으로 사용한다.

---

# 32. Stable Lookup

동일한 Cache Snapshot에서는

동일한 Key가 항상 동일한 Entry를 반환해야 한다.

Lookup 결과는 실행 순서에 영향을 받아서는 안 된다.

---

# 33. Part Summary

Part 2에서는 Cache의 내부 구조와 Lookup 모델을 정의하였다.

모든 Cache는 Entry 중심으로 구성되며, Key를 기준으로 조회되고 Policy에 따라 저장 및 갱신된다.

Part 3에서는 Cache Lifecycle, 생성, 만료, 갱신 및 삭제 과정을 정의한다.

---

# Part 3. Cache Lifecycle

---

# 34. Overview

모든 Cache는 명확한 Lifecycle을 가진다.

Lifecycle은 Cache Entry의 생성부터 제거까지의 상태 변화를 정의하며, 모든 Cache 구현은 동일한 Lifecycle을 따라야 한다.

---

# 35. Cache Lifecycle

Cache는 다음 생명주기를 따른다.

```
Create

↓

Populate

↓

Lookup

↓

Update

↓

Invalidate

↓

Evict

↓

Dispose
```

Lifecycle은 Cache 종류와 관계없이 동일한 의미를 가진다.

---

# 36. Cache Creation

Cache는 필요 시 생성된다.

생성 시 수행되는 작업

- Cache ID 초기화
- Policy 적용
- Metadata 초기화
- Statistics 초기화

생성 시 Cache Entry는 존재하지 않을 수 있다.

---

# 37. Entry Creation

새로운 계산 결과는 Cache Entry를 생성한다.

```
Compute

↓

Cache Entry

↓

Store
```

Entry 생성은 기존 Cache의 일관성을 유지해야 한다.

---

# 38. Cache Population

Population은 Cache를 실제 데이터로 채우는 과정이다.

Population 방법

- Lazy Population
- Eager Population
- Background Population

Pipeline 또는 Provider가 적절한 방식을 선택할 수 있다.

---

# 39. Cache Lookup

Lookup은 Cache Lifecycle에서 가장 빈번하게 수행되는 작업이다.

Lookup은

- Hit
- Miss

중 하나를 반환한다.

Lookup 자체는 Cache를 변경하지 않는다.

---

# 40. Cache Refresh

Refresh는 기존 Entry를 새로운 계산 결과로 교체하는 과정이다.

```
Old Entry

↓

Recompute

↓

New Entry
```

Refresh는 동일한 Cache Key를 유지하는 것을 원칙으로 한다.

---

# 41. Cache Update

Update는 Cache Value를 최신 상태로 유지하기 위한 작업이다.

Update는 다음 경우 수행된다.

- Refresh
- Explicit Update
- Background Synchronization

Update 이후 Entry는 Valid 상태가 된다.

---

# 42. Cache Expiration

Expiration은 Entry가 더 이상 사용할 수 없는 상태가 되는 것을 의미한다.

Expiration 원인

- Lifetime 초과
- Snapshot 변경
- Configuration 변경

Expired Entry는 Lookup 대상으로 사용해서는 안 된다.

---

# 43. Cache Eviction

Eviction은 Entry를 Cache에서 제거하는 과정이다.

대표적인 정책

- LRU
- LFU
- FIFO
- TTL
- Manual

Eviction은 Cache Store의 책임이다.

---

# 44. Cache Disposal

Cache가 더 이상 필요하지 않을 경우 모든 Resource를 해제한다.

예시

- Entry 제거
- Statistics 초기화
- Metadata 제거

Dispose 이후 Cache는 사용할 수 없다.

---

# 45. Lifetime Management

Lifetime은 Cache의 생존 기간을 정의한다.

대표적인 Lifetime

- Request
- Session
- Workspace
- Process

Lifetime이 종료되면 Cache는 Invalidate 또는 Dispose되어야 한다.

---

# 46. Resource Management

Cache 구현은 메모리 사용량을 관리해야 한다.

권장 사항

- Maximum Entry Count
- Maximum Memory Usage
- Automatic Cleanup
- Background Eviction

Resource 관리는 Cache의 논리적 동작을 변경해서는 안 된다.

---

# 47. Snapshot Awareness

Snapshot 기반 Cache는 Snapshot의 생명주기를 따라야 한다.

새로운 Snapshot이 생성되면 이전 Snapshot을 기반으로 한 Cache는 재검증되어야 한다.

필요 시 새로운 Cache Entry를 생성한다.

---

# 48. Concurrent Lifecycle

동시 접근 환경에서는 Lifecycle 전환이 원자적으로 수행되어야 한다.

예시

- Lookup 중 Eviction
- Refresh 중 Lookup
- Update 중 Invalidation

구현체는 Race Condition을 방지해야 한다.

---

# 49. Lifecycle Events

Cache는 다음 이벤트를 발생시킬 수 있다.

- Created
- Updated
- Refreshed
- Invalidated
- Evicted
- Disposed

이벤트는 Monitoring과 Diagnostics에 활용할 수 있다.

---

# 50. Part Summary

Part 3에서는 Cache Lifecycle을 정의하였다.

모든 Cache는 생성, 조회, 갱신, 만료 및 제거 과정을 일관된 Lifecycle로 관리하며, Snapshot 및 Lifetime과 연계하여 안정적인 성능을 제공한다.

Part 4에서는 Cache Invalidation, Incremental Update 및 Cache Policy를 정의한다.

---

# Part 4. Cache Invalidation & Incremental Updates

---

# 51. Overview

Cache는 원본 데이터(Source of Truth)의 변경을 반영하기 위해 적절한 시점에 무효화(Invalidation)되어야 한다.

Invalidation은 Cache의 정확성과 일관성을 유지하기 위한 핵심 메커니즘이다.

---

# 52. Invalidation Principle

Cache는 Source of Truth보다 오래된 정보를 제공해서는 안 된다.

Cache는 항상 Registry, Graph, Context 또는 Snapshot의 변경을 기준으로 무효화되어야 한다.

---

# 53. Invalidation Triggers

다음 이벤트는 Cache Invalidation을 유발할 수 있다.

- Registry 변경
- Graph 변경
- Context 변경
- Configuration 변경
- Workspace 변경
- Snapshot 생성
- 명시적 Refresh 요청

Pipeline 구현은 필요한 Trigger를 선택할 수 있다.

---

# 54. Full Invalidation

Full Invalidation은 Cache 전체를 무효화하는 방식이다.

```
Cache

↓

Invalidate All

↓

Empty Cache
```

대규모 변경이 발생한 경우 사용한다.

---

# 55. Partial Invalidation

Partial Invalidation은 특정 Entry만 무효화한다.

```
Cache

├── Entry A
├── Entry B
└── Entry C

↓

Invalidate Entry B
```

가능한 경우 Full Invalidation보다 우선한다.

---

# 56. Dependency-aware Invalidation

Entry 간 의존성이 존재하는 경우 변경은 의존 관계를 따라 전파된다.

```
Entry A

↓

Entry B

↓

Entry C
```

Entry A가 무효화되면 B와 C도 재검증되어야 한다.

---

# 57. Snapshot-based Invalidation

새로운 Snapshot이 생성되면 이전 Snapshot 기반 Cache는 재사용 여부를 판단해야 한다.

- 재사용 가능 → 유지
- 재사용 불가 → 무효화

Snapshot은 Cache의 일관성을 유지하는 기준이 된다.

---

# 58. Incremental Update

Incremental Update는 변경된 데이터만 다시 계산하는 방식이다.

```
Old Cache

↓

Detect Changes

↓

Update Entries

↓

New Cache
```

변경되지 않은 Entry는 유지한다.

---

# 59. Cache Consistency

모든 Cache는 다음 사항을 만족해야 한다.

- 동일 Snapshot에서는 동일 결과를 반환한다.
- Invalid Entry를 반환하지 않는다.
- Lookup 중 상태가 변경되어서는 안 된다.

Cache Consistency는 구현체가 보장해야 한다.

---

# 60. Cache Synchronization

여러 Cache가 동일한 Source를 참조하는 경우 동기화가 필요할 수 있다.

예시

- Registry Cache
- Symbol Cache
- Definition Cache

동기화는 Source of Truth를 기준으로 수행한다.

---

# 61. Refresh Policy

Refresh는 다음 정책 중 하나를 사용할 수 있다.

- Manual Refresh
- Automatic Refresh
- Background Refresh
- Scheduled Refresh

정책은 Cache 종류에 따라 선택한다.

---

# 62. Cache Policy

대표적인 Cache Policy

- Write-through
- Write-back
- Read-through
- Read-only

Specification은 정책의 존재를 정의하며, 구체적인 선택은 구현체의 책임이다.

---

# 63. Eviction Strategy

Entry 제거 전략은 다음 중 하나를 사용할 수 있다.

- Least Recently Used (LRU)
- Least Frequently Used (LFU)
- First In First Out (FIFO)
- Time To Live (TTL)
- Manual Eviction

Eviction은 Cache Store가 수행한다.

---

# 64. Monitoring

Cache는 다음 정보를 제공할 수 있다.

- Hit Ratio
- Miss Ratio
- Refresh Count
- Invalidation Count
- Eviction Count
- Memory Usage

Monitoring은 운영과 성능 분석을 위한 기능이다.

---

# 65. Part Summary

Part 4에서는 Cache Invalidation과 Incremental Update를 정의하였다.

Cache는 Source of Truth의 변경을 기준으로 무효화되며, 가능한 경우 Partial Invalidation과 Incremental Update를 통해 불필요한 재계산을 줄인다.

Part 5에서는 Best Practices, Anti-Patterns, Design Principles 및 Reference Architecture를 정의한다.

---

# Part 5. Best Practices, Anti-Patterns & Design Principles

---

# 66. Overview

본 장에서는 모든 Cache 구현이 따라야 하는 설계 원칙과 권장 사항을 정의한다.

본 규칙은 Registry Cache, Symbol Cache, Definition Cache, Completion Cache, Artifact Cache 및 향후 추가되는 모든 Cache 구현에 동일하게 적용된다.

---

# 67. Design Principles

모든 Cache는 다음 원칙을 따른다.

- Source of Truth Separation
- Immutable Cache Entries
- Deterministic Lookup
- Snapshot Compatibility
- Explicit Invalidation
- Predictable Lifetime
- Incremental Updates
- Observable Behaviour

Cache는 성능을 위한 계층이며 데이터의 소유자가 아니다.

---

# 68. Best Practices

권장 사항

- 가능한 작은 Scope의 Cache를 사용한다.
- Cache Key는 Stable하고 Deterministic하게 생성한다.
- Snapshot을 기준으로 Cache를 관리한다.
- Partial Invalidation을 우선 적용한다.
- 변경되지 않은 Entry는 재사용한다.
- Lifetime을 명확하게 정의한다.
- Cache Statistics를 지속적으로 기록한다.
- Cache Policy를 명시적으로 선언한다.

---

# 69. Anti-Patterns

다음 구현은 권장하지 않는다.

- Cache를 Source of Truth로 사용하는 구현
- Mutable Cache Entry
- Cache Key 변경
- 암묵적인 Invalidation
- 무제한 Lifetime
- 전역 Cache 남용
- Lookup 중 Entry 수정
- Cache 간 순환 의존성

이러한 구현은 일관성과 예측 가능성을 저하시킨다.

---

# 70. Cache Guarantees

모든 Cache는 다음 사항을 보장해야 한다.

- 동일한 Snapshot에서는 동일한 Lookup 결과를 반환한다.
- Invalid Entry를 반환하지 않는다.
- Cache Key는 실행 중 변경되지 않는다.
- Cache Value는 Snapshot과 일관성을 유지한다.
- Source of Truth보다 오래된 데이터를 의도적으로 유지하지 않는다.

---

# 71. Relationship with Other Models

Cache는 다른 Core Specification과 다음 관계를 가진다.

```
Identity
     │
     ▼
Registry
     │
     ▼
Graph
     │
     ▼
Context
     │
     ▼
Pipeline
     │
     ▼
Cache
```

Cache는 다른 모델을 보조하는 계층이며 독립적인 데이터 모델이 아니다.

---

# 72. Compliance

모든 Cache 구현은 다음 Specification을 준수해야 한다.

- SPEC-0013 Context Model
- SPEC-0014 Graph Model
- SPEC-0015 Pipeline Model
- SPEC-0017 Identity Model
- SPEC-0018 Registry Model

또한 CORE_DESIGN_PRINCIPLES에서 정의하는 공통 설계 원칙을 따른다.

---

# 73. Reference Architecture

Cache의 참조 구조는 다음과 같다.

```
Source of Truth

↓

Change Detection

↓

Cache Policy

↓

Cache Store

↓

Lookup

↓

Consumer
```

Consumer는 Cache를 직접 수정하지 않으며, 모든 변경은 Source of Truth의 변경을 통해 반영된다.

---

# 74. Cross References

본 문서는 다음 문서와 함께 사용한다.

- CORE_DESIGN_PRINCIPLES
- SPEC-0013 Context Model
- SPEC-0014 Graph Model
- SPEC-0015 Pipeline Model
- SPEC-0017 Identity Model
- SPEC-0018 Registry Model

RFC 및 Implementation 문서는 Cache를 정의할 때 본 Specification을 참조한다.

---

# 75. Document Summary

Cache는 VSkript Database의 성능 최적화를 위한 공통 모델이다.

모든 Cache는 Key와 Value를 중심으로 구성되며, Snapshot과 Lifetime을 기반으로 관리된다.

Cache는 Source of Truth를 대체하지 않으며, 명시적인 Invalidation과 Incremental Update를 통해 항상 최신 상태를 유지한다.

프로젝트의 모든 Cache 구현은 본 Specification을 준수해야 한다.

---

# 76. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Cache Model Specification |