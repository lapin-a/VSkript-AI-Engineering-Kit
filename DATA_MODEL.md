# DATA_MODEL.md

# VSkript Data Platform Data Model

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# Part 1. Overview

# 1. Purpose

본 문서는 VSkript Data Platform에서 사용하는 데이터 모델을 정의한다.

Data Model은 프로젝트의 모든 데이터 구조와 관계를 설명하는 공식 설계 문서이며,
Collector, Normalizer, Builder, Registry, Runtime 및 DataPack은 본 문서를 기준으로 구현된다.

---

# 2. Scope

본 문서는 다음 범위를 정의한다.

- Source Dataset
- Raw Dataset
- Canonical Dataset
- DataPack
- Registry Entry
- Runtime Cache
- Manifest
- Metadata
- Localization

구현 세부사항은 각 Specification 문서에서 정의한다.

---

# 3. Design Philosophy

Data Model은 다음 원칙을 따른다.

- Specification First
- Source of Truth
- Identity First
- Immutable Canonical Dataset
- Data-driven Design
- Runtime Independent

---

# Part 2. Data Flow

# 4. Overall Flow

```text
SkriptHub
Plugin Repository

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

Runtime

↓

User
```

---

# 5. Data Lifecycle

모든 데이터는 다음 생명주기를 가진다.

```text
Collect

↓

Normalize

↓

Validate

↓

Build

↓

Register

↓

Distribute

↓

Load

↓

Cache

↓

Update
```

---

# Part 3. Core Data Models

# 6. Raw Dataset

Raw Dataset은 외부에서 수집한 원본 데이터이다.

특징

- 수정하지 않는다.
- 원본 형태를 유지한다.
- Source 추적이 가능해야 한다.

---

# 7. Canonical Dataset

Canonical Dataset은 프로젝트의 공식 Source of Truth이다.

모든 Builder는 Canonical Dataset만 사용한다.

Canonical Dataset은

- Immutable
- Normalized
- Validated

상태를 유지해야 한다.

---

# 8. DataPack

DataPack은 Runtime에서 사용하는 배포 단위이다.

하나의 DataPack은 하나의 Addon을 나타낸다.

예시

```text
SkBee

SkQuery

skript-reflect
```

---

# 9. Registry Entry

Registry Entry는 DataPack의 메타데이터를 관리한다.

포함 정보

- Identity
- Version
- Dependency
- Manifest
- Download Information
- Metadata

---

# 10. Runtime Cache

Runtime Cache는 다운로드한 DataPack을 저장한다.

Runtime는 Registry를 조회한 후 필요한 DataPack만 Cache에 저장한다.

---

# Part 4. Identity Model

# 11. Identity

모든 DataPack은 고유 Identity를 가진다.

Identity는 프로젝트 전체에서 유일해야 한다.

예시

```text
skbee

skript-reflect

skquery
```

---

# 12. Version

모든 DataPack은 Version을 가진다.

Version은 Registry를 통해 관리한다.

---

# 13. Dependency

DataPack은 다른 DataPack에 의존할 수 있다.

예시

```text
Addon

↓

Dependency

↓

Required Addon
```

순환 Dependency는 허용하지 않는다.

---

# Part 5. DataPack Model

# 14. DataPack Structure

```text
DataPack

├── Manifest
├── Grammar
├── Classes
├── Types
├── Functions
├── Events
├── Effects
├── Expressions
├── Localization
└── Metadata
```

---

# 15. Manifest

Manifest는 DataPack의 정보를 정의한다.

예시

- Name
- Version
- Identity
- Dependencies
- Author
- Source

---

# 16. Grammar

Grammar는 Runtime이 사용하는 문법 데이터이다.

예시

- Completion
- Hover
- Signature
- Definition

---

# 17. Localization

Localization은 Grammar와 분리하여 관리한다.

지원 예정

- English
- Korean
- Japanese
- Chinese

---

# Part 6. Registry Model

# 18. Registry

Registry는 모든 DataPack을 관리한다.

Registry는

- Identity
- Version
- Dependency
- Metadata

를 기준으로 검색한다.

---

# 19. Registry Responsibilities

Registry의 책임은 다음과 같다.

- 등록
- 조회
- 버전 관리
- Dependency 관리
- Update 확인

---

# Part 7. Runtime Model

# 20. Runtime

Runtime는 필요한 DataPack만 다운로드한다.

전체 DataPack을 다운로드하지 않는다.

---

# 21. Runtime Flow

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

---

# 22. Cache

Cache는 Runtime 성능 향상을 위해 사용한다.

Cache는 Registry Version을 기준으로 갱신된다.

---

# Part 8. Relationships

# 23. Model Relationship

```text
Raw Dataset

↓

Canonical Dataset

↓

DataPack

↓

Registry

↓

Runtime
```

---

# 24. Component Relationship

```text
Collector

↓

Normalizer

↓

Builder

↓

Registry

↓

Runtime
```

---

# Part 9. Design Goals

# 25. Goals

Data Model은 다음 목표를 가진다.

- 확장 가능
- 유지보수 가능
- Runtime 독립
- Data 중심
- AI 친화적

---

# 26. Future Extensions

향후 다음 기능을 추가할 수 있다.

- Remote Registry
- CDN
- Incremental Update
- Online Validation
- Marketplace

---

# Document Information

| Item | Value |
|------|-------|
| Document | DATA_MODEL.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |

---

# Summary

DATA_MODEL은 VSkript Data Platform의 공식 데이터 모델을 정의한다.

모든 데이터는 Raw Dataset에서 시작하여 Canonical Dataset으로 정규화되고, DataPack으로 빌드된 후 Registry를 통해 배포된다.

Runtime는 Registry를 이용하여 필요한 DataPack만 다운로드하고 캐시에 저장하며, 동일한 DataPack은 다양한 Runtime 환경에서 재사용될 수 있다.