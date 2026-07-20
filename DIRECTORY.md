# DIRECTORY.md

# Directory Structure

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform의 표준 디렉터리 구조를 정의한다.

모든 프로젝트 구성 요소는 본 문서에서 정의한 구조를 따라야 한다.

---

# 2. Principles

디렉터리는 다음 원칙을 따른다.

- Separation of Concerns
- Independent Components
- Canonical Dataset
- Data First
- Specification First

---

# 3. Top Level Structure

```text
VSkript-Data-Platform/

├── docs/
├── specs/
├── registry/
├── sources/
├── datasets/
├── datapacks/
├── builder/
├── runtime/
├── tools/
├── scripts/
├── tests/
├── assets/
└── examples/
```

---

# 4. docs/

프로젝트 문서를 저장한다.

```text
docs/

├── PROJECT.md
├── REQUIREMENTS.md
├── ARCHITECTURE.md
├── DIRECTORY.md
├── CONTRIBUTING.md
├── CHANGELOG.md
└── README.md
```

---

# 5. specs/

Specification 문서를 저장한다.

```text
specs/

├── SPEC-0001
├── SPEC-0002
├── ...
```

Specification은 프로젝트의 공식 설계 문서이다.

---

# 6. registry/

Registry 데이터를 저장한다.

```text
registry/

├── addons/
├── languages/
├── datapacks/
└── manifests/
```

Registry는 Runtime이 참조하는 공식 Registry이다.

---

# 7. sources/

원본 데이터를 저장한다.

예시

```text
sources/

├── skripthub/
├── github/
├── plugins/
└── local/
```

Source는 수정하지 않는다.

---

# 8. datasets/

정규화된 Canonical Dataset을 저장한다.

```text
datasets/

├── functions/
├── events/
├── expressions/
├── effects/
├── types/
└── classes/
```

Canonical Dataset은 프로젝트의 Source of Truth이다.

---

# 9. datapacks/

빌드된 DataPack을 저장한다.

```text
datapacks/

├── skript/
├── skbee/
├── skrayfall/
└── ...
```

---

# 10. builder/

DataPack 생성기를 저장한다.

```text
builder/

├── parser/
├── normalizer/
├── generator/
└── validator/
```

---

# 11. runtime/

Runtime 관련 모듈을 저장한다.

```text
runtime/

├── downloader/
├── cache/
├── loader/
└── resolver/
```

---

# 12. tools/

프로젝트 관리 도구를 저장한다.

예시

- Registry Builder
- Migration Tool
- Validation Tool

---

# 13. scripts/

자동화 스크립트를 저장한다.

예시

- Build
- Publish
- Update
- Sync

---

# 14. tests/

테스트를 저장한다.

```text
tests/

├── unit/
├── integration/
├── runtime/
└── regression/
```

---

# 15. assets/

정적 리소스를 저장한다.

예시

- Images
- Icons
- Logos

---

# 16. examples/

예제 데이터를 저장한다.

예시

- Example DataPack
- Example Registry
- Example Dataset

---

# 17. Rules

모든 데이터는 다음 흐름을 따른다.

```text
Source

↓

Dataset

↓

Registry

↓

DataPack

↓

Runtime
```

역방향 변경은 허용하지 않는다.

---

# 18. Directory Responsibilities

| Directory | Responsibility |
|------------|----------------|
| docs | Documentation |
| specs | Specifications |
| registry | Registry Data |
| sources | Raw Sources |
| datasets | Canonical Dataset |
| datapacks | Build Output |
| builder | Build System |
| runtime | Runtime Components |
| tools | Developer Tools |
| scripts | Automation |
| tests | Testing |
| assets | Resources |
| examples | Examples |

---

# 19. Document Information

| Item | Value |
|------|-------|
| Document | DIRECTORY.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |

---

# 20. Summary

DIRECTORY.md는 VSkript Data Platform의 표준 디렉터리 구조를 정의한다.

모든 구현은 본 구조를 기준으로 이루어지며, Source → Dataset → Registry → DataPack → Runtime의 데이터 흐름을 따른다.