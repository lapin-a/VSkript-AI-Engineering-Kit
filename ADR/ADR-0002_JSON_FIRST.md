# ADR-0002_JSON_FIRST.md

# ADR-0002: JSON-First Architecture

Status: Accepted

Date: YYYY-MM-DD

Authors:
- VSkript Database Team

---

# Context

VSkript Database는 Skript 및 다양한 애드온의 문법, 타입, Locale, Registry, Manifest, Metadata를 저장하고 관리하는 데이터 저장소이다.

프로젝트 초기 설계 단계에서 데이터 저장 형식으로 다음과 같은 여러 대안을 검토하였다.

- JSON
- YAML
- TOML
- XML
- SQLite
- Markdown 기반 DSL
- TypeScript Object
- Protocol Buffers

평가 기준은 다음과 같았다.

- 사람의 가독성
- AI 친화성
- Git Diff
- Schema Validation
- Language Server 지원
- Builder 구현
- Generator 구현
- Version 관리
- Cross Platform 지원

---

# Decision

본 프로젝트는 **JSON-First Architecture**를 채택한다.

모든 핵심 데이터는 JSON 형식으로 저장한다.

적용 대상은 다음과 같다.

```
Grammar

Registry

Manifest

Metadata

Locale

Type Definitions

Dependency Information

Builder Cache

Generator Metadata
```

다른 형식은 출력(Output) 또는 문서화 목적으로만 사용할 수 있으며, JSON을 대체해서는 안 된다.

---

# Rationale

## 1. Single Data Format

프로젝트 전체에서 하나의 데이터 형식을 사용한다.

이를 통해 Builder, Validator, Generator 및 Language Server는 동일한 데이터 모델을 공유할 수 있다.

---

## 2. JSON Schema 지원

JSON은 성숙한 Schema 생태계를 제공한다.

Validator는 JSON Schema를 이용하여 구조를 자동 검증할 수 있다.

예시

```
Resource

↓

JSON Schema

↓

Validator

↓

Diagnostic
```

---

## 3. AI 친화성

LLM 및 AI 도구는 JSON을 가장 안정적으로 생성하고 분석한다.

지원 대상 예시

- ChatGPT
- Claude
- Gemini
- Codex

JSON은 구조가 명확하여 자동 생성 및 검증이 용이하다.

---

## 4. Git 친화성

JSON은 텍스트 기반 형식이므로 Git에서 변경 사항을 쉽게 추적할 수 있다.

장점

- Line Diff 지원
- Merge Conflict 최소화
- Code Review 용이

---

## 5. Language Independence

JSON은 특정 언어에 종속되지 않는다.

지원 예시

- TypeScript
- JavaScript
- Java
- Kotlin
- C#
- Go
- Rust
- Python

모든 구현체가 동일한 데이터를 사용할 수 있다.

---

## 6. Serialization

JSON은 직렬화와 역직렬화가 매우 단순하다.

예시

```
JSON

↓

Object

↓

Builder

↓

JSON
```

추가 변환 과정이 필요하지 않다.

---

## 7. Stable Specification

RFC와 SPEC 문서는 모두 JSON 구조를 기준으로 정의할 수 있다.

예시

```
Grammar Schema

↓

Grammar JSON

↓

Validator
```

구현과 문서가 항상 일치한다.

---

## 8. Performance

JSON 파서는 대부분의 언어에서 최적화되어 있다.

Builder와 Validator는 별도의 Parser를 구현할 필요가 없다.

---

# Consequences

JSON-First Architecture를 채택함으로써 다음과 같은 장점이 있다.

- 단일 데이터 모델
- JSON Schema 활용
- 높은 AI 호환성
- 쉬운 Validation
- 쉬운 직렬화
- 뛰어난 Git 호환성
- 언어 독립성

반면 다음과 같은 제약도 존재한다.

- 주석(Comment)을 기본적으로 지원하지 않는다.
- 복잡한 관계형 데이터를 표현하는 데 한계가 있다.
- 대용량 데이터에서는 메모리 사용량이 증가할 수 있다.

이러한 제약은 별도의 Metadata 및 Registry 설계를 통해 보완한다.

---

# Alternatives Considered

## YAML

장점

- 사람이 읽기 쉽다.
- 주석을 지원한다.

단점

- 들여쓰기 오류에 취약하다.
- 파서마다 동작 차이가 발생할 수 있다.
- Schema 생태계가 JSON보다 약하다.

채택하지 않았다.

---

## TOML

장점

- 설정 파일에 적합하다.
- 가독성이 좋다.

단점

- 복잡한 중첩 구조 표현이 제한적이다.
- Grammar 데이터를 표현하기 어렵다.

채택하지 않았다.

---

## XML

장점

- 풍부한 메타데이터 표현
- Schema 지원

단점

- 문법이 장황하다.
- 사람이 작성하기 어렵다.
- AI 생성 효율이 낮다.

채택하지 않았다.

---

## SQLite

장점

- 빠른 조회
- 관계형 데이터 관리

단점

- Git Diff 불가능
- 사람이 직접 수정하기 어렵다.
- Pull Request 검토가 어렵다.

채택하지 않았다.

---

## TypeScript Objects

장점

- 타입 안정성
- IDE 지원

단점

- 특정 언어에 종속된다.
- JSON Schema를 직접 사용할 수 없다.

채택하지 않았다.

---

# Implementation Notes

Builder는 모든 입력을 JSON으로 읽는다.

Validator는 JSON Schema를 기준으로 Validation을 수행한다.

Generator는 JSON Resource를 그대로 DataPack에 포함한다.

Language Server는 JSON Resource만 로드한다.

JSON은 프로젝트의 유일한 데이터 저장 형식이다.

---

# Risks

JSON은 주석을 지원하지 않는다.

이를 해결하기 위해 다음을 사용한다.

- Documentation Directory
- Metadata
- RFC
- SPEC

설명은 문서로 관리하고, 데이터는 JSON으로 유지한다.

---

# Related RFC

- RFC-0001 Manifest Schema
- RFC-0002 Registry Schema
- RFC-0003 Grammar Schema
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification

---

# Related Specifications

- SPEC-0008 JSON Schema Guide
- SPEC-0009 Versioning
- SPEC-0011 DataPack Layout

---

# Decision Summary

VSkript Database는 모든 핵심 데이터를 JSON으로 저장하는 **JSON-First Architecture**를 채택한다.

JSON은 프로젝트 전반에서 단일 데이터 모델을 제공하며, Builder, Validator, Generator, Language Server 및 AI 도구 간의 높은 상호 운용성과 일관성을 보장한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |