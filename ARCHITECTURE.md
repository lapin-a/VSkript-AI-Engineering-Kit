# ARCHITECTURE.md

# VSkript Data Platform Architecture

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform의 전체 시스템 구조를 정의한다.

Architecture는 프로젝트를 구성하는 주요 컴포넌트와 데이터 흐름, 책임 분리를 설명하며, 구현의 기준이 되는 상위 설계 문서이다.

---

# 2. Scope

본 문서는 다음 범위를 정의한다.

- 시스템 전체 구조
- 데이터 흐름
- 컴포넌트 책임
- 데이터 저장 구조
- Distribution 구조
- Runtime 구조
- AI Integration 구조

구현 세부사항은 SPEC에서 정의한다.

---

# 3. Design Philosophy

프로젝트는 다음 원칙을 따른다.

- Specification First
- Source of Truth
- Evidence First
- Runtime Independent
- Modular Architecture
- Data-driven Design

---

# 4. High Level Architecture

```text
SkriptHub

      │

      ▼

Collector

      │

      ▼

Raw Dataset

      │

      ▼

Normalizer

      │

      ▼

Canonical Dataset

      │

      ▼

Builder

      │

      ▼

DataPack

      │

      ▼

Registry

      │

      ▼

Distribution

      │

      ▼

VSCode Extension
CLI
Language Server
Documentation Generator
Static Analyzer
```

---

# 5. Core Components

시스템은 다음 컴포넌트로 구성된다.

- Collector
- Normalizer
- Builder
- Registry
- Distribution
- Runtime

각 컴포넌트는 독립적으로 동작한다.

---

# 6. Collector

Collector는 외부 데이터를 수집한다.

책임

- SkriptHub 다운로드
- Plugin Repository 분석
- Manifest 수집
- Version 확인

Collector는 데이터를 수정하지 않는다.

---

# 7. Normalizer

Normalizer는 수집한 데이터를 Canonical Model로 변환한다.

책임

- Parser
- Validation
- Canonical Mapping
- Metadata 생성

Normalizer는 Runtime 정보를 생성하지 않는다.

---

# 8. Canonical Dataset

Canonical Dataset은 프로젝트의 공식 Source of Truth이다.

모든 Builder는 Canonical Dataset만 사용한다.

---

# 9. Builder

Builder는 Canonical Dataset을 DataPack으로 변환한다.

Builder는

- JSON 생성
- Language Pack 생성
- Manifest 생성
- Registry Entry 생성

을 수행한다.

---

# 10. Registry

Registry는 생성된 DataPack을 관리한다.

Registry는

- Identity
- Version
- Dependency
- Metadata

를 관리한다.

---

# 11. Distribution

Distribution은 사용자에게 DataPack을 제공한다.

지원 대상

- VSCode Extension
- CLI
- Documentation Generator
- Static Analyzer
- Language Server

---

# 12. Runtime

Runtime는 Registry를 통해 필요한 DataPack만 로드한다.

Runtime는

- 필요한 Addon 탐지
- Dependency 확인
- DataPack 다운로드
- Cache 관리

를 수행한다.

---

# 13. Data Flow

전체 데이터 흐름은 다음과 같다.

```text
SkriptHub

↓

Collector

↓

Raw Dataset

↓

Normalizer

↓

Canonical Dataset

↓

Builder

↓

DataPack

↓

Registry

↓

Distribution

↓

Runtime

↓

User
```

---

# 14. DataPack Architecture

하나의 DataPack은 다음 구조를 가진다.

```text
DataPack

├── Manifest
├── Grammar
├── Functions
├── Events
├── Effects
├── Expressions
├── Types
├── Classes
├── Localization
└── Metadata
```

---

# 15. Registry Architecture

```text
Registry

├── Identity
├── Version
├── Dependencies
├── Manifest
├── Metadata
└── Download Information
```

Registry는 모든 DataPack을 관리한다.

---

# 16. Runtime Architecture

```text
Workspace

↓

Script Detection

↓

Addon Detection

↓

Registry Lookup

↓

Dependency Resolution

↓

Download

↓

Cache

↓

Grammar Loading
```

Runtime는 필요한 데이터만 로드한다.

---

# 17. Dependency Model

Dependency는 Registry를 통해 관리된다.

```text
DataPack

↓

Dependency

↓

Registry

↓

Required DataPack
```

순환 Dependency는 허용하지 않는다.

---

# 18. Versioning

모든 DataPack은 Version을 가진다.

Registry는 Version을 기준으로 업데이트를 관리한다.

---

# 19. Localization

Localization은 DataPack과 독립적으로 관리된다.

지원 예시

- English
- Korean
- Japanese
- Chinese

---

# 20. AI Integration

AI는 다음 단계에서 활용된다.

- Source Analysis
- Data Validation
- Documentation Generation
- Translation
- Quality Review

AI는 Canonical Dataset을 직접 수정하지 않는다.

---

# 21. Runtime Independence

Architecture는 특정 Runtime에 의존하지 않는다.

동일한 DataPack은 다양한 Runtime에서 사용할 수 있다.

예시

- VSCode Extension
- CLI
- Static Analyzer
- Documentation Generator
- Language Server

---

# 22. Design Goals

프로젝트의 목표는 다음과 같다.

- Modular
- Maintainable
- Scalable
- Runtime Independent
- Data Driven
- AI Friendly

---

# 23. Future Extensions

향후 다음 기능을 추가할 수 있다.

- Remote Registry
- CDN Distribution
- Incremental Update
- Online Validation
- Marketplace

---

# 24. Document Information

| Item | Value |
|------|-------|
| Document | ARCHITECTURE.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |

---

# 25. Summary

VSkript Data Platform은 SkriptHub 데이터를 수집하여 Canonical Dataset으로 정규화하고, DataPack을 생성한 뒤 Registry를 통해 배포하는 데이터 중심(Data-driven) 아키텍처를 따른다.

모든 Runtime은 Registry를 통해 필요한 DataPack만 선택적으로 다운로드하고 사용할 수 있으며, 동일한 DataPack은 VSCode Extension, CLI, Language Server, Static Analyzer, Documentation Generator 등 다양한 Runtime에서 재사용될 수 있다.

Architecture는 구현보다 상위의 설계 기준으로서 프로젝트 전체의 구조와 데이터 흐름을 정의하며, 세부 구현은 Specification 문서를 기준으로 수행한다.