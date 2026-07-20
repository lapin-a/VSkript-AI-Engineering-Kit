# TEMPLATE_LIST.md

# VSkript AI Engineering Kit
## Template & Document Index

Version: 2.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 관리하는 모든 문서, Template, Prompt, Checklist의 공식 인덱스를 정의한다.

새로운 문서가 추가되거나 삭제되는 경우 반드시 본 문서를 수정해야 한다.

---

# 2. Categories

프로젝트는 다음 카테고리로 문서를 관리한다.

- Core Documents
- Architecture
- Specifications
- Prompts
- Reports
- Templates
- Checklists
- Examples
- Reference

---

# 3. Core Documents

| Document | Description |
|-----------|-------------|
| README.md | 프로젝트 소개 |
| PROJECT.md | 프로젝트 목표 및 범위 |
| REQUIREMENTS.md | 요구사항 정의 |
| ARCHITECTURE.md | 시스템 아키텍처 |
| WORKFLOW.md | 표준 Workflow |
| RULES.md | 프로젝트 규칙 |
| AI_GUIDELINES.md | AI 행동 규칙 |
| STYLE_GUIDE.md | 문서 작성 규칙 |
| QUALITY_GATE.md | 품질 기준 |
| DIRECTORY.md | 표준 디렉터리 |
| PROMPT_STANDARD.md | Prompt 작성 표준 |
| REPORT_STANDARD.md | Report 작성 표준 |
| GLOSSARY.md | 용어 사전 |
| CHANGELOG.md | 변경 이력 |
| CONTRIBUTING.md | 기여 가이드 |

---

# 4. Specification Documents

```text
SPEC/

SPEC-0001

SPEC-0002

...

SPEC-9999
```

Specification은 기능의 Source of Truth이다.

---

# 5. Prompt Library

```text
prompts/

analysis/

development/

verification/

documentation/

maintenance/

shared/
```

---

## Analysis Prompt

- Project Analysis
- Architecture Review
- Dependency Analysis
- Performance Analysis

---

## Development Prompt

- Feature Implementation
- Refactoring
- Builder
- Registry

---

## Verification Prompt

- Static Verification
- Runtime Verification
- Regression Test
- Security Review

---

## Documentation Prompt

- README
- Specification
- API
- Report

---

# 6. Report Library

```text
reports/

analysis/

verification/

audit/

runtime/

release/
```

대표 Report

- Analysis Report
- Verification Report
- Audit Report
- Runtime Report
- Final Report

---

# 7. Template Library

```text
templates/

document/

report/

prompt/

specification/

review/
```

대표 Template

- Specification Template
- Prompt Template
- Report Template
- Review Template
- Checklist Template

---

# 8. Checklist Library

```text
checklists/

analysis/

development/

verification/

release/
```

대표 Checklist

- Analysis Checklist
- Runtime Checklist
- Review Checklist
- Release Checklist

---

# 9. Examples

```text
examples/

prompt/

report/

specification/
```

예제는 학습용 문서이다.

---

# 10. Reference Documents

Reference는 프로젝트 전반에서 사용하는 참고 문서이다.

예)

- RFC
- External Specification
- Standards

---

# 11. Document Relationships

```text
PROJECT
    │
    ▼
REQUIREMENTS
    │
    ▼
ARCHITECTURE
    │
    ▼
SPECIFICATIONS
    │
    ▼
PROMPTS
    │
    ▼
REPORTS
```

---

# 12. Update Rules

새로운 문서를 추가하면 반드시 수정한다.

- DIRECTORY.md
- TEMPLATE_LIST.md
- CHANGELOG.md

필요한 경우

- ROADMAP.md

---

# 13. Naming Rules

문서는 다음 규칙을 따른다.

```
UPPER_SNAKE_CASE.md
```

예)

```
PROJECT.md

QUALITY_GATE.md

PROMPT_STANDARD.md
```

Specification

```
SPEC-0001.md
```

RFC

```
RFC-0001.md
```

---

# 14. Lifecycle

모든 문서는 다음 생명주기를 가진다.

```text
Draft

↓

Review

↓

Approved

↓

Published

↓

Deprecated
```

---

# 15. Version Management

모든 문서는 Version을 가진다.

예)

```
Version: 2.0

Status: Draft
```

---

# 16. Definition of Done

문서는 다음 조건을 만족해야 한다.

□ Template 적용

□ Version 존재

□ Status 존재

□ Summary 존재

□ Document Information 존재

□ Template List 등록 완료

---

# 17. Summary

Template List는 VSkript AI Engineering Kit에서 관리하는 모든 문서, Prompt, Template, Report, Checklist의 공식 Master Index이다.

프로젝트의 모든 문서는 본 인덱스를 기준으로 관리하며, 새로운 문서가 추가되거나 삭제될 경우 반드시 함께 갱신해야 한다.