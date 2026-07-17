# ADR-0005_DATAPACK_ARCHITECTURE.md

# ADR-0005: DataPack as the Distribution Unit

Status: Accepted

Date: YYYY-MM-DD

Authors:
- VSkript Database Team

---

# Context

VSkript Database는 Builder와 Validator를 거쳐 생성된 데이터를 최종 사용자(IDE, Language Server, CI/CD, 외부 도구)에 제공해야 한다.

초기 설계에서는 다음과 같은 배포 방식을 검토하였다.

- Raw Repository 직접 사용
- JSON 개별 파일 배포
- Plugin별 ZIP
- Registry만 배포
- SQLite Database
- DataPack Package

프로젝트의 요구사항은 다음과 같았다.

- 오프라인 사용 가능
- 빠른 로딩
- 버전 관리
- 증분 업데이트 지원
- 여러 IDE 지원
- Plugin 단위 배포
- 검증 가능한 패키지

---

# Decision

VSkript Database는 **DataPack을 유일한 배포 단위(Distribution Unit)** 로 채택한다.

모든 Consumer(Language Server, VSCode Extension, CLI 등)는 Raw Repository가 아닌 DataPack만 사용한다.

Generator는 Validation을 통과한 Resource만 DataPack으로 패키징한다.

---

# DataPack Responsibilities

DataPack은 다음 정보를 포함한다.

- Manifest
- Registry
- Grammar
- Locale
- Types
- Metadata
- Assets
- Documentation
- Checksums

DataPack은 실행 가능한 코드가 아닌 읽기 전용 데이터 패키지이다.

---

# Architecture

```
Raw Repository

↓

Builder

↓

Generated Resources

↓

Validator

↓

Validated Resources

↓

Generator

↓

DataPack

↓

Language Server

↓

IDE
```

Raw Source는 IDE에서 직접 사용하지 않는다.

---

# Rationale

## 1. Clear Separation of Responsibilities

Repository는 개발을 위한 공간이다.

DataPack은 배포를 위한 결과물이다.

```
Repository

↓

Build

↓

Package

↓

Distribution
```

개발 환경과 실행 환경을 분리한다.

---

## 2. Immutable Artifacts

생성된 DataPack은 변경되지 않는다.

동일한 입력은 항상 동일한 DataPack을 생성해야 한다.

이는 재현 가능한 빌드(Reproducible Build)를 보장한다.

---

## 3. Offline Support

DataPack은 모든 필요한 데이터를 포함한다.

Language Server는 인터넷 연결 없이도 다음 기능을 제공할 수 있다.

- Completion
- Hover
- Definition
- Diagnostics
- References

---

## 4. Stable Distribution Format

DataPack은 내부 구조가 고정되어 있다.

```
manifest.json

registry.json

grammar/

locale/

types/

documentation/

assets/
```

Consumer는 동일한 구조를 가정할 수 있다.

---

## 5. Version Isolation

각 DataPack은 자신의 Version과 Schema Version을 가진다.

예시

```json
{
  "version": "3.2.0",
  "schemaVersion": "1.0"
}
```

여러 버전의 DataPack을 동시에 관리할 수 있다.

---

## 6. Plugin Isolation

각 Plugin은 독립적인 DataPack을 가진다.

예시

```
skript.zip

skbee.zip

skript-reflect.zip
```

Plugin 간 직접 접근은 허용되지 않는다.

---

## 7. Validation Boundary

Generator는 Validation을 통과한 데이터만 패키징한다.

```
Builder

↓

Validator

↓

PASS

↓

Generator
```

Validation 실패 시 DataPack은 생성되지 않는다.

---

## 8. Language Server Optimization

Language Server는 DataPack만 로드한다.

Repository 구조를 알 필요가 없다.

```
Completion

↓

Registry

↓

Grammar

↓

Locale
```

Repository 탐색 비용이 제거된다.

---

## 9. Future Distribution

DataPack은 다양한 방식으로 배포할 수 있다.

- GitHub Releases
- Marketplace
- CDN
- Local Cache
- Enterprise Repository

Distribution 방식과 데이터 형식을 분리한다.

---

# Package Structure

표준 DataPack 구조

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

checksums.json
```

모든 Generator는 동일한 구조를 생성해야 한다.

---

# Package Lifecycle

```
Repository

↓

Build

↓

Validation

↓

Package

↓

Publish

↓

Install

↓

Load

↓

Use
```

DataPack은 이 생명주기를 따른다.

---

# Consequences

DataPack Architecture를 채택함으로써 다음과 같은 장점이 있다.

- 명확한 배포 단위
- 높은 이식성
- 오프라인 사용
- 안정적인 버전 관리
- 빠른 IDE 로딩
- 쉬운 캐시 관리
- 쉬운 Marketplace 연동

단점도 존재한다.

- Package 생성 과정이 필요하다.
- 압축 및 체크섬 생성 비용이 추가된다.
- Generator가 필수 컴포넌트가 된다.

---

# Alternatives Considered

## Raw Repository Access

Language Server가 Repository를 직접 읽는다.

장점

- Package 생성이 필요 없다.

단점

- Repository 구조에 강하게 결합된다.
- 성능이 낮다.
- IDE마다 구현이 달라질 수 있다.

채택하지 않았다.

---

## Registry Only Distribution

Registry만 배포한다.

장점

- 매우 작은 크기

단점

- Grammar와 Locale를 별도로 다운로드해야 한다.
- 오프라인 사용이 어렵다.

채택하지 않았다.

---

## SQLite Database

데이터를 SQLite 파일로 배포한다.

장점

- 빠른 조회

단점

- Git 친화성이 낮다.
- 사람이 직접 수정하기 어렵다.
- 다양한 언어에서 의존성이 증가한다.

채택하지 않았다.

---

## Plugin Repository

Plugin별 Repository를 직접 참조한다.

장점

- 항상 최신 데이터

단점

- 네트워크 의존성
- 오프라인 미지원
- 버전 고정 어려움

채택하지 않았다.

---

# Implementation Notes

Builder는 DataPack을 생성하지 않는다.

Validator는 DataPack을 수정하지 않는다.

Generator만 DataPack을 생성할 수 있다.

Language Server는 DataPack만 입력으로 사용한다.

DataPack은 읽기 전용이다.

---

# Risks

DataPack 형식이 변경되면 모든 Consumer가 영향을 받을 수 있다.

이를 방지하기 위해 다음 정책을 적용한다.

- Manifest Version 관리
- Schema Version 관리
- Backward Compatibility
- Checksum Verification
- Validation Required Before Packaging

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

VSkript Database는 **DataPack을 유일한 배포 단위**로 채택한다.

모든 Consumer는 Raw Repository 대신 DataPack을 사용하며, DataPack은 Builder, Validator, Generator를 거친 검증된 데이터를 포함하는 불변(Immutable) 패키지이다.

이 구조는 배포와 개발을 분리하고, 오프라인 사용, 안정적인 버전 관리, 높은 IDE 성능, 그리고 향후 Marketplace 및 원격 배포까지 고려한 확장 가능한 아키텍처를 제공한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |