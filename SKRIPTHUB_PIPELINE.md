# SKRIPTHUB_PIPELINE.md

# VSkript Database

> SkriptHub Data Processing Pipeline

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 SkriptHub 원본 데이터를 VSkript Database에서 사용할 수 있는 DataPack으로 변환하는 전체 파이프라인을 정의한다.

SkriptHub는 Source of Truth이며,
VSkript Database는 이를 가공하여 안정적이고 버전 관리 가능한 JSON Database를 생성한다.

---

# 2. Objectives

Pipeline의 목표

- Raw 데이터 보존
- 자동 데이터 생성
- 변경 추적
- DataPack 생성
- Registry 생성
- Locale 생성
- Incremental Update
- Plugin Detection 지원

---

# 3. High Level Architecture

```

SkriptHub

↓

Snapshot

↓

Parser

↓

Builder

↓

Cleaner

↓

Normalizer

↓

Validator

↓

Locale Generator

↓

DataPack Generator

↓

Registry Generator

↓

Release

↓

CDN

↓

VSCode Extension

↓

Language Server

```

---

# 4. Pipeline Stages

| Stage | Description |
|---------|-------------|
| Snapshot | 원본 저장 |
| Parser | 데이터 파싱 |
| Builder | 내부 모델 생성 |
| Cleaner | 불필요 데이터 제거 |
| Normalizer | 형식 통일 |
| Validator | 구조 검증 |
| Locale Generator | 번역 생성 |
| DataPack Generator | DataPack 생성 |
| Registry Generator | Registry 생성 |
| Release | 배포 |

---

# 5. Source Stage

Source

```
SkriptHub
```

원칙

- 절대 수정하지 않는다.
- Snapshot 보관
- Version 기록
- Hash 기록

---

# 6. Snapshot

모든 업데이트는 Snapshot을 생성한다.

```
raw/

2026-07-01/

2026-08-01/

2026-09-01/

```

Snapshot은 비교 기준이 된다.

---

# 7. Parser

역할

Raw 데이터를 읽는다.

예

```
Documentation

↓

Internal Object
```

Parser는 데이터 변경을 수행하지 않는다.

---

# 8. Builder

Builder는 내부 모델을 생성한다.

예

```
Effect

↓

DatabaseEffect
```

Builder는

- Type 변환
- Mapping
- Object 생성

만 수행한다.

---

# 9. Cleaner

Cleaner는

- 중복 제거
- 빈 값 제거
- Deprecated 제거
- 잘못된 데이터 제거

를 수행한다.

---

# 10. Normalizer

모든 JSON을 동일한 구조로 만든다.

예

```
id

name

description

examples

since

deprecated

patterns

```

---

# 11. Validator

검사

- JSON Schema
- Required
- Enum
- Version
- Type

실패 시 Release하지 않는다.

---

# 12. Locale Generator

설명 문자열을 Locale로 분리한다.

```
description

↓

locale/en.json

locale/ko.json

locale/ja.json

```

Grammar JSON에는 번역을 저장하지 않는다.

---

# 13. DataPack Generator

Addon별 DataPack 생성

예

```
SkBee

↓

skbee/

manifest.json

effects.json

events.json

...

```

---

# 14. Registry Generator

Registry 생성

```
registry.json

```

Registry에는

- Version
- URL
- Manifest
- Dependencies

를 기록한다.

---

# 15. Version Tracker

이전 Snapshot과 비교한다.

```
Old

↓

New

↓

Diff

```

변경된 항목만 추출한다.

---

# 16. Incremental Update

변경된 DataPack만 재생성한다.

예

```
SkBee 변경

↓

SkBee만 Build

```

전체 Database를 다시 생성하지 않는다.

---

# 17. Release Pipeline

```
Validation

↓

Build

↓

Generate

↓

Checksum

↓

Registry

↓

Release

```

---

# 18. Distribution

향후 지원

```
GitHub Releases

↓

CDN

↓

Package Registry

```

---

# 19. Extension Flow

VSCode Extension

↓

Workspace Scan

↓

Plugin Detection

↓

Registry Download

↓

Manifest Compare

↓

Required Packs

↓

Download

↓

Cache

↓

Load

---

# 20. Plugin Detection

Workspace

```
plugins/

```

탐색

↓

```
SkBee.jar

SkQuery.jar

...

```

↓

Registry 검색

↓

DataPack 설치

---

# 21. Cache Strategy

```
cache/

registry.json

installed.json

packs/

```

Cache는

- Version
- Checksum
- Installed Date

를 관리한다.

---

# 22. Update Strategy

Registry 비교

↓

Manifest 비교

↓

Checksum 비교

↓

Changed Pack Download

↓

Reload

---

# 23. Error Handling

Pipeline 실패 시

- Build 중단
- Error Report 생성
- Release 중단

Validation 실패 데이터를 배포하지 않는다.

---

# 24. Future Pipeline

향후 추가

- AI Translation
- AI Validation
- Binary Index
- CDN Edge Cache
- Parallel Builder
- Incremental Locale Update

---

# 25. Quality Gates

각 Stage는 반드시 통과해야 한다.

| Stage | Required |
|---------|----------|
| Parser | ✔ |
| Builder | ✔ |
| Cleaner | ✔ |
| Validator | ✔ |
| Locale | ✔ |
| Generator | ✔ |
| Registry | ✔ |

---

# 26. Metrics

Pipeline 목표

- Incremental Build
- Zero Invalid JSON
- 100% Manifest Generation
- 100% Registry Generation
- Locale Separation

---

# 27. Summary

SkriptHub Pipeline은 SkriptHub 원본 데이터를 기반으로 VSkript Database를 자동 생성하는 전체 데이터 처리 시스템이다.

Pipeline은 Raw Snapshot을 보존하면서 Builder, Cleaner, Normalizer, Validator를 거쳐 애드온별 DataPack과 Registry를 생성하며, VSCode Extension은 Plugin Detection을 통해 필요한 DataPack만 다운로드하여 사용한다.

이 구조는 변경 사항만 처리하는 Incremental Update와 다국어 데이터 관리, 향후 CDN 및 Package Manager 기반 배포까지 고려한 확장 가능한 아키텍처를 제공한다.

---

# 28. Document Information

| Item | Value |
|------|-------|
| Document | SKRIPTHUB_PIPELINE.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |