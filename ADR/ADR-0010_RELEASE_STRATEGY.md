# ADR-0010_RELEASE_STRATEGY.md

# ADR-0010: Release Strategy

Status: Accepted

Date: YYYY-MM-DD

Authors:

- VSkript Database Team

---

# Context

VSkript Database는 Builder, Validator, Generator를 통해 생성된 DataPack을 다양한 Consumer에게 배포한다.

Consumer는 다음과 같다.

- VSCode Extension
- Language Server
- CLI
- CI/CD Pipeline
- Marketplace
- Third-party Tools

프로젝트는 장기적으로 다양한 Plugin과 DataPack을 지속적으로 배포해야 하므로 일관된 Release 정책이 필요하다.

초기 설계에서는 다음 Release 방식을 검토하였다.

- Manual Release
- Git Tag 기반 Release
- Continuous Release
- Scheduled Release
- Hybrid Release

또한 다음 사항을 함께 고려하였다.

- Semantic Versioning
- DataPack Version
- Schema Version
- Rollback
- Backward Compatibility
- CI/CD Automation

---

# Decision

VSkript Database는 **Automated Semantic Release Strategy**를 채택한다.

모든 Release는 CI/CD Pipeline을 통해 자동 생성되며, Semantic Versioning(SemVer)을 따른다.

```
MAJOR.MINOR.PATCH
```

Release는 Git Tag를 기준으로 생성한다.

---

# Release Principles

모든 Release는 다음 원칙을 따른다.

- Reproducible
- Immutable
- Automated
- Versioned
- Traceable
- Verifiable
- Rollback Supported

---

# Architecture

```
Developer

↓

Pull Request

↓

Review

↓

Merge

↓

CI

↓

Builder

↓

Validator

↓

Generator

↓

DataPack

↓

GitHub Release

↓

Marketplace

↓

Language Server
```

---

# Release Types

## PATCH

예시

```
1.0.0

↓

1.0.1
```

대상

- Bug Fix
- Documentation
- Metadata
- Validation Rule 수정

Schema 변경 없음.

---

## MINOR

예시

```
1.2.0

↓

1.3.0
```

대상

- 새로운 Grammar
- 새로운 Plugin
- 새로운 Type
- 새로운 Locale
- Optional Field 추가

기존 DataPack과 호환된다.

---

## MAJOR

예시

```
1.x

↓

2.0
```

대상

- Breaking Change
- Schema 변경
- Required Field 변경
- Resource 구조 변경

Migration이 필요하다.

---

# Version Policy

프로젝트는 다음 Version을 독립적으로 관리한다.

- Repository Version
- Builder Version
- Validator Version
- Generator Version
- Schema Version
- Plugin Version
- DataPack Version

Version은 서로 독립적이다.

---

# Release Pipeline

```
Merge

↓

Build

↓

Validation

↓

Tests

↓

Package

↓

Checksums

↓

Publish

↓

Release Notes
```

Validation 실패 시 Release를 중단한다.

---

# Build Requirements

Release 전에 반드시 수행한다.

- Full Build
- Full Validation
- Dependency Validation
- Schema Validation
- Registry Validation

Incremental Build는 Release에 사용하지 않는다.

---

# Release Artifacts

하나의 Release는 다음 결과물을 포함한다.

```
DataPack

Manifest

Registry

Checksums

Build Report

Validation Report

Release Notes
```

---

# Release Notes

Release마다 자동 생성한다.

포함 내용

- Version
- Added
- Changed
- Deprecated
- Removed
- Fixed
- Security

Release Note는 Conventional Commits 기반으로 생성할 수 있다.

---

# Git Strategy

Release는 Git Tag를 기준으로 생성한다.

예시

```
v1.2.0

v1.3.0

v2.0.0
```

Branch 이름은 Release Version이 아니다.

---

# CI/CD Integration

Release Pipeline은 자동으로 수행한다.

```
Push Tag

↓

CI

↓

Builder

↓

Validator

↓

Generator

↓

Publish
```

수동 Release는 권장하지 않는다.

---

# DataPack Publication

Release된 DataPack은 다음 위치에 게시할 수 있다.

- GitHub Releases
- Marketplace
- Enterprise Repository
- CDN
- Local Mirror

배포 대상은 Generator Output이다.

---

# Rollback Strategy

문제가 발생한 경우 이전 Release로 복원한다.

```
Latest

↓

Rollback

↓

Previous Version
```

Rollback은 DataPack 단위로 수행한다.

Repository를 되돌리지 않는다.

---

# Compatibility Policy

Language Server는 다음 Version을 확인한다.

- DataPack Version
- Schema Version
- Manifest Version

호환되지 않는 Package는 로드하지 않는다.

---

# Long-Term Support (LTS)

주요 Release는 LTS로 지정할 수 있다.

예시

```
2.0 LTS
```

LTS는

- Bug Fix
- Security Patch

만 제공한다.

새로운 기능은 추가하지 않는다.

---

# Security

Release 전에 다음 검사를 수행한다.

- Schema Validation
- Checksum Generation
- Dependency Verification
- Manifest Verification
- Registry Validation

손상된 Package는 배포하지 않는다.

---

# Consequences

Automated Release Strategy를 채택함으로써 다음과 같은 장점이 있다.

- 일관된 Version 관리
- 자동화된 Release
- 높은 신뢰성
- 쉬운 Rollback
- Marketplace 대응
- CI/CD 친화적
- 검증된 DataPack 배포

단점도 존재한다.

- CI/CD 의존성이 증가한다.
- Release Pipeline 관리가 필요하다.
- 자동화 실패 시 배포가 지연될 수 있다.

---

# Alternatives Considered

## Manual Release

개발자가 직접 Release를 수행한다.

장점

- 구현이 단순하다.

단점

- 사람의 실수 가능성
- 일관성 부족

채택하지 않았다.

---

## Continuous Deployment

모든 Merge를 즉시 Release한다.

장점

- 빠른 배포

단점

- 검증 시간이 부족하다.
- 안정성이 떨어질 수 있다.

채택하지 않았다.

---

## Scheduled Release

정기적으로 Release한다.

장점

- 예측 가능

단점

- 긴급 수정 배포가 어렵다.

채택하지 않았다.

---

# Implementation Notes

Release는 Generator Output만 배포한다.

Release Artifact는 Immutable이어야 한다.

모든 Release는 Git Tag와 연결되어야 한다.

Release Notes는 자동 생성한다.

Checksum은 모든 Artifact에 대해 생성한다.

---

# Risks

잘못된 Release는 모든 Consumer에 영향을 줄 수 있다.

이를 방지하기 위해 다음 정책을 적용한다.

- Mandatory Validation
- Automated Tests
- Checksum Verification
- Rollback Support
- Immutable Release
- Signed Release (향후 지원)

---

# Related RFC

- RFC-0005 Update Protocol
- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification
- RFC-0010 Language Server Integration

---

# Related Specifications

- SPEC-0009 Versioning
- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout
- SPEC-0012 Directory Structure

---

# Decision Summary

VSkript Database는 **Automated Semantic Release Strategy**를 채택한다.

모든 Release는 Builder, Validator, Generator를 거쳐 생성된 검증된 DataPack을 기반으로 자동 생성되며, Semantic Versioning을 적용한다.

Release는 Immutable Artifact로 관리되며, Git Tag를 기준으로 CI/CD를 통해 배포되고, Rollback 및 장기 유지보수(LTS)를 지원한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |