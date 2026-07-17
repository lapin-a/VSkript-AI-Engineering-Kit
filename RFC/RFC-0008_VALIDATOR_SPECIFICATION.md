# RFC-0008_VALIDATOR_SPECIFICATION.md

# VSkript Validator Specification

> Validation Engine & Quality Assurance Specification

RFC: RFC-0008

Version: 1.0

Status: Draft

---

# 1. Purpose

본 RFC는 VSkript Database의 Validator가 수행해야 하는 모든 검증 절차와 Validation Engine의 구조를 정의한다.

Validator는 Builder가 생성한 모든 Resource가 프로젝트의 Specification과 RFC를 준수하는지 검사하는 품질 보증(Quality Assurance) 컴포넌트이다.

Validator는 어떠한 데이터도 수정하지 않으며, 모든 검사는 Read-Only 방식으로 수행되어야 한다.

---

# 2. Goals

Validator의 목표는 다음과 같다.

- Repository 무결성 보장
- DataPack 품질 보장
- JSON Schema 검증
- Cross Reference 검증
- Dependency 검증
- Locale 검증
- Registry 검증
- Incremental Validation 지원
- CI/CD 자동 검증 지원

---

# 3. Scope

본 RFC는 다음 항목을 정의한다.

- Validation Architecture
- Validation Pipeline
- Validation Rules
- Diagnostic System
- Validation Report
- Error Codes
- CLI Integration
- Builder Integration

다음 내용은 별도 RFC에서 정의한다.

- Builder Implementation
- Generator
- Update Protocol

---

# 4. Terminology

| Term | Description |
|------|-------------|
| Validation | 규칙 기반 검사 |
| Rule | 검증 규칙 |
| Validator | 검증 엔진 |
| Context | Validation 실행 환경 |
| Diagnostic | 검증 결과 |
| Severity | 오류 심각도 |
| Resource | Grammar, Locale, Registry 등 검증 대상 |

---

# 5. Design Principles

Validator는 다음 원칙을 따른다.

## Read Only

Validator는 Resource를 수정해서는 안 된다.

---

## Deterministic

동일한 입력은 항상 동일한 Validation 결과를 생성해야 한다.

---

## Stateless

Validator는 Validation 종료 후 어떠한 상태도 유지하지 않는다.

---

## Independent Rules

각 Validation Rule은 서로 독립적으로 실행되어야 한다.

---

## Fail Safe

하나의 Rule이 실패하더라도 가능한 많은 오류를 보고해야 한다.

---

# 6. Validator Architecture

Validator는 Rule Engine 기반으로 동작한다.

```
Resources

↓

Validation Context

↓

Rule Engine

↓

Diagnostics

↓

Validation Report
```

각 Rule은 Validation Context를 공유하지만 서로 독립적으로 실행된다.

---

# 7. Validation Components

Validator는 다음 컴포넌트로 구성된다.

```
Schema Validator

Grammar Validator

Locale Validator

Registry Validator

Manifest Validator

Dependency Validator

Reference Validator

DataPack Validator

Diagnostic Engine

Report Generator
```

각 컴포넌트는 하나의 역할만 수행한다.

---

# 8. Validation Pipeline

Validation은 다음 순서를 따른다.

```
Load Resources

↓

Create Context

↓

Schema Validation

↓

Grammar Validation

↓

Locale Validation

↓

Type Validation

↓

Registry Validation

↓

Manifest Validation

↓

Dependency Validation

↓

Cross Reference Validation

↓

DataPack Validation

↓

Generate Diagnostics

↓

Generate Report
```

Validation 단계는 가능한 한 계속 진행되어야 하며, 치명적인 오류만 즉시 중단한다.

---

# 9. Validation Context

모든 Validation은 Validation Context 내부에서 수행된다.

Validation Context는 다음 정보를 포함한다.

```text
Build ID

Builder Version

Schema Version

Repository Version

Validation Mode

Loaded Resources

Registry Snapshot

Dependency Graph
```

Validation Context는 Build 종료 후 폐기된다.

---

# 10. Rule Engine

Validator는 Rule Engine을 사용하여 모든 Validation Rule을 실행한다.

Rule Engine은 다음 조건을 만족해야 한다.

- Rule 등록 가능
- Rule 우선순위 지원
- Rule 독립 실행
- 병렬 실행 가능
- Incremental Validation 지원

모든 Rule은 다음 인터페이스를 따른다.

```ts
interface ValidationRule {

    id: string;

    name: string;

    category: string;

    validate(context: ValidationContext): ValidationResult;

}
```

Rule는 Validation Context만 읽을 수 있으며 Resource를 수정해서는 안 된다.

---

# 11. Validation Categories

Validator는 다음 범주의 검증을 수행한다.

| Category | Description |
|-----------|-------------|
| Schema | JSON Schema 검증 |
| Grammar | 문법 데이터 검증 |
| Locale | 번역 데이터 검증 |
| Type | 타입 시스템 검증 |
| Registry | Registry 무결성 검증 |
| Manifest | Manifest 구조 검증 |
| Dependency | 의존성 검증 |
| Reference | 참조 무결성 검증 |
| DataPack | DataPack 구조 검증 |

---

# 12. Validation Modes

Validator는 다음 실행 모드를 지원한다.

| Mode | Description |
|------|-------------|
| Full | 전체 Validation |
| Incremental | 변경된 Resource만 검증 |
| Schema | Schema만 검증 |
| Grammar | Grammar만 검증 |
| Registry | Registry만 검증 |
| DataPack | DataPack만 검증 |

Incremental Mode는 RFC-0007 Builder의 Incremental Build와 연동된다.

---

# 13. Validation Result

모든 Rule은 ValidationResult를 반환한다.

예시

```json
{
  "status": "success",
  "warnings": 2,
  "errors": 0
}
```

ValidationResult는 Diagnostic Engine으로 전달된다.

---

# 14. Severity Levels

Validator는 다음 Severity를 사용한다.

| Level | Description |
|---------|-------------|
| Info | 참고 정보 |
| Warning | 경고 |
| Error | 오류 |
| Critical | 치명적 오류 |

Critical 오류가 발생하면 Build는 실패해야 한다.

---

# 15. Summary

Validator는 VSkript Database의 품질을 보장하는 핵심 컴포넌트이다.

모든 Resource는 Rule Engine을 통해 검증되며, Validator는 데이터를 수정하지 않고 문제를 보고하는 역할만 수행한다.

Validation은 Deterministic하며, Incremental Validation을 지원해야 한다.

---

# 16. Schema Validation

## Purpose

Schema Validation은 모든 JSON Resource가 프로젝트에서 정의한 JSON Schema를 준수하는지 확인한다.

Schema Validation은 모든 Validation의 첫 번째 단계이며, 이후 Validation의 기반이 된다.

---

### Input

```
manifest.json

registry.json

grammar/*.json

locale/*.json

types.json
```

---

### Process

Validator는 각 Resource에 대응하는 Schema를 로드한다.

```
Resource

↓

Schema Lookup

↓

JSON Schema Validation

↓

Diagnostic
```

---

### Validation Rules

검사 항목

- Required Property
- Property Type
- Enum
- Pattern
- Additional Property
- Array Constraint
- Object Constraint

---

### Error Cases

| Code | Description |
|------|-------------|
| SC001 | Schema Not Found |
| SC002 | Invalid Property |
| SC003 | Missing Required Property |
| SC004 | Invalid Enum |
| SC005 | Invalid Pattern |

---

### Output

```
ValidationResult
```

---

# 17. Grammar Validation

## Purpose

Grammar Validation은 Grammar Resource의 무결성을 검사한다.

Grammar는 Extension과 Language Server가 직접 사용하는 핵심 데이터이므로 엄격하게 검증해야 한다.

---

### Input

```
grammar/

effects.json

events.json

expressions.json

conditions.json

sections.json

functions.json
```

---

### Process

```
Grammar

↓

Pattern Validation

↓

Type Validation

↓

Alias Validation

↓

Diagnostic
```

---

### Validation Rules

Validator는 다음을 검사한다.

- Duplicate Pattern
- Empty Pattern
- Invalid Syntax
- Invalid Placeholder
- Invalid Return Type
- Unknown Type
- Invalid Category
- Deprecated Rule

---

### Example

허용

```json
{
    "pattern": "spawn %entity%"
}
```

허용하지 않음

```json
{
    "pattern": ""
}
```

---

### Error Cases

| Code | Description |
|------|-------------|
| GR001 | Duplicate Pattern |
| GR002 | Invalid Pattern |
| GR003 | Unknown Type |
| GR004 | Empty Pattern |
| GR005 | Invalid Placeholder |

---

# 18. Locale Validation

## Purpose

Locale Validation은 번역 데이터의 무결성을 검사한다.

---

### Input

```
locale/

en.json

ko.json

ja.json
```

---

### Process

```
Locale

↓

Key Validation

↓

Translation Validation

↓

Missing Key Check

↓

Diagnostic
```

---

### Validation Rules

- Duplicate Key
- Empty Translation
- Invalid Locale Code
- Missing Locale Key
- Invalid UTF-8
- Invalid Placeholder

---

### Placeholder Validation

Placeholder는 모든 Locale에서 동일해야 한다.

허용

```
Spawn %entity%
```

```
%entity% 생성
```

허용하지 않음

```
Spawn %entity%
```

```
생성
```

---

### Error Cases

| Code | Description |
|------|-------------|
| LC001 | Duplicate Locale Key |
| LC002 | Missing Translation |
| LC003 | Invalid Placeholder |
| LC004 | Empty Translation |
| LC005 | Invalid Locale Code |

---

# 19. Type Validation

## Purpose

Type System의 무결성을 검사한다.

---

### Validation Rules

검사 대상

- Primitive Type
- Custom Type
- Alias
- Converter
- Collection
- Generic Type

---

### Validation Process

```
Type

↓

Existence Check

↓

Inheritance Check

↓

Converter Check

↓

Diagnostic
```

---

### Error Cases

| Code | Description |
|------|-------------|
| TY001 | Unknown Type |
| TY002 | Duplicate Type |
| TY003 | Invalid Alias |
| TY004 | Invalid Converter |

---

# 20. Registry Validation

## Purpose

Registry의 전체 무결성을 보장한다.

Registry는 Repository 전체의 Source of Truth 역할을 수행한다.

---

### Input

```
registry.json
```

---

### Validation Rules

Validator는 다음을 검사한다.

- Duplicate Resource ID
- Duplicate Plugin ID
- Invalid Version
- Invalid Category
- Missing Resource
- Invalid Dependency
- Invalid DataPack

---

### Registry Index Validation

모든 Resource는 Registry에 존재해야 한다.

```
Grammar

↓

Registry Lookup

↓

Exists

↓

PASS
```

존재하지 않으면 오류를 발생시킨다.

---

### Error Cases

| Code | Description |
|------|-------------|
| RG001 | Duplicate ID |
| RG002 | Missing Resource |
| RG003 | Invalid Registry Entry |
| RG004 | Invalid Version |
| RG005 | Invalid Category |

---

# 21. Manifest Validation

## Purpose

Manifest는 DataPack의 Entry Point이다.

모든 DataPack은 정확한 Manifest를 가져야 한다.

---

### Validation Rules

검사 항목

- Plugin ID
- Version
- Builder Version
- Schema Version
- Dependencies
- Generated Time

---

### Validation Process

```
Manifest

↓

Schema Validation

↓

Semantic Version Check

↓

Dependency Check

↓

Diagnostic
```

---

### Error Cases

| Code | Description |
|------|-------------|
| MF001 | Missing Manifest |
| MF002 | Invalid Version |
| MF003 | Invalid Dependency |
| MF004 | Missing Plugin ID |
| MF005 | Invalid Builder Version |

