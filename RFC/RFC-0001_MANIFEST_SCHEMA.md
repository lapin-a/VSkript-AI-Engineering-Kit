# MANIFEST_SCHEMA.md

# VSkript DataPack

> Manifest Schema Specification

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript DataPack에서 사용하는 `manifest.json`의 구조와 의미를 정의한다.

Manifest는 DataPack의 메타데이터이며,

- 설치
- 업데이트
- 의존성 해결
- 버전 비교
- 호환성 확인

에 사용된다.

모든 DataPack은 반드시 Manifest를 포함해야 한다.

---

# 2. Design Goals

Manifest는 다음 정보를 제공한다.

- DataPack 식별
- 버전 관리
- 의존성
- 지원 환경
- Locale 정보
- 무결성 검증
- 다운로드 정보

---

# 3. File Location

```
pack/

manifest.json
```

---

# 4. JSON Encoding

모든 Manifest는

- UTF-8
- JSON
- 2 Spaces
- LF

를 사용한다.

---

# 5. Required Fields

| Field | Required |
|--------|----------|
| id | ✔ |
| name | ✔ |
| version | ✔ |
| addonVersion | ✔ |
| databaseVersion | ✔ |
| checksum | ✔ |
| createdAt | ✔ |
| updatedAt | ✔ |
| languages | ✔ |
| dependencies | ✔ |
| supports | ✔ |

---

# 6. Standard Manifest

```json
{
  "id": "skbee",
  "name": "SkBee",

  "version": "1.3.0",

  "addonVersion": "3.12.0",

  "databaseVersion": "1.0.0",

  "checksum": "sha256",

  "createdAt": "2026-07-18T12:00:00Z",

  "updatedAt": "2026-07-18T12:00:00Z",

  "languages": [
    "en",
    "ko"
  ],

  "dependencies": [
    "skript"
  ],

  "supports": {
    "minecraft": "1.21+",
    "skript": "2.9+"
  }
}
```

---

# 7. id

DataPack의 고유 식별자.

규칙

- 소문자
- kebab-case
- 변경 금지

예

```
skript

skbee

skquery

skrayfall
```

---

# 8. name

사용자 표시 이름.

예

```
SkBee
```

---

# 9. version

DataPack Version.

Semantic Versioning 사용.

예

```
1.0.0

1.3.2

2.0.0
```

---

# 10. addonVersion

지원하는 Addon Version.

예

```
3.12.0
```

---

# 11. databaseVersion

Database Version.

예

```
1.0.0
```

---

# 12. checksum

SHA-256.

설치 시 반드시 검증한다.

---

# 13. createdAt

Manifest 생성 시각.

ISO-8601 사용.

---

# 14. updatedAt

마지막 변경 시각.

ISO-8601 사용.

---

# 15. languages

지원 Locale.

예

```json
[
  "en",
  "ko",
  "ja"
]
```

---

# 16. dependencies

필수 DataPack.

예

```json
[
  "skript"
]
```

의존성이 없으면

```json
[]
```

---

# 17. supports

지원 환경.

```json
{
  "minecraft":"1.21+",
  "skript":"2.9+",
  "database":"1.x"
}
```

---

# 18. Optional Fields

지원 가능한 Optional 항목.

| Field | Description |
|--------|-------------|
| homepage | 프로젝트 |
| repository | Git |
| license | 라이선스 |
| author | 작성자 |
| contributors | 기여자 |
| icon | 아이콘 |
| keywords | 검색 |
| website | 웹사이트 |

---

# 19. Download

향후 지원

```json
{
  "download": {
      "url":"...",
      "size":123456
  }
}
```

---

# 20. Signature

향후 Digital Signature 지원.

```json
{
  "signature":"..."
}
```

---

# 21. Validation Rules

Manifest는 반드시

- Required Field
- JSON Schema
- SHA-256
- Semantic Version

검사를 통과해야 한다.

---

# 22. Compatibility Rules

Extension은

Manifest의

- addonVersion
- databaseVersion
- supports

를 비교하여 설치 여부를 결정한다.

---

# 23. Builder Rules

Builder는

Manifest를 자동 생성한다.

수동 작성하지 않는다.

---

# 24. Registry Integration

Registry는 Manifest를 참조한다.

```
Registry

↓

Manifest

↓

Download
```

---

# 25. Update Strategy

Manifest Version이 변경되면

Update 대상으로 판단한다.

Checksum 변경도 확인한다.

---

# 26. Error Conditions

다음 경우 Manifest를 거부한다.

- id 없음
- version 없음
- checksum 없음
- supports 없음
- JSON 오류
- Version 오류

---

# 27. JSON Schema (Draft)

```json
{
  "type":"object",
  "required":[
    "id",
    "name",
    "version",
    "checksum"
  ]
}
```

---

# 28. Future Extensions

향후 추가

- Digital Signature
- Compression
- CDN Metadata
- Delta Patch
- Package Manager

---

# 29. Summary

Manifest는 DataPack의 공식 메타데이터이다.

모든 설치, 업데이트, 버전 비교, 의존성 해결 및 호환성 검사는 Manifest를 기준으로 수행하며, Builder가 자동 생성하고 Validator가 검증한다.

---

# 30. Document Information

| Item | Value |
|------|-------|
| Document | MANIFEST_SCHEMA.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Database |
| Last Updated | YYYY-MM-DD |