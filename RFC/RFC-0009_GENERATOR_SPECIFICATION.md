# RFC-0009_GENERATOR_SPECIFICATION.md

# VSkript Generator Specification

> DataPack Generation & Packaging Specification

RFC: RFC-0009

Version: 1.0

Status: Draft

---

# 1. Purpose

본 RFC는 VSkript Database Generator의 구조와 DataPack 생성 절차를 정의한다.

Generator는 Builder가 생성하고 Validator가 검증한 Resource를 기반으로 배포 가능한 DataPack을 생성하는 최종 컴포넌트이다.

Generator는 Resource를 수정하지 않으며 Packaging만 수행한다.

---

# 2. Goals

Generator의 목표는 다음과 같다.

- DataPack 생성
- Resource Packaging
- Manifest 생성
- Compression
- Version 관리
- Release 준비
- Incremental Packaging
- CI/CD 지원

---

# 3. Scope

본 RFC는 다음 내용을 정의한다.

- Generator Architecture
- Packaging Pipeline
- DataPack Layout
- Compression
- Release Package
- Output Structure
- Distribution

다음 내용은 별도 RFC에서 정의한다.

- Builder
- Validator
- Update Protocol

---

# 4. Terminology

| Term | Description |
|------|-------------|
| Generator | DataPack 생성기 |
| Package | 배포 가능한 결과물 |
| Bundle | 여러 Resource의 묶음 |
| Archive | 압축된 DataPack |
| Distribution | 배포 대상 |

---

# 5. Design Principles

Generator는 다음 원칙을 따른다.

- Read Only Input
- Deterministic Output
- Immutable Packaging
- Stable Layout
- Version Controlled
- Reproducible Build

---

# 6. Generator Architecture

Generator는 Packaging Pipeline 기반으로 동작한다.

```
Validated Resources

↓

Package Builder

↓

Manifest Generator

↓

Archive Generator

↓

Release Package
```

---

# 7. Generator Components

Generator는 다음 구성 요소로 이루어진다.

```
Package Builder

Manifest Generator

Archive Generator

Checksum Generator

Release Builder

Report Generator
```

각 Component는 하나의 책임만 수행한다.

---

# 8. Packaging Pipeline

```
Load Resources

↓

Build Package

↓

Generate Manifest

↓

Generate Metadata

↓

Generate Checksums

↓

Compress Package

↓

Generate Report
```

모든 단계가 성공해야 Package를 생성한다.

---

# 9. Package Builder

Package Builder는 Validation을 통과한 Resource를 DataPack 구조로 변환한다.

입력

```
Grammar

Locale

Registry

Documentation

Assets
```

출력

```
DataPack
```

---

# 10. Packaging Rules

Generator는 다음 규칙을 따라야 한다.

- Resource 수정 금지
- Stable Ordering
- UTF-8 Encoding
- Unix Line Ending
- Duplicate File 금지
- Empty Directory 생성 금지

---

# 11. DataPack Layout

생성되는 DataPack은 다음 구조를 가져야 한다.

```
pack/

manifest.json

metadata.json

registry.json

grammar/

locale/

types/

documentation/

assets/
```

Layout은 SPEC-0011을 따른다.

---

# 12. Manifest Generation

Generator는 최종 Manifest를 생성한다.

Manifest에는 다음 정보를 포함한다.

- Plugin ID
- Version
- Builder Version
- Generator Version
- Schema Version
- Build Time
- Dependencies
- Checksum

---

# 13. Metadata Generation

metadata.json에는 다음 정보를 포함한다.

- Name
- Description
- Author
- License
- Homepage
- Repository
- Keywords

---

# 14. Checksum Generation

Generator는 Package의 무결성을 위해 Checksum을 생성한다.

지원 알고리즘

- SHA-256
- SHA-512

예시

```
checksums.json
```

---

# 15. Compression

Generator는 Package를 압축할 수 있다.

권장 형식

- ZIP
- TAR.GZ

압축 과정은 Resource를 변경해서는 안 된다.

---

# 16. Package Naming

기본 형식

```
plugin-version.zip
```

예시

```
skbee-3.12.2.zip
```

---

# 17. Incremental Packaging

변경되지 않은 Resource는 다시 압축하지 않는다.

Dirty Package만 다시 생성한다.

---

# 18. Output Structure

```
dist/

packs/

reports/

checksums/
```

---

# 19. Build Report

Generator는 Packaging Report를 생성한다.

예시

```json
{
  "status": "success",
  "packages": 12,
  "archives": 12,
  "checksums": 12,
  "duration": 2812
}
```

---

# 20. Distribution

Generator는 다음 배포 방식을 지원한다.

- Local Directory
- GitHub Releases
- Artifact Storage
- CI/CD Artifacts

---

# 21. CLI Integration

지원 명령

```bash
vskript-generator build

vskript-generator package

vskript-generator publish
```

---

# 22. Builder Integration

Generator는 Builder Output만 사용한다.

Builder를 우회해서는 안 된다.

---

# 23. Validator Integration

Validation이 실패한 Resource는 Packaging하지 않는다.

```
Builder

↓

Validator

↓

Generator
```

---

# 24. Update Protocol Integration

Generator는 Update Protocol과 호환되는 DataPack만 생성해야 한다.

Manifest 구조는 RFC-0005를 따른다.

---

# 25. Performance Guidelines

권장 목표

| Metric | Target |
|---------|--------|
| Package Generation | ≤ 5초 |
| Compression | ≤ 10초 |
| Memory Usage | ≤ 512MB |

---

# 26. Security Considerations

Generator는

- Checksum 생성
- Path Traversal 방지
- Invalid File Name 차단
- Duplicate Resource 차단

을 수행해야 한다.

---

# 27. Extensibility

향후 지원 가능

- 새로운 Archive Format
- 새로운 Manifest Version
- 새로운 Compression Algorithm
- 새로운 Distribution Target

---

# 28. Summary

Generator는 Validator를 통과한 Resource를 기반으로 DataPack을 생성하는 최종 컴포넌트이다.

생성된 Package는 재현 가능하고(Deterministic), 버전 관리가 가능하며, Update Protocol 및 VSCode Extension과 호환되어야 한다.

---

# Related Specifications

- SPEC-0009_VERSIONING
- SPEC-0010_DEPENDENCY_RESOLUTION
- SPEC-0011_DATAPACK_LAYOUT
- SPEC-0012_DIRECTORY_STRUCTURE

---

# Related RFC

- RFC-0005_UPDATE_PROTOCOL
- RFC-0007_BUILDER_SPECIFICATION
- RFC-0008_VALIDATOR_SPECIFICATION
- RFC-0010_LANGUAGE_SERVER_INTEGRATION

---

# Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0009 |
| Title | Generator Specification |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Depends On | RFC-0008 |
| Followed By | RFC-0010 |
| Last Updated | YYYY-MM-DD |