---

# 22. Plugin Validation

## Purpose

Plugin Resource의 메타데이터를 검증한다.

---

### Validation Rules

- Plugin ID 존재
- Plugin Name
- Version
- Namespace
- Registry 등록 여부
- DataPack 존재 여부

---

### Error Cases

| Code | Description |
|------|-------------|
| PL001 | Unknown Plugin |
| PL002 | Missing Plugin |
| PL003 | Invalid Namespace |
| PL004 | Duplicate Plugin |

---

# 23. Build Artifact Validation

Builder가 생성한 모든 산출물을 검사한다.

검사 대상

```
generated/

registry/

packs/

reports/
```

---

### Validation Rules

- Missing Output
- Invalid Output Structure
- Invalid Encoding
- Invalid File Name
- Invalid Directory Structure

---

### Error Cases

| Code | Description |
|------|-------------|
| BA001 | Missing Output |
| BA002 | Invalid Directory |
| BA003 | Invalid Encoding |
| BA004 | Invalid File Name |

---

# 24. Validation Ordering

Validator는 다음 순서를 반드시 유지한다.

```
Schema

↓

Grammar

↓

Locale

↓

Type

↓

Registry

↓

Manifest

↓

Plugin

↓

Build Artifact
```

이 순서는 Dependency를 고려하여 정의되며 변경해서는 안 된다.

---

# 25. Summary

Schema Validation부터 Build Artifact Validation까지의 검증은 Resource 자체의 무결성과 구조적 일관성을 보장한다.

이 단계에서 발견된 오류는 대부분 Builder 또는 Generator 단계의 구현 문제를 의미하며, 이후 수행되는 Dependency Validation 및 Cross Reference Validation의 기반이 된다.

---

# 26. Dependency Validation

## Purpose

Dependency Validation은 모든 Resource 및 DataPack의 의존성이 올바르게 정의되었는지 검증한다.

Dependency Validation은 SPEC-0010 Dependency Resolution을 기반으로 수행된다.

---

### Input

```
manifest.json

registry.json

Dependency Graph
```

---

### Process

```
Manifest

↓

Dependency Lookup

↓

Registry Lookup

↓

Version Check

↓

Graph Validation

↓

Diagnostic
```

---

### Validation Rules

Validator는 다음을 검사한다.

- Missing Dependency
- Invalid Dependency
- Duplicate Dependency
- Circular Dependency
- Unsupported Version
- Unknown Plugin
- Invalid Constraint

---

### Circular Dependency

다음 구조는 허용되지 않는다.

```
Plugin A

↓

Plugin B

↓

Plugin C

↓

Plugin A
```

Validator는 Graph Traversal을 사용하여 순환을 탐지해야 한다.

---

### Version Validation

예시

```
Required

>=2.9.0
```

```
Installed

2.8.7
```

↓

Validation Error

---

### Error Cases

| Code | Description |
|------|-------------|
| DP001 | Missing Dependency |
| DP002 | Circular Dependency |
| DP003 | Unsupported Version |
| DP004 | Invalid Constraint |
| DP005 | Duplicate Dependency |

---

# 27. Cross Reference Validation

## Purpose

Cross Reference Validation은 모든 Resource 간 참조 무결성을 보장한다.

---

### Validation Targets

```
Grammar → Type

Grammar → Locale

Grammar → Registry

Registry → Manifest

Manifest → Dependency

Dependency → Registry
```

---

### Process

```
Reference

↓

Registry Lookup

↓

Existence Check

↓

Diagnostic
```

---

### Validation Rules

모든 Reference는 반드시 Registry에 존재해야 한다.

Reference는 Registry ID를 사용해야 한다.

Broken Reference는 허용되지 않는다.

---

### Example

허용

```
type.player
```

허용하지 않음

```
player
```

---

### Error Cases

| Code | Description |
|------|-------------|
| RF001 | Unknown Reference |
| RF002 | Broken Reference |
| RF003 | Invalid Registry ID |
| RF004 | Invalid Namespace |

---

# 28. DataPack Validation

## Purpose

DataPack 구조가 SPEC-0011을 준수하는지 검사한다.

---

### Validation Targets

```
manifest.json

metadata.json

registry.json

grammar/

locale/

documentation/

assets/
```

---

### Validation Rules

- Required Files
- Required Directories
- Invalid Directory
- Invalid File Name
- Invalid Encoding
- Missing Manifest
- Invalid Package Layout

---

### Validation Process

```
Package

↓

Directory Validation

↓

File Validation

↓

Schema Validation

↓

Diagnostic
```

---

### Error Cases

| Code | Description |
|------|-------------|
| PK001 | Missing Directory |
| PK002 | Missing File |
| PK003 | Invalid Layout |
| PK004 | Invalid Encoding |

---

# 29. Incremental Validation

## Purpose

Incremental Validation은 변경된 Resource만 검증한다.

RFC-0007 Incremental Build와 함께 사용된다.

---

### Process

```
Hash Compare

↓

Dirty Resource

↓

Partial Validation

↓

Diagnostic
```

---

### Validation Rules

Validator는 Dirty Resource와 영향을 받는 Resource만 검사한다.

예시

```
Grammar 변경

↓

Grammar Validation

↓

Registry Validation

↓

Manifest Validation
```

Locale Validation은 수행하지 않는다.

---

### Benefits

- Validation 시간 단축
- CI/CD 속도 향상
- 대규모 Repository 대응
- Cache 활용

---

# 30. Diagnostic Engine

## Purpose

Diagnostic Engine은 모든 Validation 결과를 수집하고 표준 Diagnostic으로 변환한다.

---

### Diagnostic Structure

예시

```json
{
  "code": "GR003",
  "severity": "Error",
  "message": "Unknown Type",
  "resource": "effects.json",
  "path": "/effects/13/type"
}
```

---

### Required Fields

- Code
- Severity
- Message
- Resource
- Location
- Category

---

### Severity

| Level | Description |
|---------|-------------|
| Info | 참고 |
| Warning | 경고 |
| Error | 오류 |
| Critical | 치명적 오류 |

Critical는 Build를 중단한다.

---

# 31. Error Code System

모든 Validation Error는 고유한 Error Code를 가져야 한다.

예시

```
SC001

Schema

Grammar

Locale

Registry

Manifest

Dependency
```

---

### Prefix

| Prefix | Category |
|---------|----------|
| SC | Schema |
| GR | Grammar |
| LC | Locale |
| TY | Type |
| RG | Registry |
| MF | Manifest |
| DP | Dependency |
| RF | Reference |
| PK | Package |

Error Code는 프로젝트 전체에서 유일해야 한다.

---

# 32. Validation Report

## Purpose

Validation 결과를 사람이 읽을 수 있는 형태로 제공한다.

---

### Example

```json
{
  "status": "failed",
  "resources": 812,
  "warnings": 14,
  "errors": 2,
  "critical": 0,
  "duration": 4132
}
```

---

### Report Contents

- Validation Mode
- Duration
- Resources
- Rules Executed
- Warnings
- Errors
- Critical Errors
- Passed Rules

---

# 33. Report Formats

지원 형식

```
JSON

Markdown

Console

HTML
```

CI에서는 JSON을 권장한다.

---

# 34. Validation Events

Validator는 다음 이벤트를 발생시킨다.

```
ValidationStarted

RuleStarted

RuleCompleted

ValidationCompleted

ValidationFailed

DiagnosticCreated
```

외부 Tool은 Event를 사용할 수 있다.

---

# 35. Summary

Dependency Validation부터 Validation Report 생성까지는 Validator의 핵심 기능을 구성한다.

Validator는 Dependency Graph와 Registry를 기반으로 Resource 간 참조를 검증하고, Incremental Validation을 통해 변경된 데이터만 검사한다.

모든 결과는 Diagnostic Engine을 거쳐 표준 Error Code와 함께 Validation Report로 출력되며, Builder와 CI/CD 시스템에서 공통으로 사용할 수 있어야 한다.

---

# 36. CLI Integration

## Purpose

Validator는 독립적인 CLI(Command Line Interface)를 제공해야 한다.

CLI는 개발자, CI/CD 환경 및 자동화 도구에서 동일한 방식으로 사용할 수 있어야 한다.

---

### Supported Commands

```bash
vskript-validator validate
```

전체 Validation 수행

```bash
vskript-validator validate --incremental
```

변경된 Resource만 검증

```bash
vskript-validator validate --schema
```

Schema만 검증

```bash
vskript-validator validate --grammar
```

Grammar만 검증

```bash
vskript-validator validate --registry
```

Registry만 검증

---

### CLI Exit Codes

| Exit Code | Description |
|-----------|-------------|
| 0 | Validation Success |
| 1 | Validation Failed |
| 2 | Invalid Arguments |
| 3 | Internal Error |

---

# 37. Builder Integration

## Purpose

Validator는 Builder의 마지막 단계에서 실행된다.

Builder는 Validation 성공 여부를 기반으로 Build 성공 여부를 결정한다.

---

### Build Pipeline

```
Builder

↓

Generate Resources

↓

Validator

↓

Validation Passed

↓

Generator
```

Validation 실패 시 Generator는 실행되지 않는다.

---

### Build Context Sharing

Builder는 Validator에 다음 정보를 전달한다.

```
Build ID

Builder Version

Repository Version

Schema Version

Incremental State

Registry Snapshot
```

---

# 38. Generator Integration

## Purpose

Generator는 Validation을 통과한 Resource만 사용해야 한다.

---

### Workflow

```
Builder

↓

Validator

↓

Generator

↓

Package
```

Generator는 Validation을 우회해서는 안 된다.

---

### Validation Contract

Generator는 다음을 가정할 수 있다.

- 모든 JSON은 Schema를 만족한다.
- Registry는 무결하다.
- Locale은 완전하다.
- Dependency는 해결되었다.
- Manifest는 유효하다.

---

# 39. Language Server Integration

## Purpose

Language Server는 Validator의 Diagnostic을 재사용할 수 있다.

---

### Shared Diagnostics

동일한 Error Code를 사용한다.

예시

```
GR003

Unknown Type
```

VSCode Extension에서도 동일한 메시지를 표시한다.

---

### Diagnostic Mapping

```
Validator

↓

Diagnostic

↓

Language Server

↓

VSCode Diagnostic
```

---

# 40. CI/CD Integration

## Purpose

Validator는 CI/CD Pipeline에서 자동 실행될 수 있어야 한다.

---

### Recommended Pipeline

```
Checkout

↓

Build

↓

Validate

↓

Test

↓

Generate

↓

Package

↓

Publish
```

Validation 실패 시 Pipeline은 즉시 종료된다.

---

### GitHub Actions Example

```yaml
- name: Validate Database
  run: vskript-validator validate

- name: Generate Packages
  run: vskript-generator build
```

---

# 41. Performance Guidelines

Validator는 대규모 Repository에서도 효율적으로 동작해야 한다.

권장 목표

| Metric | Target |
|---------|--------|
| Full Validation | ≤ 30초 |
| Incremental Validation | ≤ 3초 |
| Memory Usage | ≤ 512MB |
| Cache Hit | ≥ 90% |

---

### Optimization Strategies

Validator는 다음 최적화를 사용할 수 있다.

- Incremental Validation
- Parallel Rule Execution
- Lazy Loading
- Cached Registry Lookup
- Cached Schema Parsing

---

# 42. Security Considerations

Validator는 신뢰되지 않은 입력을 항상 검증해야 한다.

---

### Required Checks

- JSON Schema Validation
- UTF-8 Validation
- Path Traversal 방지
- Invalid File Name 검사
- Invalid Encoding 검사
- Checksum Verification

---

### Forbidden Behavior

Validator는 다음 작업을 수행해서는 안 된다.

- Resource 수정
- Network 접근
- 임의 코드 실행
- 외부 Script 실행
- Build 결과 변경

Validator는 항상 Read-Only이어야 한다.

---

# 43. Extensibility

Validator는 새로운 Rule을 쉽게 추가할 수 있어야 한다.

---

### Rule Registration

새로운 Rule은 Rule Registry에 등록한다.

예시

```ts
registry.register(new GrammarValidationRule());
registry.register(new LocaleValidationRule());
registry.register(new RegistryValidationRule());
```

기존 Rule을 수정하지 않고 새로운 Rule을 추가할 수 있어야 한다.

---

### Custom Validators

향후 지원 가능한 Validator

- Plugin Validator
- Documentation Validator
- Markdown Validator
- Asset Validator
- Image Validator
- Release Validator

---

# 44. Future Compatibility

Validator는 다음 변경을 지원해야 한다.

- 새로운 Resource 유형
- 새로운 Schema Version
- 새로운 DataPack 구조
- 새로운 Plugin 유형
- 새로운 Validation Rule

기존 Rule과의 하위 호환성을 유지하는 것을 권장한다.

---

# 45. Operational Guidelines

운영 환경에서는 다음을 권장한다.

- Incremental Validation 사용
- Validation Report 보관
- 모든 Pull Request에서 Validation 수행
- Release 전 Full Validation 수행
- Warning도 지속적으로 관리

---

# 46. Compliance

Validator는 다음 Specification을 준수해야 한다.

- RFC-0001 Manifest Schema
- RFC-0002 Registry Schema
- RFC-0003 Grammar Schema
- RFC-0005 Update Protocol
- RFC-0007 Builder Specification

또한 다음 SPEC를 준수해야 한다.

- SPEC-0005 Error Codes
- SPEC-0008 JSON Schema Guide
- SPEC-0009 Versioning
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout
- SPEC-0012 Directory Structure

---

# 47. Best Practices

Validator 구현 시 다음 사항을 권장한다.

- Rule은 작고 독립적으로 유지한다.
- 하나의 Rule은 하나의 책임만 가진다.
- 모든 Rule은 테스트 가능해야 한다.
- Error Code는 절대 재사용하지 않는다.
- Diagnostic은 사람이 이해하기 쉽게 작성한다.
- Validation은 항상 결정적(Deterministic)이어야 한다.

---

# 48. Summary

Validator는 VSkript Database의 품질을 보장하는 핵심 구성 요소이다.

Builder가 생성한 모든 Resource는 Validator를 통해 구조적 무결성, 참조 무결성, 의존성 및 데이터 품질을 검증받아야 한다.

Validator는 Rule-Based Architecture를 사용하며, Incremental Validation을 지원하여 대규모 Repository에서도 효율적으로 동작해야 한다.

Validation 결과는 표준 Diagnostic과 Validation Report로 출력되며, Builder, Generator, Language Server 및 CI/CD 환경에서 공통으로 활용된다.

---

# Related Specifications

- SPEC-0005_ERROR_CODES
- SPEC-0008_JSON_SCHEMA_GUIDE
- SPEC-0009_VERSIONING
- SPEC-0010_DEPENDENCY_RESOLUTION
- SPEC-0011_DATAPACK_LAYOUT
- SPEC-0012_DIRECTORY_STRUCTURE

---

# Related RFC

- RFC-0001_MANIFEST_SCHEMA
- RFC-0002_REGISTRY_SCHEMA
- RFC-0003_GRAMMAR_SCHEMA
- RFC-0005_UPDATE_PROTOCOL
- RFC-0007_BUILDER_SPECIFICATION
- RFC-0009_GENERATOR_SPECIFICATION
- RFC-0010_LANGUAGE_SERVER_INTEGRATION

---

# Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0008 |
| Title | Validator Specification |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Depends On | RFC-0007 |
| Followed By | RFC-0009 |
| Last Updated | YYYY-MM-DD |