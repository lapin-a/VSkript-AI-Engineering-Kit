# SPEC-0009_VERSIONING.md

# VSkript Versioning Specification

> Versioning and Compatibility Standard

Specification: SPEC-0009

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database 프로젝트에서 사용하는 모든 버전 관리 정책을 정의한다.

본 명세는 Database, DataPack, Registry, Builder, Generator, Validator, VSCode Extension 및 Language Server 간의 버전 호환성을 보장하기 위한 기준이다.

---

# 2. Objectives

Versioning의 목표

- Semantic Versioning 표준화
- 호환성 보장
- Breaking Change 관리
- Migration 지원
- Update 정책 정의
- 장기 유지보수 지원

---

# 3. Scope

본 명세는 다음 구성 요소에 적용된다.

- Database
- Registry
- Manifest
- DataPack
- Grammar
- Locale
- Builder
- Generator
- Validator
- VSCode Extension
- Language Server

---

# 4. Semantic Versioning

모든 버전은 Semantic Versioning 2.0을 따른다.

형식

```
MAJOR.MINOR.PATCH
```

예시

```
1.0.0

1.3.2

2.0.0

3.12.5
```

---

# 5. Version Components

## Major

Breaking Change 발생 시 증가한다.

예

```
1.x.x

↓

2.0.0
```

---

## Minor

기존 기능과 호환되는 기능 추가

예

```
1.2.0

↓

1.3.0
```

---

## Patch

버그 수정

예

```
1.3.0

↓

1.3.1
```

---

# 6. Schema Version

모든 JSON은 Schema Version을 가진다.

예

```json
{
    "schemaVersion": "1.0"
}
```

Schema Version은 Resource Version과 독립적으로 관리된다.

---

# 7. Database Version

Database 전체에도 Version이 존재한다.

예

```json
{
    "databaseVersion": "1.2.0"
}
```

Database Version은 전체 Release를 의미한다.

---

# 8. DataPack Version

각 DataPack은 독립적인 Version을 가진다.

예

```json
{
    "id": "skbee",
    "version": "3.12.2"
}
```

DataPack Version은 Plugin Version과 동일할 수도 있고 다를 수도 있다.

---

# 9. Registry Version

Registry 역시 별도의 Version을 가진다.

```json
{
    "registryVersion": "1.4.0"
}
```

Registry 변경은 DataPack 변경과 독립적이다.

---

# 10. Builder Version

Builder Version

```json
{
    "builderVersion": "2.1.0"
}
```

Builder는 생성한 모든 데이터에 자신의 버전을 기록해야 한다.

---

# 11. Validator Version

Validator 역시 버전을 가진다.

Validator는 Schema Version에 따라 Validation Rule을 변경할 수 있다.

---

# 12. Extension Version

VSCode Extension은 지원 가능한 Database Version 범위를 선언해야 한다.

예

```json
{
    "supportedDatabase": "^1.0.0"
}
```

---

# 13. Compatibility Matrix

| Component | Compatible With |
|------------|-----------------|
| Builder | Schema |
| Validator | Schema |
| Extension | Database |
| Language Server | Database |
| Registry | DataPack |
| DataPack | Manifest |

---

# 14. Version Constraints

다음 연산자를 지원한다.

```
=

>

>=

<

<=

^

~
```

예

```
>=1.0.0

^2.5

~3.12
```

---

# 15. Breaking Changes

다음 변경은 Breaking Change이다.

- Schema 삭제
- Field 제거
- ID 변경
- Type 변경
- Pattern 변경
- Registry 구조 변경

Breaking Change는 Major Version을 증가시켜야 한다.

---

# 16. Non-Breaking Changes

다음 변경은 Minor Version으로 처리한다.

- Field 추가
- Locale 추가
- Grammar 추가
- Documentation 추가
- Alias 추가

---

# 17. Patch Changes

Patch는 다음 변경만 허용한다.

- 오타 수정
- 설명 수정
- Metadata 수정
- 버그 수정

---

# 18. Deprecated Policy

Deprecated 기능은 즉시 제거하지 않는다.

예

```json
{
    "deprecated": true,
    "deprecatedSince": "2.0.0"
}
```

Deprecated는 최소 하나의 Major Version 동안 유지해야 한다.

---

# 19. Migration

Breaking Change가 발생한 경우 Migration을 제공해야 한다.

Migration은 다음을 포함한다.

- 변경 내용
- 영향 범위
- 자동 변환 가능 여부
- 수동 수정 사항

---

# 20. Update Strategy

업데이트 절차

```
Current Version

↓

Check Compatibility

↓

Download

↓

Validate

↓

Install

↓

Activate
```

호환되지 않을 경우 업데이트를 중단해야 한다.

---

# 21. Rollback

업데이트 실패 시 이전 버전으로 복원한다.

Rollback 대상

- Registry
- DataPack
- Cache

---

# 22. Version History

모든 Release는 변경 이력을 유지해야 한다.

예

```
1.0.0

Initial Release

↓

1.1.0

Grammar Added

↓

1.1.1

Bug Fix
```

---

# 23. Release Channels

지원 채널

| Channel | Description |
|----------|-------------|
| Stable | 정식 릴리스 |
| Preview | 기능 미리보기 |
| Nightly | 개발 버전 |
| Experimental | 실험 버전 |

---

# 24. Version Metadata

Version 정보는 다음 필드를 포함할 수 있다.

```json
{
    "version": "1.3.0",
    "releaseDate": "2026-07-17",
    "channel": "stable"
}
```

---

# 25. Compatibility Rules

호환성은 다음 순서를 따른다.

```
Schema

↓

Database

↓

Registry

↓

Manifest

↓

DataPack

↓

Extension
```

각 단계에서 호환성을 확인해야 한다.

---

# 26. Validation Rules

Validator는 다음을 검사한다.

- Invalid Version
- Invalid Constraint
- Unsupported Schema
- Unsupported Database Version
- Invalid Release Channel
- Downgrade Detection

---

# 27. Summary

본 명세는 VSkript Database 프로젝트의 버전 관리와 호환성 정책을 정의한다.

모든 구성 요소는 Semantic Versioning을 따르며, Breaking Change, Migration 및 Update 정책을 준수해야 한다.

---

# 28. Related Specifications

- SPEC-0005_ERROR_CODES.md
- SPEC-0008_JSON_SCHEMA_GUIDE.md
- SPEC-0010_DEPENDENCY_RESOLUTION.md

---

# 29. Referenced RFC

- RFC-0001_MANIFEST_SCHEMA.md
- RFC-0002_REGISTRY_SCHEMA.md
- RFC-0005_UPDATE_PROTOCOL.md
- RFC-0007_BUILDER_SPECIFICATION.md

---

# 30. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0009 |
| Document | VERSIONING.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |