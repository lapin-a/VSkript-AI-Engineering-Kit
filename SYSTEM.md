# SYSTEM.md

# VSkript Data Platform System

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform이 제공하는 전체 시스템을 정의한다.

System은 프로젝트를 구성하는 모든 컴포넌트와 사용자 간의 관계를 설명하며, 프로젝트의 최상위 시스템 정의 문서이다.

Architecture는 System을 구현하기 위한 내부 구조를 설명하며, 본 문서는 시스템 자체를 설명하는 것을 목적으로 한다.

---

# 2. Scope

본 문서는 다음 내용을 정의한다.

* 시스템 목적
* 시스템 구성 요소
* 사용자
* 외부 시스템
* 시스템 경계
* 주요 데이터 흐름
* Runtime 개념
* DataPack 제공 방식

세부 구현은 Architecture와 Specification 문서에서 정의한다.

---

# 3. System Goal

VSkript Data Platform의 목표는 다음과 같다.

* Skript 생태계의 문법 데이터를 표준화한다.
* Addon별 문법 데이터를 독립적으로 관리한다.
* 필요한 DataPack만 다운로드하도록 지원한다.
* 다국어 문법 데이터를 제공한다.
* AI와 개발도구가 동일한 데이터를 사용할 수 있도록 한다.

---

# 4. System Overview

VSkript Data Platform은 Skript 관련 데이터를 수집, 정제, 배포하는 데이터 플랫폼이다.

플랫폼은 다음 기능을 제공한다.

* SkriptHub 데이터 수집
* Addon 데이터 관리
* Canonical Dataset 생성
* DataPack 생성
* Registry 관리
* Runtime 다운로드
* 다국어 데이터 제공

---

# 5. External Systems

플랫폼은 다음 외부 시스템과 연동한다.

* SkriptHub
* GitHub Repository
* Plugin Repository
* Update Server
* CDN

---

# 6. Users

플랫폼의 주요 사용자는 다음과 같다.

* VSCode Extension
* Language Server
* CLI
* Documentation Generator
* Static Analyzer
* AI Agent
* Plugin Developer
* End User

---

# 7. Core Systems

플랫폼은 다음 시스템으로 구성된다.

* Collector
* Normalizer
* Builder
* Registry
* DataPack Repository
* Distribution Server
* Runtime Loader

---

# 8. Data Source

플랫폼은 다음 데이터를 수집한다.

* SkriptHub
* Addon Repository
* Plugin Source
* Documentation
* Metadata

---

# 9. Canonical Dataset

수집된 데이터는 Canonical Dataset으로 변환된다.

Canonical Dataset은 프로젝트의 공식 Source of Truth이다.

모든 DataPack은 Canonical Dataset으로부터 생성된다.

---

# 10. DataPack Repository

생성된 DataPack은 Repository에 저장된다.

Repository는 다음 정보를 관리한다.

* Version
* Manifest
* Metadata
* Download Package

Repository는 Runtime에 직접 데이터를 제공하지 않는다.

---

# 11. Registry

Registry는 DataPack 정보를 관리한다.

Registry는 다음 정보를 제공한다.

* Identity
* Version
* Dependency
* Download Location
* Metadata

Runtime는 Registry를 통해 필요한 DataPack을 찾는다.

---

# 12. Runtime

Runtime는 필요한 DataPack만 다운로드한다.

Runtime의 책임은 다음과 같다.

* Workspace 분석
* Skript Script 탐지
* Addon 탐지
* Registry 조회
* Dependency 해결
* DataPack 다운로드
* Cache 관리
* Grammar 로드

---

# 13. Download Model

DataPack은 On-demand 방식으로 제공된다.

```text
Workspace

↓

Addon Detection

↓

Registry Lookup

↓

Download

↓

Cache

↓

Grammar Loading
```

---

# 14. Localization

모든 DataPack은 Localization을 지원할 수 있다.

예시

* English
* Korean
* Japanese
* Chinese

Localization은 DataPack과 독립적으로 관리된다.

---

# 15. AI Integration

AI는 다음 작업에 플랫폼을 활용한다.

* 코드 분석
* 문법 분석
* 문서 생성
* 자동 완성
* 번역
* 검증

AI는 Canonical Dataset을 직접 수정하지 않는다.

---

# 16. Design Principles

시스템은 다음 원칙을 따른다.

* Specification First
* Source of Truth
* Evidence First
* Modular Design
* Runtime Independence
* Incremental Update
* On-demand Download

---

# 17. Future Goals

향후 다음 기능을 지원한다.

* Online Registry
* Marketplace
* Remote DataPack
* Incremental Update
* Automatic Synchronization
* Plugin Recommendation

---

# 18. Relationship

```text
External Sources

↓

Collector

↓

Canonical Dataset

↓

Builder

↓

DataPack Repository

↓

Registry

↓

Runtime

↓

User
```

---

# 19. Document Information

| Item     | Value                 |
| -------- | --------------------- |
| Document | SYSTEM.md             |
| Version  | 1.0                   |
| Status   | Draft                 |
| Owner    | VSkript Data Platform |

---

# 20. Summary

VSkript Data Platform은 Skript 생태계의 문법 데이터를 수집, 정제, 관리, 배포하는 데이터 플랫폼이다.

플랫폼은 Canonical Dataset을 공식 데이터로 유지하며, DataPack Repository와 Registry를 통해 필요한 DataPack만 Runtime에 제공한다.

이를 통해 VSCode Extension, Language Server, CLI, AI Agent 등 다양한 Runtime이 동일한 데이터를 공유하면서도 필요한 데이터만 효율적으로 사용할 수 있다.
