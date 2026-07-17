# SPEC-0005_ERROR_CODES.md

# VSkript Error Code Specification

> Standard Error Code System

Specification: SPEC-0005

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database 프로젝트에서 사용하는 표준 오류 코드(Error Code)를 정의한다.

모든 구성 요소는 동일한 오류 코드 체계를 사용하여 오류를 보고해야 하며,
VSCode Extension, Builder, Validator 및 Language Server는 해당 코드를 기반으로 사용자에게 일관된 정보를 제공해야 한다.

---

# 2. Objectives

Error Code의 목표

- 오류 표준화
- 일관된 로그 생성
- 사용자 친화적인 오류 표시
- 자동 복구 지원
- 디버깅 용이성
- 향후 국제화(Localization) 지원

---

# 3. Error Code Format

모든 오류 코드는 다음 형식을 따른다.

```
<AREA><NUMBER>
```

예시

```
PD001
UP003
VL015
```

---

# 4. Error Categories

| Prefix | Component |
|---------|-----------|
| PD | Plugin Detection |
| RG | Registry |
| MF | Manifest |
| DP | DataPack |
| UP | Update |
| VL | Validator |
| BD | Builder |
| GN | Generator |
| LS | Language Server |
| IO | File System |
| SY | System |

---

# 5. Severity Levels

모든 오류는 Severity를 가진다.

| Level | Description |
|--------|-------------|
| INFO | 정보 |
| WARNING | 경고 |
| ERROR | 오류 |
| CRITICAL | 치명적 오류 |

---

# 6. Plugin Detection Errors

| Code | Severity | Description |
|------|----------|-------------|
| PD001 | ERROR | Plugin not found |
| PD002 | ERROR | Invalid plugin metadata |
| PD003 | ERROR | plugin.yml missing |
| PD004 | ERROR | Main class missing |
| PD005 | WARNING | Unknown plugin |
| PD006 | ERROR | Unsupported plugin version |
| PD007 | ERROR | Plugin mapping failed |
| PD008 | ERROR | Duplicate plugin detected |

---

# 7. Registry Errors

| Code | Severity | Description |
|------|----------|-------------|
| RG001 | ERROR | Registry not found |
| RG002 | ERROR | Invalid registry format |
| RG003 | ERROR | Duplicate DataPack ID |
| RG004 | ERROR | Invalid version |
| RG005 | ERROR | Missing registry entry |

---

# 8. Manifest Errors

| Code | Severity | Description |
|------|----------|-------------|
| MF001 | ERROR | Manifest missing |
| MF002 | ERROR | Invalid manifest |
| MF003 | ERROR | Missing checksum |
| MF004 | ERROR | Missing dependency |
| MF005 | ERROR | Invalid schema version |

---

# 9. DataPack Errors

| Code | Severity | Description |
|------|----------|-------------|
| DP001 | ERROR | DataPack not found |
| DP002 | ERROR | Invalid DataPack |
| DP003 | ERROR | Unsupported version |
| DP004 | ERROR | Missing grammar file |
| DP005 | ERROR | Missing locale file |
| DP006 | ERROR | Invalid checksum |

---

# 10. Update Errors

| Code | Severity | Description |
|------|----------|-------------|
| UP001 | WARNING | Update available |
| UP002 | ERROR | Download failed |
| UP003 | ERROR | Checksum mismatch |
| UP004 | ERROR | Update cancelled |
| UP005 | ERROR | Rollback failed |

---

# 11. Validator Errors

| Code | Severity | Description |
|------|----------|-------------|
| VL001 | ERROR | Invalid JSON |
| VL002 | ERROR | Missing required field |
| VL003 | ERROR | Duplicate ID |
| VL004 | ERROR | Invalid locale key |
| VL005 | ERROR | Invalid version |
| VL006 | WARNING | Deprecated field |
| VL007 | WARNING | Missing translation |
| VL008 | ERROR | Invalid dependency |
| VL009 | ERROR | Invalid grammar |
| VL010 | ERROR | Invalid type |

---

# 12. Builder Errors

| Code | Severity | Description |
|------|----------|-------------|
| BD001 | ERROR | Build failed |
| BD002 | ERROR | Parse failed |
| BD003 | ERROR | Invalid source data |
| BD004 | ERROR | Output generation failed |
| BD005 | ERROR | Duplicate resource |

---

# 13. Generator Errors

| Code | Severity | Description |
|------|----------|-------------|
| GN001 | ERROR | Generation failed |
| GN002 | ERROR | Template missing |
| GN003 | ERROR | Serialization failed |
| GN004 | ERROR | Invalid output |

---

# 14. Language Server Errors

| Code | Severity | Description |
|------|----------|-------------|
| LS001 | ERROR | Initialization failed |
| LS002 | ERROR | Hover failed |
| LS003 | ERROR | Completion failed |
| LS004 | ERROR | Definition lookup failed |
| LS005 | ERROR | Diagnostics failed |

---

# 15. File System Errors

| Code | Severity | Description |
|------|----------|-------------|
| IO001 | ERROR | File not found |
| IO002 | ERROR | Directory not found |
| IO003 | ERROR | Access denied |
| IO004 | ERROR | Read failed |
| IO005 | ERROR | Write failed |

---

# 16. System Errors

| Code | Severity | Description |
|------|----------|-------------|
| SY001 | CRITICAL | Unknown system error |
| SY002 | CRITICAL | Configuration invalid |
| SY003 | ERROR | Cache corrupted |
| SY004 | ERROR | Timeout |
| SY005 | ERROR | Unsupported platform |

---

# 17. Error Object

모든 오류는 동일한 구조를 사용한다.

```json
{
  "code": "PD005",
  "severity": "WARNING",
  "message": "Unknown plugin detected.",
  "component": "PluginDetector",
  "details": {},
  "timestamp": "2026-07-17T12:00:00Z"
}
```

---

# 18. Error Handling Rules

모든 구성 요소는 오류 발생 시 다음 절차를 따른다.

```
Detect Error

↓

Assign Error Code

↓

Create Error Object

↓

Log Error

↓

Emit Event

↓

Recovery
```

---

# 19. Recovery Policy

Severity에 따른 기본 정책

| Severity | Action |
|-----------|--------|
| INFO | Continue |
| WARNING | Continue + Log |
| ERROR | Retry or Rollback |
| CRITICAL | Abort Process |

---

# 20. Localization

사용자에게 표시되는 오류 메시지는 Locale을 사용한다.

예

```
error.PD005.title

error.PD005.message
```

오류 코드는 변경하지 않는다.

---

# 21. Logging

로그에는 다음 정보를 기록한다.

- Timestamp
- Error Code
- Component
- Severity
- Message
- Stack Trace (선택)
- Context

---

# 22. Event Integration

오류 발생 시 반드시 대응되는 Event를 발생시킨다.

예시

```
PD005

↓

PluginFailed
```

```
UP003

↓

UpdateFailed
```

---

# 23. Extensibility

새로운 오류 코드는 기존 Prefix를 재사용하거나 새로운 Prefix를 추가하여 확장할 수 있다.

예약 Prefix

```
AI
NW
DB
API
```

---

# 24. Summary

Error Code Specification은 VSkript Database 전반에서 사용하는 표준 오류 코드 체계를 정의한다.

모든 구성 요소는 본 명세를 기반으로 오류를 보고하며, 오류 객체와 이벤트 모델을 통해 일관된 처리와 복구를 수행한다.

---

# 25. Related Specifications

- SPEC-0003_STATE_MACHINE.md
- SPEC-0004_EVENT_MODEL.md
- SPEC-0008_JSON_SCHEMA_GUIDE.md

---

# 26. Referenced RFC

- RFC-0005_UPDATE_PROTOCOL.md
- RFC-0006_PLUGIN_DETECTION_PROTOCOL.md
- RFC-0008_VALIDATOR_SPECIFICATION.md

---

# 27. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0005 |
| Document | ERROR_CODES.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |