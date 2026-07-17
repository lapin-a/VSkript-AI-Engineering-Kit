# ADR-0007_LSP_DATAPACK_LOADING.md

# ADR-0007: Language Server Loads DataPack Instead of Raw Repository

Status: Accepted

Date: YYYY-MM-DD

Authors:

- VSkript Database Team

---

# Context

VSkript Language Server는 VSCode Extension 및 기타 LSP(Language Server Protocol) 클라이언트에 다음 기능을 제공한다.

- Completion
- Hover
- Definition
- References
- Signature Help
- Semantic Tokens
- Diagnostics
- Workspace Symbol Search

초기 설계에서는 Language Server가 Repository의 Raw Source를 직접 읽는 방식과 Generator가 생성한 DataPack만 읽는 방식을 모두 검토하였다.

검토 대상은 다음과 같았다.

- Startup Performance
- Memory Usage
- Offline Support
- Release Management
- Version Compatibility
- Build Pipeline
- IDE Integration
- Distribution

---

# Decision

Language Server는 **Raw Repository를 직접 읽지 않는다.**

Language Server의 유일한 입력은 Generator가 생성한 **DataPack**이다.

Repository 구조는 Builder와 Generator만 이해하며, Language Server는 DataPack의 공개 구조만 사용한다.

---

# Architecture

```
Repository

↓

Builder

↓

Validator

↓

Generator

↓

DataPack

↓

Language Server

↓

VSCode
```

Language Server는 Repository 구조를 전혀 알 필요가 없다.

---

# Data Flow

```
Raw Source

↓

Build

↓

Validation

↓

Packaging

↓

DataPack

↓

Load

↓

Completion
```

Builder와 Language Server는 직접 연결되지 않는다.

---

# Rationale

## 1. Separation of Responsibilities

Repository는 개발 환경이다.

DataPack은 실행 환경이다.

```
Repository

↓

Build

↓

DataPack

↓

Runtime
```

Language Server는 Runtime Consumer이다.

---

## 2. Stable Input

Repository는 계속 변경된다.

DataPack은 Validation을 통과한 결과만 포함한다.

Language Server는 항상 검증된 데이터만 사용한다.

---

## 3. Faster Startup

Repository를 직접 읽는 경우

```
Workspace

↓

Directory Scan

↓

JSON Parsing

↓

Dependency Resolution

↓

Registry Build
```

DataPack을 사용하는 경우

```
Load Registry

↓

Load Cache

↓

Ready
```

Startup 시간이 크게 단축된다.

---

## 4. Lower Memory Usage

Language Server는 Registry만 먼저 메모리에 로드한다.

Grammar와 Locale은 Lazy Loading한다.

```
Registry

↓

Lookup

↓

Load Grammar
```

필요한 Resource만 메모리에 적재한다.

---

## 5. Offline Support

DataPack은 필요한 Resource를 모두 포함한다.

네트워크 연결 없이도 다음 기능을 사용할 수 있다.

- Completion
- Hover
- Definition
- Diagnostics

---

## 6. Independent Release Cycle

Language Server는 Repository 변경과 무관하게 동작한다.

새로운 DataPack만 설치하면 최신 기능을 사용할 수 있다.

```
Update DataPack

↓

Restart LSP

↓

Ready
```

Language Server 자체를 업데이트할 필요가 없다.

---

## 7. Version Compatibility

DataPack은 Manifest를 포함한다.

Language Server는 다음 정보를 확인한다.

- DataPack Version
- Schema Version
- Generator Version

호환되지 않는 DataPack은 로드하지 않는다.

---

## 8. Security

Raw Repository는 임의의 파일을 포함할 수 있다.

Language Server는 검증된 DataPack만 로드한다.

```
DataPack

↓

Manifest Validation

↓

Checksum Validation

↓

Load
```

공격 표면(Attack Surface)을 줄일 수 있다.

---

## 9. Better Caching

DataPack은 Immutable Artifact이다.

따라서 캐시 키로 사용할 수 있다.

예시

```
SHA256(DataPack)

↓

Cache Hit

↓

Reuse
```

Repository 기반 캐시보다 훨씬 안정적이다.

---

## 10. Multi IDE Support

Language Server는 DataPack만 이해하면 된다.

동일한 DataPack을 다음 IDE에서 사용할 수 있다.

- VSCode
- IntelliJ
- Fleet
- Eclipse Theia
- Neovim
- Emacs

IDE별 Repository Parser를 구현할 필요가 없다.

---

# Loading Process

Language Server의 초기 로딩 절차

```
Initialize

↓

Read Manifest

↓

Verify Version

↓

Verify Checksums

↓

Load Registry

↓

Initialize Cache

↓

Ready
```

Grammar와 Locale은 이후 Lazy Loading한다.

---

# Runtime Lookup

모든 조회는 Registry를 기준으로 수행한다.

```
Completion Request

↓

Registry Lookup

↓

Grammar

↓

Completion Result
```

Directory Traversal은 수행하지 않는다.

---

# Update Flow

새로운 DataPack이 설치되면 다음 절차를 수행한다.

```
Detect Update

↓

Unload Old Package

↓

Load New Package

↓

Refresh Cache

↓

Ready
```

전체 Language Server를 다시 빌드하지 않는다.

---

# Cache Strategy

Language Server는 다음 항목을 캐시한다.

- Registry
- Completion
- Hover
- Definitions
- References
- Semantic Tokens

캐시는 DataPack Version을 기준으로 관리한다.

---

# Consequences

DataPack Loading Architecture를 채택함으로써 다음과 같은 장점이 있다.

- 빠른 시작
- 적은 메모리 사용
- 높은 안정성
- 검증된 데이터 사용
- IDE 독립성
- 쉬운 업데이트
- 높은 캐시 효율
- Offline 지원

단점도 존재한다.

- Generator가 반드시 필요하다.
- DataPack 생성 과정이 추가된다.
- Raw Repository를 직접 디버깅하기 어렵다.

---

# Alternatives Considered

## Raw Repository Loading

Repository를 직접 읽는다.

장점

- Package 생성이 필요 없다.

단점

- Startup이 느리다.
- Builder와 중복 로직이 발생한다.
- IDE마다 구현이 달라질 수 있다.

채택하지 않았다.

---

## Database Connection

Language Server가 중앙 데이터베이스를 조회한다.

장점

- 최신 데이터

단점

- 오프라인 사용 불가
- 서버 의존성
- 응답 지연

채택하지 않았다.

---

## Dynamic JSON Loading

요청 시마다 JSON 파일을 읽는다.

장점

- 메모리 절약

단점

- Hover 및 Completion이 느려진다.
- 반복적인 디스크 접근이 발생한다.

채택하지 않았다.

---

# Implementation Notes

Language Server는 Repository 구조를 알지 못한다.

Registry는 최초 로딩 시 메모리에 적재한다.

Grammar, Locale, Documentation은 Lazy Loading한다.

DataPack은 읽기 전용이다.

Manifest와 Checksum은 로딩 전에 반드시 검증한다.

---

# Risks

손상된 DataPack은 Language Server 오류를 유발할 수 있다.

이를 방지하기 위해 다음 정책을 적용한다.

- Manifest Validation
- Schema Validation
- Checksum Verification
- Version Compatibility Check
- Safe Fallback

---

# Related RFC

- RFC-0005 Update Protocol
- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification
- RFC-0010 Language Server Integration

---

# Related Specifications

- SPEC-0009 Versioning
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout
- SPEC-0012 Directory Structure

---

# Decision Summary

VSkript Language Server는 **Generator가 생성한 DataPack만을 입력으로 사용하는 아키텍처**를 채택한다.

이를 통해 Repository 구조와 실행 환경을 분리하고, 검증된 데이터만 사용하며, 빠른 시작 속도와 높은 캐시 효율, IDE 독립성 및 오프라인 지원을 제공한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |