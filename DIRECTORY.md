# DIRECTORY.md

# VSkript Data Platform Directory Standard

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform 프로젝트의 표준 디렉터리 구조를 정의한다.

모든 구현은 본 문서에서 정의한 디렉터리 구조를 기준으로 구성한다.

Directory Structure는 프로젝트의 Architecture를 반영하며,
모든 컴포넌트는 자신의 책임에 맞는 위치에 존재해야 한다.

---

# 2. Scope

본 문서는 다음 항목을 정의한다.

- Source Directory
- Data Directory
- Build Directory
- Runtime Directory
- Documentation Directory
- Specification Directory
- Test Directory

구현 세부사항은 각 Specification에서 정의한다.

---

# 3. Design Principles

디렉터리는 다음 원칙을 따른다.

- Separation of Concerns
- Specification First
- Data First
- Runtime Independence
- Modular Organization

---

# 4. Top Level Structure

```text
VSkript-Data-Platform/

├── src/
├── data/
├── build/
├── runtime/
├── docs/
├── SPEC/
├── tests/
├── scripts/
├── assets/
├── tools/
├── package.json
├── tsconfig.json
└── README.md
```

---

# 5. Source Directory

```text
src/

├── collector/
├── normalizer/
├── builder/
├── registry/
├── runtime/
├── distribution/
├── localization/
└── shared/
```

Source는 시스템 구현만 포함한다.

---

# 6. Data Directory

```text
data/

├── raw/
├── normalized/
├── canonical/
├── generated/
└── cache/
```

모든 데이터는 단계별로 관리한다.

---

# 7. Build Directory

```text
build/

├── datapacks/
├── manifests/
├── language-packs/
└── registry/
```

Builder의 결과물만 저장한다.

---

# 8. Runtime Directory

```text
runtime/

├── downloader/
├── resolver/
├── loader/
├── cache/
└── workspace/
```

Runtime은 DataPack 로딩만 담당한다.

---

# 9. Documentation Directory

```text
docs/

├── architecture/
├── guides/
├── tutorials/
├── references/
└── reports/
```

모든 문서는 목적별로 분리한다.

---

# 10. Specification Directory

```text
SPEC/

├── core/
├── contracts/
├── architecture/
├── runtime/
├── registry/
├── datapack/
└── localization/
```

모든 Specification은 SPEC 아래에서 관리한다.

---

# 11. Test Directory

```text
tests/

├── unit/
├── integration/
├── runtime/
├── regression/
└── fixtures/
```

모든 테스트는 목적별로 분리한다.

---

# 12. Script Directory

```text
scripts/

├── build/
├── release/
├── update/
└── validation/
```

자동화 스크립트를 관리한다.

---

# 13. Asset Directory

```text
assets/

├── icons/
├── images/
├── schemas/
└── templates/
```

프로젝트 리소스를 저장한다.

---

# 14. Tool Directory

```text
tools/

├── collector/
├── validator/
├── generator/
└── analyzer/
```

개발 도구를 관리한다.

---

# 15. Naming Rules

디렉터리 이름은 다음 규칙을 따른다.

- lowercase
- kebab-case 또는 snake_case 금지
- 의미가 명확해야 한다.
- 축약어 사용을 최소화한다.

---

# 16. Responsibilities

각 디렉터리는 하나의 책임만 가진다.

예시

- src → 구현
- data → 데이터
- build → 생성물
- runtime → 실행
- docs → 문서
- SPEC → 명세

---

# 17. Future Extensions

향후 다음 디렉터리를 추가할 수 있다.

```text
plugins/
examples/
benchmarks/
marketplace/
```

---

# 18. Document Information

| Item | Value |
|------|-------|
| Document | DIRECTORY.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |

---

# 19. Summary

DIRECTORY.md는 VSkript Data Platform의 표준 디렉터리 구조를 정의한다.

모든 구현은 본 구조를 기준으로 구성되며, Architecture와 책임 분리를 유지하기 위한 기준 문서로 사용된다.