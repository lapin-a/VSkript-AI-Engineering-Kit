# VSkript Data Platform

> A DataPack-based grammar platform for the Skript ecosystem.

Version: 2.0 (Draft)

---

# Overview

VSkript Data Platform은 Skript 생태계의 문법 데이터를 수집, 정규화, 관리 및 배포하기 위한 데이터 플랫폼입니다.

이 프로젝트는 SkriptHub와 다양한 Addon 프로젝트의 문법 정보를 표준화하여 하나의 Canonical Dataset으로 관리하고, 사용자가 필요한 데이터만 선택적으로 다운로드하여 사용할 수 있도록 하는 것을 목표로 합니다.

단순한 JSON 저장소가 아니라 DataPack 기반의 Grammar Distribution Platform을 구축하는 것이 프로젝트의 핵심 목적입니다.

---

# Why This Project Exists

현재 Skript 생태계는 다음과 같은 문제를 가지고 있습니다.

- Addon마다 문법 데이터 형식이 다르다.
- 문법 정보가 여러 곳에 분산되어 있다.
- VSCode Extension은 필요한 데이터만 다운로드할 수 없다.
- 업데이트 추적이 어렵다.
- 다국어 지원이 제한적이다.

VSkript Data Platform은 이러한 문제를 해결하기 위해 설계되었습니다.

---

# Goals

프로젝트의 주요 목표는 다음과 같습니다.

- SkriptHub를 Source Dataset으로 사용한다.
- 데이터를 Canonical Dataset으로 정규화한다.
- DataPack 단위로 데이터를 배포한다.
- 필요한 Addon만 자동 다운로드한다.
- 변경된 데이터만 업데이트한다.
- 다양한 Runtime에서 동일한 데이터를 재사용한다.
- 다국어 Grammar Database를 구축한다.

---

# Core Features

- SkriptHub Data Collection
- Canonical Data Normalization
- DataPack Builder
- Registry Management
- Dependency Resolution
- Incremental Updates
- Localization Support
- AI-assisted Validation

---

# System Architecture

```text
SkriptHub

    │

Collector

    │

Raw Dataset

    │

Normalizer

    │

Canonical Dataset

    │

Builder

    │

DataPack

    │

Registry

    │

Distribution

    │

VSCode Extension
CLI
Language Server
Documentation Generator
Static Analyzer
```

---

# Runtime Workflow

사용자가 프로젝트를 열면 Runtime은 다음 절차를 수행합니다.

```text
Workspace Open

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

Required DataPack Download

↓

Grammar Loading

↓

Language Features Enabled
```

필요한 DataPack만 다운로드하며, 이미 설치된 DataPack은 Cache를 사용합니다.

---

# Documentation

프로젝트는 문서 중심(Document-first)으로 개발됩니다.

주요 문서는 다음과 같습니다.

- PROJECT.md
- REQUIREMENTS.md
- ARCHITECTURE.md
- WORKFLOW.md
- SPEC/
- DIRECTORY.md

Specification은 프로젝트의 공식 Source of Truth입니다.

---

# Design Principles

프로젝트는 다음 원칙을 따릅니다.

- Specification First
- Data First
- Evidence First
- Runtime Independent
- Modular Architecture
- Incremental Update
- AI Friendly

---

# Long-term Vision

VSkript Data Platform은 전 세계 Skript 사용자를 위한 표준 Grammar Database를 구축하는 것을 목표로 합니다.

사용자는 프로젝트를 열기만 하면 필요한 Addon Grammar를 자동으로 다운로드하여 사용할 수 있으며, 모든 Runtime은 동일한 DataPack을 재사용할 수 있습니다.

---

# License

License information will be defined in a future release.