# GLOSSARY.md

# VSkript AI Engineering Kit

> Standard Terminology & Definitions

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 사용하는 모든 용어의 표준 정의를 제공한다.

모든 Prompt, Documentation, Workflow, Report 및 Template는 본 문서의 용어를 기준으로 작성해야 한다.

목적은 다음과 같다.

- 용어의 일관성 유지
- AI 간 동일한 의미 해석
- 문서 품질 향상
- 프로젝트 이해도 향상

---

# 2. Usage Rules

모든 문서는 다음 규칙을 따른다.

- 하나의 개념은 하나의 용어만 사용한다.
- 동일한 용어는 항상 동일한 의미를 가진다.
- 약어는 최초 1회 전체 이름을 함께 표기한다.
- 혼동되는 표현은 사용하지 않는다.

---

# 3. Core Project Terms

| Term | Definition |
|------|------------|
| VSkript | VSCode용 Skript 개발 환경 프로젝트 |
| VSkript Database | VSkript에서 사용하는 독립적인 문법 데이터 프로젝트 |
| AI Engineering Kit | AI 개발 및 검증 프레임워크 |
| Project | 분석 또는 개발 대상 저장소 |
| Repository | Git 저장소 |
| Workspace | 사용자가 열어둔 프로젝트 공간 |

---

# 4. Architecture Terms

| Term | Definition |
|------|------------|
| Architecture | 프로젝트 전체 구조 |
| Layer | 역할별 논리 계층 |
| Module | 독립적인 기능 단위 |
| Component | 하나의 기능을 수행하는 구성 요소 |
| Service | 비즈니스 로직 제공 객체 |
| Provider | LSP 기능을 제공하는 모듈 |
| Manager | 여러 객체를 관리하는 모듈 |
| Controller | 작업 흐름을 제어하는 모듈 |
| Registry | 객체 등록 및 조회 시스템 |
| Factory | 객체 생성 전용 클래스 |
| Adapter | 호환성 계층 |

---

# 5. Language Server Terms

| Term | Definition |
|------|------------|
| LSP | Language Server Protocol |
| Completion | 자동완성 |
| Hover | Hover 정보 |
| Definition | 정의 이동 |
| References | 참조 검색 |
| Signature Help | 함수 시그니처 |
| Diagnostics | 오류 분석 |
| Semantic Tokens | 의미 기반 토큰 |
| Document Symbol | 문서 심볼 |
| Workspace Symbol | 프로젝트 심볼 |

---

# 6. VSkript Database Terms

| Term | Definition |
|------|------------|
| DataPack | 개별 애드온 문법 데이터 패키지 |
| Manifest | DataPack 메타데이터 |
| Registry | 설치된 DataPack 목록 |
| Locale | 언어별 데이터 |
| Translation | 번역 데이터 |
| Schema | JSON 구조 정의 |
| Metadata | 부가 정보 |
| Index | 검색용 색인 |

---

# 7. DataPack Terms

| Term | Definition |
|------|------------|
| Addon | Skript 애드온 |
| Dependency | 필요한 선행 DataPack |
| Version | DataPack 버전 |
| Compatibility | 지원 버전 정보 |
| Incremental Update | 변경분만 업데이트 |
| Package | 배포 단위 |
| Cache | 임시 저장 데이터 |

---

# 8. Verification Terms

| Term | Definition |
|------|------------|
| Verification | 요구사항 충족 여부 확인 |
| Validation | 데이터 형식 검증 |
| Static Analysis | 실행 없이 분석 |
| Runtime Test | 실행 환경 테스트 |
| Regression Test | 기존 기능 유지 확인 |
| Code Audit | 코드 품질 검토 |
| Evidence | 코드 근거 |
| Finding | 발견 사항 |
| Recommendation | 개선 제안 |

---

# 9. Status Terms

| Status | Meaning |
|---------|---------|
| VERIFIED | 확인 완료 |
| PARTIALLY VERIFIED | 일부 확인 |
| NEEDS RUNTIME TEST | 실행 확인 필요 |
| ISSUE | 문제 발견 |
| NOT APPLICABLE | 해당 없음 |

---

# 10. Risk Levels

| Level | Meaning |
|---------|----------|
| Critical | 즉시 수정 필요 |
| High | 높은 위험 |
| Medium | 개선 권장 |
| Low | 영향 적음 |
| None | 위험 없음 |

---

# 11. Prompt Terms

| Term | Definition |
|------|------------|
| Prompt | AI 작업 명세 |
| Template | 재사용 가능한 기본 형식 |
| Workflow | 작업 절차 |
| Checklist | 완료 확인 목록 |
| Report | 분석 또는 검증 결과 |

---

# 12. Documentation Terms

| Term | Definition |
|------|------------|
| README | 프로젝트 소개 |
| Guide | 사용 가이드 |
| Specification | 명세 문서 |
| Standard | 표준 문서 |
| Roadmap | 장기 계획 |
| Requirements | 요구사항 |
| Rules | 공통 규칙 |

---

# 13. Database-Specific Terms

| Term | Definition |
|------|------------|
| SkriptHub Source | 원본 문법 데이터 |
| Source Snapshot | 특정 시점의 원본 데이터 |
| Data Builder | JSON 생성 시스템 |
| Data Cleaner | 데이터 정제 시스템 |
| Data Normalizer | 데이터 표준화 시스템 |
| Version Tracker | 변경 이력 추적 시스템 |
| Update Pipeline | 자동 업데이트 과정 |

---

# 14. AI Engineering Terms

| Term | Definition |
|------|------------|
| Evidence-Based Analysis | 코드 근거 기반 분석 |
| Project Understanding | 프로젝트 전체 이해 |
| Impact Analysis | 변경 영향 분석 |
| Traceability | 근거 추적 가능성 |
| Maintainability | 유지보수성 |
| Extensibility | 확장성 |
| Reusability | 재사용성 |

---

# 15. Deprecated Terms

다음 표현은 사용하지 않는다.

| Deprecated | Use Instead |
|------------|-------------|
| Check | Verification |
| Analyze Code Quickly | Project Analysis |
| Runtime Verification | Runtime Test |
| Guess | Evidence-Based Analysis |
| Plugin Data | DataPack |

---

# 16. Abbreviations

| Abbreviation | Meaning |
|--------------|---------|
| AI | Artificial Intelligence |
| LSP | Language Server Protocol |
| API | Application Programming Interface |
| JSON | JavaScript Object Notation |
| AST | Abstract Syntax Tree |
| CI | Continuous Integration |
| DoD | Definition of Done |
| PR | Pull Request |

---

# 17. Naming Conventions

모든 문서는 다음 표기법을 따른다.

```
Project Analysis
Architecture Review
Runtime Test
DataPack
Manifest
Registry
Verification
```

다음 표기는 사용하지 않는다.

```
Project Analyze
Runtime Verification
Addon Package
Plugin Database
```

---

# 18. Maintenance

새로운 용어가 추가될 경우 다음 문서를 함께 검토한다.

- STYLE_GUIDE.md
- PROMPT_STANDARD.md
- REPORT_STANDARD.md
- RULES.md

---

# 19. Document Information

| Item | Value |
|------|-------|
| Document | GLOSSARY.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

GLOSSARY.md는 VSkript AI Engineering Kit에서 사용하는 모든 핵심 용어를 표준화하기 위한 문서이다.

모든 프롬프트, 문서 및 보고서는 본 용어집을 기준으로 작성되며, 이를 통해 프로젝트 전반의 일관성과 이해도를 유지한다.
