# STYLE_GUIDE.md

# VSkript Data Platform Style Guide

> VSkript Data Platform

Version: 1.0

Status: Draft

---

# 1. Purpose

본 문서는 VSkript Data Platform 프로젝트에서 작성되는 모든 문서의 작성 기준을 정의한다.

프로젝트의 모든 문서는 동일한 형식과 용어를 사용하여 일관성을 유지해야 한다.

---

# 2. Scope

본 문서는 다음 문서에 적용된다.

- README
- PROJECT
- REQUIREMENTS
- ARCHITECTURE
- WORKFLOW
- RULES
- AI_GUIDELINES
- SPEC
- RFC
- ADR
- Prompt
- Report
- Template
- Checklist

---

# 3. Design Philosophy

문서는 다음 원칙을 따른다.

- Specification First
- Documentation First
- Consistency First
- Simplicity
- Maintainability
- Reusability

---

# 4. Writing Principles

모든 문서는 다음 원칙을 따른다.

- 명확하게 작성한다.
- 불필요한 설명을 제거한다.
- 하나의 문장은 하나의 의미만 가진다.
- 중복 설명을 최소화한다.
- 구현보다 개념을 먼저 설명한다.

---

# 5. Markdown Rules

문서는 Markdown으로 작성한다.

사용 가능한 요소

- Heading
- Table
- List
- Code Block
- Quote

HTML 사용은 권장하지 않는다.

---

# 6. Heading Rules

Heading은 계층 구조를 유지한다.

예시

```text
#

##

###

####
```

Heading Level을 건너뛰지 않는다.

---

# 7. Paragraph Rules

문단은 짧고 명확하게 작성한다.

하나의 문단은 하나의 주제를 설명한다.

---

# 8. List Rules

목록은 동일한 기준으로 작성한다.

예시

- Collector
- Builder
- Registry
- Runtime

---

# 9. Table Rules

비교가 필요한 경우 Table을 사용한다.

예시

| Item | Description |
|------|-------------|
| Registry | Source of Truth |

---

# 10. Code Block Rules

예제는 Code Block으로 작성한다.

언어가 있으면 지정한다.

예시

```text
Collector

↓

Normalizer

↓

Builder
```

---

# 11. Diagram Rules

Architecture는 Text Diagram을 사용한다.

예시

```text
Collector

↓

Canonical Dataset

↓

Builder

↓

Registry
```

---

# 12. Language

설명은 한국어를 기본으로 한다.

필요한 기술 용어는 영어를 그대로 사용한다.

예시

- Registry
- Runtime
- DataPack
- Builder
- Collector

---

# 13. Terminology

용어는 반드시 GLOSSARY를 따른다.

새로운 용어를 임의로 만들지 않는다.

---

# 14. Naming Rules

문서 이름은 대문자를 사용한다.

예시

```
PROJECT.md
WORKFLOW.md
RULES.md
```

---

# 15. Versioning

모든 문서는 Version을 가진다.

예시

```
Version: 1.0
```

---

# 16. Status

문서는 상태를 명시한다.

예시

```
Draft

Review

Approved

Deprecated
```

---

# 17. Document Header

모든 문서는 다음 Header를 가진다.

```
Document Name

Project

Version

Status
```

---

# 18. Document Footer

모든 문서는 Document Information을 포함한다.

예시

| Item | Value |
|------|-------|
| Document | STYLE_GUIDE.md |
| Version | 1.0 |
| Status | Draft |

---

# 19. Document Structure

권장 구조

1. Purpose

2. Scope

3. Overview

4. Main Contents

5. Summary

6. Document Information

---

# 20. Examples

예시는 실제 프로젝트 기준으로 작성한다.

가상의 구조를 사용하지 않는다.

---

# 21. Specification References

Specification을 참조할 경우 문서명을 명시한다.

예시

SPEC-0001

RFC-0002

---

# 22. Architecture References

Architecture를 설명할 때는 ARCHITECTURE.md와 일치해야 한다.

---

# 23. Workflow References

Workflow는 WORKFLOW.md를 기준으로 한다.

---

# 24. Rule References

행동 규칙은 RULES.md를 기준으로 한다.

---

# 25. AI References

AI 관련 설명은 AI_GUIDELINES.md를 따른다.

---

# 26. Prompt References

Prompt는 PROMPT_STANDARD.md를 따른다.

---

# 27. Report References

Report는 REPORT_STANDARD.md를 따른다.

---

# 28. Directory References

폴더 구조는 DIRECTORY.md를 따른다.

---

# 29. Examples Policy

예시는 최소한으로 작성한다.

예시는 실제 프로젝트를 기준으로 한다.

---

# 30. Reusability

문서는 특정 AI나 특정 IDE에 종속되지 않도록 작성한다.

---

# 31. Maintainability

문서는 쉽게 수정할 수 있도록 작성한다.

중복 설명을 최소화한다.

---

# 32. Consistency

동일한 개념은 항상 동일한 용어를 사용한다.

예시

Registry

Runtime

DataPack

Canonical Dataset

---

# 33. Simplicity

복잡한 문장은 피한다.

짧고 명확하게 작성한다.

---

# 34. Transparency

설명은 명확해야 한다.

생략이나 암시를 최소화한다.

---

# 35. Documentation First

구현보다 문서를 먼저 수정한다.

---

# 36. Specification First

구현보다 Specification이 우선한다.

---

# 37. Summary

STYLE_GUIDE는 VSkript Data Platform에서 작성되는 모든 문서의 형식과 표현을 표준화하기 위한 기준이다.

모든 문서는 본 Style Guide를 준수해야 하며, 프로젝트 전반에서 동일한 구조와 용어를 유지해야 한다.

---

# Document Information

| Item | Value |
|------|-------|
| Document | STYLE_GUIDE.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript Data Platform |