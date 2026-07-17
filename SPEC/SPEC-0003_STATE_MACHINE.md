# SPEC-0003_STATE_MACHINE.md

# VSkript State Machine Specification

> Common State Definitions

Specification: SPEC-0003

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database에서 사용하는 공통 상태(State)와 상태 전이(State Transition)를 정의한다.

모든 컴포넌트는 동일한 상태 모델을 사용하여 일관된 동작을 보장해야 한다.

본 명세는 Builder, Generator, Validator, VSCode Extension, Language Server 및 Update Manager에 적용된다.

---

# 2. Objectives

State Machine의 목표

- 공통 상태 정의
- 상태 전이 규칙
- 오류 처리
- 재시도 정책
- Rollback 지원
- 디버깅 용이성

---

# 3. State Categories

상태는 다음 네 가지 범주로 구분된다.

- Discovery
- Processing
- Runtime
- Failure

---

# 4. Discovery States

탐지 단계

```
UNKNOWN
```

아직 탐지되지 않음.

↓

```
DETECTED
```

Plugin 또는 Resource가 탐지됨.

↓

```
RESOLVED
```

Registry와 성공적으로 매핑됨.

---

# 5. Processing States

처리 단계

```
QUEUED
```

작업 대기.

↓

```
DOWNLOADING
```

DataPack 다운로드.

↓

```
VERIFYING
```

Checksum 검증.

↓

```
INSTALLING
```

Cache 설치.

↓

```
LOADING
```

Language Server 로드.

---

# 6. Runtime States

실행 단계

```
ACTIVE
```

정상 동작.

↓

```
INACTIVE
```

로드되지 않음.

↓

```
UNLOADED
```

메모리에서 제거됨.

---

# 7. Failure States

오류 단계

```
FAILED
```

처리 실패.

↓

```
RETRYING
```

재시도 중.

↓

```
ROLLBACK
```

기존 Cache 복원.

---

# 8. Complete State Flow

```
UNKNOWN

↓

DETECTED

↓

RESOLVED

↓

QUEUED

↓

DOWNLOADING

↓

VERIFYING

↓

INSTALLING

↓

LOADING

↓

ACTIVE
```

---

# 9. Failure Flow

```
DOWNLOADING

↓

FAILED

↓

RETRYING

↓

DOWNLOADING
```

재시도 횟수를 초과하면

```
ROLLBACK
```

을 수행한다.

---

# 10. Retry Policy

기본 정책

- 최대 3회
- 지수 백오프(Exponential Backoff)

예

```
1s

2s

4s
```

---

# 11. Rollback

Rollback 시

- 기존 Cache 복원
- Registry 유지
- 로그 기록

---

# 12. State Transition Rules

허용되는 전이

| From | To |
|------|----|
| UNKNOWN | DETECTED |
| DETECTED | RESOLVED |
| RESOLVED | QUEUED |
| QUEUED | DOWNLOADING |
| DOWNLOADING | VERIFYING |
| VERIFYING | INSTALLING |
| INSTALLING | LOADING |
| LOADING | ACTIVE |
| ACTIVE | UNLOADED |
| FAILED | RETRYING |
| RETRYING | DOWNLOADING |
| FAILED | ROLLBACK |

정의되지 않은 전이는 허용되지 않는다.

---

# 13. State Persistence

필요한 상태는 Cache에 저장한다.

예

```json
{
  "id": "skbee",
  "state": "ACTIVE",
  "version": "3.12.2"
}
```

---

# 14. Event Integration

상태 변경 시 Event를 발생시킨다.

예

```
ACTIVE

↓

PluginLoaded
```

```
FAILED

↓

PluginFailed
```

---

# 15. Logging

모든 상태 변경은 로그로 기록한다.

기록 항목

- Timestamp
- Previous State
- Current State
- Component
- Duration

---

# 16. Timeout

각 상태는 최대 시간을 가진다.

예시

| State | Timeout |
|--------|---------|
| DOWNLOADING | 60s |
| VERIFYING | 10s |
| INSTALLING | 30s |
| LOADING | 15s |

Timeout 초과 시

```
FAILED
```

상태로 전환한다.

---

# 17. Concurrency

여러 Plugin은 독립적인 상태 머신을 가진다.

병렬 처리 시에도 서로의 상태에 영향을 주지 않는다.

---

# 18. Builder Integration

Builder도 동일한 상태 모델을 사용한다.

예

```
RAW

↓

PARSING

↓

CLEANING

↓

GENERATING

↓

COMPLETE
```

Builder 전용 상태는 본 명세를 확장하여 정의할 수 있다.

---

# 19. Validator Integration

Validator 상태

```
WAITING

↓

VALIDATING

↓

PASSED

또는

FAILED
```

---

# 20. Extension Integration

VSCode Extension은 상태 머신을 기반으로

- Progress 표시
- Notification
- Retry
- Cache 관리

를 수행한다.

---

# 21. Future Extensions

향후 추가 예정

- PAUSED
- CANCELLED
- MIGRATING
- SYNCHRONIZING
- UPGRADING

---

# 22. Summary

State Machine은 VSkript Database의 모든 구성 요소에서 공통으로 사용하는 상태 모델이다.

일관된 상태 정의와 전이 규칙을 통해 오류 처리, 업데이트, 로깅 및 디버깅을 표준화한다.

---

# 23. Related Specifications

- SPEC-0002_PLUGIN_MAPPING.md
- SPEC-0004_EVENT_MODEL.md
- SPEC-0005_ERROR_CODES.md

---

# 24. Referenced RFC

- RFC-0005_UPDATE_PROTOCOL.md
- RFC-0006_PLUGIN_DETECTION_PROTOCOL.md

---

# 25. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0003 |
| Document | STATE_MACHINE.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |