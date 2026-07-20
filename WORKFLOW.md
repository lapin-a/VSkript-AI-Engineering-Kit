# WORKFLOW.md

# VSkript Data Platform Workflow

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform의 표준 개발 Workflow를 정의한다.

모든 구현은 본 Workflow를 기준으로 수행하며,
각 단계는 이전 단계의 산출물을 입력으로 사용한다.

Workflow는 프로젝트의 재현성(Reproducibility)과 일관성(Consistency)을 보장하기 위한 공식 절차이다.

---

# 2. Scope

본 문서는 다음 범위를 정의한다.

- 데이터 수집
- 데이터 정규화
- DataPack 생성
- Registry 갱신
- Runtime 배포
- 품질 검증
- 릴리즈

구현 세부사항은 각 Specification에서 정의한다.

---

# 3. Workflow Principles

Workflow는 다음 원칙을 따른다.

- Specification First
- Evidence First
- Automation First
- Incremental Processing
- Reproducible Build

---

# 4. High Level Workflow

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

Runtime

↓

Verification

↓

Release
```

---

# 5. Phase 1 — Collect

목적

외부 데이터를 수집한다.

입력

- SkriptHub
- Plugin Repository
- Plugin Manifest

출력

- Raw Dataset

---

# 6. Phase 2 — Normalize

목적

Raw Dataset을 Canonical Dataset으로 변환한다.

수행 작업

- Parsing
- Mapping
- Metadata 생성
- Canonical Model 변환

출력

- Canonical Dataset

---

# 7. Phase 3 — Validate

목적

Canonical Dataset의 품질을 검증한다.

검증 항목

- Schema Validation
- Dependency Validation
- Identity Validation
- Duplicate Detection
- Metadata Validation

출력

- Validated Dataset

---

# 8. Phase 4 — Build

목적

Validated Dataset으로부터 DataPack을 생성한다.

생성 대상

- Grammar
- Types
- Functions
- Events
- Effects
- Expressions
- Localization
- Manifest

출력

- DataPack

---

# 9. Phase 5 — Register

목적

생성된 DataPack을 Registry에 등록한다.

Registry는

- Identity
- Version
- Dependency
- Download Information
- Metadata

를 관리한다.

출력

- Registry Entry

---

# 10. Phase 6 — Distribution

목적

DataPack을 사용자에게 제공한다.

지원 대상

- VSCode Extension
- CLI
- Language Server
- Static Analyzer
- Documentation Generator

출력

- Published DataPack

---

# 11. Phase 7 — Runtime

목적

필요한 DataPack만 Runtime에서 로드한다.

Runtime는

- Workspace 분석
- Plugin 탐지
- Registry 조회
- Dependency 해결
- Download
- Cache
- Grammar Loading

을 수행한다.

출력

- Loaded DataPack

---

# 12. Phase 8 — Verification

목적

생성 결과를 검증한다.

검증 대상

- Static Validation
- Functional Validation
- Runtime Validation
- Regression Test

필요 시

- Manual Review

출력

- Verification Report

---

# 13. Phase 9 — Release

목적

검증 완료된 결과를 배포한다.

릴리즈 과정

```text
Verification

↓

Tag

↓

Package

↓

Publish

↓

Release Note

↓

Distribution
```

---

# 14. Incremental Update Workflow

변경 사항만 처리한다.

```text
Detect Change

↓

Affected Dataset

↓

Normalize

↓

Rebuild

↓

Registry Update

↓

Distribution
```

전체 재생성은 수행하지 않는다.

---

# 15. Runtime Loading Workflow

Runtime은 필요한 데이터만 다운로드한다.

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

Load Grammar
```

---

# 16. Error Handling Workflow

오류 발생 시

```text
Error

↓

Logging

↓

Rollback

↓

Report

↓

Retry
```

Recovery가 가능한 경우 자동 복구를 수행한다.

---

# 17. AI Workflow

AI는 다음 단계에서 활용된다.

- Source Analysis
- Documentation Generation
- Translation
- Data Validation
- Quality Review

AI는 Canonical Dataset을 직접 수정하지 않는다.

---

# 18. Exit Criteria

Workflow는 다음 조건을 모두 만족해야 완료된다.

- Canonical Dataset 생성 완료
- Validation 통과
- DataPack 생성 완료
- Registry 갱신 완료
- Runtime 검증 완료
- Verification Report 작성 완료

---

# 19. Future Workflow

향후 다음 Workflow를 추가할 수 있다.

- Marketplace Publishing
- CDN Synchronization
- Online Registry Update
- Remote Validation
- AI-assisted Auto Review

---

# 20. Document Information

| Item | Value |
|------|-------|
| Document | WORKFLOW.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |

---

# 21. Summary

WORKFLOW.md는 VSkript Data Platform의 표준 개발 절차를 정의한다.

모든 데이터는 Collect → Normalize → Validate → Build → Register → Distribute → Runtime → Verification → Release의 순서를 따라 처리되며, 각 단계는 독립적인 책임을 가진다.

Workflow는 프로젝트의 재현성, 자동화, 유지보수성을 보장하기 위한 공식 개발 절차로 사용된다.
```