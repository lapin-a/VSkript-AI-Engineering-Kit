# SPEC-0011_DATAPACK_LAYOUT.md

# VSkript DataPack Layout Specification

> DataPack Package Structure Standard

Specification: SPEC-0011

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 DataPack의 디렉터리 구조와 파일 규격을 정의한다.

모든 DataPack은 동일한 구조를 따라야 하며, Builder, Generator, Validator, VSCode Extension 및 Language Server는 본 명세를 기준으로 DataPack을 생성하고 해석한다.

---

# 2. Objectives

DataPack Layout의 목표

- 표준 디렉터리 구조 정의
- DataPack 간 일관성 확보
- 자동 로딩 지원
- 선택적 다운로드 지원
- 버전 관리 지원
- 유지보수성 향상

---

# 3. DataPack Overview

하나의 DataPack은 하나의 Plugin(Addon)을 나타낸다.

예시

```
SkBee

SkQuery

skript-reflect

TuSKe
```

각 DataPack은 독립적으로 다운로드 및 업데이트할 수 있다.

---

# 4. Package Structure

기본 구조

```
pack/

├── manifest.json
├── metadata.json
├── registry.json
├── grammar/
├── locale/
├── documentation/
├── assets/
└── schemas/
```

모든 DataPack은 Root Directory를 가진다.

---

# 5. Required Files

다음 파일은 반드시 존재해야 한다.

| File | Required |
|--------|----------|
| manifest.json | ✔ |
| metadata.json | ✔ |
| grammar/ | ✔ |
| locale/ | ✔ |

---

# 6. Optional Directories

선택적으로 포함할 수 있는 디렉터리

```
documentation/

assets/

schemas/

examples/

tests/
```

---

# 7. Grammar Directory

Grammar JSON 파일을 저장한다.

예시

```
grammar/

effects.json

events.json

expressions.json

conditions.json

sections.json

functions.json

types.json
```

파일은 기능별로 분리한다.

---

# 8. Locale Directory

모든 번역 파일을 저장한다.

예시

```
locale/

en.json

ko.json

ja.json

zh-CN.json

fr.json
```

파일명은 ISO Language Code를 사용한다.

---

# 9. Assets Directory

정적 리소스를 저장한다.

예시

```
assets/

icon.png

logo.svg

banner.png
```

Assets는 실행에 영향을 주지 않는다.

---

# 10. Documentation Directory

문서를 저장한다.

예시

```
documentation/

README.md

CHANGELOG.md

LICENSE.md

migration.md
```

---

# 11. Schemas Directory

DataPack 전용 JSON Schema를 저장한다.

예시

```
schemas/

grammar.schema.json

manifest.schema.json

locale.schema.json
```

---

# 12. Examples Directory

예제 Skript를 저장한다.

```
examples/

spawn.sk

gui.sk

command.sk
```

---

# 13. Tests Directory

자동 검증용 테스트를 저장한다.

```
tests/

grammar.test.json

validation.test.json
```

---

# 14. Manifest

manifest.json은 DataPack의 진입점이다.

예시

```json
{
    "id": "skbee",
    "version": "3.12.2"
}
```

---

# 15. Metadata

metadata.json은 부가 정보를 저장한다.

예시

```json
{
    "author": "VSkript Team",
    "license": "MIT"
}
```

---

# 16. Registry

registry.json은 DataPack 내부 Resource Index를 저장한다.

예시

```json
{
    "effects": 120,
    "events": 35,
    "expressions": 210
}
```

---

# 17. File Naming

모든 파일은 다음 규칙을 따른다.

- 소문자
- kebab-case
- UTF-8
- 확장자 .json

예시

```
effects.json

event-values.json

type-converter.json
```

---

# 18. Compression

배포 시 DataPack은 ZIP 형식을 사용한다.

예시

```
skbee-3.12.2.zip
```

압축 해제 후 Root Directory가 바로 나타나야 한다.

---

# 19. Loading Order

DataPack은 다음 순서로 로드한다.

```
manifest.json

↓

metadata.json

↓

registry.json

↓

grammar/

↓

locale/

↓

documentation/

↓

assets/
```

---

# 20. Validation Rules

Validator는 다음을 검사한다.

- Required Files
- Invalid Directory
- Duplicate Resource
- Invalid File Name
- Missing Manifest
- Invalid Locale
- Invalid Grammar

---

# 21. Builder Integration

Builder는

- 디렉터리 생성
- JSON 생성
- Resource 분리
- 압축 생성

을 수행한다.

---

# 22. Generator Integration

Generator는

- Grammar 생성
- Locale 생성
- Manifest 생성
- Registry 생성

을 수행한다.

---

# 23. Extension Integration

VSCode Extension은 DataPack을 로드하여

- Completion
- Hover
- Definition
- Diagnostics

생성에 사용한다.

---

# 24. Compatibility

DataPack은 다음 명세를 준수해야 한다.

- SPEC-0008_JSON_SCHEMA_GUIDE
- SPEC-0009_VERSIONING
- SPEC-0010_DEPENDENCY_RESOLUTION

---

# 25. Future Extensions

향후 다음 디렉터리를 추가할 수 있다.

```
cache/

templates/

snippets/

migration/

telemetry/
```

기존 DataPack과의 호환성을 유지해야 한다.

---

# 26. Example Layout

```
skbee/

├── manifest.json
├── metadata.json
├── registry.json
│
├── grammar/
│   ├── effects.json
│   ├── events.json
│   ├── expressions.json
│   ├── conditions.json
│   ├── sections.json
│   └── functions.json
│
├── locale/
│   ├── en.json
│   ├── ko.json
│   └── ja.json
│
├── documentation/
│   ├── README.md
│   └── CHANGELOG.md
│
├── assets/
│   └── icon.png
│
└── schemas/
    ├── manifest.schema.json
    ├── grammar.schema.json
    └── locale.schema.json
```

---

# 27. Summary

DataPack Layout는 VSkript Database에서 사용하는 DataPack의 표준 디렉터리 구조를 정의한다.

모든 DataPack은 동일한 구조를 유지해야 하며, Builder와 Extension은 본 명세를 기준으로 DataPack을 생성하고 로드한다.

---

# 28. Related Specifications

- SPEC-0008_JSON_SCHEMA_GUIDE.md
- SPEC-0009_VERSIONING.md
- SPEC-0010_DEPENDENCY_RESOLUTION.md

---

# 29. Referenced RFC

- RFC-0001_MANIFEST_SCHEMA.md
- RFC-0002_REGISTRY_SCHEMA.md
- RFC-0003_GRAMMAR_SCHEMA.md
- RFC-0007_BUILDER_SPECIFICATION.md

---

# 30. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0011 |
| Document | DATAPACK_LAYOUT.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |