# SPEC-0010_DEPENDENCY_RESOLUTION.md

# VSkript Dependency Resolution Specification

> Dependency Management and Resolution Standard

Specification: SPEC-0010

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 Dependency Resolution(의존성 해결) 규칙을 정의한다.

모든 DataPack, Registry, Builder, Validator, Generator 및 VSCode Extension은 본 명세를 기반으로 의존성을 분석하고 해결해야 한다.

본 명세의 목적은 필요한 Grammar와 Resource를 정확하게 로드하고, 불필요한 데이터의 다운로드를 방지하는 것이다.

---

# 2. Objectives

Dependency Resolution의 목표

- 의존성 표준화
- 자동 DataPack 다운로드
- 순환 의존성 방지
- 버전 호환성 보장
- 최소 다운로드
- 빠른 로딩 지원

---

# 3. Dependency Scope

의존성은 다음 요소에 적용된다.

- Plugin
- DataPack
- Grammar
- Locale
- Type
- Builder
- Extension

---

# 4. Dependency Types

지원하는 Dependency 유형

| Type | Description |
|-------|-------------|
| Required | 반드시 필요 |
| Optional | 선택 사항 |
| Recommended | 권장 |
| Development | 개발 환경 전용 |
| Runtime | 실행 시 필요 |

---

# 5. Required Dependency

Required Dependency가 존재하지 않으면 해당 Resource는 활성화될 수 없다.

예시

```json
{
  "dependencies": [
    {
      "id": "skript",
      "type": "required"
    }
  ]
}
```

---

# 6. Optional Dependency

Optional Dependency는 존재하면 활성화되며, 없더라도 정상적으로 동작해야 한다.

예시

```json
{
  "dependencies": [
    {
      "id": "skbee",
      "type": "optional"
    }
  ]
}
```

---

# 7. Recommended Dependency

Recommended Dependency는 사용자에게 설치를 권장하지만 자동 설치 대상은 아니다.

---

# 8. Runtime Dependency

Runtime Dependency는 실제 실행 시점에만 필요하다.

Builder는 이를 Build Dependency와 구분해야 한다.

---

# 9. Development Dependency

Development Dependency는 Builder와 Generator만 사용한다.

최종 DataPack에는 포함되지 않는다.

---

# 10. Dependency Object

모든 Dependency는 동일한 구조를 가진다.

```json
{
  "id": "skbee",
  "type": "required",
  "version": "^3.12.0"
}
```

---

# 11. Dependency Identifier

Dependency는 반드시 Registry ID를 사용한다.

허용

```
skript

skbee

skript-reflect
```

허용하지 않음

```
SkBee.jar

plugins/SkBee.jar

SkBee
```

---

# 12. Version Constraint

Dependency는 Version Constraint를 포함할 수 있다.

예시

```json
{
  "id": "skript",
  "version": ">=2.9.0"
}
```

지원 연산자

```
=

>

>=

<

<=

^

~
```

---

# 13. Resolution Order

Dependency Resolution 순서

```
Manifest

↓

Registry

↓

Dependency Graph

↓

Version Check

↓

Download Queue

↓

Installation

↓

Activation
```

---

# 14. Dependency Graph

모든 Dependency는 Directed Graph로 관리한다.

예시

```
Project

├── Skript

├── SkBee

│   └── Skript

└── skript-reflect

    └── Skript
```

---

# 15. Circular Dependency

순환 의존성은 허용되지 않는다.

예시

```
A

↓

B

↓

C

↓

A
```

Validator는 이를 오류로 보고해야 한다.

---

# 16. Duplicate Dependency

동일한 Dependency는 하나만 유지한다.

예시

```
SkBee

↓

SkBee

↓

SkBee
```

↓

```
SkBee
```

---

# 17. Resolution Strategy

Resolver는 다음 우선순위를 따른다.

1. Required
2. Runtime
3. Optional
4. Recommended

---

# 18. Installation Order

Dependency는 Topological Sort 결과에 따라 설치한다.

예시

```
Skript

↓

SkBee

↓

Project
```

---

# 19. Failure Handling

Required Dependency 실패

↓

Installation 중단

Optional Dependency 실패

↓

Warning 출력

Recommended Dependency 실패

↓

Notification 출력

---

# 20. Dependency Cache

설치된 Dependency는 Cache에 저장한다.

예시

```json
{
  "installed": [
    "skript",
    "skbee"
  ]
}
```

---

# 21. Update Policy

Dependency 업데이트 시

- Version 확인
- Compatibility 확인
- Registry 확인
- Cache 갱신

순으로 수행한다.

---

# 22. Validation Rules

Validator는 다음을 검사한다.

- Unknown Dependency
- Invalid Version Constraint
- Duplicate Dependency
- Circular Dependency
- Missing Required Dependency
- Unsupported Version

---

# 23. Extension Integration

VSCode Extension은 Dependency Resolver를 사용하여

- 필요한 DataPack 다운로드
- 자동 업데이트
- Cache 관리
- Hover 지원

을 수행한다.

---

# 24. Builder Integration

Builder는

- Dependency Graph 생성
- Topological Sort
- Version 검증
- Registry 연결

을 수행한다.

---

# 25. Generator Integration

Generator는 Dependency 정보를 기반으로

- Manifest 생성
- Registry 생성
- Dependency 목록 생성

을 수행한다.

---

# 26. Summary

Dependency Resolution은 VSkript Database의 모든 구성 요소가 공통으로 사용하는 의존성 관리 규격이다.

모든 의존성은 Registry ID를 기준으로 관리되며, Directed Graph와 Topological Sort를 통해 안정적으로 해결되어야 한다.

---

# 27. Related Specifications

- SPEC-0002_PLUGIN_MAPPING.md
- SPEC-0007_TYPE_SYSTEM.md
- SPEC-0008_JSON_SCHEMA_GUIDE.md
- SPEC-0009_VERSIONING.md

---

# 28. Referenced RFC

- RFC-0001_MANIFEST_SCHEMA.md
- RFC-0002_REGISTRY_SCHEMA.md
- RFC-0005_UPDATE_PROTOCOL.md
- RFC-0006_PLUGIN_DETECTION_PROTOCOL.md
- RFC-0007_BUILDER_SPECIFICATION.md

---

# 29. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0010 |
| Document | DEPENDENCY_RESOLUTION.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |