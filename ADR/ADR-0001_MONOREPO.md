# ADR-0001_MONOREPO.md

# ADR-0001: Monorepo Architecture

Status: Accepted

Date: YYYY-MM-DD

Authors:
- VSkript Database Team

---

# Context

VSkript Database 프로젝트는 Skript와 다양한 애드온(SkBee, SkQuery, SkRayFall 등)의 문법 데이터, 메타데이터, Locale 및 DataPack을 관리하는 저장소이다.

프로젝트 초기에는 각 애드온을 별도의 Repository로 관리하는 방안과 하나의 Repository(Monorepo)에서 관리하는 방안을 모두 검토하였다.

고려 대상은 다음과 같았다.

- Repository 관리 비용
- Version 관리
- Dependency 관리
- DataPack 생성
- CI/CD
- Language Server
- VSCode Extension

---

# Decision

본 프로젝트는 **Monorepo Architecture**를 채택한다.

모든 Raw Source, Metadata, Grammar, Locale, Registry 및 Generator는 하나의 Repository에서 관리한다.

예상 구조는 다음과 같다.

```
VSkript-Database

raw/

generated/

registry/

builder/

validator/

generator/

docs/
```

각 Plugin은 논리적으로 분리되지만 동일 Repository 내에서 관리된다.

---

# Rationale

Monorepo를 선택한 이유는 다음과 같다.

## 1. Single Source of Truth

모든 데이터가 하나의 Repository에 존재한다.

중복 데이터가 발생하지 않는다.

---

## 2. Unified Registry

Registry는 Repository 전체를 기준으로 생성된다.

Multi Repository에서는 Registry 생성이 복잡해진다.

---

## 3. Incremental Build

RFC-0007 Builder는 변경된 Plugin만 Build한다.

Monorepo에서는 Git 변경 이력을 이용하여 쉽게 변경 사항을 추적할 수 있다.

---

## 4. Shared Schema

모든 Plugin은 동일한 JSON Schema를 사용한다.

Schema 변경은 Repository 전체에 즉시 적용된다.

---

## 5. Unified Versioning

SPEC-0009에서 정의한 Version 정책을 Repository 전체에 적용할 수 있다.

---

## 6. Simplified CI/CD

CI는 하나의 Pipeline만 관리하면 된다.

예시

```
Push

↓

Build

↓

Validate

↓

Generate

↓

Publish
```

---

## 7. Better Developer Experience

개발자는 하나의 Repository만 Clone하면 된다.

모든 Builder와 Validator를 즉시 실행할 수 있다.

---

## 8. Language Server Integration

Language Server는 하나의 Registry만 읽으면 된다.

Plugin별 Registry를 병합할 필요가 없다.

---

# Consequences

Monorepo 채택으로 다음과 같은 장점이 있다.

- 단일 Registry
- 단일 Builder
- 단일 Validator
- 단일 Generator
- 단일 Version 정책
- 단순한 Release

단점도 존재한다.

- Repository 크기 증가
- CI 시간이 증가할 수 있음
- History가 커질 수 있음

이러한 문제는 Incremental Build 및 Cache를 통해 해결한다.

---

# Alternatives Considered

## Multi Repository

```
SkBee

SkQuery

SkRayFall

...
```

장점

- Repository 분리

단점

- Registry 병합 필요
- Dependency 관리 어려움
- Version 관리 복잡
- Release Pipeline 증가

채택하지 않았다.

---

## Git Submodule

Plugin을 Submodule로 관리하는 방안도 검토하였다.

단점

- Clone 복잡
- CI 복잡
- Version 동기화 어려움

채택하지 않았다.

---

## Git Subtree

Subtree도 고려하였다.

Builder가 여러 Source를 읽어야 하므로 구조가 복잡해진다.

채택하지 않았다.

---

# Implementation Notes

RFC-0007 Builder는 Monorepo를 기준으로 구현된다.

Generator 역시 Repository 전체를 대상으로 DataPack을 생성한다.

Registry는 Repository 전체를 기준으로 생성된다.

---

# Risks

대규모 Repository가 될 가능성이 있다.

이를 해결하기 위해 다음 기능을 함께 사용한다.

- Incremental Build
- Incremental Validation
- Cache
- Parallel Build

---

# Related RFC

- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification

---

# Related Specifications

- SPEC-0009 Versioning
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout
- SPEC-0012 Directory Structure

---

# Decision Summary

VSkript Database는 모든 Plugin과 Resource를 하나의 Repository에서 관리하는 **Monorepo Architecture**를 채택한다.

Monorepo는 Registry 생성, Builder 구현, Incremental Build 및 Language Server 통합을 가장 단순하고 안정적으로 구현할 수 있는 구조이다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |