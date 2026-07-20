# PROJECT.md

# VSkript Data Platform

Version: 1.0

Status: Draft

---

# Vision

VSkript Data Platform은 전 세계 Skript 사용자들이 필요한 문법 데이터를 언제 어디서나 빠르고 정확하게 사용할 수 있도록 지원하는 표준 데이터 플랫폼을 구축하는 것을 목표로 한다.

본 프로젝트는 Skript 애드온의 문법 정보를 수집, 정제, 표준화, 배포하는 전체 과정을 자동화하여 누구나 동일한 품질의 데이터를 사용할 수 있도록 하는 것을 비전으로 한다.

---

# Mission

본 프로젝트의 목적은 다음과 같다.

- Skript 생태계의 문법 데이터를 표준화한다.
- 애드온별 Grammar Data를 독립적으로 관리한다.
- 변경 사항만 추적하여 효율적으로 업데이트한다.
- 필요한 데이터만 선택적으로 다운로드할 수 있도록 한다.
- AI와 사람이 동일한 Specification을 기반으로 작업할 수 있도록 한다.

---

# Background

현재 Skript 생태계에는 다양한 애드온이 존재하지만,

- 문법 데이터가 분산되어 있으며,
- 버전 관리가 어렵고,
- 업데이트 추적이 어렵고,
- 다국어 지원이 부족하며,
- 필요한 애드온만 선택적으로 사용할 수 없는 문제가 존재한다.

VSkript Data Platform은 이러한 문제를 해결하기 위한 프로젝트이다.

---

# Goals

본 프로젝트의 주요 목표는 다음과 같다.

## 1. Grammar Collection

Skript 및 모든 애드온의 문법 데이터를 자동으로 수집한다.

---

## 2. Data Normalization

수집한 데이터를 프로젝트 표준 구조로 변환한다.

---

## 3. Version Tracking

변경된 데이터만 추적하여 업데이트한다.

---

## 4. DataPack Generation

애드온 단위의 DataPack을 생성한다.

예시

```
skript.json

skbee.json

skript-reflect.json

tuSKe.json

...
```

---

## 5. Runtime Distribution

사용자는 필요한 DataPack만 다운로드하여 사용할 수 있다.

---

## 6. Multilingual Grammar Database

모든 Grammar Data는 다국어 지원을 목표로 한다.

예시

- English
- Korean
- Japanese
- Chinese

---

# Non Goals

본 프로젝트는 다음을 목표로 하지 않는다.

- Skript Runtime 구현
- Skript Interpreter 개발
- IDE 개발
- Compiler 개발
- Plugin Loader 개발

---

# Core Objectives

프로젝트는 다음 핵심 목표를 가진다.

- Standardization
- Automation
- Modularity
- Maintainability
- Extensibility
- Reusability

---

# Target Users

본 프로젝트의 주요 사용자는 다음과 같다.

- VSkript Users
- Skript Plugin Developers
- VSCode Extension Users
- Language Server Developers
- Documentation Generator
- AI-assisted Developers

---

# Core Components

프로젝트는 다음 주요 컴포넌트로 구성된다.

```
Collector

↓

Normalizer

↓

Registry

↓

Builder

↓

DataPack

↓

Distribution

↓

Runtime
```

각 컴포넌트는 하나의 책임만 수행한다.

---

# High-Level Architecture

전체 시스템은 다음과 같은 흐름으로 동작한다.

```
SkriptHub

↓

Collector

↓

Raw Data

↓

Normalizer

↓

Registry

↓

Builder

↓

DataPack

↓

Distribution

↓

User Runtime
```

---

# Design Principles

본 프로젝트는 다음 원칙을 따른다.

- Specification First
- Evidence First
- Registry First
- Data First
- Modular Architecture
- Incremental Update
- Runtime Independence
- Deterministic Build

---

# Source of Truth

프로젝트의 공식 Source of Truth는 Specification과 Registry이다.

Runtime 또는 Generated Artifact는 Source of Truth가 아니다.

```
Specification

↓

Registry

↓

Generated DataPack
```

---

# Relationship with AI Engineering Kit

VSkript AI Engineering Kit는 본 프로젝트를 구축하기 위한 Engineering Framework이다.

역할은 다음과 같이 구분된다.

| Project | Role |
|----------|------|
| VSkript AI Engineering Kit | 개발 방법론 |
| VSkript Data Platform | 실제 구현 대상 |

AI Engineering Kit는 프로젝트의 분석, 설계, 구현, 검증 과정을 표준화하며,

VSkript Data Platform은 그 방법론을 적용하여 구축되는 실제 시스템이다.

---

# Long-term Roadmap

장기적으로 다음 기능을 목표로 한다.

- SkriptHub 자동 동기화
- Incremental Update
- DataPack Repository
- Package Manifest
- Runtime Downloader
- Online Registry
- Language Pack
- AI Search Support
- Documentation Generator
- Offline Cache

---

# Success Criteria

프로젝트는 다음 조건을 만족할 때 목표를 달성한 것으로 본다.

- 모든 주요 Skript 애드온의 문법 데이터를 수집할 수 있다.
- 변경 사항만 자동으로 추적할 수 있다.
- 애드온별 DataPack을 생성할 수 있다.
- 필요한 DataPack만 선택적으로 다운로드할 수 있다.
- 다국어 문법 데이터를 제공할 수 있다.
- AI와 사람이 동일한 Specification을 기반으로 작업할 수 있다.

---

# Document Information

| Item | Value |
|------|-------|
| Document | PROJECT.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |

---

# Summary

VSkript Data Platform은 Skript 생태계의 문법 데이터를 수집, 정제, 표준화, 버전 관리 및 배포하기 위한 데이터 플랫폼이다.

본 프로젝트는 AI Engineering Kit를 기반으로 구축되며, 전 세계 VSkript 사용자가 필요한 DataPack만 효율적으로 다운로드하고 활용할 수 있는 자동화된 다국어 Grammar Database를 제공하는 것을 최종 목표로 한다.