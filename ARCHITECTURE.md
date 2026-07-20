# ARCHITECTURE.md

# VSkript Data Platform Architecture

Version: 2.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform의 전체 시스템 아키텍처를 정의한다.

Architecture는 구현보다 상위 개념의 설계 기준이며,
모든 Specification과 구현은 본 문서를 따른다.

---

# 2. Core Architecture

전체 시스템은 다음 계층으로 구성된다.

```text
External Sources

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

Applications
```

---

# 3. External Sources

지원되는 외부 데이터 소스는 다음과 같다.

- SkriptHub
- Plugin Repository
- Official Addons
- Community Addons

---

# 4. Collector

Collector는 외부 데이터를 수집한다.

책임

- Download
- Version Check
- Manifest Collection
- Source Tracking

Collector는 데이터를 수정하지 않는다.

---

# 5. Raw Dataset

Raw Dataset은 수집된 원본 데이터이다.

Raw Dataset은 Source Backup 역할을 수행한다.

---

# 6. Normalizer

Normalizer는 Raw Dataset을 Canonical Dataset으로 변환한다.

책임

- Parsing
- Validation
- Canonical Mapping
- Metadata Generation

---

# 7. Canonical Dataset

Canonical Dataset은 프로젝트의 Source of Truth이다.

모든 Builder는 Canonical Dataset만 사용한다.

---

# 8. Builder

Builder는 Canonical Dataset을 DataPack으로 생성한다.

생성 대상

- Grammar
- Functions
- Events
- Effects
- Expressions
- Types
- Classes
- Localization
- Manifest

---

# 9. DataPack

DataPack은 독립적으로 배포 가능한 데이터 단위이다.

```text
DataPack

├── Manifest
├── Grammar
├── Localization
├── Metadata
└── Version
```

---

# 10. Registry

Registry는 모든 DataPack을 관리한다.

관리 정보

- Identity
- Version
- Dependencies
- Download URL
- Metadata

---

# 11. Distribution

Distribution은 DataPack을 사용자에게 제공한다.

지원 대상

- VSCode Extension
- CLI
- Language Server
- Static Analyzer
- Documentation Generator

---

# 12. Runtime

Runtime는 필요한 DataPack만 다운로드한다.

```text
Workspace

↓

Script Detection

↓

Plugin Detection

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

# 13. Cache

다운로드된 DataPack은 Cache에 저장된다.

동일 Version은 다시 다운로드하지 않는다.

---

# 14. Dependency Model

Dependency는 Registry를 통해 관리된다.

```text
DataPack

↓

Dependencies

↓

Registry

↓

Required DataPack
```

순환 Dependency는 허용하지 않는다.

---

# 15. Version Model

모든 DataPack은 Version을 가진다.

업데이트는 Version 비교를 통해 수행한다.

---

# 16. Localization

Localization은 DataPack과 분리되어 관리된다.

지원 언어

- English
- Korean
- Japanese
- Chinese

---

# 17. AI Integration

AI는 다음 영역에서 활용된다.

- Validation
- Documentation
- Translation
- Review

AI는 Canonical Dataset을 직접 수정하지 않는다.

---

# 18. Design Principles

Architecture는 다음 원칙을 따른다.

- Specification First
- Source of Truth
- Data Driven
- Modular
- Runtime Independent
- AI Friendly

---

# 19. Long-term Architecture

향후 지원 예정

- Remote Registry
- CDN Distribution
- Marketplace
- Incremental Sync
- Online Validation

---

# 20. Document Information

| Item | Value |
|------|-------|
| Document | ARCHITECTURE.md |
| Version | 2.0 |
| Status | Draft |
| Owner | VSkript Data Platform |

---

# 21. Summary

VSkript Data Platform은 SkriptHub 데이터를 Canonical Dataset으로 정규화한 뒤 DataPack으로 생성하고,
Registry를 통해 배포하는 데이터 중심(Data-driven) 플랫폼이다.

모든 Runtime은 Registry를 통해 필요한 DataPack만 선택적으로 다운로드하여 동일한 Grammar 데이터를 재사용한다.