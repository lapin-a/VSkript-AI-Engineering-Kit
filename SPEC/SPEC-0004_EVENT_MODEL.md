# SPEC-0004_EVENT_MODEL.md

# VSkript Event Model Specification

> Common Event System Specification

Specification: SPEC-0004

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Database 프로젝트에서 사용되는 공통 이벤트(Event) 모델을 정의한다.

모든 컴포넌트는 상태(State)의 변화 또는 주요 작업이 발생했을 때 표준 이벤트를 발생시켜야 한다.

본 명세는 VSCode Extension, Language Server, Builder, Generator, Validator 및 Update Manager에 공통적으로 적용된다.

---

# 2. Objectives

Event Model의 목표

- 공통 이벤트 정의
- 컴포넌트 간 느슨한 결합(Loose Coupling)
- 비동기 처리 지원
- 상태 변경 알림
- 로깅 및 디버깅 표준화
- 향후 Event Bus 확장 지원

---

# 3. Event Architecture

모든 이벤트는 다음 구조를 따른다.

```
Component

↓

Event

↓

Event Dispatcher

↓

Subscribers

↓

Action
```

---

# 4. Event Categories

이벤트는 다음과 같이 분류한다.

| Category | Description |
|----------|-------------|
| Workspace | Workspace 관련 |
| Plugin | Plugin 관련 |
| DataPack | DataPack 관련 |
| Update | 업데이트 관련 |
| Validation | 검증 관련 |
| Builder | Builder 관련 |
| Runtime | Language Server 관련 |
| System | 시스템 이벤트 |

---

# 5. Event Naming

이벤트 이름은 PascalCase를 사용한다.

형식

```
<Component><Action>
```

예시

```
WorkspaceOpened
PluginDetected
PluginLoaded
DataPackInstalled
UpdateStarted
ValidationCompleted
```

---

# 6. Event Object

모든 이벤트는 동일한 구조를 사용한다.

```json
{
  "id": "evt-001",
  "type": "PluginDetected",
  "timestamp": "2026-07-17T12:00:00Z",
  "source": "PluginDetector",
  "payload": {}
}
```

---

# 7. Common Fields

| Field | Required | Description |
|--------|----------|-------------|
| id | ✔ | Event ID |
| type | ✔ | Event Type |
| timestamp | ✔ | UTC Timestamp |
| source | ✔ | Event Source |
| payload | ✔ | Event Data |

---

# 8. Workspace Events

지원 이벤트

```
WorkspaceOpened
WorkspaceClosed
WorkspaceReloaded
WorkspaceChanged
WorkspaceSaved
```

---

# 9. Plugin Events

지원 이벤트

```
PluginDetected
PluginResolved
PluginDownloaded
PluginInstalled
PluginLoaded
PluginUnloaded
PluginUpdated
PluginRemoved
PluginFailed
```

---

# 10. DataPack Events

지원 이벤트

```
DataPackRequested
DataPackDownloaded
DataPackVerified
DataPackInstalled
DataPackUpdated
DataPackRemoved
DataPackReloaded
```

---

# 11. Update Events

지원 이벤트

```
UpdateCheckStarted
UpdateAvailable
UpdateStarted
UpdateCompleted
UpdateFailed
```

---

# 12. Validation Events

지원 이벤트

```
ValidationStarted
ValidationPassed
ValidationWarning
ValidationFailed
ValidationCompleted
```

---

# 13. Builder Events

지원 이벤트

```
BuildStarted
BuildParsing
BuildCleaning
BuildGenerating
BuildCompleted
BuildFailed
```

---

# 14. Runtime Events

Language Server 이벤트

```
ServerStarting
ServerReady
ServerReloaded
HoverRequested
CompletionRequested
DefinitionRequested
DiagnosticsUpdated
```

---

# 15. System Events

지원 이벤트

```
CacheLoaded
CacheUpdated
CacheCleared
ConfigurationChanged
Shutdown
```

---

# 16. Event Payload

Payload는 이벤트 종류에 따라 달라진다.

예시

```json
{
  "pluginId": "skbee",
  "version": "3.12.2",
  "source": "plugin-folder"
}
```

---

# 17. Event Ordering

동일한 Component에서 발생하는 이벤트는 순서를 보장해야 한다.

예시

```
PluginDetected

↓

PluginResolved

↓

PluginDownloaded

↓

PluginInstalled

↓

PluginLoaded
```

---

# 18. Event Delivery

기본 정책

- At Least Once
- 비동기 처리
- 순서 보장(동일 Component)

---

# 19. Event Subscription

모든 Component는 필요한 이벤트를 구독할 수 있다.

예시

```
PluginDetected

↓

Registry Resolver

↓

Manifest Resolver

↓

Downloader
```

---

# 20. Error Events

오류 발생 시 반드시 실패 이벤트를 발생시킨다.

예시

```
PluginFailed

UpdateFailed

ValidationFailed

BuildFailed
```

---

# 21. Logging

모든 이벤트는 로그에 기록할 수 있다.

기록 항목

- Event ID
- Event Type
- Timestamp
- Duration
- Result

---

# 22. Event Filtering

Subscriber는 다음 조건으로 필터링할 수 있다.

- Event Type
- Category
- Source
- Plugin ID

---

# 23. Future Extensions

향후 지원 예정

- Event Priority
- Event Replay
- Distributed Event Bus
- Remote Event Streaming
- Metrics Integration

---

# 24. Example Flow

```
WorkspaceOpened

↓

PluginDetected

↓

PluginResolved

↓

DataPackDownloaded

↓

PluginLoaded

↓

ServerReady
```

---

# 25. Summary

Event Model은 VSkript Database의 모든 구성 요소에서 사용하는 공통 이벤트 시스템을 정의한다.

모든 상태 변화와 주요 작업은 표준 이벤트를 발생시키며, 각 컴포넌트는 필요한 이벤트를 구독하여 독립적으로 동작한다.

---

# 26. Related Specifications

- SPEC-0002_PLUGIN_MAPPING.md
- SPEC-0003_STATE_MACHINE.md
- SPEC-0005_ERROR_CODES.md

---

# 27. Referenced RFC

- RFC-0005_UPDATE_PROTOCOL.md
- RFC-0006_PLUGIN_DETECTION_PROTOCOL.md

---

# 28. Document Information

| Item | Value |
|------|-------|
| Specification | SPEC-0004 |
| Document | EVENT_MODEL.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |