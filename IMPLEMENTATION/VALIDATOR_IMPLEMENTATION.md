# VALIDATOR_IMPLEMENTATION.md

Version: 1.0

Status: Draft

Related Documents

- RFC-0008 Validator Specification
- ADR-0004 Registry as Single Source of Truth
- ADR-0008 Schema Versioning
- ADR-0009 Unified Caching Strategy
- SPEC-0008 JSON Schema Guide
- SPEC-0010 Dependency Resolution

---

# Part 1. Validation Architecture

---

# 1. Introduction

Validator는 Builder가 생성한 Build Context를 검증하여 Repository가 프로젝트 규칙을 만족하는지 확인하는 컴포넌트이다.

Validator는 단순한 JSON Schema 검사기가 아니다.

Validator는 다음을 수행한다.

- Schema Validation
- Structural Validation
- Semantic Validation
- Dependency Validation
- Registry Validation
- Rule Validation
- Plugin Validation
- Diagnostic Generation

Validator는 Repository를 수정하지 않는다.

모든 Validation은 읽기 전용(Read-Only)으로 수행된다.

---

# 2. Responsibilities

Validator의 주요 책임은 다음과 같다.

- Build Context 검증
- JSON Schema 검증
- Resource 무결성 확인
- Dependency 검사
- Semantic Rule 검사
- Plugin Rule 검사
- Registry 일관성 확인
- Diagnostic 생성
- Validation Report 생성

Validator는 다음 작업을 수행하지 않는다.

- Resource 수정
- Package 생성
- Registry 생성
- Build 수행
- Language Server 처리

---

# 3. Goals

Validator는 다음 목표를 가진다.

- 정확한 오류 탐지
- 낮은 False Positive
- 높은 성능
- Incremental Validation 지원
- Machine Readable Diagnostics
- IDE 친화적 출력
- CI/CD 자동화 지원

---

# 4. Non Goals

Validator의 범위에 포함되지 않는 작업은 다음과 같다.

- Source Code Formatting
- Auto Refactoring
- Runtime Behavior 분석
- Performance Profiling
- Package Generation

이러한 기능은 별도의 컴포넌트에서 수행한다.

---

# 5. Validation Principles

Validator는 다음 원칙을 따른다.

## Deterministic

동일 입력은 동일한 Diagnostic을 생성해야 한다.

---

## Read Only

Repository를 절대로 수정하지 않는다.

---

## Independent

Validation Rule은 서로 독립적으로 실행 가능해야 한다.

---

## Extensible

새로운 Rule은 기존 Validator 수정 없이 추가 가능해야 한다.

---

## Explainable

모든 Diagnostic은 원인과 해결 방법을 제공해야 한다.

---

# 6. Validation Architecture

```
            Build Context

                  │

        ┌─────────┴─────────┐

        ▼                   ▼

 Schema Validator    Rule Engine

        │                   │

        └─────────┬─────────┘

                  ▼

          Semantic Validator

                  │

                  ▼

        Dependency Validator

                  │

                  ▼

          Diagnostic Builder

                  │

                  ▼

          Validation Report
```

Validator는 계층적으로 실행된다.

---

# 7. Validation Pipeline

Validation은 다음 순서로 수행한다.

```
Load Context

↓

Schema Validation

↓

Structural Validation

↓

Dependency Validation

↓

Semantic Validation

↓

Plugin Validation

↓

Diagnostics

↓

Report
```

각 단계는 독립적으로 테스트 가능해야 한다.

---

# 8. Validation Context

Validator의 입력은 Build Context이다.

```text
ValidationContext

├── Registry
├── Manifest
├── Resources
├── Dependency Graph
├── Plugin List
├── Build Configuration
└── Diagnostics
```

Validator는 Repository를 다시 탐색하지 않는다.

---

# 9. Validation Levels

Validator는 네 가지 Validation Level을 지원한다.

### Level 1

Schema Validation

JSON 구조 검사

---

### Level 2

Structural Validation

Resource 구조 검사

---

### Level 3

Semantic Validation

의미 규칙 검사

---

### Level 4

Cross Resource Validation

Plugin 및 Registry 전체 검사

---

# 10. Validation Categories

Validation Rule은 다음 범주를 가진다.

- Schema
- Structure
- Resource
- Registry
- Dependency
- Namespace
- Plugin
- Semantic
- Documentation

Rule은 하나 이상의 Category에 속할 수 있다.

---

# 11. Diagnostic Model

모든 Validation 결과는 Diagnostic으로 표현한다.

```text
Diagnostic

id

severity

category

resource

location

message

suggestion

documentation
```

Diagnostic은 사람이 읽을 수 있고 기계도 처리할 수 있어야 한다.

---

# 12. Severity Model

Validator는 다음 Severity를 사용한다.

| Severity | 의미 |
|-----------|------|
| INFO | 참고 정보 |
| WARNING | 권장 수정 |
| ERROR | Validation 실패 |
| FATAL | Validation 즉시 중단 |

Severity는 Builder와 동일한 체계를 사용한다.

---

# 13. Error Codes

모든 Diagnostic은 Error Code를 가진다.

예시

```
VAL0001

Missing Manifest
```

```
VAL0124

Duplicate Namespace
```

```
VAL0320

Circular Dependency
```

Error Code는 프로젝트 전체에서 유일해야 한다.

---

# 14. Validation Configuration

예시

```yaml
schemaValidation: true

semanticValidation: true

pluginValidation: true

dependencyValidation: true

warningsAsErrors: false

parallel: true
```

Configuration은 Validation 시작 전에 고정된다.

---

# 15. Validation Lifecycle

```
Initialize

↓

Load Context

↓

Run Validation Rules

↓

Collect Diagnostics

↓

Generate Report

↓

Complete
```

Validation은 Build Context를 변경하지 않는다.

---

# Part 1 Summary

Part 1에서는 Validator의 전체 구조와 Validation Pipeline을 정의하였다.

Validator는 Builder가 생성한 Build Context를 입력으로 받아 Schema, Structural, Semantic 및 Dependency 검사를 수행하고 Diagnostic과 Validation Report를 생성한다.

Part 2에서는 다음 내용을 다룬다.

- Schema Loader
- Schema Registry
- JSON Validation
- Manifest Validation
- Grammar Validation
- Namespace Validation
- Duplicate Detection
- Validation Cache

---

# Part 2. Schema & Resource Validation

---

# 16. Overview

Schema & Resource Validation은 Validator의 첫 번째 실행 단계이다.

이 단계의 목적은 모든 Resource가 구조적으로 유효하며, 정의된 Schema 및 프로젝트 규칙을 만족하는지 확인하는 것이다.

Validation은 다음 순서로 수행한다.

```
Resource

↓

Schema Lookup

↓

JSON Validation

↓

Resource Validation

↓

Namespace Validation

↓

Identity Validation

↓

Diagnostics
```

Semantic Rule은 다음 단계에서 수행한다.

---

# 17. Schema Registry

Validator는 모든 Schema를 Schema Registry를 통해 조회한다.

```
Schema Registry

├── manifest.schema.json
├── grammar.schema.json
├── locale.schema.json
├── metadata.schema.json
├── plugin.schema.json
└── registry.schema.json
```

Schema Registry는 단일 조회 지점(Single Source of Truth)이다.

---

# 18. Schema Loader

Schema Loader는 Validation 시작 시 모든 Schema를 메모리에 로드한다.

```text
ISchemaLoader

load()

reload()

getSchema()

listSchemas()
```

Schema는 Immutable 객체로 관리한다.

---

# 19. Schema Resolution

각 Resource는 자신의 Schema를 가진다.

예시

```
effects/spawn.json

↓

grammar.schema.json
```

Schema 선택은 Resource Type으로 결정한다.

Directory 이름으로 판단하지 않는다.

---

# 20. Validation Flow

```
Raw JSON

↓

JSON Parser

↓

Schema Validation

↓

Internal Model

↓

Resource Validation

↓

Success
```

Schema Validation에 실패하면 이후 단계는 수행하지 않는다.

---

# 21. JSON Schema Validation

Validator는 JSON Schema Draft 2020-12를 기준으로 검증한다.

검사 항목

- Required Properties
- Type
- Enum
- Format
- Pattern
- Min/Max
- Array Constraints
- Object Constraints

프로젝트는 특정 Draft 버전을 명시적으로 지원해야 한다.

---

# 22. Manifest Validation

Manifest는 Repository의 진입점이다.

필수 검사 항목

- id
- name
- version
- schemaVersion
- plugins

추가 검사

- 중복 Plugin
- 잘못된 Version 형식
- 지원되지 않는 Schema Version

Manifest 오류는 FATAL로 처리한다.

---

# 23. Grammar Validation

Grammar Resource는 다음 항목을 검사한다.

- ID
- Namespace
- Syntax
- Return Type
- Parameters
- Examples
- Since Version

Grammar의 문법적 구조만 검사하며, 의미적 검사는 Part 3에서 수행한다.

---

# 24. Locale Validation

Locale Resource는 다음을 확인한다.

- Locale ID
- Language Tag
- Translation Key
- Duplicate Key
- Placeholder 형식

누락된 번역은 WARNING으로 처리할 수 있다.

---

# 25. Metadata Validation

Metadata는 프로젝트의 부가 정보를 정의한다.

검사 항목

- Author
- License
- Homepage
- Repository
- Tags

URL 및 SPDX License 형식도 검증 대상이다.

---

# 26. Documentation Validation

Documentation Resource는 다음을 검사한다.

- 제목 존재 여부
- 본문 존재 여부
- 참조(Resource ID) 유효성
- Markdown 링크 형식

깨진 내부 참조는 WARNING 또는 ERROR 정책에 따라 처리한다.

---

# 27. Plugin Validation

Plugin 정의는 다음을 검증한다.

- Plugin ID
- Name
- Version
- Dependency
- Entry Resource

Plugin ID는 Repository 전체에서 유일해야 한다.

---

# 28. Namespace Validation

Namespace는 충돌이 없어야 한다.

예시

```
skript.effect.teleport
```

검사 항목

- 형식
- 예약어 사용 여부
- 중복 Namespace
- 대소문자 정책

Namespace는 대소문자를 구분하지 않는 환경도 고려해야 한다.

---

# 29. Resource Identity

모든 Resource는 전역적으로 식별 가능해야 한다.

Resource Identity는 다음 요소로 구성된다.

```
Plugin

+

Namespace

+

Resource Type

+

Resource ID
```

동일 Identity는 허용되지 않는다.

---

# 30. Duplicate Detection

Validator는 중복을 검사한다.

대상

- Resource ID
- Namespace
- Plugin ID
- Locale Key
- Metadata Key

중복은 Registry 생성 전에 반드시 탐지되어야 한다.

---

# 31. Resource Constraints

Resource별 제약 조건을 적용한다.

예시

Grammar

- 최소 1개의 Syntax
- Return Type 필수

Locale

- Translation Key 필수

Manifest

- Plugin 목록 비어 있지 않음

---

# 32. Validation Rule Engine

Schema Validation 이후 Rule Engine이 실행된다.

```
Internal Model

↓

Rule Engine

↓

Rule Result

↓

Diagnostics
```

Rule은 코드가 아닌 독립된 단위로 관리하는 것을 권장한다.

---

# 33. Validation Rule Interface

권장 인터페이스

```text
IValidationRule<T>

id()

category()

severity()

validate(resource, context)
```

모든 Rule은 동일한 계약(Contract)을 따른다.

---

# 34. Rule Registration

Rule은 Registry에 등록된다.

```text
Rule Registry

↓

Schema Rules

↓

Resource Rules

↓

Plugin Rules

↓

Namespace Rules
```

Validator Core는 개별 Rule을 직접 호출하지 않는다.

---

# 35. Validation Cache

Schema Validation 결과는 Cache에 저장할 수 있다.

Cache 대상

- Parsed Schema
- Validation Result
- Rule Result

Cache Key

```
Resource Hash

+

Schema Version
```

---

# 36. Failure Policy

Schema Validation 실패 시

```
Resource

↓

Diagnostic

↓

Skip Remaining Rules
```

다음 Resource의 검사는 계속 수행한다.

---

# 37. Complexity Analysis

권장 시간 복잡도

| Operation | Complexity |
|-----------|-----------:|
| Schema Lookup | O(1) |
| Resource Validation | O(n) |
| Duplicate Lookup | O(1) 평균 |
| Rule Dispatch | O(r) |
| Registry Lookup | O(1) |

r은 적용되는 Rule 수이다.

---

# 38. Testing Matrix

다음 항목을 반드시 테스트한다.

- Invalid JSON
- Missing Required Field
- Invalid Enum
- Invalid Pattern
- Duplicate Namespace
- Duplicate Resource ID
- Invalid Plugin
- Missing Manifest
- Invalid Locale
- Invalid Metadata

---

# 39. Implementation Notes

- Schema는 시작 시 한 번만 로드한다.
- Rule은 Stateless하게 구현한다.
- Validation 순서는 결정적(Deterministic)이어야 한다.
- Rule 간 암묵적 의존성을 만들지 않는다.
- Duplicate 검사는 Hash Map 기반 구현을 권장한다.

---

# 40. Part 2 Summary

Part 2에서는 Schema Registry, Resource Validation, Namespace 검사 및 Rule Engine 구조를 정의하였다.

Validator는 JSON Schema와 프로젝트 규칙을 기반으로 모든 Resource의 구조적 유효성을 검증하며, Rule Engine을 통해 확장 가능한 Validation 체계를 제공한다.

Part 3에서는 다음 내용을 다룬다.

- Dependency Graph Validation
- Circular Dependency Detection
- Semantic Validation
- Symbol Resolution
- Type Validation
- Cross Plugin Validation
- Registry Consistency Validation

---

# Part 3. Dependency & Semantic Validation

---

# 41. Overview

Semantic Validation은 Resource가 단순히 올바른 구조를 가지는지를 넘어,
프로젝트 전체의 의미적(semantic) 일관성을 검증하는 단계이다.

이 단계에서는 다음을 검사한다.

- Dependency
- Symbol
- Type
- Registry
- Plugin 관계
- Version 제약
- Cross Resource 참조

Semantic Validation은 Build Context 전체를 대상으로 수행된다.

---

# 42. Semantic Analysis Pipeline

```
Build Context

↓

Dependency Validation

↓

Symbol Resolution

↓

Type Validation

↓

Registry Validation

↓

Plugin Validation

↓

Rule Composition

↓

Diagnostics
```

Schema Validation이 완료된 Resource만 Semantic Validation 대상이 된다.

---

# 43. Dependency Graph Validation

Validator는 Builder가 생성한 Dependency Graph를 검증한다.

검사 항목

- Missing Node
- Duplicate Edge
- Invalid Edge
- Self Dependency
- Orphan Node

Dependency Graph는 DAG(Directed Acyclic Graph)여야 한다.

---

# 44. Circular Dependency Detection

순환 의존성은 허용되지 않는다.

예시

```
Expression A

↓

Type B

↓

Effect C

↓

Expression A
```

순환이 발견되면 관련 Resource 모두 ERROR로 보고한다.

권장 알고리즘

- Tarjan SCC
- DFS Cycle Detection

---

# 45. Missing Dependency

Dependency가 존재하지 않으면 Validation에 실패한다.

예시

```json
{
  "returnType": "skript.type.player"
}
```

Registry에 `skript.type.player`가 없으면 ERROR를 생성한다.

---

# 46. Version Constraint Validation

Plugin Dependency는 Version Constraint를 만족해야 한다.

예시

```text
requires

Skript >=2.8.0
```

검사 항목

- 최소 버전
- 최대 버전
- 범위 지정
- Semantic Version 형식

---

# 47. Cross Plugin Validation

Plugin 간 참조는 Manifest에 선언된 Dependency를 통해서만 허용된다.

허용

```
Plugin A

↓

Plugin B

(Manifest Dependency 존재)
```

금지

```
Plugin A

↓

Plugin C

(Dependency 선언 없음)
```

---

# 48. Registry Consistency Validation

Registry와 Build Context는 항상 일치해야 한다.

검사 항목

- Registry Entry 존재 여부
- Registry 중복
- Registry 누락
- Resource ↔ Registry 매핑

Registry는 모든 Resource의 단일 참조 지점이어야 한다.

---

# 49. Symbol Resolution

Semantic Validation은 모든 Symbol을 해석한다.

예시

```
skript.type.player
```

↓

```
Registry Lookup
```

↓

```
Type Resource
```

해석 실패 시 Diagnostic을 생성한다.

---

# 50. Symbol Table

Validator는 Symbol Table을 사용한다.

```text
Symbol Table

Type

Expression

Condition

Effect

Event

Function
```

Symbol은 Namespace 기준으로 관리한다.

---

# 51. Type Validation

모든 Type 참조를 검증한다.

검사 항목

- Return Type
- Parameter Type
- Generic Type
- Array Type
- Nullable Type

존재하지 않는 Type은 ERROR이다.

---

# 52. Expression Validation

Expression은 다음을 검사한다.

- Return Type
- Syntax
- Parameter 개수
- Optional Parameter
- Generic Compatibility

Expression 자체의 의미 규칙도 검사한다.

---

# 53. Effect Validation

Effect는 다음을 검증한다.

- Parameter Type
- Context
- Required Symbol
- Namespace
- Documentation Reference

---

# 54. Condition Validation

Condition은 다음을 검사한다.

- Boolean Return
- Context Compatibility
- Parameter Types
- Required Dependencies

---

# 55. Event Validation

Event는 다음을 검사한다.

- Event ID
- Event Context
- Available Variables
- Trigger Compatibility

Event Context는 Registry와 일치해야 한다.

---

# 56. Rule Composition

Rule은 조합 가능해야 한다.

예시

```
Schema Rule

+

Type Rule

+

Plugin Rule

↓

Grammar Validation
```

복합 Rule은 재사용 가능한 작은 Rule들의 조합으로 구현한다.

---

# 57. Validation Ordering

Semantic Rule 실행 순서

```
Dependency

↓

Registry

↓

Symbol

↓

Type

↓

Resource

↓

Plugin
```

순서는 결정적이어야 한다.

---

# 58. Failure Propagation

상위 Resource가 실패하면
의존 Resource의 Semantic Validation은 생략할 수 있다.

예시

```
Type

FAILED

↓

Expression

SKIPPED

↓

Effect

SKIPPED
```

불필요한 연쇄 오류를 줄인다.

---

# 59. Complexity Analysis

권장 복잡도

| Operation | Complexity |
|-----------|-----------:|
| Registry Lookup | O(1) |
| Symbol Lookup | O(1) |
| Type Validation | O(n) |
| Cycle Detection | O(V+E) |
| Version Check | O(1) |

---

# 60. Testing Matrix

Semantic Validation은 다음 시나리오를 포함해야 한다.

- Missing Dependency
- Circular Dependency
- Invalid Version Constraint
- Missing Symbol
- Invalid Type
- Cross Plugin Access
- Registry Mismatch
- Duplicate Symbol
- Invalid Event Context
- Invalid Expression Return Type

---

# 61. Implementation Notes

- Symbol Lookup은 Registry만 사용한다.
- Directory Traversal을 통한 참조는 금지한다.
- Rule은 순수 함수(Pure Function)로 구현하는 것을 권장한다.
- Semantic Validation은 Build Context를 변경해서는 안 된다.
- 동일 입력에 대해 항상 동일한 Diagnostic을 생성해야 한다.

---

# 62. Example Validation Flow

```
Expression

↓

Return Type Lookup

↓

Registry

↓

Type Exists?

↓

YES

↓

Parameter Validation

↓

Semantic Rules

↓

Success
```

---

# 63. Part 3 Summary

Part 3에서는 Dependency Graph, Symbol Resolution, Type Validation 및 Cross Plugin Validation을 정의하였다.

Validator는 Build Context 전체를 대상으로 의미적 일관성을 검증하며, Registry를 단일 참조 지점으로 사용하여 모든 Symbol과 Dependency를 확인한다.

Part 4에서는 다음 내용을 다룬다.

- Diagnostic Builder
- Rich Diagnostics
- Suggestions
- Auto Fix
- Validation Report
- SARIF Export
- CI Integration
- Performance Metrics

---

# Part 4. Diagnostics & Reporting

---

# 64. Overview

Diagnostics는 Validator의 최종 산출물이다.

Validation Rule은 직접 화면에 메시지를 출력하지 않는다.

모든 Validation 결과는 반드시 Diagnostic으로 변환되어야 한다.

```
Validation Rule

↓

Diagnostic

↓

Diagnostic Collector

↓

Diagnostic Report

↓

CLI / IDE / CI
```

Validator와 UI는 완전히 분리되어야 한다.

---

# 65. Diagnostic Architecture

```
Validation Rule

↓

Diagnostic Builder

↓

Diagnostic Collector

↓

Diagnostic Formatter

↓

Diagnostic Exporter
```

Builder는 Diagnostic을 생성만 한다.

출력 형식은 Exporter가 결정한다.

---

# 66. Diagnostic Object

모든 Diagnostic은 동일한 구조를 가진다.

```typescript
interface Diagnostic {

    id: string;

    code: string;

    severity: Severity;

    category: Category;

    resource: ResourceId;

    location?: SourceLocation;

    message: string;

    explanation?: string;

    suggestion?: string;

    documentation?: string;

    related?: DiagnosticReference[];

}
```

이 구조는 CLI, IDE, LSP에서 공통으로 사용한다.

---

# 67. Source Location

Diagnostic은 가능한 경우 정확한 위치를 포함해야 한다.

```typescript
interface SourceLocation {

    file: string;

    line: number;

    column: number;

    endLine: number;

    endColumn: number;

}
```

위치 정보가 없으면 Resource 단위 Diagnostic으로 보고한다.

---

# 68. Severity Model

Severity는 프로젝트 전반에서 통일한다.

| Severity | 의미 |
|------------|---------------------------|
| INFO | 참고 정보 |
| HINT | 개선 제안 |
| WARNING | 수정 권장 |
| ERROR | Validation 실패 |
| FATAL | Validation 즉시 종료 |

IDE에서는 HINT와 WARNING을 별도로 표현할 수 있어야 한다.

---

# 69. Category Model

Diagnostic Category

```
Schema

Dependency

Registry

Namespace

Plugin

Type

Symbol

Expression

Event

Metadata

Documentation

Internal
```

Category는 필터링과 통계에 사용된다.

---

# 70. Diagnostic Codes

Diagnostic Code는 프로젝트 전체에서 유일하다.

예시

```
VAL0001

Missing Manifest
```

```
VAL0002

Invalid JSON
```

```
VAL0104

Duplicate Namespace
```

```
VAL0212

Unknown Symbol
```

```
VAL0305

Circular Dependency
```

Code는 변경하지 않는 것을 원칙으로 한다.

---

# 71. Message Guidelines

메시지는 다음 원칙을 따른다.

좋은 예

```
Unknown type

skript.type.foo
```

나쁜 예

```
Type Error
```

메시지는

- 명확해야 한다.
- 간결해야 한다.
- 해결 가능한 내용을 포함해야 한다.

---

# 72. Explanation

Diagnostic은 상세 설명을 포함할 수 있다.

예시

```
The referenced type does not exist.

Every return type must be registered
before it can be referenced.
```

IDE에서는 펼침 형태로 표시할 수 있다.

---

# 73. Suggestions

가능한 경우 수정 방법을 제공한다.

예시

```
Did you mean

skript.type.player
?
```

또는

```
Add dependency

SkBee >=3.0
```

Suggestion은 자동 수정과 별개이다.

---

# 74. Related Diagnostics

하나의 오류는 여러 위치와 관련될 수 있다.

예시

```
Duplicate Namespace

Resource A

Resource B
```

Diagnostic은 Related Location을 지원해야 한다.

---

# 75. Auto Fix Model

자동 수정 가능한 경우 Fix를 제공한다.

```typescript
interface DiagnosticFix {

    title: string;

    edits: TextEdit[];

}
```

Auto Fix는 항상 선택 사항이다.

Validator가 자동 적용해서는 안 된다.

---

# 76. Diagnostic Collector

Collector는 모든 Diagnostic을 저장한다.

```text
DiagnosticCollector

add()

remove()

clear()

list()

count()
```

Collector는 Validation Rule과 독립적이다.

---

# 77. Report Generation

Validation 종료 후 Report를 생성한다.

```text
Validation Report

Resources

Errors

Warnings

Hints

Execution Time

Cache Hit

Validated Rules
```

Report는 사람이 읽기 쉬워야 한다.

---

# 78. JSON Report

Machine-readable Report를 제공한다.

예시

```json
{
  "summary": {
    "errors": 3,
    "warnings": 7
  },
  "diagnostics": [
    {
      "code": "VAL0212",
      "severity": "ERROR"
    }
  ]
}
```

CI 및 자동화 도구는 JSON Report를 사용한다.

---

# 79. SARIF Export

Validator는 SARIF 2.1.0 Export를 지원한다.

```
Diagnostic

↓

SARIF Converter

↓

GitHub Code Scanning
```

Code Scanning과 직접 연동 가능해야 한다.

---

# 80. LSP Mapping

Diagnostic은 LSP 형식으로 변환 가능해야 한다.

```
Validator Diagnostic

↓

LSP Diagnostic

↓

VSCode
```

Severity Mapping

```
ERROR

↓

DiagnosticSeverity.Error
```

동일한 의미를 유지해야 한다.

---

# 81. CLI Output

CLI는 사람이 읽기 쉬운 형식을 제공한다.

예시

```
ERROR

VAL0212

Unknown Type

player.foo

effects/spawn.json:18
```

색상 출력은 선택 사항이다.

---

# 82. Statistics

Validator는 통계를 생성한다.

예시

```
Resources

5,102

Rules

164

Diagnostics

14

Validation Time

0.84 sec
```

Statistics는 CI Dashboard에서 활용한다.

---

# 83. Performance

권장 목표

| Operation | Target |
|-----------|---------|
| Diagnostic Creation | O(1) |
| Collector Insert | O(1) |
| JSON Export | O(n) |
| SARIF Export | O(n) |

Diagnostic 생성은 Validation 병목이 되어서는 안 된다.

---

# 84. Testing Matrix

반드시 테스트할 항목

- Severity Mapping
- Related Locations
- JSON Export
- SARIF Export
- LSP Mapping
- Auto Fix
- Statistics
- Report Generation
- Stable Diagnostic Code

---

# 85. Implementation Notes

- Diagnostic Code는 절대 재사용하지 않는다.
- 메시지는 지역화(Localizable) 가능해야 한다.
- Exporter는 Formatter와 독립적으로 구현한다.
- Validation Rule은 Diagnostic Collector 외부를 참조하지 않는다.
- Diagnostic 생성은 부작용(Side Effect)이 없어야 한다.

---

# 86. Part 4 Summary

Part 4에서는 Diagnostic Architecture와 Reporting 시스템을 정의하였다.

Validator는 모든 Validation 결과를 표준화된 Diagnostic으로 변환하며, JSON, SARIF, LSP 등 다양한 출력 형식을 통해 CLI, IDE, CI/CD와 통합된다.

Part 5에서는 다음 내용을 다룬다.

- Plugin Validation Extensions
- Parallel Validation
- Incremental Validation
- Validation Cache
- Testing Strategy
- Compliance
- Best Practices
- Future Improvements
- Document Summary

---

# Part 5. Extension, Incremental Validation & Testing

---

# 87. Overview

Validator는 프로젝트 규모가 증가하더라도 확장 가능해야 하며,
Plugin 기반 Validation과 Incremental Validation을 지원해야 한다.

본 장에서는 다음 내용을 정의한다.

- Plugin Validation
- Extension Model
- Incremental Validation
- Parallel Validation
- Cache
- Testing
- Compliance

---

# 88. Validation Extension Architecture

Validator Core는 모든 Rule을 직접 구현하지 않는다.

새로운 Rule은 Extension 형태로 추가된다.

```
Validator

↓

Extension Manager

↓

Validation Extension

↓

Rule Set

↓

Diagnostics
```

Core는 Extension의 존재를 알 필요가 없다.

---

# 89. Validation Extension Interface

권장 인터페이스

```typescript
interface ValidationExtension {

    id(): string;

    version(): string;

    register(registry: RuleRegistry): void;

}
```

Extension은 Rule만 등록한다.

Validation 실행은 Core가 담당한다.

---

# 90. Plugin Validation

Plugin은 자체 Validation Rule을 제공할 수 있다.

예시

```
SkBee

↓

SkBeeValidationExtension

↓

Rule Registry
```

Plugin Rule은 Core Rule 이후 실행된다.

---

# 91. Rule Priority

Rule 실행 우선순위

| Priority | Description |
|-----------|-------------|
| Core | 기본 Validation |
| Registry | Registry 검사 |
| Dependency | Dependency 검사 |
| Plugin | Plugin Rule |
| Custom | 사용자 Rule |

동일 Priority에서는 Rule ID 기준으로 정렬한다.

---

# 92. Incremental Validation

변경된 Resource만 다시 검증한다.

```
Resource Changed

↓

Dependency Analysis

↓

Affected Resources

↓

Validation

↓

Diagnostics Update
```

영향받지 않은 Resource는 재검증하지 않는다.

---

# 93. Dirty Resource Detection

Dirty Resource는 다음 조건 중 하나를 만족한다.

- 파일 내용 변경
- Manifest 변경
- Plugin 변경
- Schema 변경
- Dependency 변경

Dirty 여부는 Hash 기반으로 판단한다.

---

# 94. Dependency Revalidation

Resource 변경 시 의존 Resource도 다시 검증한다.

```
Type

↓

Expression

↓

Effect

↓

Condition
```

Dependency Graph를 사용하여 영향 범위를 계산한다.

---

# 95. Validation Cache

Validation 결과는 Cache에 저장할 수 있다.

Cache 항목

- Parsed JSON
- Schema Validation Result
- Rule Result
- Symbol Resolution
- Dependency Lookup

Cache는 Build Context와 함께 관리한다.

---

# 96. Cache Invalidation

다음 경우 Cache를 무효화한다.

- Resource 변경
- Schema 변경
- Plugin 변경
- Validator Version 변경
- Rule Version 변경

부분 무효화(Partial Invalidation)를 우선한다.

---

# 97. Parallel Validation

독립적인 Resource는 병렬로 검증할 수 있다.

```
Worker 1

Grammar

Worker 2

Locale

Worker 3

Metadata
```

Dependency가 없는 Resource만 병렬 실행한다.

---

# 98. Thread Safety

병렬 Validation 시 다음 객체는 Immutable이어야 한다.

- Registry
- BuildContext
- Schema
- Symbol Table

Diagnostic Collector는 Thread-Safe해야 한다.

---

# 99. Validation Scheduler

Scheduler는 Validation 작업을 관리한다.

```text
Validation Queue

↓

Scheduler

↓

Worker Pool

↓

Completed Tasks
```

Scheduler는 Dependency 순서를 보장해야 한다.

---

# 100. Recovery Strategy

Validation 오류는 가능한 한 다음 Resource의 검증을 계속 수행한다.

예외

- Manifest 손상
- Registry 생성 실패
- Schema Registry 초기화 실패

이 경우 Validation을 중단할 수 있다.

---

# 101. Compliance Levels

Validator는 다음 Compliance Level을 지원한다.

| Level | Description |
|--------|-------------|
| Basic | Schema Validation |
| Standard | Semantic Validation |
| Strict | 모든 Rule 활성화 |

CI 환경에서는 Strict 사용을 권장한다.

---

# 102. Testing Strategy

Validation은 다음 수준으로 테스트한다.

- Unit Test
- Integration Test
- Regression Test
- Snapshot Test
- Performance Test

모든 Rule은 최소 하나 이상의 Unit Test를 가져야 한다.

---

# 103. Test Fixtures

Fixture Repository를 유지한다.

예시

```
fixtures/

valid/

invalid/

duplicate/

dependency/

plugin/
```

Fixture는 가능한 한 최소한의 데이터로 구성한다.

---

# 104. Regression Testing

수정된 Rule은 기존 Fixture 전체에 대해 재검증한다.

Regression Suite는 CI에서 자동 실행한다.

새로운 Rule 추가 시 Regression Fixture를 반드시 포함한다.

---

# 105. Performance Benchmark

권장 목표

| Metric | Target |
|---------|--------|
| 1,000 Resources | < 500 ms |
| 10,000 Resources | < 5 sec |
| Incremental Validation | < 200 ms |
| Memory Usage | Stable |

실제 목표는 프로젝트 규모에 따라 조정할 수 있다.

---

# 106. Logging

Validator는 구조화된 로그를 제공한다.

로그 레벨

- TRACE
- DEBUG
- INFO
- WARN
- ERROR

Validation Rule은 Console 출력 대신 Logger를 사용한다.

---

# 107. Telemetry

선택적으로 다음 정보를 수집할 수 있다.

- Validation Time
- Rule Count
- Cache Hit Ratio
- Diagnostics Count

Telemetry는 기본적으로 비활성화한다.

---

# 108. Best Practices

권장 사항

- Rule은 Stateless하게 구현한다.
- Rule 간 의존성을 만들지 않는다.
- Registry만 참조한다.
- Directory 탐색을 직접 수행하지 않는다.
- Diagnostic Code를 재사용하지 않는다.
- Pure Function 구현을 권장한다.

---

# 109. Future Improvements

향후 고려 사항

- Remote Validation Service
- Distributed Validation
- WASM Validator
- AI-assisted Diagnostics
- Live Validation API
- Validation Rule Marketplace

본 문서는 이러한 확장을 제한하지 않는다.

---

# 110. Document Summary

Validator는 BuildContext를 기반으로 Repository 전체를 정적 분석하는 컴포넌트이다.

Validator는 다음을 수행한다.

- Schema Validation
- Structural Validation
- Semantic Validation
- Dependency Validation
- Registry Validation
- Plugin Validation
- Diagnostic Generation

Validator는 Repository를 수정하지 않으며,
모든 결과는 Diagnostic과 Validation Report로 표현된다.

Builder와 동일하게 Validator는 읽기 전용(Immutable) BuildContext를 입력으로 사용하며,
Incremental Validation과 병렬 실행을 통해 대규모 Repository에서도 높은 성능을 유지하도록 설계한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial Validator Implementation Guide |