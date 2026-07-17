# ADR-0006_PLUGIN_ISOLATION.md

# ADR-0006: Plugin Isolation Architecture

Status: Accepted

Date: YYYY-MM-DD

Authors:
- VSkript Database Team

---

# Context

VSkript Database는 Skript뿐만 아니라 다양한 애드온(Plugin)의 Grammar, Type, Locale 및 Metadata를 하나의 Repository에서 관리한다.

대표적인 Plugin은 다음과 같다.

- Skript
- SkBee
- SkQuery
- skript-reflect
- SkRayFall
- TuSKe
- 기타 Community Plugin

초기 설계에서는 모든 Plugin이 동일한 Resource 공간을 공유하는 구조와 Plugin별로 독립적인 Resource 공간을 가지는 구조를 모두 검토하였다.

고려 대상은 다음과 같았다.

- Namespace 충돌
- Version 관리
- Dependency 관리
- Builder
- Validator
- Language Server
- DataPack Packaging

---

# Decision

VSkript Database는 **Plugin Isolation Architecture**를 채택한다.

각 Plugin은 독립적인 Resource 공간을 가지며, 다른 Plugin의 내부 Resource를 직접 참조해서는 안 된다.

Plugin 간 상호작용은 Registry와 Dependency Resolution을 통해서만 수행한다.

---

# Plugin Responsibilities

각 Plugin은 다음 Resource를 독립적으로 관리한다.

- Grammar
- Locale
- Types
- Documentation
- Metadata
- Assets
- Manifest

Plugin은 자신의 Resource에 대한 소유권(Ownership)을 가진다.

---

# Architecture

```
                Registry
                    │
     ┌──────────────┼──────────────┐
     │              │              │
     ▼              ▼              ▼
 Skript         SkBee      skript-reflect
     │              │              │
 Grammar       Grammar       Grammar
 Locale        Locale        Locale
 Types         Types         Types
```

Plugin은 Registry를 통해서만 서로를 참조한다.

---

# Rationale

## 1. Loose Coupling

Plugin은 서로 강하게 결합되어서는 안 된다.

직접 파일을 참조하는 대신 Registry ID를 사용한다.

```
Plugin

↓

Registry

↓

Target Resource
```

---

## 2. Namespace Isolation

모든 Resource는 Namespace를 가진다.

예시

```
skript.effect.spawn

skbee.expression.nbt

reflect.expression.class
```

동일한 이름이라도 Namespace가 다르면 충돌하지 않는다.

---

## 3. Independent Versioning

Plugin은 독립적으로 Version을 가진다.

예시

```
Skript

2.9.0
```

```
SkBee

3.12.2
```

Version은 Plugin별로 관리된다.

---

## 4. Independent Release

각 Plugin은 독립적인 DataPack을 생성한다.

```
skript.zip

skbee.zip

reflect.zip
```

다른 Plugin의 Release에 영향을 주지 않는다.

---

## 5. Dependency Resolution

Plugin은 필요한 Plugin만 의존한다.

예시

```
SkBee

↓

Depends On

↓

Skript
```

Dependency는 Registry와 Manifest를 통해 관리된다.

---

## 6. Builder Simplicity

Builder는 Plugin 단위로 Build를 수행한다.

```
Plugin

↓

Build

↓

Registry Update
```

Repository 전체를 다시 Build할 필요가 없다.

---

## 7. Validator Simplicity

Validator는 Plugin 단위 Validation을 수행할 수 있다.

```
Plugin

↓

Validation

↓

Diagnostics
```

Validation 범위를 최소화할 수 있다.

---

## 8. Generator Simplicity

Generator는 Plugin별 DataPack을 생성한다.

```
Plugin

↓

Package

↓

ZIP
```

Plugin 간 Package 병합은 수행하지 않는다.

---

## 9. Language Server Optimization

Language Server는 필요한 Plugin만 로드할 수 있다.

예시

```
Workspace

↓

Installed Plugins

↓

Load Required DataPacks
```

메모리 사용량을 줄일 수 있다.

---

## 10. Marketplace Readiness

향후 Marketplace가 도입될 경우 Plugin은 독립적인 배포 단위가 된다.

예시

```
Marketplace

↓

Install Plugin

↓

Download DataPack

↓

Load
```

Plugin Isolation은 Marketplace 구조와 자연스럽게 호환된다.

---

# Ownership Rules

Plugin은 자신의 Resource만 수정할 수 있다.

허용

```
SkBee

↓

SkBee Grammar 수정
```

허용하지 않음

```
SkBee

↓

Skript Grammar 수정
```

공통 Resource는 Core Plugin이 관리한다.

---

# Communication Model

Plugin 간 통신은 Registry를 통해 이루어진다.

```
Plugin

↓

Registry

↓

Dependency

↓

Resource
```

직접 파일 접근은 허용되지 않는다.

---

# Dependency Model

Dependency는 Manifest에 선언한다.

예시

```json
{
  "dependencies": [
    {
      "plugin": "Skript",
      "version": ">=2.9.0"
    }
  ]
}
```

Validator는 Dependency Resolution을 수행한다.

---

# Consequences

Plugin Isolation을 채택함으로써 다음과 같은 장점이 있다.

- Namespace 충돌 방지
- 독립적인 Version 관리
- 독립적인 Release
- 쉬운 Validation
- 쉬운 Build
- 쉬운 Package 생성
- Marketplace 대응
- Language Server 최적화

단점도 존재한다.

- Dependency 관리가 필요하다.
- Registry가 필수적이다.
- 공통 Resource의 위치를 명확히 정의해야 한다.

---

# Alternatives Considered

## Shared Resource Space

모든 Plugin이 동일한 Resource 공간을 공유한다.

장점

- 구현이 단순하다.

단점

- 충돌 가능성
- Ownership 불명확
- Version 관리 어려움

채택하지 않았다.

---

## Directory-Based Isolation Only

디렉터리만 분리하고 Registry는 사용하지 않는다.

장점

- 구조가 단순하다.

단점

- Cross Reference가 어려움
- Language Server 성능 저하
- Builder 복잡도 증가

채택하지 않았다.

---

## Multi Repository

Plugin별 Repository를 운영한다.

장점

- 완전한 독립성

단점

- Registry 병합 필요
- CI/CD 복잡
- Version 관리 어려움

채택하지 않았다.

---

# Implementation Notes

Plugin은 독립적인 Manifest를 가진다.

Builder는 Plugin 단위 Build를 수행한다.

Validator는 Plugin 단위 Validation을 지원한다.

Generator는 Plugin별 DataPack을 생성한다.

Language Server는 필요한 Plugin만 로드할 수 있다.

---

# Risks

Plugin Dependency가 복잡해질 수 있다.

이를 해결하기 위해 다음 정책을 적용한다.

- Registry 기반 참조
- Manifest Dependency 선언
- 순환 의존성(Circular Dependency) 금지
- Dependency Validation
- Semantic Version 관리

---

# Related RFC

- RFC-0005 Update Protocol
- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification
- RFC-0010 Language Server Integration

---

# Related Specifications

- SPEC-0009 Versioning
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout
- SPEC-0012 Directory Structure

---

# Decision Summary

VSkript Database는 **Plugin Isolation Architecture**를 채택한다.

각 Plugin은 독립적인 Resource 공간과 Manifest, Version 및 DataPack을 가지며, 다른 Plugin과의 상호작용은 Registry와 Dependency Resolution을 통해서만 수행한다.

이를 통해 낮은 결합도, 높은 확장성, 독립적인 배포 및 향후 Marketplace 지원이 가능한 아키텍처를 제공한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |