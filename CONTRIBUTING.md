# CONTRIBUTING.md

# Contributing Guide

> VSkript AI Engineering Kit

Version: 1.0

---

# Welcome

VSkript AI Engineering Kit에 기여해주셔서 감사합니다.

본 프로젝트는 사람과 AI가 함께 개발하는 AI Engineering Framework입니다.

따라서 모든 기여는 프로젝트의 표준 문서와 Workflow를 따라야 합니다.

---

# Project Philosophy

우리는 다음 원칙을 중요하게 생각합니다.

- Evidence First
- No Guessing
- Maintainability
- Reproducibility
- Documentation First

---

# Before Contributing

작업을 시작하기 전에 반드시 다음 문서를 읽어야 합니다.

1. RULES.md
2. PROJECT.md
3. REQUIREMENTS.md
4. WORKFLOW.md
5. AI_GUIDELINES.md
6. STYLE_GUIDE.md
7. QUALITY_GATE.md

---

# Contribution Types

다음 유형의 기여를 환영합니다.

## Documentation

- README 개선
- Guide 작성
- Template 작성

---

## Prompt

- Analysis Prompt
- Development Prompt
- Verification Prompt
- Report Prompt

---

## Workflow

- 새로운 Workflow 작성
- Workflow 개선

---

## Templates

- Report Template
- Checklist
- Prompt Template

---

## Examples

- Prompt Examples
- Report Examples
- Verification Examples

---

# Development Workflow

모든 작업은 아래 순서를 따른다.

```text
Fork Repository

↓

Read Project Documents

↓

Select Workflow

↓

Create Branch

↓

Analyze

↓

Implement

↓

Verify

↓

Update Documentation

↓

Pull Request
```

---

# Branch Naming

기능

```
feature/addon-datapack
```

버그

```
fix/runtime-loading
```

문서

```
docs/workflow
```

Prompt

```
prompt/project-analysis
```

---

# Commit Message

Conventional Commits를 사용한다.

예시

```
feat(database): add datapack registry

fix(runtime): resolve manifest loading

docs(readme): update installation guide

refactor(provider): simplify cache logic
```

---

# Pull Request Requirements

PR에는 반드시 포함되어야 한다.

- 변경 목적
- 변경 내용
- 영향 범위
- 수행한 Workflow Phase
- Verification 결과
- 관련 문서 수정 여부

---

# Documentation Requirements

기능 변경 시 반드시 관련 문서를 수정한다.

예

- README
- REQUIREMENTS
- WORKFLOW
- DIRECTORY
- TEMPLATE_LIST

---

# Prompt Contribution Rules

새 Prompt 작성 시 반드시 다음 문서를 따른다.

- PROMPT_STANDARD.md
- RULES.md
- QUALITY_GATE.md

---

# Verification Requirements

모든 기여는 다음 검증을 통과해야 한다.

- Static Verification
- Functional Verification
- Documentation Review
- QUALITY_GATE.md의 Definition of Done

필요 시

- Runtime Test

---

# Quality Requirements

기여 전 확인한다.

□ Evidence 포함

□ 추측 없음

□ Workflow Selection 확인

□ 영향도 분석

□ Documentation 수정

□ Prompt Standard 준수

□ GLOSSARY 용어 사용

□ Markdown 검사 완료

---

# AI Contribution

AI는 최종 설계 결정을 내리지 않는다.

모든 최종 승인과 판단은 Maintainer가 수행한다.

AI는

- 코드 생성
- 문서 작성
- Prompt 생성
- 보고서 작성

을 지원하지만, 최종 승인 권한은 Maintainer에게 있다.

---

# Review Process

모든 Pull Request는 아래 절차를 따른다.

```text
Submit PR

↓

Review

↓

Request Changes

↓

Verification

↓

Quality Gate

↓

Approval

↓

Merge
```

---

# Code of Conduct

모든 참여자는 다음 원칙을 따른다.

- Respect
- Collaboration
- Transparency
- Evidence-Based Discussion

---

# Issue Reporting

Issue 작성 시 포함한다.

- Summary
- Workflow Phase
- Environment
- Steps to Reproduce
- Expected Behavior
- Actual Behavior
- Evidence

---

# Feature Request

기능 제안 시 포함한다.

- Problem
- Proposal
- Expected Benefit
- Affected Documents
- Alternatives
- Additional Notes

---

# Updating Documentation

새로운 문서를 추가한 경우 반드시 수정한다.

- DIRECTORY.md
- TEMPLATE_LIST.md
- ROADMAP.md

---

# Release Requirements

릴리즈 전 확인한다.

□ 모든 Prompt 검증

□ 모든 문서 최신화

□ CONTRACT_AUTHORING_GUIDE 준수

□ Changelog 작성

□ Quality Gate 통과

---

# Document Information

| Item | Value |
|------|-------|
| Document | CONTRIBUTING.md |
| Version | 1.0 |
| Status | Stable |
| Owner | VSkript AI Engineering Kit |

---

# Summary

CONTRIBUTING.md는 VSkript AI Engineering Kit의 기여 절차를 정의한다.

모든 기여자는 프로젝트의 표준 문서와 Workflow를 준수해야 하며, AI가 생성한 결과 역시 동일한 품질 기준과 검증 절차를 따라야 한다.
