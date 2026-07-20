# PROJECT.md

# VSkript Data Platform

Version: 2.0 (Draft)

---

# 1. Vision

전 세계 Skript 생태계를 위한 표준 Grammar Data Platform을 구축한다.

모든 Skript Runtime은 동일한 Grammar DataPack을 재사용하며, 필요한 데이터만 선택적으로 다운로드하여 사용할 수 있어야 한다.

---

# 2. Mission

VSkript Data Platform의 목적은 SkriptHub 및 다양한 Addon 프로젝트의 문법 데이터를 수집하고, 이를 표준화된 Canonical Dataset으로 관리하여 DataPack 형태로 배포하는 것이다.

이를 통해 유지보수 가능하고 확장 가능한 Grammar Distribution Platform을 제공한다.

---

# 3. Background

현재 Skript 생태계는 다음과 같은 문제를 가지고 있다.

- 문법 데이터가 여러 저장소에 분산되어 있다.
- Addon마다 데이터 형식이 다르다.
- 업데이트 추적이 어렵다.
- VSCode Extension이 전체 데이터를 포함해야 한다.
- 다국어 지원이 제한적이다.

본 프로젝트는 이러한 문제를 해결하기 위해 시작되었다.

---

# 4. Problem Statement

현재 시스템에서는 다음 문제가 존재한다.

- 필요한 Addon만 다운로드할 수 없다.
- 변경 사항만 업데이트할 수 없다.
- 문법 데이터의 표준 모델이 없다.
- Runtime마다 데이터를 중복 관리한다.

---

# 5. Goals

프로젝트의 목표는 다음과 같다.

- SkriptHub 데이터를 수집한다.
- Canonical Dataset을 구축한다.
- DataPack을 생성한다.
- Registry를 구축한다.
- Runtime에서 필요한 DataPack만 다운로드한다.
- 변경된 데이터만 업데이트한다.
- 다국어 Grammar Database를 구축한다.

---

# 6. Non Goals

본 프로젝트는 다음을 목표로 하지 않는다.

- Skript Runtime 구현
- Skript Compiler 개발
- Plugin Loader 개발
- SkriptHub 자체 운영

---

# 7. Target Users

본 프로젝트의 대상은 다음과 같다.

- VSkript Users
- VSCode Extension Users
- Language Server Developers
- Static Analysis Tools
- Documentation Generator
- AI-assisted Development Tools

---

# 8. Scope

프로젝트는 다음 범위를 포함한다.

- Data Collection
- Data Normalization
- Canonical Dataset
- DataPack Generation
- Registry
- Distribution
- Runtime Loading
- Localization
- Version Management

---

# 9. Success Criteria

프로젝트는 다음 조건을 만족해야 한다.

- 필요한 DataPack만 다운로드된다.
- 모든 Grammar는 Canonical Dataset으로 관리된다.
- 변경 사항만 업데이트된다.
- Runtime은 Registry만 참조한다.
- 동일한 DataPack을 여러 Runtime에서 재사용한다.

---

# 10. Core Principles

프로젝트는 다음 원칙을 따른다.

- Specification First
- Source of Truth
- Evidence First
- Runtime Independent
- Data-driven Design
- Modular Architecture
- AI Friendly

---

# 11. Expected Outcomes

프로젝트가 완료되면 다음이 가능해야 한다.

- Skript 파일을 열면 필요한 Addon이 자동 탐지된다.
- 필요한 Grammar DataPack만 다운로드된다.
- 업데이트는 변경된 데이터만 수행된다.
- 여러 Runtime이 동일한 DataPack을 공유한다.
- 전 세계 사용자가 동일한 Grammar Database를 사용할 수 있다.

---

# 12. Long-term Roadmap

장기적으로 다음 기능을 목표로 한다.

- Remote Registry
- CDN Distribution
- Incremental Synchronization
- AI-assisted Documentation
- Multi-language Grammar Database
- Marketplace Integration

---

# 13. Project Philosophy

VSkript Data Platform은 코드 중심 프로젝트가 아니라 데이터 중심(Data-driven) 플랫폼이다.

모든 Runtime은 Registry와 Canonical Dataset을 기반으로 동작하며, 구현보다 Specification을 우선한다.

---

# 14. Document Information

| Item | Value |
|------|-------|
| Document | PROJECT.md |
| Version | 2.0 |
| Status | Draft |
| Owner | VSkript Data Platform |

---

# 15. Summary

VSkript Data Platform은 SkriptHub 데이터를 표준화하여 Canonical Dataset으로 관리하고, DataPack 기반으로 배포하는 Grammar Distribution Platform이다.

최종 목표는 사용자가 프로젝트를 열면 필요한 Addon만 자동으로 탐지하고 필요한 Grammar DataPack만 다운로드하여 사용할 수 있는 자동화된 다국어 데이터 플랫폼을 구축하는 것이다.