# RFC-0006_PLUGIN_DETECTION_PROTOCOL.md

# VSkript Plugin Detection Protocol

> Automatic DataPack Discovery & Installation

RFC: 0006

Version: 1.0

Status: Draft

---

# 1. Purpose

본 RFC는 VSCode Extension이 Workspace를 분석하여 설치된 Skript 애드온을 자동으로 탐지하고,
필요한 DataPack만 선택적으로 다운로드 및 로드하는 절차를 정의한다.

이 프로토콜은 VSkript Database의 핵심 기능이며,
불필요한 DataPack 다운로드를 방지하고 초기 로딩 속도를 향상시키는 것을 목표로 한다.

---

# 2. Objectives

Plugin Detection Protocol의 목표

- Workspace 자동 분석
- Plugin 자동 탐지
- Script Header 분석
- 필요한 DataPack만 다운로드
- Lazy Loading
- 자동 업데이트
- Hot Reload 지원

---

# 3. Detection Sources

Extension은 다음 순서대로 정보를 수집한다.

```
Workspace

↓

plugins/

↓

Script Headers

↓

Registry

↓

Manifest

↓

Required DataPacks
```

---

# 4. Detection Priority

| Priority | Source |
|----------|--------|
| 1 | plugins/ 폴더 |
| 2 | Script Header 주석 |
| 3 | 프로젝트 설정 파일 |
| 4 | 사용자 수동 지정 |

---

# 5. Plugin Folder Detection

Workspace에서

```
plugins/
```

폴더를 탐색한다.

예

```
plugins/

Skript.jar

SkBee.jar

SkQuery.jar

SkRayFall.jar
```

---

# 6. Plugin Identification

Extension은

각 JAR를 식별한다.

식별 기준

- 파일명
- plugin.yml
- Plugin Name
- Main Class
- Version

---

# 7. Script Header Detection

스크립트 파일에서

Header를 분석한다.

예

```vb
# Requires: SkBee

# Requires: SkQuery

# Database: 1.0
```

또는

```vb
# addons:
# - SkBee
# - SkQuery
```

---

# 8. Detection Result

탐지 결과

```
Detected Plugins

↓

Skript

SkBee

SkQuery
```

---

# 9. Registry Lookup

탐지된 Plugin을

Registry ID로 변환한다.

예

```
SkBee

↓

skbee

↓

Registry
```

---

# 10. Manifest Resolution

Registry에서

Manifest를 찾는다.

```
Registry

↓

Manifest

↓

Dependency Check
```

---

# 11. Dependency Resolution

Manifest의

```
dependencies
```

를 검사한다.

예

```
SkBee

↓

Skript
```

필요한 DataPack도 함께 설치한다.

---

# 12. Download Decision

다음 조건이면 다운로드한다.

- Cache 없음
- Version 변경
- Checksum 변경
- Dependency 변경

---

# 13. Installation

설치 순서

```
Download

↓

Checksum

↓

Cache

↓

Load

```

---

# 14. Lazy Loading

탐지되지 않은 Plugin은

로드하지 않는다.

예

Workspace

```
SkBee
```

↓

로드

```
SkBee

Skript
```

↓

미로드

```
SkQuery

SkRayFall

TuSKe
```

---

# 15. Cache Reuse

이미 설치된 DataPack은

재다운로드하지 않는다.

---

# 16. Workspace Change Detection

다음 이벤트를 감시한다.

- plugins/ 변경
- Script 저장
- Workspace 열기
- Workspace 변경

---

# 17. Hot Reload

Plugin이 추가되면

```
Plugin Detection

↓

Registry

↓

Download

↓

Load
```

재시작 없이 적용한다.

---

# 18. Plugin Removal

Plugin 제거 시

해당 DataPack을 언로드한다.

Cache는 유지한다.

---

# 19. Unknown Plugin

Registry에 없는 Plugin

↓

Unknown Addon

↓

Warning

↓

Skip

↓

Report

---

# 20. Error Handling

실패 시

- Cache 사용
- 로그 생성
- 사용자 알림

---

# 21. Security

Plugin 이름만 신뢰하지 않는다.

가능한 경우

- plugin.yml
- Main Class
- Manifest

를 함께 검증한다.

---

# 22. Performance

목표

- Workspace Scan 최소화
- Cache 활용
- Lazy Loading
- 병렬 다운로드

---

# 23. Future Extensions

향후 지원

- Maven Metadata Detection
- Gradle Project Detection
- Paper Plugin Index
- Plugin Marketplace
- Automatic Compatibility Report

---

# 24. Example Workflow

```
Workspace Open

↓

plugins/

↓

SkBee.jar

↓

Registry

↓

Manifest

↓

Download

↓

Cache

↓

Language Server Load
```

---

# 25. Summary

Plugin Detection Protocol은 Workspace와 Script Header를 분석하여 필요한 Skript 애드온을 자동으로 식별하고,
Registry 및 Manifest를 통해 필요한 DataPack만 다운로드하여 Language Server에 로드하는 절차를 정의한다.

이를 통해 최소한의 다운로드와 빠른 초기화, 효율적인 메모리 사용, 자동 의존성 해결을 제공한다.

---

# 26. Document Information

| Item | Value |
|------|-------|
| RFC | RFC-0006 |
| Document | PLUGIN_DETECTION_PROTOCOL.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |