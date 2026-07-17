# RFC-0010_LANGUAGE_SERVER_INTEGRATION.md

# VSkript Language Server Integration

> Language Server Protocol Integration Specification

RFC: RFC-0010

Version: 1.0

Status: Draft

---

# 1. Purpose

본 RFC는 VSkript Database와 Language Server(LSP) 간의 통합 구조를 정의한다.

Language Server는 Builder와 Generator가 생성한 DataPack을 사용하여 코드 완성, Hover, Definition, Reference, Diagnostic 등 IDE 기능을 제공한다.

본 문서는 VSCode Extension뿐 아니라 JetBrains IDE, Neovim, Eclipse Theia 등 Language Server Protocol을 지원하는 모든 클라이언트에 적용된다.

---

# 2. Goals

Language Server의 목표는 다음과 같다.

- DataPack 기반 IntelliSense
- Hover 제공
- Go To Definition
- Find References
- Signature Help
- Semantic Tokens
- Diagnostics
- Code Actions
- Rename Support
- Workspace Symbol Search
- Incremental Update 지원

---

# 3. Scope

본 RFC는 다음 내용을 정의한다.

- LSP Architecture
- DataPack Loading
- Workspace Management
- Synchronization
- Feature Integration
- Performance
- Cache
- Diagnostics

---

# 4. Terminology

| Term | Description |
|------|-------------|
| LSP | Language Server Protocol |
| Client | VSCode Extension |
| Server | VSkript Language Server |
| Workspace | 열린 프로젝트 |
| DataPack | Generator가 생성한 패키지 |
| Registry | Resource Index |

---

# 5. Design Principles

Language Server는 다음 원칙을 따른다.

- DataPack First
- Read Only Database
- Incremental Workspace
- Stateless Requests
- Lazy Loading
- Cached Resources
- Deterministic Response

---

# 6. Overall Architecture

```
VSCode

↓

Extension

↓

Language Client

↓

Language Server

↓

Registry

↓

DataPack

↓

Grammar

↓

Locale

↓

Types
```

모든 기능은 DataPack을 기반으로 수행된다.

---

# 7. Initialization

Language Server는 시작 시 다음 순서를 따른다.

```
Initialize

↓

Load Configuration

↓

Load Registry

↓

Load Installed DataPacks

↓

Load Workspace

↓

Ready
```

초기화가 완료되기 전에는 요청을 처리하지 않는다.

---

# 8. DataPack Loading

Language Server는 Generator가 생성한 DataPack만 로드한다.

지원 대상

```
manifest.json

registry.json

grammar/

locale/

types/
```

Raw Source는 직접 읽지 않는다.

---

# 9. Workspace Management

Workspace가 변경되면 다음 순서를 수행한다.

```
Workspace Changed

↓

Detect Changes

↓

Reload Incrementally

↓

Update Cache

↓

Refresh Features
```

전체 Reload는 가능한 한 피해야 한다.

---

# 10. Registry Integration

Registry는 모든 Resource 검색의 기준이다.

Language Server는 Registry를 사용하여

- Symbol 검색
- Type 검색
- Plugin 검색
- Definition 검색

을 수행한다.

---

# 11. Completion

Completion은 Grammar Resource를 기반으로 생성한다.

지원 항목

- Effects
- Events
- Expressions
- Conditions
- Sections
- Functions
- Types

Completion은 Context-Aware 방식이어야 한다.

---

# 12. Hover

Hover는 Locale Resource를 사용한다.

표시 내용

- Display Name
- Description
- Version
- Plugin
- Documentation Link
- Deprecated 여부

---

# 13. Go To Definition

Definition Provider는 Registry ID를 기준으로 동작한다.

```
Cursor

↓

Grammar

↓

Registry

↓

Definition
```

Definition은 항상 Source 위치를 반환해야 한다.

---

# 14. Find References

Reference Provider는 Registry를 이용하여 참조를 검색한다.

검색 대상

- Workspace
- DataPack
- Imported Packages

---

# 15. Signature Help

Signature Help는 Grammar Pattern과 Type 정보를 기반으로 생성한다.

지원 내용

- Parameter
- Return Type
- Documentation
- Example

---

# 16. Semantic Tokens

Semantic Highlighting 지원

지원 종류

- Event
- Effect
- Expression
- Condition
- Function
- Type
- Variable
- Keyword

---

# 17. Diagnostics

Language Server는 Validator Diagnostic을 그대로 사용한다.

```
Validator

↓

Diagnostic

↓

Language Server

↓

VSCode
```

Error Code는 RFC-0008을 따른다.

---

# 18. Code Actions

지원 기능

- Import Plugin
- Replace Deprecated Syntax
- Quick Fix
- Add Missing Dependency
- Update Version

---

# 19. Rename

Rename는 Registry 기반으로 수행한다.

```
Rename

↓

Registry Lookup

↓

Workspace Update
```

Broken Reference가 발생해서는 안 된다.

---

# 20. Symbol Search

지원

- Workspace Symbol
- Global Symbol
- Plugin Symbol

검색은 Registry Index를 사용한다.

---

# 21. Incremental Update

Workspace 변경 시

```
Changed File

↓

Incremental Parse

↓

Incremental Cache Update

↓

Incremental Diagnostics
```

전체 Index를 다시 생성하지 않는다.

---

# 22. Cache

Cache 대상

- Registry
- Grammar
- Locale
- Completion
- Hover
- Definition

---

# 23. Performance

권장 목표

| Metric | Target |
|---------|--------|
| Startup | ≤ 2초 |
| Completion | ≤ 30ms |
| Hover | ≤ 20ms |
| Definition | ≤ 20ms |
| Reference | ≤ 100ms |

---

# 24. Memory Management

Language Server는

- Lazy Loading
- Shared Cache
- Weak Reference
- Resource Pool

사용을 권장한다.

---

# 25. Builder Integration

Builder가 새로운 DataPack을 생성하면

```
Builder

↓

DataPack

↓

Language Server Reload
```

Reload는 Incremental하게 수행한다.

---

# 26. Validator Integration

Validator Diagnostic을 그대로 사용한다.

Language Server는 자체 Error Code를 만들지 않는다.

---

# 27. Generator Integration

Generator Output은 Language Server의 유일한 입력이다.

Raw Resource를 직접 읽지 않는다.

---

# 28. Extension Integration

VSCode Extension은 Language Client 역할을 수행한다.

Language Server와는 JSON-RPC(LSP)를 통해 통신한다.

---

# 29. Security Considerations

Language Server는

- 신뢰되지 않은 DataPack 차단
- Manifest 검증
- Checksum 검증
- Path Traversal 방지

를 수행해야 한다.

---

# 30. Future Compatibility

향후 지원

- Multiple Workspace
- Remote DataPack
- Marketplace
- Dynamic Plugin Loading
- AI Completion Integration

---

# 31. Summary

Language Server는 VSkript Database 생태계의 최종 소비자(Consumer)이다.

Builder가 생성하고 Validator가 검증하며 Generator가 패키징한 DataPack을 기반으로 IntelliSense, Hover, Definition, Reference, Diagnostics 등 모든 IDE 기능을 제공한다.

Language Server는 DataPack을 유일한 데이터 소스로 사용하며, Incremental Update와 Registry 기반 검색을 통해 높은 성능과 일관성을 유지해야 한다.

---

# Related RFC

- RFC-0001 Manifest Schema
- RFC-0002 Registry Schema
- RFC-0003 Grammar Schema
- RFC-0005 Update Protocol
- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification

---

# Related Specifications

- SPEC-0008 JSON Schema Guide
- SPEC-0009 Versioning
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout
- SPEC-0012 Directory Structure

---

# Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0010 |
| Title | Language Server Integration |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Depends On | RFC-0009 |
| Final Architecture RFC | Yes |
| Last Updated | YYYY-MM-DD |