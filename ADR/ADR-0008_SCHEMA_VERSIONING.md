# ADR-0008_SCHEMA_VERSIONING.md

# ADR-0008: Schema Versioning Strategy

Status: Accepted

Date: YYYY-MM-DD

Authors:

- VSkript Database Team

---

# Context

VSkript Database는 모든 데이터를 JSON Schema를 기반으로 관리한다.

주요 Schema는 다음과 같다.

- Manifest Schema
- Registry Schema
- Grammar Schema
- Locale Schema
- Metadata Schema
- Dependency Schema
- DataPack Schema

프로젝트가 장기간 유지되면 Schema는 반드시 변경된다.

예를 들어

- 새로운 필드 추가
- 기존 필드 제거
- 타입 변경
- Validation Rule 변경
- 새로운 Resource 추가

Schema Version 정책이 없다면 다음과 같은 문제가 발생한다.

- Builder와 Generator의 불일치
- Language Server의 호환성 문제
- 오래된 DataPack 사용 불가
- Migration 비용 증가
- Validation 오류 증가

---

# Decision

VSkript Database는 **Semantic Schema Versioning**을 채택한다.

모든 Schema는 독립적인 Version을 가지며 Semantic Versioning(SemVer)을 따른다.

```
MAJOR.MINOR.PATCH
```

예시

```
1.0.0

1.1.0

2.0.0
```

---

# Scope

Schema Version은 다음 대상에 적용된다.

- Manifest
- Registry
- Grammar
- Locale
- Metadata
- Dependency
- DataPack

각 Schema는 독립적으로 Version을 관리한다.

---

# Architecture

```
Schema

↓

Version

↓

Validation

↓

Builder

↓

Generator

↓

Language Server
```

Schema Version은 전체 Build Pipeline에서 공통으로 사용된다.

---

# Rationale

## 1. Backward Compatibility

Minor Version에서는 기존 DataPack이 계속 동작해야 한다.

예시

```
Schema 1.0

↓

Schema 1.1

↓

Compatible
```

Language Server는 기존 DataPack을 계속 사용할 수 있어야 한다.

---

## 2. Predictable Changes

Semantic Version은 변경 수준을 명확하게 표현한다.

### PATCH

- 오타 수정
- 설명 수정
- Validation 개선
- 문서 수정

기존 구현에 영향이 없다.

---

### MINOR

- Optional Field 추가
- 새로운 Resource Type 추가
- 새로운 Metadata 추가

기존 DataPack은 계속 사용할 수 있다.

---

### MAJOR

- Required Field 변경
- 구조 변경
- 기존 필드 제거
- Validation Rule 변경

Migration이 필요하다.

---

## 3. Stable Builder

Builder는 지원 가능한 Schema Version을 명시한다.

예시

```json
{
  "supportedSchemas": [
    "1.x"
  ]
}
```

지원하지 않는 Schema는 Build하지 않는다.

---

## 4. Stable Validator

Validator는 Schema Version에 따라 Validation Rule을 선택한다.

```
Manifest

↓

Schema Version

↓

Validation Rule

↓

Diagnostics
```

---

## 5. Stable Generator

Generator는 DataPack에 Schema Version을 기록한다.

예시

```json
{
  "schemaVersion": "1.2.0"
}
```

---

## 6. Language Server Compatibility

Language Server는 DataPack의 Schema Version을 확인한다.

```
DataPack

↓

schemaVersion

↓

Compatible?

↓

Load
```

호환되지 않으면 DataPack을 거부한다.

---

## 7. Incremental Migration

Schema Migration은 단계적으로 수행한다.

```
1.0

↓

1.1

↓

1.2

↓

2.0
```

한 번에 여러 Major Version을 건너뛰지 않는다.

---

## 8. Multiple Schema Support

Builder와 Validator는 동시에 여러 Minor Version을 지원할 수 있다.

예시

```
1.0

1.1

1.2
```

이를 통해 Migration 기간을 확보한다.

---

## 9. Independent Schema Evolution

Manifest와 Registry는 서로 독립적으로 Version을 증가시킬 수 있다.

예시

```
Manifest

2.0
```

```
Registry

1.4
```

Schema는 서로 강제로 동일한 Version을 사용하지 않는다.

---

## 10. Long-Term Stability

Schema Version은 프로젝트의 장기적인 유지보수를 위한 핵심 요소이다.

모든 변경은 Version 증가를 통해 추적 가능해야 한다.

---

# Version Rules

| Change | Version |
|----------|---------|
| Documentation | PATCH |
| Optional Field | MINOR |
| New Resource | MINOR |
| Required Field | MAJOR |
| Remove Field | MAJOR |
| Rename Field | MAJOR |
| Type Change | MAJOR |

---

# Compatibility Policy

Builder는 다음 정책을 따른다.

```
Older Schema

↓

Migration

↓

Current Schema
```

자동 Migration이 가능한 경우 수행한다.

---

# Migration Strategy

Migration은 별도의 Migration Engine이 수행한다.

```
Old JSON

↓

Migration

↓

New JSON
```

Migration 과정은 반드시 검증 가능해야 한다.

---

# Consequences

Schema Versioning을 채택함으로써 다음과 같은 장점이 있다.

- 장기 유지보수
- Backward Compatibility
- 예측 가능한 변경
- 쉬운 Migration
- 안정적인 Validation
- DataPack 호환성 관리

단점도 존재한다.

- Version 관리 비용 증가
- Migration 코드 필요
- Builder 복잡도 증가

---

# Alternatives Considered

## No Version

Schema Version을 관리하지 않는다.

장점

- 단순하다.

단점

- 변경 추적 불가
- 호환성 문제
- Migration 불가능

채택하지 않았다.

---

## Date-Based Version

예시

```
2026-07-17
```

장점

- 변경 시점을 알 수 있다.

단점

- 변경 수준을 알 수 없다.

채택하지 않았다.

---

## Repository Version

Repository Version만 사용한다.

장점

- 관리가 쉽다.

단점

- Schema별 변경을 표현할 수 없다.

채택하지 않았다.

---

# Implementation Notes

모든 Schema는 `$schemaVersion` 또는 `schemaVersion` 필드를 포함해야 한다.

Builder, Validator, Generator, Language Server는 Schema Version을 반드시 확인해야 한다.

Schema 변경 시 관련 RFC 및 SPEC 문서를 함께 업데이트한다.

---

# Risks

Schema Version 정책이 제대로 적용되지 않으면 DataPack 호환성이 깨질 수 있다.

이를 방지하기 위해 다음 정책을 적용한다.

- Semantic Versioning
- Compatibility Validation
- Automated Migration
- Regression Test
- Version Gate

---

# Related RFC

- RFC-0001 Manifest Schema
- RFC-0002 Registry Schema
- RFC-0003 Grammar Schema
- RFC-0004 Locale Schema
- RFC-0005 Update Protocol
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification
- RFC-0010 Language Server Integration

---

# Related Specifications

- SPEC-0008 JSON Schema Guide
- SPEC-0009 Versioning
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout

---

# Decision Summary

VSkript Database는 **Semantic Schema Versioning**을 채택한다.

모든 Schema는 독립적인 Semantic Version을 가지며, Builder, Validator, Generator 및 Language Server는 이를 기준으로 호환성을 판단한다.

이 정책은 장기적인 프로젝트 유지보수, 안전한 Migration, 그리고 DataPack 생태계의 안정성을 보장한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |