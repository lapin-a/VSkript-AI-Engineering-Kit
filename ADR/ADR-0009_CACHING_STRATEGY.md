# ADR-0009_CACHING_STRATEGY.md

# ADR-0009: Unified Caching Strategy

Status: Accepted

Date: YYYY-MM-DD

Authors:

- VSkript Database Team

---

# Context

VSkript Database는 Builder, Validator, Generator 및 Language Server가 동일한 Resource를 반복적으로 처리하는 구조를 가진다.

Repository 규모가 커질수록 다음과 같은 문제가 발생한다.

- 동일 JSON 반복 Parsing
- Registry 반복 생성
- Validation 반복 수행
- DataPack 반복 생성
- Completion 반복 계산
- Hover 반복 생성

이러한 중복 작업은 Build 시간과 Language Server 응답 속도에 직접적인 영향을 준다.

프로젝트는 장기적으로 수만 개의 Resource를 처리할 수 있도록 설계되어야 한다.

---

# Decision

VSkript Database는 **Unified Multi-Level Caching Strategy**를 채택한다.

캐시는 Builder, Validator, Generator, Language Server 전체에서 공통 개념으로 사용되며, 각 컴포넌트는 자신의 책임 범위 내에서 캐시를 관리한다.

캐시는 항상 재생성 가능한 데이터만 저장하며, 원본 데이터(Source of Truth)를 대체하지 않는다.

---

# Architecture

```
Repository

↓

Builder Cache

↓

Validator Cache

↓

Generator Cache

↓

DataPack

↓

Language Server Cache
```

각 계층은 독립적인 캐시를 가진다.

---

# Cache Principles

모든 캐시는 다음 원칙을 따른다.

- Read Through
- Immutable Input
- Deterministic Output
- Automatic Invalidation
- Version Aware
- Rebuildable

캐시는 언제든 삭제 가능해야 한다.

---

# Rationale

## 1. Performance

동일 Resource를 반복적으로 계산하지 않는다.

예시

```
Grammar

↓

Parse

↓

Cache

↓

Reuse
```

---

## 2. Incremental Build

변경되지 않은 Resource는 기존 결과를 그대로 사용한다.

```
Resource

↓

Hash

↓

Match

↓

Reuse
```

---

## 3. Faster Validation

Schema Validation 결과를 캐시에 저장한다.

```
JSON

↓

Schema Validation

↓

Diagnostic Cache
```

동일 JSON은 다시 검사하지 않는다.

---

## 4. Generator Optimization

Generator는 변경되지 않은 Package를 다시 생성하지 않는다.

```
Registry

↓

Hash

↓

Package Cache
```

---

## 5. Language Server Performance

Language Server는 자주 사용하는 데이터를 메모리에 유지한다.

예시

- Completion
- Hover
- Definitions
- References
- Semantic Tokens

---

## 6. Memory Efficiency

모든 Resource를 메모리에 유지하지 않는다.

```
Registry

↓

Lookup

↓

Lazy Load

↓

Cache
```

필요한 Resource만 적재한다.

---

## 7. Deterministic Builds

동일한 입력은 동일한 캐시 결과를 생성해야 한다.

캐시 사용 여부와 관계없이 Build 결과는 동일해야 한다.

---

## 8. Rebuild Safety

캐시가 손상되어도 삭제 후 전체 Build가 가능해야 한다.

캐시는 필수 데이터가 아니다.

---

# Cache Levels

## Level 1

Builder Cache

저장 대상

- Parsed JSON
- AST
- Dependency Graph
- Registry Snapshot

---

## Level 2

Validator Cache

저장 대상

- Validation Result
- Diagnostics
- Schema Resolution

---

## Level 3

Generator Cache

저장 대상

- Generated Package
- Manifest
- Metadata
- Checksums

---

## Level 4

Language Server Cache

저장 대상

- Registry
- Completion
- Hover
- Definitions
- References
- Semantic Tokens

---

# Cache Keys

캐시는 다음 정보를 기반으로 생성한다.

```
Resource Hash

+

Schema Version

+

Builder Version

+

Generator Version
```

동일 Key는 동일 결과를 의미한다.

---

# Cache Invalidation

다음 경우 캐시는 반드시 무효화한다.

- Resource 변경
- Schema Version 변경
- Builder Version 변경
- Generator Version 변경
- Dependency 변경
- Registry 변경
- Manifest 변경
- Configuration 변경

---

# Cache Lifetime

| Cache | Lifetime |
|---------|----------|
| Builder | Build Session |
| Validator | Build Session |
| Generator | Build Session |
| Language Server | Runtime |

Language Server Cache는 종료 시 삭제할 수 있다.

---

# Persistent Cache

필요한 경우 디스크 기반 캐시를 사용할 수 있다.

예시

```
.cache/

builder/

validator/

generator/
```

Persistent Cache는 선택 사항이다.

---

# Memory Cache

Language Server는 메모리 캐시를 기본으로 사용한다.

```
Registry

↓

Memory Cache

↓

Completion
```

디스크 접근을 최소화한다.

---

# Cache Consistency

캐시는 Registry와 항상 일치해야 한다.

```
Registry

↓

Cache Refresh

↓

Ready
```

오래된 Registry를 참조해서는 안 된다.

---

# Cache Eviction

메모리 부족 시 다음 순서로 제거한다.

1. Documentation
2. Hover Cache
3. Semantic Tokens
4. Completion Cache
5. Grammar Cache

Registry Cache는 마지막까지 유지한다.

---

# Consequences

Unified Caching Strategy를 채택함으로써 다음과 같은 장점이 있다.

- Build 시간 감소
- Validation 속도 향상
- Generator 성능 향상
- Language Server 응답 속도 향상
- 메모리 효율 향상
- Incremental Build 지원

단점도 존재한다.

- Cache 관리가 필요하다.
- Invalid Cache 처리 로직이 필요하다.
- 구현 복잡도가 증가한다.

---

# Alternatives Considered

## No Cache

모든 작업을 매번 수행한다.

장점

- 구현이 단순하다.

단점

- 매우 느리다.
- 대규모 Repository에서 사용할 수 없다.

채택하지 않았다.

---

## Global Shared Cache

모든 컴포넌트가 하나의 캐시를 공유한다.

장점

- 구현이 간단하다.

단점

- 책임이 불명확하다.
- 동기화 문제가 발생한다.

채택하지 않았다.

---

## Database Cache

SQLite 등 외부 저장소를 사용한다.

장점

- 영속성

단점

- 의존성 증가
- 관리 비용 증가

채택하지 않았다.

---

# Implementation Notes

모든 캐시는 Hash 기반으로 동작한다.

캐시는 원본 데이터를 수정해서는 안 된다.

캐시 적중(Cache Hit)은 결과의 정확성에 영향을 주어서는 안 된다.

Builder, Validator, Generator, Language Server는 각각 독립적인 캐시를 가진다.

---

# Risks

잘못된 Cache Invalidation은 오래된 데이터를 사용할 위험이 있다.

이를 방지하기 위해 다음 정책을 적용한다.

- Hash Verification
- Version Verification
- Registry Verification
- Full Rebuild Option
- Automatic Cache Cleanup

---

# Related RFC

- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification
- RFC-0010 Language Server Integration

---

# Related Specifications

- SPEC-0009 Versioning
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout

---

# Decision Summary

VSkript Database는 **Unified Multi-Level Caching Strategy**를 채택한다.

각 컴포넌트는 독립적인 캐시를 유지하며, 캐시는 항상 재생성 가능한 데이터만 저장한다.

이를 통해 Build 성능, Validation 속도, Language Server 응답성 및 대규모 Repository의 확장성을 향상시키면서도 시스템의 일관성과 재현 가능성을 유지한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |