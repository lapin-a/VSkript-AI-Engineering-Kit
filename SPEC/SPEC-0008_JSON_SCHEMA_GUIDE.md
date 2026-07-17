# SPEC-0008_JSON_SCHEMA_GUIDE.md

# VSkript JSON Schema Guide

> JSON Document Standards

Specification: SPEC-0008

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 모든 JSON 문서의 공통 규칙을 정의한다.

모든 JSON 파일은 본 명세를 준수해야 하며, Builder, Validator, Generator, VSCode Extension 및 Language Server는 이를 기준으로 JSON 데이터를 생성하고 검증한다.

---

# 2. Objectives

본 명세의 목표

- JSON 구조 표준화
- Schema 기반 검증
- 일관된 데이터 표현
- 버전 관리
- 호환성 유지
- 자동 생성 지원

---

# 3. Scope

본 명세는 다음 JSON 파일에 적용된다.

- manifest.json
- registry.json
- grammar.json
- locale.json
- metadata.json
- index.json
- cache.json

---

# 4. Encoding

모든 JSON 파일은 다음 규칙을 따른다.

- UTF-8
- BOM 사용 금지
- Unix Line Ending (LF)
- JSON Object 사용
- JSON 배열은 필요한 경우에만 사용

---

# 5. Root Object

모든 JSON 파일은 Object로 시작해야 한다.

예시

```json
{
  "schemaVersion": "1.0",
  "id": "skbee"
}
```

Root에 Array는 허용되지 않는다.

---

# 6. Common Fields

모든 JSON 문서는 가능한 경우 다음 필드를 포함한다.

| Field | Required | Description |
|--------|----------|-------------|
| schemaVersion | ✔ | Schema Version |
| id | ✔ | Unique Identifier |
| version | ✔ | Resource Version |
| name | ✔ | Display Name |

---

# 7. Identifier Rules

모든 ID는 다음 규칙을 따른다.

- 소문자
- kebab-case
- 공백 금지
- 영구 식별자
- 중복 금지

예시

```
skript

skbee

skquery

player-location
```

---

# 8. Version Format

Version은 Semantic Versioning을 따른다.

예시

```
1.0.0

2.5.1

3.12.2
```

---

# 9. Boolean

Boolean은 반드시 JSON Boolean을 사용한다.

허용

```json
true
```

```json
false
```

허용하지 않음

```
"true"

"false"

1

0
```

---

# 10. Null

Null은 값이 없는 경우에만 사용한다.

예시

```json
{
  "deprecatedSince": null
}
```

필드 자체를 생략할 수 있다면 Null보다 생략을 권장한다.

---

# 11. Arrays

배열은 순서를 보장해야 하는 경우에만 사용한다.

예시

```json
{
  "patterns": [
    "...",
    "..."
  ]
}
```

---

# 12. Objects

중첩 Object는 논리적인 그룹을 표현한다.

예시

```json
{
  "documentation": {
    "summary": "...",
    "examples": []
  }
}
```

---

# 13. Naming Convention

JSON Key는 camelCase를 사용한다.

예시

```
schemaVersion

displayName

deprecatedSince

minimumVersion
```

허용하지 않음

```
SchemaVersion

schema_version

Schema_Version
```

---

# 14. Date Format

날짜는 ISO-8601 UTC를 사용한다.

예시

```
2026-07-17T12:00:00Z
```

---

# 15. File References

다른 JSON 파일은 ID를 통해 참조한다.

예시

```json
{
  "dependencies": [
    "skript",
    "skbee"
  ]
}
```

상대 경로는 사용하지 않는다.

---

# 16. Localization

사용자에게 표시되는 문자열은 Locale Key를 사용한다.

예시

```json
{
  "displayName": "grammar.effect.spawn"
}
```

직접 문자열 사용은 권장하지 않는다.

---

# 17. Validation Rules

Validator는 다음을 검사한다.

- Schema Version
- Required Fields
- Unknown Fields
- Duplicate ID
- Invalid Version
- Invalid Type
- Invalid Locale Key

---

# 18. Schema Evolution

Schema 변경 시

- 이전 버전과의 호환성 유지
- Deprecated 필드 유지
- Major Version 증가 시 Breaking Change 허용

---

# 19. Deprecated Fields

Deprecated 필드는 즉시 제거하지 않는다.

예시

```json
{
  "deprecated": true,
  "deprecatedSince": "2.0.0"
}
```

---

# 20. Metadata

Metadata는 Object로 관리한다.

예시

```json
{
  "metadata": {
    "author": "Lapin",
    "license": "MIT"
  }
}
```

---

# 21. Documentation

문서 정보는 별도 Object로 관리한다.

```json
{
  "documentation": {
    "summary": "...",
    "examples": []
  }
}
```

---

# 22. Compatibility

JSON 문서는 가능한 한 이전 버전과 호환되어야 한다.

Builder는 필요한 경우 Migration을 수행할 수 있다.

---

# 23. Example

```json
{
  "schemaVersion": "1.0",
  "id": "effect.spawn",
  "version": "1.0.0",
  "name": "Spawn Effect",
  "deprecated": false,
  "documentation": {
    "summary": "Spawn an entity."
  }
}
```

---

# 24. Summary

본 명세는 VSkript Database에서 사용하는 모든 JSON 문서의 공통 구조와 작성 규칙을 정의한다.

모든 JSON 데이터는 본 명세를 기준으로 생성 및 검증되어야 한다.

---

# 25. Related Specifications

- SPEC-0006_PATTERN_SYNTAX.md
- SPEC-0007_TYPE_SYSTEM.md
- SPEC-0009_VERSIONING.md

---

# 26. Referenced RFC

- RFC-0001_MANIFEST_SCHEMA.md
- RFC-0002_REGISTRY_SCHEMA.md
- RFC-0003_GRAMMAR_SCHEMA.md
- RFC-0004_LOCALE_SCHEMA.md

---

# 27. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0008 |
| Document | JSON_SCHEMA_GUIDE.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |
