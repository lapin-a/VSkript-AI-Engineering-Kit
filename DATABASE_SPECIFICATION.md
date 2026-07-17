# DATABASE_SPECIFICATION.md

# VSkript Database

> Database Architecture Specification

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript Database 프로젝트의 전체 데이터베이스 구조를 정의한다.

VSkript Database는 SkriptHub 데이터를 기반으로 정제된 JSON Database를 생성하며,
사용자는 필요한 DataPack만 다운로드하여 사용할 수 있다.

본 문서는 데이터 구조, 관리 방식, 버전 정책 및 업데이트 전략의 기준 문서이다.

---

# 2. Goals

VSkript Database는 다음 목표를 가진다.

- SkriptHub 기반 데이터 구축
- Addon 단위 DataPack 제공
- 필요한 데이터만 다운로드
- 자동 업데이트
- Incremental Update
- 다국어 지원
- Version Compatibility
- 높은 유지보수성

---

# 3. High-Level Architecture

```
SkriptHub Source

↓

Raw Snapshot

↓

Data Builder

↓

Normalizer

↓

Data Cleaner

↓

Data Validator

↓

DataPack Generator

↓

Registry Generator

↓

Distribution
```

---

# 4. Database Structure

```
database/

manifest.json

registry.json

locales/

addons/

skript/

skbee/

skquery/

skrayfall/

...

```

---

# 5. Core Components

## Raw Source

SkriptHub에서 수집한 원본 데이터.

특징

- 수정하지 않는다.
- Snapshot 보관.
- Version 관리.

---

## Builder

Raw 데이터를 내부 구조로 변환한다.

역할

- Parsing
- Mapping
- Type Conversion

---

## Cleaner

불필요한 데이터를 제거한다.

예

- 중복
- 빈 값
- Deprecated

---

## Normalizer

모든 데이터를 동일한 형식으로 맞춘다.

예

```
name

description

examples

since

deprecated

patterns
```

---

## Validator

JSON Schema 검증.

검사 항목

- Required
- Type
- Enum
- Version

---

## Generator

최종 DataPack 생성.

출력

```
addon.json

manifest.json

locale.json
```

---

# 6. DataPack Structure

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

ko.json

en.json

ja.json

```

---

# 7. Manifest Specification

모든 DataPack은 Manifest를 가진다.

필수 항목

```
id

name

version

addonVersion

databaseVersion

dependencies

languages

checksum

updatedAt
```

---

# 8. Registry Specification

Registry는 설치 가능한 모든 DataPack 목록이다.

예

```
registry.json

{
"skript": "...",

"skbee": "...",

"skquery": "...",

...
}
```

Registry는

- 검색
- 다운로드
- 버전 확인

에 사용된다.

---

# 9. Locale System

모든 번역은 Locale로 분리한다.

```
locale/

ko.json

en.json

ja.json

zh-CN.json

zh-TW.json

```

문법 데이터와 번역 데이터를 분리한다.

---

# 10. Plugin Detection

VSCode Extension은

Workspace를 검사하여

```
plugins/

SkBee.jar

SkQuery.jar

```

를 탐지한다.

Registry와 비교하여 필요한 DataPack만 요청한다.

---

# 11. Data Download

Extension은

```
Registry

↓

Manifest

↓

Required DataPack

↓

Download

↓

Cache
```

순서로 동작한다.

---

# 12. Cache

다운로드된 DataPack은 Cache에 저장한다.

Cache에는

- Version
- Checksum
- Installed Date

를 기록한다.

---

# 13. Incremental Update

전체 Database를 다시 다운로드하지 않는다.

동작

```
Registry 비교

↓

Version 비교

↓

Changed Manifest

↓

Changed DataPack

↓

Update
```

---

# 14. Version Policy

Semantic Versioning 사용.

```
Database

1.4.0

Addon

2.3.1
```

독립적으로 관리한다.

---

# 15. Compatibility

Manifest는 지원 버전을 가진다.

예

```
supports

Skript 2.8

Minecraft 1.21

Database 1.x
```

---

# 16. JSON Standards

모든 JSON은

UTF-8

2 Spaces

Alphabetical Order

Trailing Comma 금지

---

# 17. Directory Structure

```
database/

raw/

builder/

cleaner/

generator/

registry/

locale/

packs/

cache/

schemas/
```

---

# 18. Future Extensions

향후 추가 예정

- CDN Distribution
- Package Manager
- Online Registry
- Delta Patch
- Compression
- Binary Index

---

# 19. Quality Requirements

모든 DataPack은

- JSON Schema 통과
- Validation 완료
- Manifest 생성
- Locale 분리
- Checksum 생성

---

# 20. Summary

VSkript Database는 SkriptHub 기반의 정제된 JSON 데이터베이스를 구축하여,
사용자가 필요한 애드온 DataPack만 선택적으로 다운로드할 수 있도록 하는 것을 목표로 한다.

이를 위해 Builder, Cleaner, Validator, Generator, Registry 및 Locale 시스템을 중심으로 설계하며,
향후 자동 업데이트, 버전 관리, 다국어 지원 및 CDN 배포까지 확장 가능한 구조를 유지한다.