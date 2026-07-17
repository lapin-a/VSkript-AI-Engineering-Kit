# ADR-0004_REGISTRY_AS_SOURCE_OF_TRUTH.md

# ADR-0004: Registry as the Single Source of Truth

Status: Accepted

Date: YYYY-MM-DD

Authors:
- VSkript Database Team

---

# Context

VSkript Database는 다양한 종류의 Resource를 관리한다.

대표적인 Resource는 다음과 같다.

- Grammar
- Locale
- Types
- Plugin Metadata
- Manifest
- Documentation
- Assets
- DataPack

초기 설계에서는 각 Resource가 서로를 직접 참조하는 구조도 고려하였다.

예를 들어

```
Grammar

↓

Plugin.json

↓

Locale

↓

Type
```

또는

```
Grammar

↓

Manifest

↓

Locale
```

와 같은 구조였다.

그러나 프로젝트 규모가 증가하면서 다음 문제가 발생할 것으로 예상되었다.

- Reference 중복
- Dependency 중복
- Resource 검색 비용 증가
- Builder 복잡도 증가
- Validator 구현 복잡도 증가
- Language Server 조회 속도 저하

---

# Decision

VSkript Database는 **Registry를 모든 Resource의 Single Source of Truth(Source of Truth)** 로 사용한다.

모든 Resource는 Registry를 통해서만 검색되고 참조된다.

직접 Resource를 탐색해서는 안 된다.

---

# Registry Responsibilities

Registry는 다음 정보를 관리한다.

- Resource ID
- Namespace
- Plugin
- Version
- Category
- Type
- Location
- Dependencies
- Metadata
- Locale Index

Registry는 Resource의 실제 내용을 저장하지 않는다.

Registry는 Resource의 위치와 메타데이터만 관리한다.

---

# Architecture

```
             Registry
                 │
 ┌───────────────┼───────────────┐
 │               │               │
 ▼               ▼               ▼
Grammar       Locale         Types
 │               │               │
 └───────────────┼───────────────┘
                 ▼
             Language Server
```

모든 조회는 Registry를 거쳐 수행된다.

---

# Rationale

## 1. Single Source of Truth

모든 Resource 정보는 Registry 하나에 존재한다.

동일한 정보를 여러 JSON 파일에 중복 저장하지 않는다.

---

## 2. Fast Lookup

Resource 검색 과정

```
Symbol

↓

Registry Lookup

↓

Resource Path

↓

Resource Load
```

Directory 전체를 탐색하지 않는다.

---

## 3. Stable Identifier

모든 Resource는 Registry ID를 가진다.

예시

```
skript.effect.spawn

skbee.expression.nbt

skript.type.player
```

ID는 파일 위치가 변경되어도 유지된다.

---

## 4. Builder Integration

Builder는 Registry를 생성한다.

```
Raw Resources

↓

Builder

↓

Registry
```

Registry는 Build 결과물이다.

---

## 5. Validator Integration

Validator는 Registry를 사용하여 다음을 검증한다.

- 존재 여부
- 중복 ID
- Broken Reference
- Dependency
- Namespace

---

## 6. Generator Integration

Generator는 Registry를 기준으로 DataPack을 생성한다.

Registry가 Package Index 역할을 수행한다.

---

## 7. Language Server Integration

Language Server는 Registry만 메모리에 로드한다.

필요한 Resource만 Lazy Loading한다.

```
Completion

↓

Registry

↓

Grammar
```

성능이 크게 향상된다.

---

## 8. Plugin Isolation

Plugin은 서로의 디렉터리를 탐색하지 않는다.

```
Plugin

↓

Registry

↓

Target Resource
```

Plugin 간 결합도를 낮춘다.

---

## 9. Dependency Resolution

Dependency Graph는 Registry를 기반으로 생성된다.

```
Plugin

↓

Registry

↓

Dependencies
```

Dependency Resolution이 단순해진다.

---

## 10. Incremental Build

Builder는 Registry Snapshot을 사용하여 변경 사항을 계산한다.

```
Previous Registry

↓

Current Registry

↓

Diff

↓

Dirty Resources
```

Incremental Build의 핵심 데이터가 된다.

---

# Registry Schema

Registry Entry 예시

```json
{
  "id": "skript.effect.spawn",
  "plugin": "Skript",
  "category": "effect",
  "version": "2.9.0",
  "path": "grammar/effects/spawn.json"
}
```

Registry는 Resource 자체를 포함하지 않는다.

---

# Lookup Flow

모든 검색은 다음 절차를 따른다.

```
Identifier

↓

Registry Lookup

↓

Metadata

↓

Resource Path

↓

Resource Load
```

직접 파일 탐색은 허용되지 않는다.

---

# Consequences

Registry 중심 구조를 채택함으로써 다음과 같은 장점이 있다.

- 빠른 검색
- 단일 ID 체계
- 쉬운 Validation
- 쉬운 Builder 구현
- 쉬운 Generator 구현
- 높은 캐시 효율
- Language Server 성능 향상

단점도 존재한다.

- Registry 생성이 필수적이다.
- Registry가 손상되면 전체 시스템에 영향을 준다.
- Registry Version 관리가 필요하다.

---

# Alternatives Considered

## Directory Traversal

매번 디렉터리를 탐색하여 Resource를 찾는다.

장점

- Registry가 필요 없다.

단점

- 매우 느리다.
- 캐시 효율이 낮다.
- Language Server 구현이 복잡하다.

채택하지 않았다.

---

## Plugin Local Registry

Plugin마다 Registry를 가진다.

장점

- Plugin 독립성

단점

- Registry 병합 필요
- 중복 ID 검사 어려움
- Global Symbol Search가 복잡

채택하지 않았다.

---

## Database Lookup

SQLite 등 데이터베이스를 사용한다.

장점

- 빠른 조회

단점

- Git 친화적이지 않다.
- 빌드 과정이 복잡해진다.
- 오프라인 수정이 어렵다.

채택하지 않았다.

---

# Implementation Notes

Registry는 Builder가 생성한다.

Validator는 Registry를 검증한다.

Generator는 Registry를 Package Index로 사용한다.

Language Server는 Registry를 조회 엔진으로 사용한다.

Registry는 읽기 전용(Read-Only)이다.

---

# Risks

Registry 손상 시 모든 기능이 영향을 받을 수 있다.

이를 방지하기 위해 다음을 적용한다.

- JSON Schema Validation
- Registry Validation
- Checksum Verification
- Version Validation
- Full Registry Regeneration

---

# Related RFC

- RFC-0002 Registry Schema
- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification
- RFC-0010 Language Server Integration

---

# Related Specifications

- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout
- SPEC-0012 Directory Structure

---

# Decision Summary

VSkript Database는 **Registry를 모든 Resource의 Single Source of Truth**로 채택한다.

모든 Resource 조회, 참조, 의존성 분석 및 Language Server 기능은 Registry를 기반으로 수행된다.

이를 통해 일관된 식별 체계, 빠른 조회 성능, 단순한 Builder/Validator/Generator 구현, 그리고 확장 가능한 아키텍처를 제공한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |