# ADR-0003_INCREMENTAL_BUILD.md

# ADR-0003: Incremental Build Architecture

Status: Accepted

Date: YYYY-MM-DD

Authors:
- VSkript Database Team

---

# Context

VSkript Database는 수천 개 이상의 Grammar, Locale, Registry 및 Plugin Resource를 관리하는 대규모 저장소를 목표로 한다.

초기 설계에서는 모든 변경 시 전체 Repository를 다시 빌드하는 **Full Build** 방식을 고려하였다.

그러나 프로젝트 규모가 증가할수록 다음과 같은 문제가 예상되었다.

- Build 시간 증가
- CI/CD 실행 시간 증가
- 불필요한 Generator 실행
- Validator 재검사 비용 증가
- 개발자의 반복 작업 증가

이에 따라 Incremental Build(증분 빌드) 전략을 검토하였다.

---

# Decision

VSkript Database는 **Incremental Build Architecture**를 채택한다.

Builder는 변경된 Resource(Dirty Resource)만 다시 Build한다.

Incremental Build는 Builder, Validator, Generator 전체에서 공통적으로 사용된다.

---

# Rationale

## 1. Build Time Reduction

전체 Build

```
Repository

↓

Build All Plugins

↓

Validate All

↓

Generate All
```

Incremental Build

```
Changed Plugin

↓

Build

↓

Validate

↓

Generate
```

변경되지 않은 Plugin은 다시 처리하지 않는다.

---

## 2. Better Developer Experience

개발자는 하나의 Grammar를 수정할 때 전체 Repository를 다시 Build할 필요가 없다.

예시

```
grammar/effects.json 변경

↓

effects.json Build

↓

Registry Update

↓

Package Update
```

---

## 3. Faster CI/CD

Pull Request에서 변경된 Plugin만 Build한다.

예시

```
Git Diff

↓

Dirty Resources

↓

Incremental Build

↓

Incremental Validation

↓

Incremental Package
```

CI 실행 시간을 크게 단축할 수 있다.

---

## 4. Scalable Architecture

Repository 규모가 증가해도 Build 시간이 선형적으로 증가하지 않는다.

변경량에 비례하여 처리 시간이 결정된다.

---

## 5. Cache Reuse

Incremental Build는 기존 Build 결과를 재사용한다.

캐시 대상

- Parsed JSON
- Schema
- Registry Snapshot
- Dependency Graph
- Generated Package
- Validation Result

---

## 6. Generator Efficiency

Generator는 변경된 Package만 다시 생성한다.

예시

```
Plugin A 변경

↓

Plugin A.zip 재생성

↓

Plugin B.zip 유지
```

---

## 7. Validator Integration

Validator 역시 Incremental Validation을 수행한다.

변경된 Resource와 영향을 받는 Resource만 검사한다.

예시

```
Locale 변경

↓

Locale Validation

↓

Manifest Validation

↓

완료
```

Grammar Validation은 수행하지 않는다.

---

## 8. Git Integration

Dirty Resource는 Git 변경 내역을 기반으로 탐지할 수 있다.

예시

```
git diff

↓

Changed Files

↓

Dirty Resources

↓

Incremental Build
```

Git 기반 워크플로우와 자연스럽게 통합된다.

---

# Dirty Resource Detection

Dirty Resource는 다음 조건 중 하나를 만족하면 변경된 것으로 간주한다.

- 파일 내용 변경
- 파일 추가
- 파일 삭제
- 파일 이름 변경
- Manifest 변경
- Registry 영향
- Dependency 변경

---

# Dependency Propagation

직접 변경되지 않았더라도 영향을 받는 Resource는 함께 재빌드한다.

예시

```
Type 변경

↓

Grammar 영향

↓

Registry 영향

↓

Generator 영향
```

Builder는 Dependency Graph를 사용하여 영향을 계산한다.

---

# Cache Strategy

Builder는 다음 정보를 캐시에 저장할 수 있다.

```
Hash

↓

Build Result

↓

Validation Result

↓

Package Result
```

동일한 Hash의 Resource는 다시 Build하지 않는다.

---

# Cache Invalidation

다음 경우 캐시를 무효화한다.

- Schema Version 변경
- Builder Version 변경
- Generator Version 변경
- Registry 변경
- Dependency 변경
- Build Configuration 변경

---

# Consequences

Incremental Build를 채택함으로써 다음과 같은 장점이 있다.

- Build 시간 감소
- Validation 시간 감소
- Package 생성 시간 감소
- CI/CD 성능 향상
- 대규모 Repository 대응

단점도 존재한다.

- Cache 관리가 필요하다.
- Dependency 추적이 복잡해진다.
- Dirty Resource 계산이 필요하다.

---

# Alternatives Considered

## Full Build Only

모든 변경 시 Repository 전체를 Build한다.

장점

- 구현이 단순하다.
- Cache가 필요 없다.

단점

- Build 시간이 매우 길어진다.
- 대규모 Repository에서 비효율적이다.

채택하지 않았다.

---

## Timestamp-Based Build

파일 수정 시간을 기준으로 Build를 수행한다.

장점

- 구현이 쉽다.

단점

- Git Checkout 시 오동작 가능
- Timestamp 변경에 취약

채택하지 않았다.

---

## Manual Build Selection

사용자가 직접 Build 대상을 지정한다.

장점

- 구현이 단순하다.

단점

- 사람의 실수가 발생할 수 있다.
- 자동화에 적합하지 않다.

채택하지 않았다.

---

# Implementation Notes

Incremental Build는 다음 RFC와 연계된다.

- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification

Dirty Resource 계산은 Builder가 담당한다.

Validator와 Generator는 Builder가 제공한 Build Context를 사용한다.

---

# Risks

잘못된 Dependency 계산은 오래된 Package를 생성할 수 있다.

이를 방지하기 위해 다음을 적용한다.

- Dependency Graph Validation
- Registry Validation
- Full Build 옵션 제공
- Cache 무효화 정책

---

# Related RFC

- RFC-0007 Builder Specification
- RFC-0008 Validator Specification
- RFC-0009 Generator Specification

---

# Related Specifications

- SPEC-0010 Dependency Resolution
- SPEC-0011 DataPack Layout
- SPEC-0012 Directory Structure

---

# Decision Summary

VSkript Database는 **Incremental Build Architecture**를 채택한다.

Builder는 변경된 Resource와 영향을 받는 Resource만 처리하며, Validator와 Generator는 동일한 Build Context를 공유한다.

이를 통해 Build 성능을 향상시키고, CI/CD 시간을 단축하며, 대규모 Repository에서도 안정적인 개발 경험을 제공한다.

---

# Revision History

| Version | Description |
|----------|-------------|
| 1.0 | Initial ADR |