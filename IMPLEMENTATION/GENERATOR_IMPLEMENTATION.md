# GENERATOR_IMPLEMENTATION.md

Version: 1.0

Status: Draft

Related Documents

- RFC-0009 Generator Specification
- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- ADR-0004 Registry as Single Source of Truth
- ADR-0007 Build Context
- SPEC-0008 JSON Schema Guide
- SPEC-0010 Dependency Resolution

---

# Part 1. Generator Architecture

---

# 1. Introduction

Generator는 Builder가 생성한 BuildContext와 Validator의 ValidationResult를 기반으로 최종 산출물(Artifacts)을 생성하는 컴포넌트이다.

Generator는 Repository를 분석하지 않는다.

모든 입력은 BuildContext를 통해 전달된다.

Generator의 주요 역할은 다음과 같다.

- Registry 생성
- Documentation 생성
- Package 생성
- Release Artifact 생성
- Type Definition 생성
- Manifest 생성
- Schema 생성

Generator는 BuildContext를 변경하지 않는다.

---

# 2. Responsibilities

Generator는 다음 책임을 가진다.

- BuildContext 읽기
- ValidationResult 확인
- Artifact 생성
- Output 구조 생성
- Package 작성
- Release Metadata 생성
- Statistics 생성

Generator는 다음 작업을 수행하지 않는다.

- Repository 탐색
- Validation 수행
- Dependency 분석
- Registry 수정

---

# 3. Goals

Generator는 다음 목표를 가진다.

- 결정적(Deterministic) 출력
- 빠른 생성 속도
- Incremental Generation
- 병렬 생성
- 확장 가능한 Renderer
- 다양한 Output Format 지원

---

# 4. Non Goals

Generator는 다음 기능을 포함하지 않는다.

- Validation
- Source Formatting
- Runtime 실행
- IDE 기능
- Repository 변경

---

# 5. Generator Principles

Generator는 다음 원칙을 따른다.

## Read Only

BuildContext는 수정하지 않는다.

---

## Deterministic

동일한 입력은 항상 동일한 결과를 생성해야 한다.

---

## Stateless

Generator는 내부 상태를 유지하지 않는다.

---

## Extensible

새로운 Artifact는 Plugin 형태로 추가할 수 있다.

---

## Reproducible

동일 버전에서는 항상 동일한 Release를 생성해야 한다.

---

# 6. Generator Architecture

```
            BuildContext
                  │
                  ▼
          Generation Planner
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
 Artifact Workers      Package Workers
        │                   │
        └─────────┬─────────┘
                  ▼
         Artifact Collector
                  ▼
          Output Pipeline
                  ▼
          Package Writer
                  ▼
             Final Output
```

Generator는 Planner 기반 Pipeline으로 동작한다.

---

# 7. Artifact Pipeline

모든 Artifact는 동일한 Pipeline을 따른다.

```
BuildContext

↓

Planner

↓

Renderer

↓

Artifact

↓

Collector

↓

Writer
```

Artifact 생성과 파일 기록은 분리되어야 한다.

---

# 8. Generator Context

Generator는 다음 Context를 사용한다.

```text
GeneratorContext

├── BuildContext
├── ValidationResult
├── Configuration
├── OutputDirectory
├── ArtifactCache
└── Metrics
```

Context는 Generator 실행 중 Immutable이다.

---

# 9. Generation Lifecycle

```
Initialize

↓

Plan

↓

Generate

↓

Collect

↓

Write

↓

Complete
```

각 단계는 독립적으로 테스트 가능해야 한다.

---

# 10. Generator Categories

Generator는 다음 Category를 지원한다.

- Registry
- Documentation
- Package
- Metadata
- Schema
- Locale
- Manifest
- Bundle
- Report

각 Category는 독립적으로 실행할 수 있다.

---

# 11. Output Model

모든 출력은 Artifact로 표현한다.

```typescript
interface Artifact {

    id: string;

    category: string;

    format: string;

    content: unknown;

}
```

Artifact는 메모리에서 생성된 후 Writer에 전달된다.

---

# 12. Artifact Model

Artifact는 파일과 동일하지 않다.

예시

```
Registry Artifact

↓

registry.json
```

```
Documentation Artifact

↓

README.md
```

```
Type Definition Artifact

↓

registry.d.ts
```

하나의 Artifact가 여러 파일을 생성할 수도 있다.

---

# 13. Generator Configuration

예시

```yaml
output: dist

generateDocs: true

generateSchemas: true

generateTypes: true

compress: true

parallel: true
```

Configuration은 실행 중 변경되지 않는다.

---

# 14. Metrics

Generator는 다음 정보를 수집한다.

- Generation Time
- Artifact Count
- Output Size
- Cache Hit Ratio
- Files Generated

Metrics는 Report와 Telemetry에 활용된다.

---

# 15. Part Summary

Part 1에서는 Generator의 전체 구조와 Artifact Pipeline을 정의하였다.

Generator는 BuildContext를 입력으로 받아 다양한 Artifact를 생성하며, Planner–Renderer–Collector–Writer 구조를 통해 결정적이고 확장 가능한 출력 시스템을 제공한다.

Part 2에서는 다음 내용을 다룬다.

- Registry Generation
- Manifest Generation
- Documentation Generation
- Bundle Generation
- Type Definition Generation
- Schema Generation
- Package Generation

---

# Part 2. Artifact Generation

---

# 16. Overview

Artifact Generation은 Generator의 핵심 단계이다.

Builder가 생성한 BuildContext를 기반으로
모든 최종 산출물(Artifact)을 생성한다.

Generator는 Resource를 직접 읽지 않는다.

모든 데이터는 BuildContext를 통해 전달된다.

Artifact 생성 순서는 다음과 같다.

```
BuildContext

↓

Artifact Planner

↓

Artifact Renderer

↓

Artifact Collector

↓

Output Writer
```

---

# 17. Artifact Planner

Planner는 생성해야 할 Artifact 목록을 계산한다.

예시

```
Registry

Documentation

Schema

Type Definitions

Packages

Reports
```

Planner는 Configuration과 BuildContext를 함께 고려한다.

---

# 18. Registry Generation

Registry Generator는 BuildContext를 Registry Artifact로 변환한다.

입력

```
BuildContext
```

출력

```
registry.json
```

생성 항목

- Resources
- Plugins
- Types
- Expressions
- Events
- Conditions
- Effects

Registry는 항상 결정적 순서로 생성한다.

---

# 19. Manifest Generation

Manifest Generator는 Release Manifest를 생성한다.

출력 예시

```
manifest.json
```

포함 정보

- Version
- Build Number
- Schema Version
- Plugins
- Checksums

Manifest는 Release의 진입점이다.

---

# 20. Metadata Generation

Metadata Generator는 프로젝트 메타데이터를 생성한다.

예시

```
metadata.json
```

포함 정보

- Authors
- License
- Homepage
- Repository
- Generated Time
- Generator Version

---

# 21. Documentation Generation

Documentation Generator는 Markdown 문서를 생성한다.

예시

```
README.md

Effects.md

Events.md

Types.md
```

Documentation은 BuildContext만을 사용한다.

Source Markdown을 직접 복사하지 않는다.

---

# 22. Locale Generation

Locale Generator는 번역 리소스를 생성한다.

예시

```
en_us.json

ko_kr.json

ja_jp.json
```

누락된 Locale은 Warning으로 보고할 수 있다.

---

# 23. Grammar Generation

Grammar Generator는 Grammar Artifact를 생성한다.

출력 예시

```
grammar.json
```

포함 정보

- Syntax
- Parameters
- Return Types
- Documentation Links

Grammar는 Registry와 일치해야 한다.

---

# 24. Plugin Package Generation

Plugin Generator는 Plugin 단위 Package를 생성한다.

예시

```
plugins/

skript/

skbee/

skript-reflect/
```

각 Plugin은 독립적으로 배포 가능해야 한다.

---

# 25. Bundle Generation

Bundle Generator는 여러 Artifact를 하나의 Bundle로 결합한다.

예시

```
bundle.json
```

Bundle은 Package 생성의 입력이 된다.

---

# 26. Asset Generation

Asset Generator는 정적 자산을 생성한다.

예시

```
icons/

images/

templates/

css/
```

Asset은 Generator가 직접 생성하거나 복사할 수 있다.

---

# 27. Schema Generation

Schema Generator는 JSON Schema를 생성한다.

예시

```
grammar.schema.json

manifest.schema.json

plugin.schema.json
```

Schema는 SPEC와 일치해야 한다.

---

# 28. Type Definition Generation

Type Generator는 TypeScript Definition을 생성한다.

예시

```
registry.d.ts

diagnostic.d.ts

plugin.d.ts
```

생성된 타입은 Language Server에서도 사용한다.

---

# 29. Index Generation

Index Generator는 빠른 조회를 위한 Index를 생성한다.

예시

```
index.json
```

Index는 다음을 포함할 수 있다.

- Namespace Index
- Plugin Index
- Resource Index
- Symbol Index

---

# 30. Resource Packaging

Artifact를 Package 단위로 구성한다.

예시

```
Registry

↓

Documentation

↓

Schemas

↓

Types

↓

Bundle

↓

Package
```

Package는 Release Generator의 입력이 된다.

---

# 31. Compression

Package는 압축될 수 있다.

지원 예시

- ZIP
- TAR.GZ

압축은 선택 사항이다.

---

# 32. Artifact Dependency

Artifact도 의존성을 가진다.

예시

```
Registry

↓

Documentation

↓

Package
```

Planner는 Artifact Dependency를 고려해야 한다.

---

# 33. Artifact Identity

모든 Artifact는 고유한 ID를 가진다.

예시

```
registry

documentation

types

schema

manifest
```

동일한 ID는 허용되지 않는다.

---

# 34. Artifact Registry

생성된 Artifact는 Registry에 등록된다.

```
Artifact Registry

↓

Lookup

↓

Collector

↓

Writer
```

Artifact Registry는 메모리에서만 유지된다.

---

# 35. Renderer

Renderer는 Artifact를 내부 표현으로 생성한다.

예시

```
Markdown Renderer

↓

Markdown Artifact
```

```
TypeScript Renderer

↓

Type Artifact
```

Renderer는 파일을 직접 작성하지 않는다.

---

# 36. Artifact Collector

Collector는 생성된 Artifact를 수집한다.

```
Artifact

↓

Collector

↓

Output Queue
```

Collector는 생성 순서를 보존한다.

---

# 37. Writer

Writer는 Artifact를 파일로 기록한다.

```
Artifact

↓

Writer

↓

Filesystem
```

Writer는 유일하게 파일 시스템에 접근하는 컴포넌트이다.

---

# 38. Complexity Analysis

권장 복잡도

| Operation | Complexity |
|-----------|-----------:|
| Artifact Planning | O(n) |
| Registry Generation | O(n) |
| Documentation Generation | O(n) |
| Type Generation | O(n) |
| Writer | O(n) |

---

# 39. Testing Matrix

다음 항목을 반드시 테스트한다.

- Registry Generation
- Manifest Generation
- Documentation Generation
- Schema Generation
- Type Generation
- Plugin Package
- Bundle Generation
- Writer
- Artifact Dependency

---

# 40. Part Summary

Part 2에서는 Generator가 생성하는 모든 Artifact와 Artifact Pipeline을 정의하였다.

Generator는 BuildContext를 기반으로 Registry, Documentation, Schema, Type Definitions, Package 등 다양한 산출물을 생성하며, Planner–Renderer–Collector–Writer 구조를 통해 Artifact를 관리한다.

Part 3에서는 다음 내용을 다룬다.

- Package Layout
- Release Structure
- Archive Generation
- Checksums
- Manifest Signing
- Publishing
- CI/CD Integration

---

# Part 3. Packaging & Distribution

---

# 41. Overview

Artifact 생성이 완료되면 Generator는 이를 배포 가능한 Package로 구성한다.

Packaging 단계의 목적은 다음과 같다.

- Artifact 정리
- Release 구성
- Archive 생성
- Manifest 생성
- Checksum 생성
- Publishing 준비

Packaging은 Artifact를 변경하지 않는다.

---

# 42. Package Layout

권장 출력 구조

```
dist/

    registry.json

    manifest.json

    artifact-manifest.json

    schemas/

    docs/

    types/

    locales/

    assets/

    reports/

    packages/

    release.zip
```

모든 출력은 결정적인 디렉터리 구조를 가진다.

---

# 43. Output Directory

Generator는 단일 Root Output Directory를 사용한다.

예시

```
output/

dist/

release/
```

Output Directory는 실행 전에 비어있는 것을 권장한다.

Incremental Generation에서는 기존 Output을 재사용할 수 있다.

---

# 44. Release Structure

Release는 다음 계층으로 구성된다.

```
Release

├── Metadata

├── Artifacts

├── Documentation

├── Schemas

├── Type Definitions

├── Packages

└── Reports
```

Release는 하나의 논리적 단위이다.

---

# 45. Artifact Graph

Artifact는 독립적으로 생성되지 않는다.

생성 순서는 Graph를 따른다.

예시

```
Registry

↓

Documentation

↓

Bundle

↓

Package

↓

Release
```

Artifact Graph는 DAG(Directed Acyclic Graph)를 유지해야 한다.

순환 의존성은 허용하지 않는다.

---

# 46. Artifact Registry

생성된 모든 Artifact는 Registry에 등록된다.

Registry는 다음 정보를 가진다.

- Artifact ID
- Category
- Format
- Path
- Dependencies
- Status

Registry는 Writer의 입력으로 사용된다.

---

# 47. Artifact Manifest

Generator는 최종적으로 Artifact Manifest를 생성한다.

예시

```
artifact-manifest.json
```

Manifest는 생성된 모든 Artifact를 기술한다.

포함 정보

- Artifact ID
- File Path
- Size
- SHA-256 Checksum
- Category
- Generator Version
- Generated Time

Artifact Manifest는 Release 검증의 기준 문서이다.

---

# 48. Artifact Identity

모든 Artifact는 결정적인(Deterministic) ID를 가진다.

예시

```
registry

documentation

types

schemas

bundle

release
```

동일 입력에서는 항상 동일한 Artifact ID를 생성해야 한다.

---

# 49. Artifact Lifecycle

Artifact는 다음 생명주기를 따른다.

```
Planned

↓

Generated

↓

Registered

↓

Written

↓

Packaged

↓

Released
```

각 단계는 명확히 구분되어야 한다.

---

# 50. Package Manifest

Package마다 Manifest를 생성할 수 있다.

예시

```
package.json

plugin-manifest.json
```

Package Manifest는 Package 내부 정보를 제공한다.

---

# 51. Archive Generation

Release는 Archive 형태로 생성할 수 있다.

지원 예시

- ZIP
- TAR.GZ

Archive는 선택 기능이다.

---

# 52. Compression Strategy

압축 시 다음 원칙을 따른다.

- 무손실 압축
- 결정적 압축 순서
- 동일 입력 → 동일 Archive

압축 과정은 Metadata를 변경하지 않는다.

---

# 53. Checksum Generation

모든 Release Artifact는 Checksum을 생성한다.

권장 알고리즘

```
SHA-256
```

Checksum은 Artifact Manifest에도 기록한다.

---

# 54. Manifest Signing

선택적으로 Manifest에 전자 서명을 추가할 수 있다.

예시

```
manifest.sig
```

서명은 Release 무결성 검증에 사용된다.

---

# 55. Version Metadata

Release에는 Version Metadata를 포함한다.

예시

```
Version

Schema Version

Generator Version

Build Version

Commit Hash
```

Version 정보는 Manifest와 일치해야 한다.

---

# 56. Compatibility Metadata

Release는 호환성 정보를 제공한다.

예시

```
Supported Schema

Supported Builder

Supported Validator

Minimum Generator
```

호환성은 Semantic Versioning을 따른다.

---

# 57. Publishing Targets

Generator는 다양한 Publishing Target을 지원할 수 있다.

예시

- Local Directory
- GitHub Release
- Package Registry
- CDN
- Object Storage

Publishing은 Generator의 선택 기능이다.

---

# 58. CI/CD Integration

Generator는 CI 환경에서 동작할 수 있어야 한다.

권장 Pipeline

```
Build

↓

Validate

↓

Generate

↓

Package

↓

Release

↓

Publish
```

CI는 Strict Validation 이후에만 Generator를 실행한다.

---

# 59. Release Validation

Release 전 다음 항목을 검증한다.

- Manifest 존재
- Artifact Manifest 존재
- Checksum 생성
- Required Artifact 존재
- Package 구조
- Version 정보

검증 실패 시 Release를 중단한다.

---

# 60. Distribution Strategy

Release는 변경되지 않는(Immutable) Artifact로 취급한다.

Release 이후 Artifact 수정은 허용하지 않는다.

새로운 Release는 새로운 Version으로 생성한다.

---

# 61. Part Summary

Part 3에서는 Generator의 Packaging과 Distribution 구조를 정의하였다.

Generator는 Artifact Graph를 기반으로 Package를 구성하며, Artifact Registry와 Artifact Manifest를 통해 Release 전체를 추적한다.

Packaging은 생성과 기록을 분리하며, 결정적 출력과 재현 가능한 Release 생성을 목표로 한다.

Part 4에서는 다음 내용을 다룬다.

- Parallel Generation
- Incremental Generation
- Artifact Cache
- Scheduler
- Streaming
- Recovery
- Performance Benchmark

---

# Part 4. Performance & Parallel Generation

---

# 62. Overview

Generator는 대규모 Repository에서도 안정적인 성능을 제공해야 한다.

이를 위해 다음 기능을 지원한다.

- Parallel Generation
- Incremental Generation
- Artifact Cache
- Dependency Scheduling
- Memory Optimization
- Streaming Output

Generator의 성능 최적화는 결과물의 결정성(Determinism)을 훼손해서는 안 된다.

---

# 63. Generation Scheduler

Generation Scheduler는 Artifact 실행 순서를 결정한다.

입력

- Artifact Graph
- Configuration
- Worker Count

출력

- Execution Plan

Scheduler는 Artifact Graph를 기반으로 DAG 순서를 계산한다.

---

# 64. Worker Pool

Generator는 Worker Pool을 사용할 수 있다.

```
Planner

↓

Scheduler

↓

Worker Pool

↓

Collector
```

각 Worker는 서로 독립적인 Artifact만 생성한다.

---

# 65. Parallel Generation

의존성이 없는 Artifact는 병렬 생성할 수 있다.

예시

```
Registry
            \
             +--> Documentation
            /
Types
```

Registry와 Types가 완료되면 Documentation이 생성된다.

병렬 실행은 Artifact Graph를 위반해서는 안 된다.

---

# 66. Dependency Scheduling

Scheduler는 Graph의 위상 정렬(Topological Ordering)을 사용한다.

예시

```
Registry

↓

Schemas

↓

Documentation

↓

Package
```

모든 선행 Artifact가 완료된 후에만 다음 Artifact를 생성한다.

---

# 67. Incremental Generation

변경되지 않은 Artifact는 다시 생성하지 않는다.

Incremental Generation은 다음 정보를 사용한다.

- Input Hash
- Dependency Hash
- Configuration Hash
- Generator Version

모든 조건이 동일하면 기존 Artifact를 재사용한다.

---

# 68. Dirty Detection

Artifact는 다음 경우 Dirty 상태가 된다.

- BuildContext 변경
- ValidationResult 변경
- Configuration 변경
- Generator Version 변경
- Dependency Artifact 변경

Dirty Artifact만 다시 생성한다.

---

# 69. Artifact Cache

Generator는 Artifact Cache를 유지할 수 있다.

Cache 대상

- Rendered Markdown
- JSON Output
- Type Definitions
- Schema Output
- Package Metadata

Cache는 Artifact ID를 기준으로 관리한다.

---

# 70. Cache Invalidation

다음 경우 Cache를 무효화한다.

- Artifact 변경
- Generator 업데이트
- Schema 변경
- Renderer 변경
- Configuration 변경

가능한 한 부분 무효화(Partial Invalidation)를 사용한다.

---

# 71. Streaming Generation

대용량 Artifact는 Streaming 방식으로 생성할 수 있다.

예시

```
Renderer

↓

Stream

↓

Writer
```

전체 내용을 메모리에 유지하지 않는 것을 권장한다.

---

# 72. Memory Strategy

Generator는 불필요한 메모리 사용을 최소화한다.

권장 사항

- Immutable 객체 사용
- Streaming 우선
- Copy 최소화
- Shared Registry 활용

Artifact 생성 후 즉시 해제할 수 있어야 한다.

---

# 73. Progress Reporting

Generator는 진행 상황을 보고할 수 있다.

예시

```
Planning

Generating Registry

Generating Documentation

Packaging

Writing Output

Completed
```

Progress Reporting은 UI와 CI에서 사용할 수 있다.

---

# 74. Failure Recovery

Artifact 생성 실패 시 가능한 범위에서 계속 진행한다.

Recovery 가능한 오류

- Documentation 생성 실패
- Report 생성 실패

즉시 중단해야 하는 오류

- Registry 생성 실패
- Manifest 생성 실패
- Artifact Graph 손상

---

# 75. Retry Strategy

일시적인 오류는 재시도할 수 있다.

권장 정책

- 최대 3회
- Exponential Backoff
- 동일 입력 유지

논리 오류(Logical Error)는 재시도하지 않는다.

---

# 76. Logging

Generator는 구조화된 로그를 제공한다.

로그 레벨

- TRACE
- DEBUG
- INFO
- WARN
- ERROR

Artifact ID와 Generation Step을 함께 기록한다.

---

# 77. Metrics

수집 가능한 지표

- Generation Time
- Worker Utilization
- Cache Hit Ratio
- Artifact Count
- Output Size
- Retry Count

Metrics는 Telemetry 및 Benchmark에 활용된다.

---

# 78. Performance Targets

권장 목표

| Metric | Target |
|---------|--------:|
| 100 Artifacts | < 300 ms |
| 1,000 Artifacts | < 3 sec |
| Incremental Generation | < 200 ms |
| Cache Hit Ratio | > 90% |

목표치는 프로젝트 규모에 따라 조정할 수 있다.

---

# 79. Scalability

Generator는 다음 확장을 고려한다.

- Multi-Core
- Remote Workers
- Distributed Generation
- Cloud Build
- Containerized Build

현재 설계는 이러한 확장을 제한하지 않는다.

---

# 80. Part Summary

Part 4에서는 Generator의 성능 모델을 정의하였다.

Generator는 Artifact Graph 기반 Scheduler와 Worker Pool을 사용하여 병렬 생성과 Incremental Generation을 지원한다.

Artifact Cache와 Streaming Generation을 통해 대규모 프로젝트에서도 안정적인 성능을 제공한다.

Part 5에서는 다음 내용을 다룬다.

- Extension Architecture
- Generator Plugin
- Renderer
- Template Engine
- Testing
- Best Practices
- Document Summary

---

# Part 5. Extension, Testing & Compliance

---

# 81. Overview

Generator는 다양한 출력 형식과 배포 환경을 지원하기 위해 확장 가능한 구조를 제공해야 한다.

본 장에서는 다음 내용을 정의한다.

- Generator Extension
- Renderer Architecture
- Template Engine
- Testing Strategy
- Compliance
- Best Practices

---

# 82. Extension Architecture

Generator는 Plugin 기반 확장을 지원한다.

```
Generator

↓

Extension Manager

↓

Generator Extension

↓

Renderer

↓

Artifact
```

Core는 Extension의 존재를 알 필요가 없다.

---

# 83. Generator Extension Interface

권장 인터페이스

```typescript
interface GeneratorExtension {

    id(): string;

    version(): string;

    register(registry: GeneratorRegistry): void;

}
```

Extension은 새로운 Generator나 Renderer를 등록할 수 있다.

---

# 84. Custom Artifact Generator

Plugin은 새로운 Artifact Generator를 제공할 수 있다.

예시

```
Markdown Generator

↓

PDF Generator

↓

HTML Generator

↓

Custom Artifact
```

Generator Core는 모든 Generator를 동일하게 취급한다.

---

# 85. Renderer Architecture

Renderer는 내부 모델을 출력 형식으로 변환한다.

```
Artifact

↓

Renderer

↓

Output Model
```

Renderer는 파일을 직접 기록하지 않는다.

---

# 86. Built-in Renderers

기본 제공 Renderer

- JSON Renderer
- Markdown Renderer
- TypeScript Renderer
- YAML Renderer
- Plain Text Renderer

필요에 따라 추가 Renderer를 등록할 수 있다.

---

# 87. Template Engine

Renderer는 Template Engine을 사용할 수 있다.

예시

```
Artifact

↓

Template

↓

Rendered Output
```

Template은 Logic을 포함하지 않는 것을 원칙으로 한다.

---

# 88. Output Formats

Generator는 다양한 출력 형식을 지원한다.

예시

- JSON
- Markdown
- YAML
- TypeScript
- HTML
- XML

모든 형식은 동일한 Artifact Model을 사용한다.

---

# 89. Generator Registry

Generator Registry는 사용 가능한 Generator를 관리한다.

관리 항목

- Generator ID
- Category
- Version
- Supported Formats
- Dependencies

Registry는 실행 중 변경되지 않는다.

---

# 90. Generator Discovery

Generator는 자동 등록될 수 있다.

예시

```
Extensions

↓

Discovery

↓

Registry

↓

Planner
```

Discovery 순서는 결정적이어야 한다.

---

# 91. Testing Strategy

Generator는 다음 수준으로 테스트한다.

- Unit Test
- Integration Test
- Snapshot Test
- Regression Test
- Performance Test

각 Artifact Generator는 최소 하나 이상의 Unit Test를 가져야 한다.

---

# 92. Snapshot Testing

Markdown, JSON, TypeScript와 같은 출력은 Snapshot Test를 권장한다.

예시

```
Generated Output

↓

Snapshot

↓

Comparison
```

Snapshot 변경은 코드 리뷰를 거쳐야 한다.

---

# 93. Golden Files

Release 수준의 출력은 Golden File 기반으로 검증할 수 있다.

예시

```
Expected Output

↓

Generated Output

↓

Binary Comparison
```

Golden File은 결정적 출력만 포함한다.

---

# 94. Regression Testing

Generator 변경 시 전체 Artifact를 재생성하여 비교한다.

검증 항목

- 파일 수
- 내용
- Checksum
- Manifest
- Package 구조

Regression Suite는 CI에서 자동 실행한다.

---

# 95. Compliance

Generator는 다음 규칙을 준수해야 한다.

- Deterministic Output
- Immutable BuildContext
- Immutable Release
- Stable Artifact IDs
- Stable Directory Layout

Compliance 위반은 Generator Bug로 간주한다.

---

# 96. Best Practices

권장 사항

- Generator는 Stateless하게 구현한다.
- Renderer는 Pure Function으로 작성한다.
- Artifact를 직접 수정하지 않는다.
- Writer를 우회하지 않는다.
- BuildContext를 변경하지 않는다.
- Artifact Graph를 항상 유지한다.

---

# 97. Future Improvements

향후 고려 사항

- Distributed Generation
- Remote Renderer
- WASM Renderer
- Incremental Publishing
- AI-assisted Documentation Generation
- Multi-target Release Pipeline

현재 설계는 이러한 확장을 제한하지 않는다.

---

# 98. Generator Design Principles

Generator는 다음 원칙을 따른다.

- Plan before Generate
- Generate before Write
- Register before Release
- Never mutate Artifacts
- Always preserve determinism

모든 구현은 이 원칙을 준수해야 한다.

---

# 99. Document Summary

Generator는 BuildContext와 ValidationResult를 기반으로 Release Artifact를 생성하는 컴포넌트이다.

Generator는 다음 기능을 제공한다.

- Artifact Planning
- Artifact Graph
- Artifact Registry
- Artifact Manifest
- Rendering
- Packaging
- Distribution
- Release Generation

Generator는 Planner–Graph–Registry–Manifest–Writer Pipeline을 사용하며, 결정적이고 재현 가능한 Release를 생성하는 것을 목표로 한다.

---

# 100. Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Generator Implementation Guide |