# SPEC-0002_PLUGIN_MAPPING.md

# VSkript Plugin Mapping Specification

> Plugin Identification & Mapping Standard

Specification: SPEC-0002

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 Minecraft Plugin(JAR)을 VSkript Database의 DataPack으로 변환하기 위한 공식 Mapping 규칙을 정의한다.

Plugin Mapping은 VSCode Extension, Builder, Registry Generator 및 Plugin Detection Protocol에서 공통으로 사용되는 규격이다.

본 명세의 목적은 Plugin의 이름이나 파일명이 변경되더라도 동일한 DataPack을 안정적으로 식별할 수 있도록 하는 것이다.

---

# 2. Objectives

Plugin Mapping의 목표

- Plugin 자동 식별
- DataPack ID 생성
- Registry 조회
- Alias 지원
- 충돌 방지
- 안정적인 식별

---

# 3. Overview

Plugin Detection은 다음 순서로 Plugin을 식별한다.

```
Plugin JAR

↓

plugin.yml

↓

Plugin Metadata

↓

Mapping

↓

Registry ID

↓

DataPack
```

---

# 4. Detection Priority

Plugin 식별은 반드시 다음 우선순위를 따른다.

| Priority | Source | Required |
|----------|--------|----------|
| 1 | plugin.yml name | ✔ |
| 2 | plugin.yml main | ✔ |
| 3 | Registry Mapping | ✔ |
| 4 | Alias Table | Optional |
| 5 | JAR Filename | Fallback |

파일명만으로 Plugin을 식별해서는 안 된다.

---

# 5. plugin.yml

Plugin Metadata의 기본 정보는 plugin.yml에서 읽는다.

예시

```yaml
name: SkBee

main: com.shanebeestudios.skbee.SkBee

version: 3.12.2
```

필수 항목

- name
- main
- version

---

# 6. Mapping Object

Builder는 다음 구조를 생성한다.

```json
{
  "pluginName": "SkBee",

  "mainClass": "com.shanebeestudios.skbee.SkBee",

  "version": "3.12.2",

  "dataPackId": "skbee"
}
```

---

# 7. DataPack ID

모든 Plugin은 하나의 DataPack ID를 가진다.

규칙

- 소문자
- kebab-case
- 영구 식별자
- 변경 금지

예

```
skript

skbee

skquery

skrayfall
```

---

# 8. Alias

Plugin은 여러 Alias를 가질 수 있다.

예

```json
{
  "aliases": [
    "SkBee",
    "skbee",
    "Sk Bee"
  ]
}
```

Alias는 검색용으로만 사용한다.

DataPack ID는 절대 변경하지 않는다.

---

# 9. Registry Mapping

Registry에는 Mapping 정보가 포함된다.

예

```json
{
  "id":"skbee",

  "pluginName":"SkBee",

  "aliases":[
      "Sk Bee"
  ]
}
```

---

# 10. Version Mapping

Plugin Version은 Manifest와 비교한다.

```
Plugin

↓

Version

↓

Manifest

↓

Compatibility
```

---

# 11. Duplicate Detection

다음 항목은 중복될 수 없다.

- DataPack ID
- Main Class

Plugin Name은 Alias를 통해 중복될 수 있다.

---

# 12. Conflict Resolution

동일한 이름의 Plugin이 존재할 경우

우선순위

```
Main Class

↓

Registry

↓

Alias

↓

Filename
```

---

# 13. Unknown Plugin

Registry에 없는 Plugin은

```
Unknown
```

상태로 처리한다.

Extension은

- Warning 출력
- Report 생성
- 자동 다운로드 금지

를 수행한다.

---

# 14. Mapping Cache

Extension은 Mapping 결과를 Cache한다.

```
cache/

plugin-map.json
```

---

# 15. Update Rules

Plugin Version이 변경되면

Manifest와 다시 비교한다.

DataPack ID는 변경하지 않는다.

---

# 16. Validation Rules

Validator는

- Duplicate ID
- Duplicate Main Class
- Invalid Alias
- Missing Metadata
- Invalid Version

을 검사한다.

---

# 17. Builder Rules

Builder는

- plugin.yml 파싱
- Alias 생성
- Registry Mapping 생성
- Cache 갱신

을 자동 수행한다.

---

# 18. Extension Integration

VSCode Extension은

Plugin Detection 후

```
Plugin

↓

Mapping

↓

Registry

↓

Manifest

↓

DataPack
```

순서로 처리한다.

---

# 19. Example

Plugin

```yaml
name: SkBee

main: com.shanebeestudios.skbee.SkBee

version: 3.12.2
```

↓

Registry

```json
{
  "id":"skbee",

  "pluginName":"SkBee",

  "aliases":[
      "Sk Bee"
  ]
}
```

↓

Download

```
packs/skbee.zip
```

---

# 20. Future Extensions

향후 추가 예정

- plugin.yml dependency 분석
- paper-plugin.yml 지원
- Multi-Version Mapping
- Plugin Family Mapping
- Community Alias Registry
- Marketplace Integration

---

# 21. Summary

Plugin Mapping은 Minecraft Plugin을 VSkript Database의 DataPack으로 연결하는 공식 규격이다.

Plugin은 plugin.yml을 기준으로 식별되며, Registry와 Manifest를 통해 DataPack으로 매핑된다.

Alias는 검색 편의를 위해 제공되며, DataPack ID는 프로젝트 전체에서 영구적인 식별자로 유지된다.

---

# 22. Related Specifications

- SPEC-0001_SCRIPT_HEADER.md
- SPEC-0003_STATE_MACHINE.md
- SPEC-0005_ERROR_CODES.md

---

# 23. Referenced RFC

- RFC-0002_REGISTRY_SCHEMA.md
- RFC-0005_UPDATE_PROTOCOL.md
- RFC-0006_PLUGIN_DETECTION_PROTOCOL.md

---

# 24. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0002 |
| Document | PLUGIN_MAPPING.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |