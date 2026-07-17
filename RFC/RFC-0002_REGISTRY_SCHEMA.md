# RFC-0002_REGISTRY_SCHEMA.md

# VSkript DataPack

> Registry Schema Specification (RFC-0002)

Version: 1.0

Status: Draft

---

# 1. Purpose

Registry는 VSkript Database에서 제공하는 모든 DataPack의 인덱스(Index)이다.

VSCode Extension은 Registry를 가장 먼저 다운로드하며,
이를 기반으로 필요한 DataPack을 검색, 비교, 다운로드 및 업데이트한다.

Registry는 DataPack의 실제 내용을 포함하지 않으며,
Manifest를 찾기 위한 메타데이터만 제공한다.

---

# 2. Objectives

Registry는 다음 기능을 제공한다.

- 설치 가능한 DataPack 목록
- 최신 버전 조회
- Manifest 위치
- 다운로드 URL
- Checksum
- 지원 버전
- 검색 기능
- 증분 업데이트 판단

---

# 3. File Location

```
registry.json
```

Database Root에 위치한다.

---

# 4. Encoding

Registry는

- UTF-8
- JSON
- LF
- 2 Spaces

를 사용한다.

---

# 5. High Level Structure

```json
{
  "version": "1.0.0",
  "generatedAt": "2026-07-18T00:00:00Z",
  "packages": {}
}
```

---

# 6. Root Fields

| Field | Required | Description |
|--------|----------|-------------|
| version | ✔ | Registry Version |
| generatedAt | ✔ | 생성 시각 |
| packages | ✔ | DataPack 목록 |

---

# 7. Package Structure

```json
{
  "packages": {
    "skbee": {
      ...
    }
  }
}
```

Package Key는 반드시 Manifest의 id와 동일해야 한다.

---

# 8. Package Fields

| Field | Required |
|--------|----------|
| id | ✔ |
| name | ✔ |
| version | ✔ |
| addonVersion | ✔ |
| manifest | ✔ |
| download | ✔ |
| checksum | ✔ |
| size | ✔ |
| supports | ✔ |

---

# 9. Example

```json
{
  "id": "skbee",
  "name": "SkBee",
  "version": "1.2.0",
  "addonVersion": "3.12.0",

  "manifest": "packs/skbee/manifest.json",

  "download": "packs/skbee.zip",

  "checksum": "...",

  "size": 138412,

  "supports": {
    "minecraft": "1.21+",
    "skript": "2.9+"
  }
}
```

---

# 10. Version Policy

Registry Version은

Database Version과 동일하다.

예

```
Database

1.4.0

↓

Registry

1.4.0
```

---

# 11. Update Strategy

Extension은

```
Registry

↓

Version Compare

↓

Package Compare

↓

Manifest Compare

↓

Download
```

순서로 동작한다.

---

# 12. Incremental Update

Registry는 변경된 Package만 알려준다.

예

```
Old Registry

↓

New Registry

↓

Changed Packages

↓

Download
```

---

# 13. Package Lookup

Extension은

```
Plugin Name

↓

Package ID

↓

Registry

↓

Manifest

```

순으로 검색한다.

---

# 14. Plugin Mapping

Plugin

```
SkBee.jar
```

↓

```
skbee
```

↓

Registry

↓

Manifest

---

# 15. Download Rules

Registry는

DataPack 내용을 저장하지 않는다.

오직

- URL
- Manifest
- Metadata

만 저장한다.

---

# 16. Search

Registry는

- id
- name
- keyword

검색을 지원해야 한다.

향후 Tag 기반 검색도 지원한다.

---

# 17. Validation Rules

Registry는

- JSON Schema
- Duplicate ID
- Duplicate URL
- Invalid Version
- Invalid Manifest

검사를 통과해야 한다.

---

# 18. Builder Rules

Registry는

Builder가 자동 생성한다.

수동 편집을 금지한다.

---

# 19. Cache

Extension은 Registry를 Cache한다.

```
cache/

registry.json
```

Cache에는

- Version
- Download Date

를 저장한다.

---

# 20. Refresh Policy

Registry는

기본적으로 하루 1회 확인한다.

사용자가 강제 업데이트를 요청하면 즉시 다시 다운로드한다.

---

# 21. Error Handling

Registry가 손상된 경우

- Cache 사용
- 사용자 알림
- 다운로드 중단

---

# 22. Future Extensions

향후 지원

- Multiple Mirrors
- CDN Priority
- Region Selection
- Digital Signature
- Delta Registry
- Package Tags
- Categories

---

# 23. JSON Schema (Draft)

```json
{
  "type":"object",
  "required":[
    "version",
    "generatedAt",
    "packages"
  ]
}
```

---

# 24. Quality Requirements

Registry는

- Duplicate ID 없음
- Duplicate URL 없음
- 모든 Manifest 존재
- 모든 Download URL 존재
- Version 일치

를 만족해야 한다.

---

# 25. Summary

Registry는 VSkript Database의 중앙 인덱스이다.

VSCode Extension은 Registry를 기준으로 필요한 DataPack을 검색하고,
Manifest를 조회하며,
변경된 DataPack만 다운로드하여 증분 업데이트를 수행한다.

Registry는 DataPack의 메타데이터만 관리하며,
실제 데이터는 각 DataPack과 Manifest에서 제공한다.

---

# 26. Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0002 |
| Document | REGISTRY_SCHEMA.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |