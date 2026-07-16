# STYLE_GUIDE.md

# VSkript AI Engineering Kit

> Documentation and Prompt Style Guide

Version: 1.0

---

# 1. Purpose

본 문서는 VSkript AI Engineering Kit에서 생성되는 모든 문서, 프롬프트, 보고서 및 템플릿의 작성 규칙을 정의한다.

목표는 다음과 같다.

- 일관된 문서 품질 유지
- AI 간 동일한 결과 생성
- GitHub 친화적인 Markdown 작성
- 유지보수성 향상

---

# 2. Writing Principles

모든 문서는 다음 원칙을 따른다.

## Clarity

문장은 간결하고 명확해야 한다.

좋은 예

> Analyze the project structure before implementation.

나쁜 예

> Maybe it would be better to analyze something first.

---

## Consistency

같은 용어는 항상 동일하게 사용한다.

예

항상

- Project Analysis
- Architecture Review
- Runtime Test

사용

혼용 금지

- Analyze Project
- Architecture Check
- Runtime Verification

---

## Reusability

특정 프로젝트에 종속되지 않도록 작성한다.

---

## Maintainability

문서는 쉽게 수정 가능해야 한다.

---

# 3. Language Rules

기본 언어

한국어 (제목 및 표준 용어는 영어 사용)

설명

Heading, Prompt/Report 표준 용어(예: Purpose, Scope, Evidence Policy 등)는 영어를 유지하고, 설명 본문은 한국어로 작성한다.

---

# 4. Markdown Rules

모든 문서는 Markdown을 사용한다.

반드시 포함

- Heading
- Table
- Bullet List
- Numbered List
- Code Block

---

## Heading Rules

```
# Document Title

## Section

### Subsection
```

4단계 이상은 지양한다.

---

# 5. Code Block Rules

언어를 반드시 지정한다.

예

```ts
function activate() {}
```

```json
{}
```

```text
Workflow
```

---

# 6. Table Rules

표는 동일한 형식을 유지한다.

예

| Item | Description |
|------|-------------|

---

# 7. Terminology

| Term | Definition |
|------|------------|
| Provider | LSP 기능 제공 모듈 |
| Registry | 등록 시스템 |
| Manifest | 메타데이터 |
| DataPack | 개별 데이터 패키지 |
| Adapter | 호환 계층 |

---

# 8. Prompt Style

모든 Prompt는 다음 순서를 따른다.

1. Purpose
2. Scope
3. Role
4. Required Skills
5. Input
6. Rules
7. Workflow
8. Evidence Policy
9. Verification Policy
10. Output
11. Quality Checklist

---

# 9. Report Style

모든 보고서는 아래 순서를 따른다.

1. Executive Summary
2. Findings
3. Evidence
4. Risk Assessment
5. Recommendations
6. Next Steps

---

# 10. Naming Convention

문서

```
Project_Analysis.md
```

Prompt

```
01_Project_Analysis.md
```

Template

```
Verification_Template.md
```

---

# 11. File Naming Rules

사용

```
Pascal_Case.md
```

금지

```
projectanalysis.md
```

```
ProjectAnalysis.md
```

---

# 12. Versioning

형식

```
Major.Minor
```

예

```
1.0

1.1

2.0
```

---

# 13. Example Structure

```
# Title

Purpose

Scope

Workflow

Output

Checklist
```

---

# 14. Quality Checklist

모든 문서는 다음을 만족해야 한다.

□ Markdown 표준 준수

□ 제목 존재

□ 목적 존재

□ 버전 존재

□ 표 사용

□ 예제 포함

□ 체크리스트 포함

---

# 15. Document Information

| Item | Value |
|------|-------|
| Document | STYLE_GUIDE.md |
| Version | 1.0 |
| Status | Draft |
| Owner | VSkript AI Engineering Kit |
| Last Updated | YYYY-MM-DD |

---

# Summary

STYLE_GUIDE.md는 VSkript AI Engineering Kit의 문서 및 프롬프트 작성 규칙을 정의한다.

모든 문서는 본 가이드를 기준으로 작성된다.
