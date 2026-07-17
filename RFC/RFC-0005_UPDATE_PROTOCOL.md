# RFC-0005_UPDATE_PROTOCOL.md

# VSkript Update Protocol

> Incremental Update Protocol

RFC: 0005

Version: 1.0

Status: Draft

---

# 1. Purpose

본 RFC는 VSCode Extension과 VSkript Database 간의 업데이트 절차를 정의한다.

Update Protocol은 전체 Database를 다시 다운로드하지 않고,
변경된 DataPack만 효율적으로 갱신하는 것을 목표로 한다.

---

# 2. Objectives

Update Protocol의 목표

- 최소 다운로드
- 빠른 업데이트
- Cache 재사용
- Version 비교
- Checksum 검증
- 자동 복구

---

# 3. Update Flow

```
Extension

↓

Registry Request

↓

Registry Compare

↓

Manifest Compare

↓

Changed Packs

↓

Download

↓

Checksum Verify

↓

Install

↓

Cache Update

↓

Reload
```

---

# 4. Update Trigger

업데이트는 다음 상황에서 수행된다.

- Extension 시작
- Workspace 변경
- Plugin 변경
- 사용자 수동 업데이트
- 주기적 자동 확인

---

# 5. Registry Synchronization

Extension은 먼저 Registry를 확인한다.

```
Local Registry

↓

Remote Registry

↓

Version Compare
```

Registry Version이 동일하면 종료한다.

---

# 6. Registry Version

비교 대상

```
registry.version
```

예

```
1.4.0

↓

1.5.0
```

변경 시 다음 단계 진행.

---

# 7. Package Detection

Registry에서

설치된 Package 목록을 조회한다.

```
installed.json

↓

Registry

↓

Compare
```

---

# 8. Manifest Comparison

Manifest 비교 항목

- version
- checksum
- addonVersion
- databaseVersion

---

# 9. Checksum Verification

다운로드 후

```
SHA-256
```

검증을 수행한다.

불일치 시

- 설치 취소
- Cache 유지
- 오류 기록

---

# 10. Incremental Update

변경된 DataPack만 다운로드한다.

예

```
Registry

SkBee 변경

↓

Download

SkBee.zip
```

나머지는 재사용한다.

---

# 11. Dependency Update

DataPack 업데이트 시

의존성도 검사한다.

예

```
SkBee

↓

Requires Skript

↓

Version Check
```

---

# 12. Cache Management

Cache 구조

```
cache/

registry.json

installed.json

packs/
```

---

# 13. Installed Database

installed.json

예

```json
{
  "skbee":"1.2.0",
  "skript":"1.0.0"
}
```

---

# 14. Update Decision

DataPack는 다음 조건 중 하나를 만족하면 업데이트한다.

- Version 변경
- Checksum 변경
- Dependency 변경
- Manifest 변경

---

# 15. Download Queue

여러 Package는 Queue로 처리한다.

```
Queue

↓

Download

↓

Install

↓

Verify

↓

Next
```

병렬 다운로드도 지원 가능하다.

---

# 16. Rollback

업데이트 실패 시

```
Old Cache

↓

Restore

↓

Retry
```

---

# 17. Failure Recovery

오류 발생 시

- 기존 DataPack 유지
- 오류 로그 생성
- 다음 실행 시 재시도

---

# 18. Offline Mode

인터넷 연결이 없으면

Cache만 사용한다.

새 다운로드는 수행하지 않는다.

---

# 19. Update Frequency

기본 정책

- 시작 시 확인
- 24시간마다 확인

사용자는 변경 가능하다.

---

# 20. Security

모든 다운로드는

- HTTPS
- SHA-256 검증

을 수행해야 한다.

향후

Digital Signature 지원.

---

# 21. Builder Integration

Builder는

Manifest 변경 시

자동으로 Registry를 갱신한다.

---

# 22. Extension Reload

업데이트 완료 후

Language Server는

변경된 DataPack만 다시 로드한다.

---

# 23. Logging

다음 정보를 기록한다.

- Update Time
- Package
- Version
- Download Size
- Duration
- Result

---

# 24. Validation

업데이트 완료 후

다음을 검증한다.

- Registry
- Manifest
- Checksum
- Dependencies

---

# 25. Future Extensions

향후 지원

- Delta Patch
- Binary Diff
- CDN Edge Cache
- Multi Mirror
- Background Update
- Package Priority

---

# 26. Example Workflow

```
Extension Start

↓

Registry Download

↓

Compare

↓

Changed Packs

↓

Download

↓

Checksum

↓

Install

↓

Reload
```

---

# 27. Summary

Update Protocol은 VSCode Extension과 VSkript Database 간의 증분 업데이트 절차를 정의한다.

Registry와 Manifest를 비교하여 변경된 DataPack만 다운로드하며,
Checksum 검증과 Cache 관리를 통해 안정적인 업데이트를 제공한다.

---

# 28. Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0005 |
| Document | UPDATE_PROTOCOL.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |