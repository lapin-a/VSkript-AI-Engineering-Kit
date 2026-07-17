# RFC-0007_BUILDER_SPECIFICATION.md

# VSkript Builder Specification

> Builder Architecture & Build Pipeline

RFC: RFC-0007

Version: 1.0

Status: Draft

---

# 1. Purpose

본 RFC는 VSkript Database Builder의 전체 아키텍처와 Build Pipeline을 정의한다.

Builder는 Raw Source로부터 Grammar, Locale, Registry 및 DataPack을 생성하는 핵심 구성 요소이며, Repository 내 모든 자동 생성 데이터의 유일한 생성기(Single Source Generator)이다.

본 문서는 Builder의 내부 구조와 데이터 흐름을 정의하며, 실제 구현의 기준 문서로 사용된다.

---

# 2. Goals

Builder의 목표는 다음과 같다.

- Raw Source를 표준 데이터로 변환
- Grammar 생성
- Locale 생성
- Registry 생성
- Manifest 생성
- DataPack 생성
- Incremental Build 지원
- 자동 검증 지원
- CI/CD 통합 지원

---

# 3. Scope

본 RFC는 다음 기능을 정의한다.

- Build Architecture
- Source Discovery
- Parser
- Intermediate Representation
- Data Normalization
- Build Pipeline
- Build Context
- Lifecycle

다음 내용은 별도 RFC에서 정의한다.

- Validator
- Generator
- Update Protocol
- Plugin Detection

---

# 4. Terminology

| Term | Description |
|------|-------------|
| Raw Source | Builder의 입력 데이터 |
| IR | Intermediate Representation |
| Registry | Resource Index |
| DataPack | 배포 가능한 데이터 패키지 |
| Build Context | 현재 Build 상태 |
| Build Step | Build의 개별 단계 |

---

# 5. Builder Overview

Builder는 Repository의 Source of Truth인 `raw/` 디렉터리를 입력으로 사용한다.

Builder는 Source를 직접 수정하지 않는다.

모든 출력은 Generated Resource이다.

Builder의 출력은 다음과 같다.

- Grammar
- Locale
- Registry
- Manifest
- DataPack

---

# 6. Architecture

Builder는 Pipeline Architecture를 사용한다.

```
Raw Source

↓

Discovery

↓

Parser

↓

Normalizer

↓

IR

↓

Builders

↓

Validator

↓

Generator

↓

Output
```

각 단계는 독립적으로 동작해야 하며, 이전 단계의 출력만을 입력으로 사용한다.

---

# 7. Builder Components

Builder는 다음 구성 요소로 이루어진다.

```
Source Discovery

Parser

Normalizer

Grammar Builder

Locale Builder

Registry Builder

Manifest Builder

Validator

Generator

Reporter
```

각 Component는 단일 책임 원칙(SRP)을 준수해야 한다.

---

# 8. Build Pipeline

전체 Build 순서는 다음과 같다.

```
Discover Source

↓

Load Files

↓

Parse

↓

Normalize

↓

Generate IR

↓

Generate Grammar

↓

Generate Locale

↓

Generate Registry

↓

Generate Manifest

↓

Validate

↓

Generate DataPack

↓

Write Output
```

각 단계는 이전 단계가 성공해야만 실행된다.

---

# 9. Source Discovery

Builder는 `raw/` 디렉터리를 탐색하여 Build 대상 데이터를 수집한다.

탐색 대상

```
raw/

addons/

metadata/

skripthub/
```

탐색 과정에서는 다음을 수행한다.

- 디렉터리 검색
- 파일 검색
- 파일 형식 확인
- 중복 제거

---

# 10. Source Validation

Builder는 Source를 읽기 전에 다음 항목을 검사한다.

- 디렉터리 존재 여부
- 필수 파일 존재 여부
- 파일 인코딩
- 파일 접근 권한

오류 발생 시 Build를 중단한다.

---

# 11. Parser

Parser는 Source를 읽어 내부 객체로 변환한다.

입력

```
JSON

Markdown

Metadata

Source Files
```

출력

```
Parsed Objects
```

Parser는 Source를 변경해서는 안 된다.

---

# 12. Intermediate Representation (IR)

Builder는 모든 데이터를 Intermediate Representation(IR)으로 변환한다.

IR은 Builder 내부에서만 사용되는 표준 객체 모델이다.

모든 Builder Component는 IR만을 사용한다.

```
Source

↓

Parser

↓

IR

↓

Builders
```

---

# 13. IR Requirements

IR은 다음 조건을 만족해야 한다.

- Immutable
- Serializable
- Versioned
- Language Independent
- Plugin Independent

IR은 최종 출력 형식과 독립적이어야 한다.

---

# 14. Data Normalization

IR 생성 후 Builder는 데이터를 정규화한다.

정규화 작업

- 공백 제거
- 중복 제거
- Alias 처리
- Type 통일
- Version 통일
- Locale Key 정규화

정규화 이후의 데이터만 Builder에서 사용한다.

---

# 15. Build Context

Build 실행 중 Builder는 Build Context를 유지한다.

Build Context는 다음 정보를 포함한다.

- Build ID
- Start Time
- Builder Version
- Source Version
- Target Version
- Build Mode
- Active Plugin
- Loaded Resources

Build Context는 Build 종료 시 폐기된다.

---

# 16. Build Modes

Builder는 다음 Build Mode를 지원한다.

| Mode | Description |
|------|-------------|
| Full | 전체 Build |
| Incremental | 변경된 데이터만 Build |
| Clean | 캐시 제거 후 Build |
| Validate | Build 없이 Validation만 수행 |

Build Mode는 CLI 또는 CI에서 지정할 수 있다.

---

# 17. Build Lifecycle

Builder의 생명주기는 다음과 같다.

```
Initialize

↓

Discover

↓

Parse

↓

Normalize

↓

Generate

↓

Validate

↓

Package

↓

Complete
```

오류 발생 시 Failure 상태로 전환된다.

---

# 18. Design Principles

Builder는 다음 원칙을 따른다.

- Single Source of Truth
- Deterministic Build
- Reproducible Output
- Immutable Processing
- Stateless Components
- Incremental Friendly
- Fail Fast

---

# 19. Error Handling

각 단계는 오류를 자체적으로 감지하고 Error Code를 생성해야 한다.

오류 발생 시 Build Context에 기록한다.

Error Code는 SPEC-0005_ERROR_CODES를 따른다.

---

# 20. Summary

Builder는 VSkript Database의 핵심 구성 요소이며, Raw Source를 표준화된 Intermediate Representation으로 변환한 후 Grammar, Locale, Registry 및 DataPack을 생성한다.

모든 Build 과정은 Pipeline Architecture를 따르며, 각 단계는 독립적으로 동작하고 동일한 입력에 대해 항상 동일한 결과를 생성해야 한다.

---

# 21. Purpose

본 문서는 Builder가 Intermediate Representation(IR)을 기반으로 실제 Data Resource를 생성하는 과정을 정의한다.

Builder는 IR을 입력으로 받아 Grammar, Locale, Registry, Manifest 및 DataPack을 생성하며, 생성 과정은 결정적(Deterministic)이어야 한다.

---

# 22. Build Process Overview

IR 생성 이후 Build Pipeline은 다음 순서를 따른다.

```
IR

↓

Grammar Builder

↓

Locale Builder

↓

Type Builder

↓

Registry Builder

↓

Manifest Builder

↓

Dependency Resolver

↓

Validator

↓

Build Report
```

각 단계는 이전 단계의 결과를 기반으로 수행된다.

---

# 23. Grammar Builder

Grammar Builder는 IR로부터 Grammar Resource를 생성한다.

입력

```
Intermediate Representation
```

출력

```
grammar/
```

생성 대상

- Effects
- Events
- Expressions
- Conditions
- Sections
- Functions
- Types

Grammar는 Resource Category별로 분리하여 생성한다.

---

# 24. Grammar Generation Rules

Grammar Builder는 다음 규칙을 따른다.

- Pattern 중복 제거
- Type 검증
- Alias 적용
- Deprecated 표시
- Category 분류
- Stable Ordering 유지

동일 입력은 항상 동일한 Grammar를 생성해야 한다.

---

# 25. Locale Builder

Locale Builder는 사용자에게 표시되는 문자열을 생성한다.

출력

```
locale/

en.json

ko.json

ja.json

...
```

Locale Builder는 Grammar와 독립적으로 동작한다.

---

# 26. Locale Generation Rules

Locale Builder는

- Display Name
- Description
- Documentation
- Hover
- Diagnostic Message

를 Locale Resource로 분리한다.

문자열은 직접 저장하지 않고 Locale Key를 생성한다.

예시

```
grammar.effect.spawn.name

grammar.effect.spawn.description
```

---

# 27. Type Builder

Type Builder는 모든 Type 정보를 생성한다.

출력

```
types.json
```

Type Builder는

- Primitive
- Object
- Collection
- Alias
- Custom Type

을 생성한다.

Type은 SPEC-0007을 따른다.

---

# 28. Registry Builder

Registry Builder는 Repository 전체 Resource Index를 생성한다.

출력

```
registry.json
```

Registry에는 다음 정보가 포함된다.

- Resource ID
- Category
- Version
- DataPack
- Dependency

Registry는 Builder가 생성하는 가장 중요한 데이터 중 하나이다.

---

# 29. Registry Generation Rules

Registry는

- 중복 ID 제거
- Stable Ordering
- Version 검증
- Dependency 연결

을 수행한다.

Registry는 항상 정렬된 상태로 저장한다.

---

# 30. Manifest Builder

Manifest Builder는 DataPack의 Manifest를 생성한다.

출력

```
manifest.json
```

Manifest에는 다음 정보가 포함된다.

- ID
- Version
- Builder Version
- Schema Version
- Dependencies
- Generated Time

---

# 31. Manifest Rules

Manifest는 반드시 다음 조건을 만족해야 한다.

- 고유 ID
- Semantic Version
- Dependency 목록
- Builder Version 포함
- Schema Version 포함

Manifest는 DataPack의 Entry Point이다.

---

# 32. Dependency Resolution

Manifest 생성 이후 Builder는 Dependency를 해결한다.

절차

```
Dependency Load

↓

Registry Lookup

↓

Version Check

↓

Graph Build

↓

Validation
```

Dependency Resolution은 SPEC-0010을 따른다.

---

# 33. Resource Linking

Builder는 생성된 Resource를 상호 연결한다.

예

```
Grammar

↓

Locale

↓

Type

↓

Registry

↓

Manifest
```

모든 Reference는 Registry ID를 사용한다.

---

# 34. Cross Reference Validation

Builder는 다음 연결을 검증한다.

- Grammar → Type
- Grammar → Locale
- Registry → Manifest
- Dependency → Registry

Broken Reference는 허용되지 않는다.

---

# 35. Validation Phase

모든 Resource 생성 이후 Validator를 실행한다.

검사 대상

- JSON Schema
- Pattern
- Type
- Locale
- Registry
- Dependency

Validation 실패 시 Output은 생성되지 않는다.

---

# 36. Error Handling

Builder는 단계별 오류를 생성한다.

예

```
Grammar Build Failed

↓

BD001
```

```
Invalid Registry

↓

RG003
```

오류는 Build Report에 기록한다.

---

# 37. Logging

Builder는 Build 과정 전체를 기록한다.

로그 항목

- Start Time
- End Time
- Resource Count
- Warning Count
- Error Count
- Generated Files
- Duration

로그는 사람이 읽을 수 있는 형식을 권장한다.

---

# 38. Build Report

Build 종료 후 Report를 생성한다.

예시

```json
{
  "status": "success",
  "duration": 5120,
  "generated": {
    "grammar": 126,
    "locale": 3,
    "registry": 1,
    "manifest": 1
  },
  "warnings": 2,
  "errors": 0
}
```

Build Report는 CI/CD에서 사용할 수 있어야 한다.

---

# 39. Build Output

Builder의 출력은 다음과 같다.

```
generated/

registry/

packs/

reports/
```

각 출력은 SPEC-0011과 SPEC-0012를 따라야 한다.

---

# 40. Summary

Builder는 Intermediate Representation을 기반으로 Grammar, Locale, Type, Registry 및 Manifest를 생성한다.

생성된 모든 Resource는 상호 연결되고 Validation을 통과해야 하며, Build Report를 통해 결과를 기록한다.

Builder는 항상 동일한 입력에 대해 동일한 출력과 동일한 Registry를 생성해야 한다.

---

# 41. Purpose

본 문서는 VSkript Builder의 Incremental Build 시스템과 Cache 관리, 성능 최적화 및 장애 복구 전략을 정의한다.

Builder는 변경되지 않은 데이터를 다시 생성하지 않아야 하며, 최소한의 작업만 수행하여 DataPack을 생성해야 한다.

Incremental Build는 VSkript Database의 핵심 기능이다.

---

# 42. Objectives

Incremental Build의 목표

- 변경된 Resource만 재생성
- Build 시간 최소화
- Cache 재사용
- 불필요한 Validation 제거
- 최소 다운로드 지원
- CI/CD 최적화

---

# 43. Incremental Build Overview

Incremental Build는 이전 Build 결과와 현재 Source를 비교하여 변경된 Resource만 다시 생성한다.

```
Previous Build

↓

Compare

↓

Changed Resources

↓

Partial Build

↓

Update Output
```

변경되지 않은 Resource는 재생성하지 않는다.

---

# 44. Change Detection

Builder는 Build 시작 전에 변경 여부를 확인한다.

비교 대상

- Raw Source
- Metadata
- Manifest
- Registry
- Locale
- Grammar

다음 항목 중 하나라도 변경되면 해당 Resource는 Dirty 상태가 된다.

---

# 45. Dirty State

모든 Resource는 Build 상태를 가진다.

| State | Description |
|--------|-------------|
| Clean | 변경 없음 |
| Dirty | 변경됨 |
| Generated | 생성 완료 |
| Failed | 생성 실패 |
| Skipped | 생성 생략 |

Dirty Resource만 Build 대상이 된다.

---

# 46. Hash Calculation

변경 여부는 Hash를 기반으로 판단한다.

지원 알고리즘

- SHA-256
- SHA-512

Builder는 동일한 Hash를 가진 Resource를 다시 생성하지 않는다.

---

# 47. Resource Fingerprint

모든 Resource는 Fingerprint를 가진다.

예시

```json
{
  "id": "effect.spawn",
  "hash": "sha256:xxxxxxxx",
  "version": "1.2.0"
}
```

Fingerprint는 Cache 검증에 사용된다.

---

# 48. Dependency Rebuild

Resource가 변경되면 의존하는 Resource도 재생성한다.

예시

```
Type

↓

Grammar

↓

Registry

↓

Manifest
```

Dependency Graph를 이용하여 영향을 계산한다.

---

# 49. Build Graph

Builder는 Build Graph를 생성한다.

```
Raw

↓

Grammar

↓

Registry

↓

Manifest

↓

DataPack
```

Build Graph는 Build 순서를 결정한다.

---

# 50. Topological Build

Build Graph는 Topological Sort를 사용한다.

```
Skript

↓

SkBee

↓

Project
```

순환 Build는 허용되지 않는다.

---

# 51. Cache System

Builder는 Cache를 사용한다.

Cache 저장 대상

- Parsed Objects
- Intermediate Representation
- Grammar
- Registry
- Manifest
- Dependency Graph

---

# 52. Cache Structure

예시

```
build/

cache/

├── parser/

├── ir/

├── registry/

├── manifest/

├── hash/

└── reports/
```

Cache는 Builder 전용이다.

---

# 53. Cache Validation

Cache는 다음 조건을 만족해야 한다.

- Hash 일치
- Schema Version 일치
- Builder Version 일치
- Database Version 일치

하나라도 다르면 Cache를 폐기한다.

---

# 54. Cache Invalidation

다음 경우 Cache를 제거한다.

- Clean Build
- Schema 변경
- Builder Version 변경
- Database Version 변경
- Cache 손상

---

# 55. Partial Validation

Incremental Build에서는 변경된 Resource만 Validation한다.

예시

```
Grammar Changed

↓

Grammar Validation

↓

Registry Validation
```

전체 Validation은 수행하지 않는다.

---

# 56. Parallel Build

Builder는 독립적인 Resource를 병렬 생성할 수 있다.

예시

```
Grammar

Locale

Documentation
```

↓

동시에 생성

Dependency가 존재하는 Resource는 병렬 처리하지 않는다.

---

# 57. Worker Model

Builder는 Worker 기반 병렬 처리를 지원할 수 있다.

```
Master

├── Worker 1

├── Worker 2

├── Worker 3

└── Worker N
```

각 Worker는 독립적인 Build Context를 사용한다.

---

# 58. Memory Management

Builder는 사용하지 않는 데이터를 즉시 해제해야 한다.

우선 해제 대상

- Parsed Objects
- Temporary Buffers
- Intermediate Collections

Memory Leak는 허용되지 않는다.

---

# 59. Performance Targets

권장 목표

| Metric | Target |
|---------|--------|
| Full Build | ≤ 60초 |
| Incremental Build | ≤ 5초 |
| Cache Hit | ≥ 90% |
| Memory Usage | ≤ 1GB |

구현 환경에 따라 조정할 수 있다.

---

# 60. Build Optimization

Builder는 다음 최적화를 수행할 수 있다.

- Lazy Loading
- Streaming Parser
- Parallel Processing
- Resource Pooling
- Incremental Validation
- Cached Serialization

---

# 61. Failure Recovery

Build 실패 시 Builder는 마지막 정상 상태를 유지해야 한다.

절차

```
Failure

↓

Restore Cache

↓

Restore Registry

↓

Abort Build
```

손상된 Output은 배포해서는 안 된다.

---

# 62. Rollback

Build 중 오류가 발생하면 이전 Build 결과를 유지한다.

Rollback 대상

- Registry
- Manifest
- DataPack
- Generated Files

Rollback은 Atomic하게 수행되어야 한다.

---

# 63. Metrics

Builder는 다음 지표를 수집한다.

- Build Duration
- Cache Hit Ratio
- Files Processed
- Resources Generated
- Resources Skipped
- Validation Count
- Error Count

Metrics는 Build Report에 포함된다.

---

# 64. Build Report Extension

Build Report는 Incremental 정보를 포함한다.

예시

```json
{
  "incremental": true,
  "processed": 8,
  "generated": 3,
  "skipped": 127,
  "cacheHit": 96.8
}
```

---

# 65. CI/CD Integration

CI에서는 Incremental Build를 사용할 수 있다.

권장 절차

```
Checkout

↓

Restore Cache

↓

Incremental Build

↓

Validation

↓

Package

↓

Release
```

Cache가 없을 경우 Full Build를 수행한다.

---

# 66. Performance Monitoring

Builder는 성능 측정을 지원해야 한다.

측정 항목

- CPU Time
- Wall Time
- Memory Usage
- IO Time
- Cache Efficiency

---

# 67. Summary

Builder는 Incremental Build를 기본 전략으로 사용하며, 변경된 Resource만 재생성해야 한다.

Hash 기반 변경 감지, Dependency Graph, Cache 및 Topological Build를 통해 Build 시간을 최소화하고 안정적인 DataPack을 생성해야 한다.

---

# 68. Purpose

본 문서는 VSkript Builder가 다른 구성 요소와 연동되는 방식과 CLI 인터페이스, Release Pipeline 및 운영 환경에서의 동작을 정의한다.

Builder는 단독으로 동작하는 프로그램이 아니라 VSkript Database 생태계 전체를 연결하는 중심 컴포넌트이다.

---

# 69. Integration Overview

Builder는 다음 구성 요소와 연동된다.

```
Plugin Detector

↓

Source Importer

↓

Builder

↓

Validator

↓

Generator

↓

Registry

↓

DataPack

↓

VSCode Extension

↓

Language Server
```

모든 통신은 표준 데이터 구조를 사용한다.

---

# 70. Plugin Detection Integration

Builder는 Plugin Detection 결과를 입력으로 사용할 수 있다.

입력 예시

```json
{
    "plugin": "SkBee",
    "version": "3.12.2"
}
```

Builder는 해당 Plugin에 필요한 Resource만 생성할 수 있다.

---

# 71. Source Import Integration

Builder는 Source Importer와 연동하여 외부 데이터를 수집한다.

지원 대상

- SkriptHub
- Git Repository
- Local Source
- Release Package

Import 단계는 Builder 외부에서 수행된다.

---

# 72. Validator Integration

Builder는 Resource 생성 후 Validator를 호출한다.

```
Builder

↓

Validator

↓

Result
```

Validation 실패 시 Build는 실패 상태가 된다.

---

# 73. Generator Integration

Generator는 Builder가 생성한 데이터를 기반으로 최종 DataPack을 생성한다.

```
Builder Output

↓

Generator

↓

DataPack
```

Builder는 Generator 내부 로직을 포함하지 않는다.

---

# 74. Registry Integration

Builder는 Registry를 생성하고 갱신한다.

Registry는 다음 기능을 수행한다.

- Resource Index
- Dependency Lookup
- Version Lookup
- DataPack Lookup

Registry는 Build 완료 후 갱신된다.

---

# 75. Update Protocol Integration

Builder는 Update Protocol과 연동된다.

Build 완료 후

```
Build

↓

Registry Update

↓

Manifest Update

↓

Publish
```

Update Protocol은 RFC-0005를 따른다.

---

# 76. CLI Overview

Builder는 Command Line Interface를 제공해야 한다.

기본 명령

```
vskript-builder
```

CLI는 자동화 환경에서도 동일하게 동작해야 한다.

---

# 77. CLI Commands

지원 명령

```
build

clean

validate

generate

update

package

publish

version

help
```

각 명령은 독립적으로 실행 가능해야 한다.

---

# 78. Build Command

전체 Build

```
vskript-builder build
```

Incremental Build

```
vskript-builder build --incremental
```

Clean Build

```
vskript-builder build --clean
```

---

# 79. Validation Command

Validation만 수행

```
vskript-builder validate
```

Build 결과는 생성하지 않는다.

---

# 80. Package Command

DataPack 생성

```
vskript-builder package
```

출력

```
packs/
```

---

# 81. Configuration

Builder는 Configuration File을 사용할 수 있다.

예시

```
builder.config.json
```

설정 항목

- Build Mode
- Output Directory
- Cache Directory
- Worker Count
- Locale
- Log Level

---

# 82. Environment Variables

환경 변수 사용 가능

예시

```
VSKRIPT_BUILD_MODE

VSKRIPT_CACHE

VSKRIPT_OUTPUT

VSKRIPT_LOG_LEVEL
```

CLI 옵션이 환경 변수보다 우선한다.

---

# 83. Output Structure

Builder 출력

```
generated/

registry/

packs/

reports/

logs/
```

Output 구조는 SPEC-0012를 따른다.

---

# 84. Release Pipeline

권장 Release 순서

```
Import Source

↓

Build

↓

Validate

↓

Generate

↓

Package

↓

Publish

↓

Release
```

모든 단계가 성공해야 Release가 가능하다.

---

# 85. CI/CD Integration

Builder는 CI/CD에서 다음 절차를 지원한다.

```
Checkout

↓

Restore Cache

↓

Build

↓

Validate

↓

Test

↓

Package

↓

Publish Release
```

자동화 환경에서도 동일한 결과를 생성해야 한다.

---

# 86. Logging Levels

지원 로그 레벨

| Level | Description |
|---------|-------------|
| Trace | 상세 추적 |
| Debug | 디버그 |
| Info | 일반 정보 |
| Warning | 경고 |
| Error | 오류 |
| Fatal | 치명적 오류 |

기본 레벨은 `Info`이다.

---

# 87. Build Events

Builder는 이벤트를 발생시킬 수 있다.

예시

```
BuildStarted

BuildCompleted

BuildFailed

ResourceGenerated

ValidationStarted

ValidationCompleted

PackageCreated
```

이벤트는 외부 시스템에서 사용할 수 있다.

---

# 88. Security Considerations

Builder는 다음 보안 원칙을 따른다.

- Source Validation
- Path Traversal 방지
- 임의 코드 실행 금지
- JSON Schema Validation
- Checksum Verification
- 안전한 File Write

신뢰되지 않은 입력은 항상 검증해야 한다.

---

# 89. Extensibility

Builder는 향후 확장을 고려하여 설계한다.

지원 가능한 확장

- 새로운 Parser
- 새로운 Generator
- 새로운 Validator
- 새로운 DataPack Format
- 새로운 Build Step
- 새로운 Source Provider

기존 Build Pipeline과의 호환성을 유지해야 한다.

---

# 90. Future Compatibility

Builder는 다음 변경을 수용할 수 있어야 한다.

- Schema Version 증가
- 신규 Resource 추가
- 신규 Locale 추가
- 신규 Plugin 유형 추가
- 신규 Build Mode 추가

기존 프로젝트의 Build를 손상시키지 않아야 한다.

---

# 91. Operational Guidelines

운영 환경에서는 다음 사항을 권장한다.

- Incremental Build 사용
- Cache 활성화
- 병렬 Build 활성화
- Build Report 보관
- Release 전 Validation 수행

---

# 92. Summary

Builder는 VSkript Database의 핵심 Build Engine이며, Repository의 Source를 표준 Resource로 변환하고 DataPack을 생성하는 역할을 수행한다.

Builder는 Validator, Generator, Registry, Update Protocol 및 VSCode Extension과 긴밀하게 연동되며, CLI와 CI/CD 환경에서도 동일한 Build 결과를 생성해야 한다.

Incremental Build와 표준화된 Build Pipeline을 통해 효율적이고 재현 가능한 빌드 환경을 제공한다.

---

# Related Specifications

- SPEC-0005_ERROR_CODES
- SPEC-0007_TYPE_SYSTEM
- SPEC-0008_JSON_SCHEMA_GUIDE
- SPEC-0009_VERSIONING
- SPEC-0010_DEPENDENCY_RESOLUTION
- SPEC-0011_DATAPACK_LAYOUT

---

# Related RFC

- RFC-0001_MANIFEST_SCHEMA
- RFC-0002_REGISTRY_SCHEMA
- RFC-0003_GRAMMAR_SCHEMA
- RFC-0008_VALIDATOR_SPECIFICATION
- RFC-0009_GENERATOR_SPECIFICATION

---

# Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0007 |
| Part | 2 / 4 |
| Title | Build Process & Resource Generation |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |