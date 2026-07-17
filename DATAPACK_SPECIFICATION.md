# DATAPACK_SPECIFICATION.md

# VSkript DataPack

> DataPack Specification

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database에서 제공하는 DataPack의 구조와 규격을 정의한다.

DataPack은 특정 Skript 애드온의 문법 데이터를 독립적으로 제공하는 최소 배포 단위이며, VSCode Extension은 필요한 DataPack만 선택적으로 다운로드하여 사용한다.

본 문서는 DataPack의 파일 구조, Manifest, 버전 정책, 의존성, 업데이트 및 무결성 검증 기준을 정의한다.

---

# 2. Design Goals

DataPack은 다음 목표를 가진다.

- Addon 단위 독립 배포
- 선택적 다운로드
- 빠른 업데이트
- 증분(Incremental) 업데이트 지원
- 다국어 지원
- 버전 호환성 관리
- 높은 확장성

---

# 3. DataPack Lifecycle

```

SkriptHub Source

↓

Builder

↓

Cleaner

↓

Generator

↓

DataPack

↓

Registry

↓

Distribution

↓

VSCode Extension

↓

Cache

↓

Language Server

```

---

# 4. Standard Directory Structure

```
skbee/

manifest.json

addon.json

events.json

effects.json

expressions.json

conditions.json

sections.json

functions.json

classes.json

locale/

en.json

ko.json

ja.json

assets/

icon.png

README.md

LICENSE

```

---

# 5. Required Files

| File | Required |
|--------|----------|
| manifest.json | ✔ |
| addon.json | ✔ |
| locale | ✔ |
| README.md | Optional |
| LICENSE | Optional |
| assets | Optional |

---

# 6. Manifest

모든 DataPack은 Manifest를 가진다.

예시

```json
{
  "id": "skbee",
  "name": "SkBee",
  "version": "3.12.0",
  "databaseVersion": "1.0.0",
  "addonVersion": "3.12.0",
  "checksum": "...",
  "languages": [
    "en",
    "ko"
  ]
}
```

---

# 7. Manifest Fields

필수 항목

| Field | Description |
|---------|-------------|
| id | 고유 ID |
| name | 이름 |
| version | DataPack Version |
| addonVersion | Addon Version |
| databaseVersion | Database Version |
| checksum | SHA-256 |
| updatedAt | 생성 시간 |
| dependencies | 의존성 |
| languages | 지원 언어 |

---

# 8. Grammar Files

문법은 종류별 JSON으로 분리한다.

```
events.json

effects.json

conditions.json

expressions.json

functions.json

sections.json

classes.json

```

각 파일은 독립적으로 로딩 가능해야 한다.

---

# 9. Locale

모든 설명은 Locale로 분리한다.

예

```
locale/

en.json

ko.json

ja.json

```

Grammar JSON에는 번역을 저장하지 않는다.

---

# 10. Dependency

DataPack은 다른 DataPack을 요구할 수 있다.

예

```
SkBee

↓

Skript
```

Manifest

```json
{
  "dependencies": [
    "skript"
  ]
}
```

---

# 11. Compatibility

Manifest는 지원 버전을 명시한다.

```json
{
  "supports": {
    "minecraft": "1.21+",
    "skript": "2.9+",
    "database": "1.x"
  }
}
```

---

# 12. Plugin Detection

Extension은 Workspace를 검사한다.

```
plugins/

SkBee.jar

```

↓

Registry 검색

↓

Manifest 확인

↓

DataPack 설치

---

# 13. Installation Flow

```
Registry

↓

Manifest

↓

Dependency Check

↓

Download

↓

Checksum

↓

Install

↓

Cache

↓

Load

```

---

# 14. Cache Structure

```
cache/

registry.json

installed.json

packs/

```

---

# 15. Update Flow

```
Registry

↓

Version Compare

↓

Changed Manifest

↓

Changed Pack

↓

Download

↓

Replace

```

---

# 16. Incremental Update

변경된 DataPack만 다시 다운로드한다.

예

```
SkBee

3.11

↓

3.12

```

SkBee만 다운로드한다.

---

# 17. Compression

권장 형식

```
ZIP

```

향후

```
.vspack

```

지원 예정.

---

# 18. Integrity

모든 DataPack은 SHA-256 Checksum을 가진다.

설치 시 반드시 검증한다.

---

# 19. Registry Integration

Registry는 모든 DataPack을 관리한다.

예

```json
{
  "skbee": {
    "version": "3.12.0",
    "manifest": "...",
    "download": "..."
  }
}
```

---

# 20. Loading Strategy

Language Server는 필요한 DataPack만 메모리에 로딩한다.

예

```
Workspace

↓

Detected Plugins

↓

Required Packs

↓

Load

```

---

# 21. Performance Goals

목표

- 빠른 다운로드
- 최소 메모리 사용
- Lazy Loading
- 병렬 다운로드 지원

---

# 22. Validation

모든 DataPack은

- JSON Schema
- Manifest
- Checksum
- Dependency

검증을 통과해야 한다.

---

# 23. Future Extensions

향후 지원

- CDN
- Delta Patch
- Binary Index
- Package Manager
- Digital Signature
- Compression Optimization

---

# 24. Quality Requirements

모든 DataPack은

- Manifest 생성
- Locale 분리
- JSON Validation
- Dependency Validation
- Version Validation
- Checksum 생성

을 만족해야 한다.

---

# 25. Summary

DataPack은 VSkript Database의 최소 배포 단위이다.

각 애드온은 독립적인 DataPack으로 관리되며, VSCode Extension은 Workspace의 Plugin을 분석하여 필요한 DataPack만 다운로드하고 로딩한다.

이를 통해 다운로드 용량을 최소화하고, 업데이트 속도를 높이며, 다국어 및 향후 CDN 기반 배포까지 지원할 수 있는 구조를 제공한다.

---

# 26. Document Information

| Item | Value |
|------|-------|
| Document | DATAPACK_SPECIFICATION.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |
