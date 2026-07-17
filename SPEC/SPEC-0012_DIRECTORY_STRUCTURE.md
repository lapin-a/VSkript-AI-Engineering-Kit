# SPEC-0012_DIRECTORY_STRUCTURE.md

# VSkript Directory Structure Specification

> Repository Structure Standard

Specification: SPEC-0012

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database Repository의 공식 디렉터리 구조를 정의한다.

Repository의 모든 디렉터리와 파일은 본 명세를 따라야 하며, Builder, Generator, Validator, VSCode Extension 및 CI/CD는 동일한 구조를 기반으로 동작한다.

본 명세는 프로젝트의 장기적인 유지보수성과 확장성을 보장하기 위한 기준이다.

---

# 2. Objectives

Repository Structure의 목표

- 표준 디렉터리 구조 정의
- 역할 분리
- Build Pipeline 지원
- 자동 생성 지원
- CI/CD 지원
- 유지보수성 향상

---

# 3. Repository Overview

Repository는 크게 다음 영역으로 구성된다.

```
Source

↓

Build

↓

Generated

↓

Distribution

↓

Documentation
```

각 영역은 명확하게 분리되어야 한다.

---

# 4. Root Layout

```
/

├── raw/
├── build/
├── generated/
├── registry/
├── packs/
├── locale/
├── schemas/
├── scripts/
├── docs/
├── tests/
├── tools/
├── .github/
├── package.json
├── tsconfig.json
└── README.md
```

---

# 5. raw/

원본 데이터를 저장한다.

예시

```
raw/

skripthub/

addons/

metadata/

sources/
```

Builder는 이 디렉터리만 읽는다.

직접 수정 가능한 영역이다.

---

# 6. build/

빌드 중 생성되는 임시 데이터를 저장한다.

예시

```
build/

cache/

temp/

logs/

reports/
```

빌드 완료 후 삭제 가능하다.

Git에는 포함하지 않는다.

---

# 7. generated/

Builder가 생성한 중간 산출물을 저장한다.

예시

```
generated/

grammar/

locale/

registry/

manifest/
```

자동 생성 전용이다.

수동 수정해서는 안 된다.

---

# 8. registry/

Registry 데이터를 저장한다.

예시

```
registry/

registry.json

plugins.json

types.json

aliases.json
```

Extension은 Registry를 먼저 로드한다.

---

# 9. packs/

최종 DataPack을 저장한다.

예시

```
packs/

skript/

skbee/

skquery/
```

각 DataPack은 SPEC-0011을 따라야 한다.

---

# 10. locale/

공통 Locale을 저장한다.

예시

```
locale/

en/

ko/

ja/

zh-CN/
```

DataPack 공통 번역을 제공한다.

---

# 11. schemas/

공식 JSON Schema를 저장한다.

예시

```
schemas/

manifest.schema.json

grammar.schema.json

registry.schema.json

locale.schema.json
```

Validator는 이 디렉터리를 사용한다.

---

# 12. scripts/

자동화 스크립트를 저장한다.

예시

```
scripts/

build.ts

update.ts

validate.ts

generate.ts
```

---

# 13. docs/

프로젝트 문서를 저장한다.

예시

```
docs/

architecture/

specifications/

rfc/

guides/
```

---

# 14. tests/

테스트를 저장한다.

예시

```
tests/

builder/

validator/

generator/

integration/
```

---

# 15. tools/

개발 도구를 저장한다.

예시

```
tools/

migration/

benchmark/

profiling/

utilities/
```

---

# 16. .github/

GitHub 관련 설정을 저장한다.

예시

```
.github/

workflows/

ISSUE_TEMPLATE/

PULL_REQUEST_TEMPLATE/
```

---

# 17. Build Flow

Repository의 데이터 흐름

```
raw/

↓

Builder

↓

generated/

↓

Validator

↓

registry/

↓

DataPack Generator

↓

packs/
```

---

# 18. Source of Truth

Repository의 유일한 원본(Source of Truth)은

```
raw/
```

이다.

다른 모든 데이터는 raw로부터 생성된다.

---

# 19. Generated Content

다음 디렉터리는 자동 생성된다.

```
generated/

registry/

packs/
```

직접 수정해서는 안 된다.

---

# 20. Git Rules

Git 관리 정책

관리 대상

```
raw/

docs/

schemas/

scripts/

tests/

tools/
```

자동 생성 파일은 필요 시 Release Artifact로 배포한다.

---

# 21. Build Rules

Builder는 다음 순서를 따른다.

```
raw/

↓

Parser

↓

Cleaner

↓

Generator

↓

Validator

↓

Registry

↓

DataPack

↓

Release
```

---

# 22. CI/CD Integration

CI는 다음 작업을 수행한다.

- Build
- Validation
- Schema Check
- Test
- Package
- Release

---

# 23. Extension Integration

VSCode Extension은 다음 디렉터리 구조를 기준으로 동작한다.

```
registry/

↓

packs/

↓

locale/
```

Extension은 raw를 직접 읽지 않는다.

---

# 24. Validation Rules

Validator는 다음을 검사한다.

- Directory 존재 여부
- Required File
- Invalid File Name
- Duplicate Resource
- Broken Reference
- Missing Schema

---

# 25. Future Extensions

향후 추가 가능한 디렉터리

```
analytics/

telemetry/

benchmarks/

plugins/

migration/

templates/
```

기존 구조와의 호환성을 유지해야 한다.

---

# 26. Repository Example

```
vskript-database/

├── raw/
├── build/
├── generated/
├── registry/
├── packs/
├── locale/
├── schemas/
├── scripts/
├── docs/
├── tests/
├── tools/
├── package.json
├── tsconfig.json
└── README.md
```

---

# 27. Summary

Directory Structure는 VSkript Database Repository의 공식 구조를 정의한다.

모든 데이터는 `raw/`를 기준으로 생성되며, Builder와 Generator는 생성물을 `generated/`, `registry/`, `packs/`에 출력한다.

Repository 구조를 표준화함으로써 자동화된 Build Pipeline, DataPack 생성 및 유지보수를 일관성 있게 수행할 수 있다.

---

# 28. Related Specifications

- SPEC-0008_JSON_SCHEMA_GUIDE.md
- SPEC-0010_DEPENDENCY_RESOLUTION.md
- SPEC-0011_DATAPACK_LAYOUT.md

---

# 29. Referenced RFC

- RFC-0001_MANIFEST_SCHEMA.md
- RFC-0002_REGISTRY_SCHEMA.md
- RFC-0005_UPDATE_PROTOCOL.md
- RFC-0007_BUILDER_SPECIFICATION.md
- RFC-0009_GENERATOR_SPECIFICATION.md

---

# 30. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0012 |
| Document | DIRECTORY_STRUCTURE.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |